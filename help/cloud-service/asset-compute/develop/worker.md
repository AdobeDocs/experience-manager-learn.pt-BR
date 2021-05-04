---
title: Desenvolver um trabalhador do Asset compute
description: Os trabalhadores do Asset compute são o núcleo de um projeto do Asset compute que fornece funcionalidade personalizada que executa, ou coordena, o trabalho executado em um ativo para criar uma nova representação.
feature: Microsserviços Asset compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6282
thumbnail: KT-6282.jpg
topic: Integrações, desenvolvimento
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: 2efb7050b0b0c783c5f34c1f2e375cf21fa7a6cd
workflow-type: tm+mt
source-wordcount: '1426'
ht-degree: 0%

---


# Desenvolver um trabalhador do Asset compute

Os trabalhadores do Asset compute são o núcleo de um projeto do Asset compute que fornece funcionalidade personalizada que executa, ou coordena, o trabalho executado em um ativo para criar uma nova representação.

O projeto do Asset compute gera automaticamente um trabalhador simples que copia o binário original do ativo em uma representação nomeada, sem qualquer transformação. Neste tutorial, vamos modificar esse trabalhador para fazer uma representação mais interessante, para ilustrar o poder dos trabalhadores do Asset compute.

Criaremos um trabalhador do Asset compute que gera uma nova representação de imagem horizontal, que abrange o espaço vazio à esquerda e à direita da representação do ativo com uma versão borrada do ativo. A largura, a altura e o desfoque da representação final serão parametrizados.

## Fluxo lógico de uma invocação do Asset compute worker

Os trabalhadores do Asset compute implementam o contrato da API do trabalhador do SDK do Asset compute, na função `renditionCallback(...)`, que é conceitualmente:

+ __Entrada:__ Os parâmetros originais do binário e do perfil de processamento de um ativo AEM
+ __Saída:__ uma ou mais representações a serem adicionadas ao ativo de AEM

![Fluxo lógico do trabalhador de asset compute](./assets/worker/logical-flow.png)

1. O serviço Autor do AEM chama o trabalhador do Asset compute, fornecendo os __(1a)__ binários originais do ativo (parâmetro`source`) e __(1b)__ quaisquer parâmetros definidos no Perfil de processamento (parâmetro`rendition.instructions`).
1. O SDK do Asset compute orquestra a execução da função `renditionCallback(...)` do trabalhador de metadados do Asset compute personalizado, gerando uma nova representação binária, com base no __(1a)__ binário original do ativo e em qualquer parâmetro __(1b)__.

   + Neste tutorial, a representação é criada &quot;em andamento&quot;, o que significa que o trabalhador compõe a representação, no entanto, o binário de origem também pode ser enviado para outras APIs de serviço da Web para geração de representação.

1. O trabalhador do Asset compute salva os dados binários da nova representação em `rendition.path`.
1. Os dados binários gravados em `rendition.path` são transportados por meio do SDK do Asset compute para o AEM Author Service e expostos como __(4a)__ uma representação de texto e __(4b)__ persistiram no nó de metadados do ativo.

