---
title: Desenvolver um trabalhador de metadados do Asset Compute
description: Saiba como criar um trabalhador de metadados do Asset Compute que deriva as cores mais usadas em um ativo de imagem e grava os nomes das cores de volta nos metadados do ativo no AEM.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 6ece6e82-efe9-41eb-adf8-78d9deed131e
duration: 432
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1405'
ht-degree: 0%

---

# Desenvolver um trabalhador de metadados do Asset Compute

Os funcionários personalizados da Asset Compute podem produzir dados do XMP (XML) que são enviados de volta para o AEM e armazenados como metadados em um ativo.

Casos de uso comuns incluem:

+ Integrações com sistemas de terceiros, como um PIM (Product Information Management System), em que os metadados adicionais devem ser recuperados e armazenados no ativo
+ Integrações com os serviços da Adobe, como IA de conteúdo e Commerce, para aumentar os metadados de ativos com atributos adicionais de aprendizado de máquina
+ Derivar metadados sobre o ativo de seu binário e armazená-lo como metadados de ativos no AEM as a Cloud Service

## O que você fará

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

Neste tutorial, criaremos um trabalhador de metadados do Asset Compute que deriva as cores mais usadas em um ativo de imagem e grava os nomes das cores de volta nos metadados do ativo no AEM. Embora o próprio trabalhador seja básico, este tutorial o usa para explorar como os trabalhadores do Asset Compute podem ser usados para gravar metadados nos ativos no AEM as a Cloud Service.

## Fluxo lógico de uma invocação de trabalho de metadados do Asset Compute

A invocação de trabalhadores de metadados do Asset Compute é quase idêntica à de [trabalhadores geradores de representação binária](../develop/worker.md), sendo que a diferença principal é o tipo de retorno de uma representação do XMP (XML) cujos valores também são gravados nos metadados do ativo.

Os trabalhadores do Asset Compute implementam o contrato de API do trabalhador do Asset Compute SDK, na função `renditionCallback(...)`, que é conceitualmente:

+ __Entrada:__ Os parâmetros binário e perfil de processamento originais de um ativo do AEM
+ __Saída:__ uma representação XMP (XML) persistiu no ativo AEM como uma representação e nos metadados do ativo

![Fluxo lógico do trabalhador de metadados do Asset Compute](./assets/metadata/logical-flow.png)

1. O serviço do Autor do AEM invoca o trabalhador de metadados do Asset Compute, fornecendo o binário original __(1a)__ do ativo e __(1b)__ quaisquer parâmetros definidos no Perfil de Processamento.
1. O Asset Compute SDK orquestra a execução da função `renditionCallback(...)` do trabalhador de metadados Asset Compute personalizado, derivando uma representação de XMP (XML), com base no binário __(1a)__ do ativo e em qualquer parâmetro do Perfil de Processamento __(1b)__.
1. O trabalhador do Asset Compute salva a representação do XMP (XML) em `rendition.path`.
1. Os dados do XMP (XML) gravados em `rendition.path` são transportados por meio do Asset Compute SDK para o Serviço de Autor do AEM e os expõem como __(4a)__ uma representação de texto e __(4b)__ persistentes para o nó de metadados do ativo.

## Configurar o manifest.yml{#manifest}

Todos os trabalhadores do Asset Compute devem estar registrados no [manifest.yml](../develop/manifest.md).

Abra o `manifest.yml` do projeto e adicione uma entrada de trabalho que configure o novo trabalhador, neste caso `metadata-colors`.

_Lembre-se de que `.yml` diferencia espaços em branco._

```
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
      metadata-colors:
        function: actions/metadata-colors/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          memorySize: 512 # in MB   
```

