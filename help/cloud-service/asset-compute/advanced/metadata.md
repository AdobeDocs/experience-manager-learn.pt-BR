---
title: Desenvolver um trabalhador de metadados do Asset compute
description: Saiba como criar um trabalhador de metadados de Asset compute que deriva as cores mais usadas em um ativo de imagem e grava os nomes das cores de volta nos metadados do ativo no AEM.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 6ece6e82-efe9-41eb-adf8-78d9deed131e
duration: 461
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1405'
ht-degree: 0%

---

# Desenvolver um trabalhador de metadados do Asset compute

Os trabalhadores de Assets compute personalizados podem produzir dados XMP (XML) que são enviados de volta para AEM e armazenados como metadados em um ativo.

Casos de uso comuns incluem:

+ Integrações com sistemas de terceiros, como um PIM (Product Information Management System), em que os metadados adicionais devem ser recuperados e armazenados no ativo
+ Integrações com os serviços da Adobe, como IA de conteúdo e comércio, para aumentar os metadados de ativos com atributos adicionais de aprendizado de máquina
+ Derivar metadados sobre o ativo de seu binário e armazená-lo como metadados de ativos no AEM as a Cloud Service

## O que você fará

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

Neste tutorial, criaremos um trabalhador de metadados do Asset compute que deriva as cores mais usadas em um ativo de imagem e grava os nomes das cores de volta nos metadados do ativo no AEM. Embora o próprio trabalhador seja básico, este tutorial o usa para explorar como os trabalhadores do Asset compute podem ser usados para gravar metadados em ativos no AEM as a Cloud Service.

## Fluxo lógico de uma invocação de trabalho de metadados de Asset compute

A invocação de trabalhadores de metadados de Asset compute é quase idêntica à de [representação binária que gera trabalhadores](../develop/worker.md), sendo a principal diferença o tipo de retorno, é uma representação XMP (XML) cujos valores também são gravados nos metadados do ativo.

Os trabalhadores do Asset compute implementam o contrato de API do trabalhador do SDK do Asset compute, no `renditionCallback(...)` conceitualmente:

+ __Entrada:__ Parâmetros binários e de perfil de processamento originais de um ativo AEM
+ __Saída:__ Uma representação XMP (XML) persistiu no ativo AEM como uma representação e nos metadados do ativo

![Fluxo lógico do trabalhador de metadados de asset compute](./assets/metadata/logical-flow.png)

1. O serviço de Autor do AEM chama o trabalhador de metadados do Asset compute, fornecendo a __(1-A)__ binário original e __(1-B)__ quaisquer parâmetros definidos no Perfil de processamento.
1. O SDK do Asset compute orquestra a execução do trabalhador de metadados do Asset compute personalizado `renditionCallback(...)` função, derivando uma representação XMP (XML), com base no binário do ativo __(1-A)__ e qualquer parâmetro do perfil de processamento __(1-B)__.
1. O trabalhador do Asset compute salva a representação XMP (XML) em `rendition.path`.
1. Os dados do XMP (XML) gravados no `rendition.path` é transportado via SDK do Asset compute para o AEM Author Service e o expõe como __(4-A)__ uma representação de texto e __4-B)__ persistida no nó de metadados do ativo.

## Configurar o manifest.yml{#manifest}

Todos os trabalhadores Assets compute devem estar registrados na [manifest.yml](../develop/manifest.md).

Abra o `manifest.yml` e adicionar uma entrada de trabalhador que configura o novo trabalhador, neste caso `metadata-colors`.

_Lembrar `.yml` O diferencia espaços em branco._

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

