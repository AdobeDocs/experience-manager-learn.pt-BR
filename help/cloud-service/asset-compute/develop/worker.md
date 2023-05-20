---
title: Desenvolver um trabalhador do Asset compute
description: Os trabalhadores do Asset compute são o núcleo de um projeto do Asset compute que fornece funcionalidade personalizada que executa, ou coordena, o trabalho executado em um ativo para criar uma nova representação.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6282
thumbnail: KT-6282.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 7d51ec77-c785-4b89-b717-ff9060d8bda7
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1416'
ht-degree: 0%

---

# Desenvolver um trabalhador do Asset compute

Os trabalhadores do Asset compute são o núcleo de um projeto do Asset compute que fornece funcionalidade personalizada que executa, ou coordena, o trabalho executado em um ativo para criar uma nova representação.

O projeto do Asset compute gera automaticamente um trabalhador simples que copia o binário original do ativo em uma representação nomeada, sem transformações. Neste tutorial, modificaremos esse worker para fazer uma representação mais interessante, a fim de ilustrar o poder dos workers de Asset compute.

Criaremos um trabalhador do Asset compute que gera uma nova representação de imagem horizontal, que abrange o espaço vazio à esquerda e à direita da representação do ativo com uma versão indefinida do ativo. A largura, a altura e o desfoque da representação final são parametrizados.

## Fluxo lógico de uma invocação de trabalhador de Asset compute

Os trabalhadores do Asset compute implementam o contrato de API do trabalhador do SDK do Asset compute, no `renditionCallback(...)` conceitualmente:

+ __Entrada:__ Parâmetros binários e de perfil de processamento originais de um ativo AEM
+ __Saída:__ Uma ou mais representações a serem adicionadas ao ativo AEM

![Fluxo lógico do trabalhador do asset compute](./assets/worker/logical-flow.png)

1. O serviço do Autor do AEM chama o trabalhador do Asset compute, fornecendo a __(1-A)__ binário original (`source` parâmetro) e __(1-B)__ quaisquer parâmetros definidos no Perfil de processamento (`rendition.instructions` parâmetro).
1. O SDK do Asset compute orquestra a execução do trabalhador de metadados do Asset compute personalizado `renditionCallback(...)` gerando uma nova representação binária, com base no binário original do ativo __(1-A)__ e quaisquer parâmetros __(1-B)__.

   + Neste tutorial, a representação é criada &quot;em andamento&quot;, o que significa que o trabalhador compõe a representação. No entanto, o binário de origem também pode ser enviado para outras APIs de serviço Web para geração de representação.

1. O trabalhador do Asset compute salva os dados binários da nova representação em `rendition.path`.
1. Os dados binários gravados em `rendition.path` é transportado por meio do SDK do Asset compute para o Serviço do autor do AEM e exposto como __(4-A)__ uma representação de texto e __4-B)__ persistida no nó de metadados do ativo.

O diagrama acima articula as preocupações do desenvolvedor do Asset compute e o fluxo lógico para a invocação do trabalhador do Asset compute. Para os curiosos, o [detalhes internos da execução do Asset compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/custom-application-internals.html) estão disponíveis, no entanto, somente os contratos de API do SDK do Asset compute público podem ser dependentes.

## Anatomia de um trabalhador

Todos os trabalhadores Assets compute seguem a mesma estrutura básica e o contrato de entrada/saída.

```javascript
'use strict';

// Any npm module imports used by the worker
const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

/**
Exports the worker implemented by a custom rendition callback function, which parametrizes the input/output contract for the worker.
 + `source` represents the asset's original binary used as the input for the worker.
 + `rendition` represents the worker's output, which is the creation of a new asset rendition.
 + `params` are optional parameters, which map to additional key/value pairs, including a sub `auth` object that contains Adobe I/O access credentials.
**/
exports.main = worker(async (source, rendition, params) => {
    // Perform any necessary source (input) checks
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        // Throw appropriate errors whenever an erring condition is met
        throw new SourceCorruptError('source file is empty');
    }

    // Access any custom parameters provided via the Processing Profile configuration
    let param1 = rendition.instructions.exampleParam;

    /** 
    Perform all work needed to transform the source into the rendition.
    
    The source data can be accessed:
        + In the worker via a file available at `source.path`
        + Or via a presigned GET URL at `source.url`
    **/
    if (success) {
        // A successful worker must write some data back to `renditions.path`. 
        // This example performs a trivial 1:1 copy of the source binary to the rendition
        await fs.copyFile(source.path, rendition.path);
    } else {
        // Upon failure an Asset Compute Error (exported by @adobe/asset-compute-commons) should be thrown.
        throw new GenericError("An error occurred!", "example-worker");
    }
});

/**
Optionally create helper classes or functions the worker's rendition callback function invokes to help organize code.

Code shared across workers, or to complex to be managed in a single file, can be broken out across supporting JavaScript files in the project and imported normally into the worker. 
**/
function customHelperFunctions() { ... }
```