`function` aponta para a implementação de trabalho criada na [próxima etapa](#metadata-worker). Nomeie os trabalhadores semanticamente (por exemplo, `actions/worker/index.js` pode ter sido melhor nomeado como `actions/rendition-circle/index.js`), como eles mostram na [URL do trabalhador](#deploy) e também determinam o [nome da pasta do conjunto de testes do trabalhador](#test).

O `limits` e o `require-adobe-auth` são configurados discretamente por trabalhador. Neste trabalho, `512 MB` de memória está alocado à medida que o código inspeciona (potencialmente) dados de imagem binária grandes. Os outros `limits` são removidos para usar os padrões.

## Desenvolver um trabalhador de metadados{#metadata-worker}

Crie um novo arquivo JavaScript do trabalhador de metadados no projeto do Asset Compute no caminho [defined manifest.yml para o novo trabalhador](#manifest), em `/actions/metadata-colors/index.js`

### Instalar módulos npm

Instale os módulos npm extras ([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-colors](https://www.npmjs.com/package/get-image-colors) e [color-namer](https://www.npmjs.com/package/color-namer)) usados neste trabalhador do Asset Compute.

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### Código de trabalhador de metadados

Este trabalhador é muito semelhante ao [trabalhador gerador de representação](../develop/worker.md). A principal diferença é que ele grava dados do XMP (XML) no `rendition.path` para ser salvo novamente no AEM.


```javascript
"use strict";

const { worker, SourceCorruptError } = require("@adobe/asset-compute-sdk");
const fs = require("fs").promises;

// Require the @adobe/asset-compute-xmp module to create XMP 
const { serializeXmp } = require("@adobe/asset-compute-xmp");

// Require supporting npm modules to derive image colors from image data
const getColors = require("get-image-colors");
// Require supporting npm modules to convert image colors to color names
const namer = require("color-namer");

exports.main = worker(async (source, rendition, params) => {
  // Perform any necessary source (input) checks
  const stats = await fs.stat(source.path);
  if (stats.size === 0) {
    // Throw appropriate errors whenever an erring condition is met
    throw new SourceCorruptError("source file is empty");
  }
  const MAX_COLORS = 10;
  const DEFAULT_COLORS_FAMILY = 'basic';

  // Read the color family parameter to use to derive the color names
  let colorsFamily = rendition.instructions.colorsFamily || DEFAULT_COLORS_FAMILY;

  if (['basic', 'hex', 'html', 'ntc', 'pantone', 'roygbiv'].indexOf(colorsFamily) === -1) { 
      colorsFamily = DEFAULT_COLORS_FAMILY;
  }
  
  // Use the `get-image-colors` module to derive the most common colors from the image
  let colors = await getColors(source.path, { options: MAX_COLORS });

  // Convert the color Chroma objects to their closest names
  let colorNames = colors.map((color) => getColorName(colorsFamily, color));

  // Serialize the data to XMP metadata
  // These properties are written to the [dam:Asset]/jcr:content/metadata resource
  // This stores
  // - The list of color names is stored in a JCR property named `wknd:colors`
  // - The colors family used to derive the color names is stored in a JCR property named `wknd:colorsFamily`
  const xmp = serializeXmp({
      // Use a Set to de-duplicate color names
      "wknd:colors": [...new Set(colorNames)],
      "wknd:colorsFamily": colorsFamily
    }, {
      // Define any property namespaces used in the above property/value definition
      // These namespaces are automatically registered in AEM if they do not yet exist
      namespaces: {
        wknd: "https://wknd.site/assets/1.0/",
      },
    }
  );

  // Save the XMP metadata to be written back to the asset's metadata node
  await fs.writeFile(rendition.path, xmp, "utf-8");
});

/**
 * Helper function that derives the closest color name for the color, based on the colors family
 * 
 * @param {*} colorsFamily the colors name family to use
 * @param {*} color the color to convert to a name
 */
function getColorName(colorsFamily, color) {
    if ('hex' === colorsFamily) {  return color; }

    let names = namer(color.rgb())[colorsFamily];

    if (names.length >= 1) { return names[0].name; }
}
```

## Executar o trabalho de metadados localmente{#development-tool}

Com o código do trabalhador concluído, ele pode ser executado usando a Ferramenta de desenvolvimento Asset Compute local.

Como nosso projeto do Asset Compute contém dois trabalhadores (a [representação de círculo](../develop/worker.md) anterior e este `metadata-colors` trabalhador), a [definição de perfil da ](../develop/development-tool.md) da Ferramenta de Desenvolvimento do Asset Compute lista perfis de execução para ambos os trabalhadores. A segunda definição de perfil aponta para o novo trabalhador `metadata-colors`.

![Representação de metadados XML](./assets/metadata/metadata-rendition.png)

1. Na raiz do projeto Asset Compute
1. Executar `aio app run` para iniciar a Ferramenta de Desenvolvimento do Asset Compute
1. Na lista suspensa __Selecionar um arquivo...__, escolha uma [imagem de exemplo](../assets/samples/sample-file.jpg) para processar
1. Na segunda configuração de definição de perfil, que aponta para o trabalhador `metadata-colors`, atualize `"name": "rendition.xml"`, pois esse trabalhador gera uma representação XMP (XML). Opcionalmente, adicione um parâmetro `colorsFamily` (valores com suporte `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`).

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.xml",
               "colorsFamily": "pantone"
           }
       ]
   }
   ```

1. Toque em __Executar__ e aguarde a geração da representação XML
   + Como ambos os trabalhadores estão listados na definição do perfil, ambas as representações serão geradas. Como opção, a definição do perfil superior apontando para o [trabalhador de representação de círculo](../develop/worker.md) pode ser excluída, para evitar sua execução a partir da Ferramenta de desenvolvimento.
1. A seção __Representações__ pré-visualiza a representação gerada. Toque em `rendition.xml` para baixá-lo e abri-lo no Código VS (ou no seu editor de texto/XML favorito) para examinar.

## Testar o trabalhador{#test}

Os trabalhadores de metadados podem ser testados usando a [mesma estrutura de testes do Asset Compute que as representações binárias](../test-debug/test.md). A única diferença é que o arquivo `rendition.xxx` no caso de teste deve ser a representação XMP (XML) esperada.

1. Crie a seguinte estrutura no projeto do Asset Compute:

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. Use o [arquivo de amostra](../assets/samples/sample-file.jpg) como o `file.jpg` do caso de teste.
3. Adicione o seguinte JSON ao `params.json`.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   Observe que `"fmt": "xml"` é necessário para instruir o conjunto de testes a gerar uma representação baseada em texto `.xml`.

4. Forneça o XML esperado no arquivo `rendition.xml`. Isso pode ser obtido por:
   + Executar o arquivo de entrada de teste por meio da Ferramenta de desenvolvimento e salvar a representação XML (validada).

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. Execute `aio app test` a partir da raiz do projeto Asset Compute para executar todos os conjuntos de testes.

### Implantar o trabalhador no Adobe I/O Runtime{#deploy}

Para chamar esse novo trabalhador de metadados do AEM Assets, ele deve ser implantado no Adobe I/O Runtime, usando o comando:

```
$ aio app deploy
```

![implantação do aplicativo aio](./assets/metadata/aio-app-deploy.png)

Observe que isso implantará todos os trabalhadores no projeto. Revise as [instruções de implantação não abreviada](../deploy/runtime.md) para saber como implantar em espaços de trabalho de Preparo e Produção.

### Integrar a perfis de processamento do AEM{#processing-profile}

Chame o trabalhador no AEM criando um novo serviço de perfil de processamento personalizado ou modificando um já existente que chame esse trabalhador implantado.

![Processando perfil](./assets/metadata/processing-profile.png)

1. Faça login no serviço de Autor do AEM as a Cloud Service como um __Administrador do AEM__
1. Navegue até __Ferramentas > Assets > Processando Perfis__
1. __Criar__ um novo, ou __editar__ e um perfil de processamento existente
1. Toque na guia __Personalizado__ e em __Adicionar novo__
1. Definir o novo serviço
   + __Criar representação de metadados__: alternar para ativo
   + __Ponto de extremidade:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + Esta é a URL para o trabalhador obtida durante a [implantação](#deploy) ou usando o comando `aio app get-url`. Verifique se o URL aponta para o espaço de trabalho correto com base no ambiente do AEM as a Cloud Service.
   + __Parâmetros de serviço__
      + Toque em __Adicionar parâmetro__
         + Chave: `colorFamily`
         + Valor: `pantone`
            + Valores com suporte: `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __Tipos MIME__
      + __Inclui:__ `image/jpeg`, `image/png`, `image/gif`, `image/svg`
         + Esses são os únicos tipos MIME aceitos pelos módulos npm de terceiros usados para derivar as cores.
      + __Exclusões:__ `Leave blank`
1. Toque em __Salvar__ na parte superior direita
1. Aplicar o perfil de processamento a uma pasta do AEM Assets se ainda não tiver sido feito

### Atualizar o esquema de metadados{#metadata-schema}

Para revisar os metadados de cores, mapeie dois novos campos no esquema de metadados da imagem para as novas propriedades de dados de metadados que o trabalhador preenche.

![Esquema de metadados](./assets/metadata/metadata-schema.png)

1. No serviço de Autor do AEM, navegue até __Ferramentas > Assets > Esquemas de metadados__
1. Navegue até __padrão__, selecione e edite a __imagem__ e adicione campos de formulário somente leitura para expor os metadados de cor gerados
1. Adicionar um __Texto de uma Linha__
   + __Rótulo do campo__: `Colors Family`
   + __Mapear para a propriedade__: `./jcr:content/metadata/wknd:colorsFamily`
   + __Regras > Campo > Desabilitar edição__: marcado
1. Adicionar um __Texto de Vários Valores__
   + __Rótulo do campo__: `Colors`
   + __Mapear para a propriedade__: `./jcr:content/metadata/wknd:colors`
1. Toque em __Salvar__ na parte superior direita

## Processamento de ativos

![Detalhes do ativo](./assets/metadata/asset-details.png)

1. No serviço de Autor do AEM, navegue até __Assets > Arquivos__
1. Navegue até a pasta ou subpasta à qual o Perfil de Processamento é aplicado
1. Carregue uma nova imagem (JPEG, PNG, GIF ou SVG) para a pasta ou processe novamente as imagens existentes usando o [Perfil de Processamento](#processing-profile) atualizado
1. Quando o processamento estiver concluído, selecione o ativo e toque em __propriedades__ na barra de ação superior para exibir seus metadados
1. Revise os `Colors Family` e `Colors` [campos de metadados](#metadata-schema) para obter os metadados gravados do trabalho de metadados personalizado do Asset Compute.

Com os metadados de cores gravados nos metadados do ativo, no recurso `[dam:Asset]/jcr:content/metadata`, esses metadados são indexados por uma maior capacidade de descoberta de ativos usando esses termos por meio de pesquisa, e eles podem até ser gravados no binário do ativo se o fluxo de trabalho __Writeback de metadados DAM__ for chamado nele.

### Representação de metadados no AEM Assets

![Arquivo de representação de metadados do AEM Assets](./assets/metadata/cqdam-metadata-rendition.png)

O arquivo XMP real gerado pelo trabalhador de metadados do Asset Compute também é armazenado como uma representação discreta no ativo. Esse arquivo geralmente não é usado, em vez disso, os valores aplicados ao nó de metadados do ativo são usados, mas a saída XML bruta do worker está disponível no AEM.

## código de trabalho de cores de metadados no Github

O `metadata-colors/index.js` final está disponível no Github em:

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

O conjunto de testes `test/asset-compute/metadata-colors` final está disponível no Github em:

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
