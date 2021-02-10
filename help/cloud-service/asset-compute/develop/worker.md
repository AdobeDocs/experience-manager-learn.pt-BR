---
title: Desenvolver um Asset compute
description: Os funcionários do asset compute são o núcleo de um projeto do Asset compute, pois fornecem funcionalidade personalizada que executa, ou orquestra, o trabalho executado em um ativo para criar uma nova execução.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6282
thumbnail: KT-6282.jpg
translation-type: tm+mt
source-git-commit: 2d7ae5e46acb25eaaf7a1a35d9bbf20f7c14042e
workflow-type: tm+mt
source-wordcount: '1421'
ht-degree: 0%

---


# Desenvolver um Asset compute

Os funcionários do asset compute são o núcleo de um projeto do Asset compute, fornecendo funcionalidade personalizada que executa, ou orquestra, o trabalho executado em um ativo para criar uma nova execução.

O projeto do Asset compute gera automaticamente um trabalhador simples que copia o binário original do ativo em uma representação nomeada, sem transformações. Neste tutorial, modificaremos esse funcionário para fazer uma representação mais interessante, para ilustrar o poder dos trabalhadores Asset computes.

Criaremos um trabalhador de Asset compute que gera uma nova representação de imagem horizontal, que cobre o espaço vazio à esquerda e à direita da representação do ativo com uma versão borrada do ativo. A largura, a altura e o desfoque da representação final serão parametrizados.

## Fluxo lógico de uma invocação do trabalhador de Asset compute

Os funcionários do asset compute implementam o contrato da API de trabalho do SDK do Asset compute, na função `renditionCallback(...)`, que é conceitualmente:

+ __Entrada:__ os parâmetros originais do binário e do Perfil de processamento de um ativo AEM
+ __Saída:__ uma ou mais representações a serem adicionadas ao ativo AEM

![Fluxo lógico do trabalhador do asset compute](./assets/worker/logical-flow.png)

1. O serviço de autor de AEM chama o funcionário do Asset compute, fornecendo __(1a)__ binário original do ativo (`source` parâmetro) e __(1b)__ quaisquer parâmetros definidos no Perfil de processamento (`rendition.instructions` parâmetro).
1. O SDK do Asset compute orquestra a execução da função `renditionCallback(...)` do funcionário de metadados do Asset compute personalizado, gerando uma nova execução binária, com base no __(1a)__ binário original do ativo e em qualquer parâmetro __(1b)__.

   + Neste tutorial, a execução é criada &quot;em andamento&quot;, o que significa que o trabalhador compõe a execução, no entanto, o binário de origem também pode ser enviado para outras APIs de serviço da Web para geração de renderização.

1. O funcionário do Asset compute salva os dados binários da nova execução em `rendition.path`.
1. Os dados binários gravados em `rendition.path` são transportados via SDK do Asset compute para o serviço de autor de AEM e expostos como __(4a)__ uma execução de texto e __(4b)__ persistiram no nó de metadados do ativo.