O diagrama acima articula as preocupações do desenvolvedor do Asset compute e o fluxo lógico para a invocação do trabalhador do Asset compute. Para o curioso, os [detalhes internos da execução do Asset compute](https://docs.adobe.com/content/help/en/asset-compute/using/extend/custom-application-internals.html) estão disponíveis, no entanto, somente os contratos de API do SDK do Asset compute público podem ser dependentes.

## Anatomia de um trabalhador

Todos os trabalhadores do Asset compute seguem a mesma estrutura básica e o mesmo contrato de entrada/saída.

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

## Abrir o arquivo index.js do trabalhador

![Index.js gerado automaticamente](./assets/worker/autogenerated-index-js.png)

1. Verifique se o projeto do Asset compute está aberto no Código VS
1. Navegue até a pasta `/actions/worker`
1. Abra o arquivo `index.js`

Este é o arquivo JavaScript do trabalhador que vamos modificar neste tutorial.

## Instalar e importar módulos npm de suporte

Por serem baseados em Node.js, os projetos do Asset compute se beneficiam do robusto ecossistema do módulo [npm](https://npmjs.com). Para aproveitar os módulos npm, primeiro devemos instalá-los em nosso projeto do Asset compute.

Neste trabalhador, aproveitamos o [jimp](https://www.npmjs.com/package/jimp) para criar e manipular a imagem de representação diretamente no código Node.js .

>[!WARNING]
>
>Nem todos os módulos npm para manipulação de ativos são suportados pelo Asset compute. Os módulos npm que dependem da existência de aplicativos como o ImageMagick ou outras bibliotecas dependentes de SO não são compatíveis. É melhor limitar ao uso de módulos npm somente JavaScript.

1. Abra a linha de comando na raiz do seu projeto Asset compute (isso pode ser feito no Código VS via __Terminal > Novo Terminal__) e execute o comando:

   ```
   $ npm install jimp
   ```

1. Importe o módulo `jimp` no código do trabalhador para que ele possa ser usado por meio do objeto JavaScript `Jimp` .
Atualize as diretivas `require` na parte superior do `index.js` do trabalhador para importar o objeto `Jimp` do módulo `jimp`:

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

Os trabalhadores do Asset compute podem ler nos parâmetros que podem ser transmitidos por meio de Perfis de processamento definidos em AEM como um serviço de Autor do Cloud Service. Os parâmetros são passados para o trabalhador por meio do objeto `rendition.instructions` .

Elas podem ser lidas acessando `rendition.instructions.<parameterName>` no código do trabalhador.

Aqui, lemos os `SIZE`, `BRIGHTNESS` e `CONTRAST` das representações configuráveis, fornecendo valores padrão se nenhum tiver sido fornecido por meio do Perfil de processamento. Observe que `renditions.instructions` são passadas como strings quando chamadas de AEM como Perfis de processamento de Cloud Service, portanto, assegure-se de que sejam transformadas nos tipos de dados corretos no código de trabalho.

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

## Emitir erros{#errors}

Os trabalhadores assets compute podem encontrar situações que resultam em erros. O SDK do Adobe Asset compute fornece [um conjunto de erros predefinidos](https://github.com/adobe/asset-compute-commons#asset-compute-errors) que podem ser lançados quando essas situações são encontradas. Se nenhum tipo de erro específico se aplicar, o `GenericError` poderá ser usado ou `ClientErrors` personalizado específico poderá ser definido.

Antes de começar a processar a representação, verifique se todos os parâmetros são válidos e suportados no contexto deste trabalhador:

+ Verifique se os parâmetros de instrução de representação para `SIZE`, `CONTRAST` e `BRIGHTNESS` são válidos. Caso contrário, envie um erro personalizado `RenditionInstructionsError`.
   + Uma classe `RenditionInstructionsError` personalizada que estende `ClientError` é definida na parte inferior desse arquivo. O uso de um erro específico e personalizado é útil quando [gravar testes](../test-debug/test.md) para o trabalhador.

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

Com os parâmetros lidos, limpos e validados, o código é gravado para gerar a representação. O pseudo código para a geração de representação é o seguinte:

1. Crie uma nova tela `renditionImage` em dimensões quadradas especificadas por meio do parâmetro `size`.
1. Crie um objeto `image` do binário do ativo de origem
1. Use a biblioteca __Jimp__ para transformar a imagem:
   + Recortar a imagem original em um quadrado centralizado
   + Recortar um círculo a partir do centro da imagem &quot;quadrada&quot;
   + Escala para se ajustar às dimensões definidas pelo valor do parâmetro `SIZE`
   + Ajustar o contraste com base no valor do parâmetro `CONTRAST`
   + Ajuste o brilho com base no valor do parâmetro `BRIGHTNESS`
1. Coloque o `image` transformado no centro do `renditionImage` que tem um plano de fundo transparente
1. Grave o composto, `renditionImage` em `rendition.path` para que ele possa ser salvo novamente em AEM como uma representação de ativo.

Esse código emprega as [Jimp APIs](https://github.com/oliver-moran/jimp#jimp) para executar essas transformações de imagem.

Os trabalhadores do Asset compute devem terminar seu trabalho de forma síncrona e `rendition.path` deve ser totalmente gravado de volta antes que `renditionCallback` do trabalhador seja concluído. Isso requer que as chamadas de funções assíncronas sejam sincronizadas usando o operador `await` . Se não estiver familiarizado com as funções assíncronas do JavaScript e como executá-las de maneira síncrona, familiarize-se com o operador de espera do [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await).

O trabalhador acabado `index.js` deve ter a seguinte aparência:

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

## Executar o trabalhador

Agora que o código do trabalhador foi concluído e foi registrado e configurado anteriormente no [manifest.yml](./manifest.md), ele pode ser executado usando a ferramenta de desenvolvimento de Asset compute local para ver os resultados.

1. Na raiz do projeto do Asset compute
1. Executar `aio app run`
1. Aguarde até que a Ferramenta de desenvolvimento de Assets compute seja aberta em uma nova janela
1. No __Selecione um arquivo...__ , selecione uma imagem de amostra para processar
   + Selecione um arquivo de imagem de amostra para usar como binário de ativo de origem
   + Se ainda não existir, toque no __(+)__ à esquerda e faça upload de um arquivo [de imagem de amostra](../assets/samples/sample-file.jpg) e atualize a janela do navegador Ferramentas de Desenvolvimento
1. Atualize `"name": "rendition.png"` como esse trabalhador para gerar um PNG transparente.
   + Observe que esse parâmetro &quot;name&quot; é usado apenas para a Ferramenta de desenvolvimento e não deve ser confiável.

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

1. Toque em __Executar__ e aguarde a representação ser gerada
1. A seção __Representações__ visualiza a representação gerada. Toque na visualização da representação para baixar a representação completa

   ![Representação PNG padrão](./assets/worker/default-rendition.png)

### Executar o trabalhador com parâmetros

Os parâmetros, transmitidos por meio das configurações do Perfil de processamento, podem ser simulados nas Ferramentas de desenvolvimento do Asset compute, fornecendo-os como pares de chave/valor no parâmetro de representação JSON.

>[!WARNING]
>
>Durante o desenvolvimento local, os valores podem ser transmitidos usando vários tipos de dados, quando passados de AEM como Perfis de processamento de Cloud Service como strings, portanto, verifique se os tipos de dados corretos são analisados, se necessário.
> Por exemplo, a função `crop(width, height)` de Jimp requer que seus parâmetros sejam `int`. Se `parseInt(rendition.instructions.size)` não for analisado em um int, a chamada para `jimp.crop(SIZE, SIZE)` falhará, pois os parâmetros serão incompatíveis com o tipo &#39;String&#39;.

Nosso código aceita parâmetros para:

+ `size` define o tamanho da representação (altura e largura como números inteiros)
+ `contrast` define o ajuste de contraste, deve estar entre -1 e 1, como flutuantes
+ `brightness`  define o ajuste claro, deve estar entre -1 e 1, como flutuantes

Elas são lidas no trabalhador `index.js` por:

+ `const SIZE = parseInt(rendition.instructions.size) || 800`
+ `const CONTRAST = parseFloat(rendition.instructions.contrast) || 0`
+ `const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0`

1. Atualize os parâmetros de representação para personalizar o tamanho, contraste e brilho.

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

1. Toque em __Executar__ novamente
1. Toque na visualização da representação para baixar e revisar a representação gerada. Observe suas dimensões e como o contraste e o brilho foram alterados em comparação com a representação padrão.

   ![Representação PNG parametrizada](./assets/worker/parameterized-rendition.png)

1. Faça upload de outras imagens para a lista suspensa __Source file__ e tente executar o trabalhador em relação a elas com parâmetros diferentes!

## Worker index.js no Github

O `index.js` final está disponível no Github em:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Resolução de problemas

+ [Representação retornada parcialmente desenhada/corrompida](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