## Abrir o index.js do trabalhador

![Gerado automaticamente index.js](./assets/worker/autogenerated-index-js.png)

1. Verifique se o projeto do Asset compute está aberto no código VS
1. Navegue até a `/actions/worker` pasta
1. Abra o `index.js` arquivo

Este é o arquivo JavaScript do trabalhador que modificaremos neste tutorial.

## Instalar e importar módulos npm de suporte

Por serem baseados em Node.js, os projetos do Asset compute se beneficiam da arquitetura robusta [ecosistema do módulo npm](https://npmjs.com). Para aproveitar os módulos npm, precisamos primeiro instalá-los em nosso projeto do Asset compute.

Neste trabalhador, aproveitamos o [jimp](https://www.npmjs.com/package/jimp) para criar e manipular a imagem de representação diretamente no código Node.js.

>[!WARNING]
>
>Nem todos os módulos npm para manipulação de ativos são compatíveis com o Asset compute. Os módulos npm que dependem da existência de aplicativos como ImageMagick ou outras bibliotecas dependentes de SO não são compatíveis. É melhor limitar ao uso de módulos npm somente para JavaScript.

1. Abra a linha de comando na raiz do projeto Asset compute (isso pode ser feito no Código VS via __Terminal > Novo Terminal__) e execute o comando:

   ```
   $ npm install jimp
   ```

1. Importe o `jimp` no código do trabalhador para que ele possa ser usado por meio da variável `Jimp` Objeto JavaScript.
Atualize o `require` diretivas na parte superior do `index.js` para importar o `Jimp` objeto do `jimp` módulo:

   ```javascript
   'use strict';
   
   const Jimp = require('jimp');
   const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
   const fs = require('fs').promises;
   
   exports.main = worker(async (source, rendition, params) => {
       // Check handle a corrupt input source
       const stats = await fs.stat(source.path);
       if (stats.size === 0) {
           throw new SourceCorruptError('source file is empty');
       }
   
       // Do work here
   });
   ```

## Ler parâmetros

Os trabalhadores do Asset compute podem ler em parâmetros que podem ser transmitidos por meio de Perfis de processamento definidos no serviço de Autor as a Cloud Service AEM. Os parâmetros são passados para o trabalhador por meio do `rendition.instructions` objeto.

Elas podem ser lidas acessando `rendition.instructions.<parameterName>` no código do trabalhador.

Aqui, leremos nas representações configuráveis `SIZE`, `BRIGHTNESS` e `CONTRAST`, fornecendo valores padrão se nenhum tiver sido fornecido por meio do Perfil de processamento. Observe que `renditions.instructions` são passados como cadeias de caracteres quando chamados de Perfis de processamento as a Cloud Service AEM, para garantir que sejam transformados nos tipos de dados corretos no código do trabalhador.

```javascript
'use strict';

const Jimp = require('jimp');
const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read in parameters and set defaults if parameters are provided
    // Processing Profiles pass in instructions as Strings, so make sure to parse to correct data types
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    // Do work here
}
```

## Gerando erros{#errors}

Os trabalhadores do Asset compute podem encontrar situações que resultam em erros. O SDK do Asset compute Adobe fornece [um conjunto de erros predefinidos](https://github.com/adobe/asset-compute-commons#asset-compute-errors) que pode ser lançado quando tais situações são encontradas. Se nenhum tipo de erro específico se aplicar, a variável `GenericError` pode ser usado ou personalizado específico `ClientErrors` pode ser definido.

Antes de começar a processar a representação, verifique se todos os parâmetros são válidos e compatíveis no contexto deste trabalhador:

+ Verifique os parâmetros de instrução de representação para `SIZE`, `CONTRAST`, e `BRIGHTNESS` são válidos. Caso contrário, gerar um erro personalizado `RenditionInstructionsError`.
   + Um personalizado `RenditionInstructionsError` classe que estende `ClientError` está definido na parte inferior deste arquivo. O uso de um erro específico e personalizado é útil quando [gravação de testes](../test-debug/test.md) para o trabalhador.

```javascript
'use strict';

const Jimp = require('jimp');
// Import the Asset Compute SDK provided `ClientError` 
const { worker, SourceCorruptError, ClientError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read in parameters and set defaults if parameters are provided
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        // Ensure size is within allowable bounds
        throw new RenditionInstructionsError("'size' must be between 10 and 1,0000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        // Ensure contrast is valid value
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        // Ensure contrast is valid value
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Do work here
}

// Create a new ClientError to handle invalid rendition.instructions values
class RenditionInstructionsError extends ClientError {
    constructor(message) {
        // Provide a:
        // + message: describing the nature of this erring condition
        // + name: the name of the error; usually same as class name
        // + reason: a short, searchable, unique error token that identifies this error
        super(message, "RenditionInstructionsError", "rendition_instructions_error");

        // Capture the strack trace
        Error.captureStackTrace(this, RenditionInstructionsError);
    }
}
```

## Criação da representação

Com os parâmetros lidos, higienizados e validados, o código é escrito para gerar a representação. O pseudocódigo para a geração de representação é o seguinte:

1. Criar um novo `renditionImage` tela de desenho em dimensões quadradas especificadas pelo `size` parâmetro.
1. Criar um `image` do binário do ativo de origem
1. Use o __Jimp__ biblioteca para transformar a imagem:
   + Cortar a imagem original em um quadrado centralizado
   + Recortar um círculo do centro da imagem &quot;quadrada&quot;
   + Dimensionar para caber nas dimensões definidas pelo `SIZE` valor do parâmetro
   + Ajustar o contraste com base na variável `CONTRAST` valor do parâmetro
   + Ajustar o brilho com base no `BRIGHTNESS` valor do parâmetro
1. Colocar o transformado `image` no centro do `renditionImage` que tenha um fundo transparente
1. Escreva o composto, `renditionImage` para `rendition.path` para que possa ser salvo novamente no AEM como uma representação de ativo.

Esse código emprega o [APIs Jimp](https://github.com/oliver-moran/jimp#jimp) para executar essas transformações de imagem.

os trabalhadores assets compute devem terminar o seu trabalho de forma síncrona, e a `rendition.path` deve ser completamente devolvido antes de o trabalhador `renditionCallback` é concluída. Isso requer que as chamadas de funções assíncronas sejam tornadas síncronas usando o `await` operador. Se você não estiver familiarizado com as funções assíncronas do JavaScript e como executá-las de maneira síncrona, familiarize-se com [Operador await do JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await).

O trabalhador concluído `index.js` deve ser semelhante a:

```javascript
'use strict';

const Jimp = require('jimp');
const { worker, SourceCorruptError, ClientError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read/parse and validate parameters
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        throw new RenditionInstructionsError("'size' must be between 10 and 1,0000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Create target rendition image 
    let renditionImage =  new Jimp(SIZE, SIZE, 0x0);

    // Read and perform transformations on the source binary image
    let image = await Jimp.read(source.path);

    // Crop a circle from the source asset, and then apply contrast and brightness
    image.crop(
            image.bitmap.width < image.bitmap.height ? 0 : (image.bitmap.width - image.bitmap.height) / 2,
            image.bitmap.width < image.bitmap.height ? (image.bitmap.height - image.bitmap.width) / 2 : 0,
            image.bitmap.width < image.bitmap.height ? image.bitmap.width : image.bitmap.height,
            image.bitmap.width < image.bitmap.height ? image.bitmap.width : image.bitmap.height
        )   
        .circle()
        .scaleToFit(SIZE, SIZE)
        .contrast(CONTRAST)
        .brightness(BRIGHTNESS);

    // Place the transformed image onto the transparent renditionImage to save as PNG
    renditionImage.composite(image, 0, 0)

    // Write the final transformed image to the asset's rendition
    await renditionImage.writeAsync(rendition.path);
});

// Custom error used for renditions.instructions parameter checking
class RenditionInstructionsError extends ClientError {
    constructor(message) {
        super(message, "RenditionInstructionsError", "rendition_instructions_error");
        Error.captureStackTrace(this, RenditionInstructionsError);
    }
}
```

## Executando o trabalhador

Agora que o código do trabalhador está completo e foi anteriormente registrado e configurado no [manifest.yml](./manifest.md), ela pode ser executada usando a Ferramenta de desenvolvimento de Asset compute local para ver os resultados.

1. Na raiz do projeto do Asset compute
1. Executar `aio app run`
1. Aguarde até que a Ferramenta de desenvolvimento do Asset compute seja aberta em uma nova janela
1. No __Selecione um arquivo...__ selecione uma imagem de amostra para processar
   + Selecione um arquivo de imagem de amostra para usar como binário do ativo de origem
   + Se ainda não houver nenhum, toque no __(+)__ à esquerda e faça upload de um [imagem de amostra](../assets/samples/sample-file.jpg) e atualize a janela do navegador Ferramentas de desenvolvimento
1. Atualizar `"name": "rendition.png"` como esse trabalhador para gerar um PNG transparente.
   + Observe que esse parâmetro &quot;name&quot; é usado somente para a Ferramenta de desenvolvimento e não deve ser usado.

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.png"
           }
       ]
   }
   ```

1. Toque __Executar__ e aguardar a representação ser gerada
1. A variável __Representações__ pré-visualiza a representação gerada. Toque na pré-visualização de representação para baixar a representação completa

   ![Representação PNG padrão](./assets/worker/default-rendition.png)

### Executar o trabalhador com parâmetros

Os parâmetros, transmitidos por meio das configurações do Perfil de processamento, podem ser simulados nas Ferramentas de desenvolvimento do Asset compute, fornecendo-os como pares de chave/valor no parâmetro de representação JSON.

>[!WARNING]
>
>Durante o desenvolvimento local, os valores podem ser passados usando vários tipos de dados, quando passados do AEM como Perfis de processamento de Cloud Service como cadeias de caracteres, portanto, verifique se os tipos de dados corretos são analisados, se necessário.
> Por exemplo, Jimp&#39;s `crop(width, height)` A função requer que seus parâmetros sejam `int`do. Se `parseInt(rendition.instructions.size)` não for analisado para um int, a chamada para `jimp.crop(SIZE, SIZE)` falha, pois os parâmetros são incompatíveis com o tipo &#39;String&#39;.

Nosso código aceita parâmetros para:

+ `size` define o tamanho da representação (altura e largura como números inteiros)
+ `contrast` define o ajuste do contraste; deve estar entre -1 e 1, como flutuações
+ `brightness`  define o ajuste brilhante; deve estar entre -1 e 1, como flutuantes

Eles são lidos no worker `index.js` via:

+ `const SIZE = parseInt(rendition.instructions.size) || 800`
+ `const CONTRAST = parseFloat(rendition.instructions.contrast) || 0`
+ `const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0`

1. Atualize os parâmetros de representação para personalizar o tamanho, o contraste e o brilho.

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.png",
               "size": "450",
               "contrast": "0.30",
               "brightness": "0.15"
           }
       ]
   }
   ```

1. Toque __Executar__ novamente
1. Toque na visualização da representação para baixar e revisar a representação gerada. Observe suas dimensões e como o contraste e o brilho foram alterados em comparação à representação padrão.

   ![Representação PNG com parâmetros](./assets/worker/parameterized-rendition.png)

1. Faça upload de outras imagens para o __Arquivo de origem__ e tente executar o trabalhador com parâmetros diferentes!

## Trabalhador index.js no Github

A versão final `index.js` está disponível no Github em:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Resolução de problemas

+ [Representação retornada parcialmente desenhada/corrompida](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