O diagrama acima articula as preocupações voltadas para o desenvolvedor do Asset compute e o fluxo lógico para a invocação do trabalhador do Asset compute. Para o curioso, os [detalhes internos da execução do Asset compute](https://docs.adobe.com/content/help/en/asset-compute/using/extend/custom-application-internals.html) estão disponíveis, no entanto, somente os contratos de API SDK do Asset compute público podem ser dependentes.

## Anatomia de um trabalhador

Todos os trabalhadores do Asset compute seguem a mesma estrutura básica e contrato de entrada/saída.

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

## Abrindo o arquivo index.js do trabalhador

![Index.js gerado automaticamente](./assets/worker/autogenerated-index-js.png)

1. Verifique se o projeto do Asset compute está aberto no Código VS
1. Navegue até a pasta `/actions/worker`
1. Abra o arquivo `index.js`

Este é o arquivo JavaScript do trabalhador que modificaremos neste tutorial.

## Instalar e importar módulos npm de suporte

Sendo baseados em Node.js, os projetos de Asset compute se beneficiam do robusto ecossistema do módulo [npm](https://npmjs.com). Para potencializar os módulos npm, primeiro é necessário instalá-los em nosso projeto de Asset compute.

Neste trabalhador, aproveitamos o [jimp](https://www.npmjs.com/package/jimp) para criar e manipular a imagem de execução diretamente no código Node.js.

>[!WARNING]
>
>Nem todos os módulos npm para manipulação de ativos são suportados pelo Asset compute. os módulos npm que dependem da existência de aplicativos como o ImageMagick ou outras bibliotecas dependentes do SO não são suportados. É melhor limitar o uso de módulos npm somente JavaScript.

1. Abra a linha de comando na raiz do projeto do Asset compute (isso pode ser feito no Código VS via __Terminal > Novo terminal__) e execute o comando:

   ```
   $ npm install jimp
   ```

1. Importe o módulo `jimp` para o código de trabalho para que ele possa ser usado por meio do objeto JavaScript `Jimp`.
Atualize as diretivas `require` na parte superior de `index.js` do trabalhador para importar o objeto `Jimp` do módulo `jimp`:

   ```javascript
   'use strict';
   
   const { Jimp } = require('jimp');
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

Os funcionários do asset compute podem ler os parâmetros que podem ser passados por Perfis de processamento definidos em AEM como um serviço de Autor do Cloud Service. Os parâmetros são passados para o trabalhador por meio do objeto `rendition.instructions`.

Eles podem ser lidos acessando `rendition.instructions.<parameterName>` no código de trabalho.

Aqui nós lemos os valores padrão das execuções configuráveis `SIZE`, `BRIGHTNESS` e `CONTRAST`, fornecendo valores padrão se nenhum foi fornecido pelo Perfil de processamento. Observe que `renditions.instructions` são passados como strings quando chamados de AEM como Perfis de processamento de Cloud Service, portanto, verifique se são transformados nos tipos de dados corretos no código de trabalho.

```javascript
'use strict';

const { Jimp } = require('jimp');
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

## Emitindo erros{#errors}

Os funcionários do asset compute podem encontrar situações que resultam em erros. O SDK do Asset compute Adobe fornece [um conjunto de erros predefinidos](https://github.com/adobe/asset-compute-commons#asset-compute-errors) que podem ser lançados quando essas situações são encontradas. Se nenhum tipo de erro específico se aplicar, `GenericError` poderá ser usado ou `ClientErrors` personalizado específico poderá ser definido.

Antes de começar a processar a representação, verifique se todos os parâmetros são válidos e suportados no contexto deste trabalhador:

+ Verifique se os parâmetros de instrução de execução para `SIZE`, `CONTRAST` e `BRIGHTNESS` são válidos. Caso contrário, envie um erro personalizado `RenditionInstructionsError`.
   + Uma classe personalizada `RenditionInstructionsError` que estende `ClientError` é definida na parte inferior deste arquivo. O uso de um erro específico e personalizado é útil quando [gravar testes](../test-debug/test.md) para o trabalhador.

```javascript
'use strict';

const { Jimp } = require('jimp');
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

Com os parâmetros lidos, sanitizados e validados, o código é gravado para gerar a execução. O pseudo código para a geração de representação é o seguinte:

1. Crie uma nova tela `renditionImage` em dimensões quadradas especificadas pelo parâmetro `size`.
1. Criar um objeto `image` a partir do binário do ativo de origem
1. Use a biblioteca __Jimp__ para transformar a imagem:
   + Recortar a imagem original em um quadrado centralizado
   + Recortar um círculo do centro da imagem &quot;quadrada&quot;
   + Dimensionar para se ajustar às dimensões definidas pelo valor do parâmetro `SIZE`
   + Ajustar o contraste com base no valor do parâmetro `CONTRAST`
   + Ajuste o brilho com base no valor do parâmetro `BRIGHTNESS`
1. Coloque o `image` transformado no centro do `renditionImage` que tem um fundo transparente
1. Grave o composto, `renditionImage` em `rendition.path` para que possa ser salvo novamente em AEM como uma representação de ativo.

Este código emprega as [APIs Jimp](https://github.com/oliver-moran/jimp#jimp) para executar essas transformações de imagem.

Os funcionários do asset compute devem terminar seu trabalho de forma síncrona e `rendition.path` deve ser gravado completamente para que o `renditionCallback` do trabalhador seja concluído. Isso requer que as chamadas de funções assíncronas sejam sincronizadas usando o operador `await`. Se você não estiver familiarizado com as funções assíncronas do JavaScript e como fazê-las executar de maneira síncrona, familiarize-se com o operador de espera [do JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await).

O trabalho finalizado `index.js` deve ter a seguinte aparência:

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

    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        throw new RenditionInstructionsError("'size' must be between 10 and 10,000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Create target rendition image of the target size with a transparent background (0x0)
    let renditionImage =  new Jimp(SIZE, SIZE, 0x0);

    // Read and perform transformations on the source binary image
    let image = await Jimp.read(source.path);

    // Crop a circle from the source asset, and then apply contrast and brightness using Jimp
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
    renditionImage.write(rendition.path);
});

// Custom error used for renditions.instructions parameter checking
class RenditionInstructionsError extends ClientError {
    constructor(message) {
        super(message, "RenditionInstructionsError", "rendition_instructions_error");
        Error.captureStackTrace(this, RenditionInstructionsError);
    }
}
```

## Execução do trabalhador

Agora que o código de trabalho está completo e foi registrado e configurado anteriormente em [manifest.yml](./manifest.md), ele pode ser executado usando a Ferramenta de Desenvolvimento de Asset computes local para ver os resultados.

1. Da raiz do projeto do Asset compute
1. Executar `aio app run`
1. Aguarde até que a Ferramenta de desenvolvimento de Asset computes abra em uma nova janela
1. Em __Selecione um ficheiro...__ menu suspenso, selecione uma imagem de amostra para processar
   + Selecione um arquivo de imagem de amostra para usar como binário de ativo de origem
   + Se ainda não houver nenhum, toque em __(+)__ à esquerda e carregue um arquivo [de imagem de amostra](../assets/samples/sample-file.jpg) e atualize a janela do navegador de Ferramentas de Desenvolvimento
1. Atualize `"name": "rendition.png"` como este trabalhador para gerar um PNG transparente.
   + Observe que esse parâmetro &quot;name&quot; é usado apenas para a ferramenta de desenvolvimento e não deve ser utilizado.

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
1. Toque em __Executar__ e aguarde a execução ser gerada
1. A seção __Representações__ pré-visualização a representação gerada. Toque na pré-visualização de execução para baixar a execução completa

   ![Execução PNG padrão](./assets/worker/default-rendition.png)

### Executar o trabalhador com parâmetros

Parâmetros, passados pelas configurações de Perfil de processamento, podem ser simulados nas Ferramentas de desenvolvimento de Asset computes, fornecendo-os como pares de chave/valor no parâmetro de execução JSON.

>[!WARNING]
>
>Durante o desenvolvimento local, os valores podem ser transmitidos usando vários tipos de dados, quando passados de AEM como Perfis de processamento de Cloud Service como strings, portanto, verifique se os tipos de dados corretos são analisados, se necessário.
> Por exemplo, a função `crop(width, height)` de Jimp requer que seus parâmetros sejam `int`&#39;s. Se `parseInt(rendition.instructions.size)` não for analisado como um int, a chamada para `jimp.crop(SIZE, SIZE)` falhará, pois os parâmetros serão do tipo &#39;String&#39; incompatível.

Nosso código aceita parâmetros para:

+ `size` define o tamanho da representação (altura e largura como números inteiros)
+ `contrast` define o ajuste de contraste, deve estar entre -1 e 1, como flutuantes
+ `brightness`  define o ajuste claro, deve estar entre -1 e 1, como flutuante

Estes são lidos no trabalhador `index.js` via:

+ `const SIZE = parseInt(rendition.instructions.size) || 800`
+ `const CONTRAST = parseFloat(rendition.instructions.contrast) || 0`
+ `const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0`

1. Atualize os parâmetros de execução para personalizar o tamanho, o contraste e o brilho.

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
1. Toque na pré-visualização de execução para baixar e revisar a execução gerada. Observe suas dimensões e como o contraste e o brilho foram alterados em comparação à execução padrão.

   ![Execução PNG com parâmetros](./assets/worker/parameterized-rendition.png)

1. Faça upload de outras imagens para o menu suspenso __Arquivo de origem__ e tente executar o trabalhador em relação a elas com parâmetros diferentes!

## Worker index.js no Github

O `index.js` final está disponível no Github em:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Resolução de problemas

+ [Representação devolvida parcialmente desenhada/corrompida](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