`function` aponta para a implementação do trabalhador criada na [próxima etapa](#metadata-worker). Nomeie os trabalhadores semanticamente (por exemplo, o `actions/worker/index.js` pode ter sido melhor nomeado `actions/rendition-circle/index.js`), como estes mostram no [URL do trabalhador](#deploy) e também determinar a [nome da pasta do conjunto de testes do trabalhador](#test).

A variável `limits` e `require-adobe-auth` são configurados discretamente por funcionário. Neste trabalhador, `512 MB` de memória é alocada à medida que o código inspeciona (potencialmente) dados de imagens binárias grandes. O outro `limits` são removidos para usar os padrões.

## Desenvolver um trabalhador de metadados{#metadata-worker}

Crie um novo arquivo JavaScript do trabalhador de metadados no projeto do Asset compute no caminho [manifest.yml definido para o novo trabalhador](#manifest), em `/actions/metadata-colors/index.js`

### Instalar módulos npm

Instalar os módulos npm extras ([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-colors](https://www.npmjs.com/package/get-image-colors), e [color-namer](https://www.npmjs.com/package/color-namer)) que é usado neste trabalhador do Asset compute.

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### Código de trabalhador de metadados

Esse trabalhador é muito semelhante ao [trabalhador que gera representação](../develop/worker.md), a principal diferença é que ele grava dados XMP (XML) no `rendition.path` para ser salvo novamente no AEM.


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

Com o código do trabalhador concluído, ele pode ser executado usando a Ferramenta de desenvolvimento de Assets compute local.

Como nosso projeto do Asset compute contém dois workers (o anterior [representação circular](../develop/worker.md) e este `metadata-colors` trabalhador), a [Ferramentas de desenvolvimento do Asset compute](../develop/development-tool.md) a definição do perfil lista os perfis de execução de ambos os trabalhadores. A segunda definição de perfil aponta para o novo `metadata-colors` trabalhador.

![Representação de metadados XML](./assets/metadata/metadata-rendition.png)

1. Na raiz do projeto do Asset compute
1. Executar `aio app run` para iniciar a Ferramenta de desenvolvimento do Asset compute
1. No __Selecione um arquivo...__ selecione um [imagem de amostra](../assets/samples/sample-file.jpg) para processar
1. Na segunda configuração de definição de perfil, que aponta para a variável `metadata-colors` trabalhador, atualizar `"name": "rendition.xml"` conforme esse trabalhador gera uma representação XMP (XML). Opcionalmente, adicione um `colorsFamily` parâmetro do (valores suportados) `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`).

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

1. Toque __Executar__ e aguardar a geração da representação XML
   + Como ambos os trabalhadores estão listados na definição do perfil, ambas as representações serão geradas. Opcionalmente, a definição do perfil superior apontando para a variável [trabalhador de representação de círculo](../develop/worker.md) podem ser excluídos para evitar sua execução na Ferramenta de desenvolvimento.
1. A variável __Representações__ pré-visualiza a representação gerada. Toque no `rendition.xml` para baixá-lo e abri-lo no VS Code (ou no editor de texto/XML favorito) para revisão.

## Testar o trabalhador{#test}

Os trabalhadores de metadados podem ser testados usando o [mesma estrutura de teste de Asset compute que representações binárias](../test-debug/test.md). A única diferença é a `rendition.xxx` o arquivo no caso de teste deve ser a representação XMP (XML) esperada.

1. Crie a seguinte estrutura no projeto do Asset compute:

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. Use o [arquivo de amostra](../assets/samples/sample-file.jpg) como do caso de teste `file.jpg`.
3. Adicione o seguinte JSON à `params.json`.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   Observe que `"fmt": "xml"` é necessário instruir o conjunto de testes para gerar uma `.xml` representação baseada em texto.

4. Forneça o XML esperado no `rendition.xml` arquivo. Isso pode ser obtido por:
   + Executar o arquivo de entrada de teste por meio da Ferramenta de desenvolvimento e salvar a representação XML (validada).

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. Executar `aio app test` na raiz do projeto do Asset compute para executar todos os conjuntos de testes.

### Implantar o trabalhador no Adobe I/O Runtime{#deploy}

Para chamar esse novo trabalhador de metadados do AEM Assets, ele deve ser implantado no Adobe I/O Runtime, usando o comando:

```
$ aio app deploy
```

![implantação do aplicativo aio](./assets/metadata/aio-app-deploy.png)

Observe que isso implantará todos os trabalhadores no projeto. Revise o [instruções de implantação não resumida](../deploy/runtime.md) para saber como implantar em espaços de trabalho de Preparo e Produção.

### Integrar a perfis de processamento AEM{#processing-profile}

Chame o trabalhador do AEM criando um novo serviço de perfil de processamento personalizado ou modificando um já existente que chame esse trabalhador implantado.

![Processando perfil](./assets/metadata/processing-profile.png)

1. Faça logon no serviço de Autor as a Cloud Service do AEM como um __Administrador AEM__
1. Navegue até __Ferramentas > Ativos > Perfis de processamento__
1. __Criar__ um novo, ou __editar__ e existente, Processando perfil
1. Toque no __Personalizado__ e toque em __Adicionar novo__
1. Definir o novo serviço
   + __Criar representação de metadados__: alternar para ativo
   + __Ponto de extremidade:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + Este é o URL para o trabalhador obtido durante a [implantar](#deploy) ou usando o comando `aio app get-url`. Verifique se o URL aponta para o espaço de trabalho correto com base no ambiente as a Cloud Service AEM.
   + __Parâmetros de serviço__
      + Toque __Adicionar parâmetro__
         + Chave: `colorFamily`
         + Valor: `pantone`
            + Valores compatíveis: `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __Tipos de Mime__
      + __Inclui:__ `image/jpeg`, `image/png`, `image/gif`, `image/svg`
         + Esses são os únicos tipos MIME aceitos pelos módulos npm de terceiros usados para derivar as cores.
      + __Exclui:__ `Leave blank`
1. Toque __Salvar__ na parte superior direita
1. Aplicar o perfil de processamento a uma pasta do AEM Assets se ainda não tiver sido feito

### Atualizar o esquema de metadados{#metadata-schema}

Para revisar os metadados de cores, mapeie dois novos campos no esquema de metadados da imagem para as novas propriedades de dados de metadados que o trabalhador preenche.

![Esquema de metadados](./assets/metadata/metadata-schema.png)

1. No serviço de Autor do AEM, navegue até __Ferramentas > Ativos > Esquemas de metadados__
1. Navegue até __padrão__ e selecione e edite __imagem__ e adicionar campos de formulário somente leitura para expor os metadados de cores gerados
1. Adicionar um __Texto em linha única__
   + __Rótulo do campo__: `Colors Family`
   + __Mapear para a propriedade__: `./jcr:content/metadata/wknd:colorsFamily`
   + __Regras > Campo > Desativar edição__: Marcado
1. Adicionar um __Texto multivalor__
   + __Rótulo do campo__: `Colors`
   + __Mapear para a propriedade__: `./jcr:content/metadata/wknd:colors`
1. Toque __Salvar__ na parte superior direita

## Processamento de ativos

![Detalhes do ativo](./assets/metadata/asset-details.png)

1. No serviço de Autor do AEM, navegue até __Ativos > Arquivos__
1. Navegue até a pasta ou subpasta à qual o Perfil de Processamento é aplicado
1. Carregue uma nova imagem (JPEG, PNG, GIF ou SVG) na pasta ou processe novamente as imagens existentes usando o [Processando perfil](#processing-profile)
1. Quando o processamento estiver concluído, selecione o ativo e toque em __propriedades__ na barra de ação superior para exibir seus metadados
1. Revise o `Colors Family` e `Colors` [campos de metadados](#metadata-schema) para os metadados gravados do worker de metadados do Asset compute personalizado.

Com os metadados de cores gravados nos metadados do ativo, no `[dam:Asset]/jcr:content/metadata` recurso, esses metadados são indexados como maior capacidade de descoberta de ativos usando esses termos por meio de pesquisa, e podem até ser gravados no binário do ativo se __Writeback de metadados DAM__ fluxo de trabalho é chamado nela.

### Representação de metadados no AEM Assets

![Arquivo de representação de metadados do AEM Assets](./assets/metadata/cqdam-metadata-rendition.png)

O arquivo XMP real gerado pelo trabalhador de metadados do Asset compute também é armazenado como uma representação discreta no ativo. Esse arquivo geralmente não é usado, em vez disso, os valores aplicados ao nó de metadados do ativo são usados, mas a saída XML bruta do trabalhador está disponível no AEM.

## código de trabalho de cores de metadados no Github

A versão final `metadata-colors/index.js` está disponível no Github em:

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

A versão final `test/asset-compute/metadata-colors` o conjunto de testes está disponível no Github em:

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
