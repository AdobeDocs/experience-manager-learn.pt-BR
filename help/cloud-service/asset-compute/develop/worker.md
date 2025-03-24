---
title: Desenvolver um trabalhador do Asset Compute
description: Os trabalhadores do Asset Compute são o núcleo de um projeto do Asset Compute que fornece funcionalidade personalizada que executa, ou coordena, o trabalho executado em um ativo para criar uma nova representação.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6282
thumbnail: KT-6282.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 7d51ec77-c785-4b89-b717-ff9060d8bda7
duration: 451
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1386'
ht-degree: 0%

---

# Desenvolver um trabalhador do Asset Compute

Os trabalhadores do Asset Compute são o núcleo de um projeto do Asset Compute que fornece funcionalidade personalizada que executa, ou coordena, o trabalho executado em um ativo para criar uma nova representação.

O projeto do Asset Compute gera automaticamente um trabalhador simples que copia o binário original do ativo em uma representação nomeada, sem transformações. Neste tutorial, modificaremos este trabalhador para fazer uma representação mais interessante, a fim de ilustrar o poder dos trabalhadores do Asset Compute.

Criaremos um trabalhador do Asset Compute que gera uma nova representação de imagem horizontal, que abrange o espaço vazio à esquerda e à direita da representação do ativo com uma versão indefinida do ativo. A largura, a altura e o desfoque da representação final são parametrizados.

## Fluxo lógico de uma invocação de trabalhador do Asset Compute

Os trabalhadores do Asset Compute implementam o contrato de API do trabalhador do Asset Compute SDK, na função `renditionCallback(...)`, que é conceitualmente:

+ __Entrada:__ Os parâmetros binário e perfil de processamento originais de um ativo do AEM
+ __Saída:__ Uma ou mais representações a serem adicionadas ao ativo do AEM

![Fluxo lógico do Asset Compute Worker](./assets/worker/logical-flow.png)

1. O serviço do Autor do AEM invoca o trabalhador do Asset Compute, fornecendo o binário original __(1a)__ do ativo (`source` parâmetro) e __(1b)__ quaisquer parâmetros definidos no Perfil de Processamento (`rendition.instructions` parâmetro).
1. O Asset Compute SDK orquestra a execução da função `renditionCallback(...)` do trabalhador de metadados Asset Compute personalizado, gerando uma nova representação binária, com base no binário original do ativo __(1a)__ e em quaisquer parâmetros __(1b)__.

   + Neste tutorial, a representação é criada &quot;em andamento&quot;, o que significa que o trabalhador compõe a representação. No entanto, o binário de origem também pode ser enviado para outras APIs de serviço Web para geração de representação.

1. O trabalhador do Asset Compute salva os dados binários da nova representação em `rendition.path`.
1. Os dados binários gravados em `rendition.path` são transportados via Asset Compute SDK para o Serviço de Autor do AEM e expostos como __(4a)__ uma representação de texto e __(4b)__ persistem no nó de metadados do ativo.

O diagrama acima articula as preocupações do desenvolvedor do Asset Compute e o fluxo lógico para a invocação do trabalhador do Asset Compute. Para os curiosos, os [detalhes internos de execução do Asset Compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/custom-application-internals.html) estão disponíveis, no entanto, somente os contratos públicos de API do Asset Compute SDK podem ser dependentes.

## Anatomia de um trabalhador

Todos os trabalhadores da Asset Compute seguem a mesma estrutura básica e o contrato de entrada/saída.

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

![Index.js](./assets/worker/autogenerated-index-js.png) gerado automaticamente

1. Verifique se o projeto do Asset Compute está aberto no código VS
1. Navegar até a pasta `/actions/worker`
1. Abrir o arquivo `index.js`

Este é o arquivo JavaScript do trabalhador que modificaremos neste tutorial.

## Instalar e importar módulos npm de suporte

Por serem baseados no Node.js, os projetos da Asset Compute se beneficiam do robusto [npm module ecosystem](https://npmjs.com). Para aproveitar os módulos npm, precisamos primeiro instalá-los em nosso projeto do Asset Compute.

Neste trabalhador, usamos o [jimp](https://www.npmjs.com/package/jimp) para criar e manipular a imagem de representação diretamente no código Node.js.

>[!WARNING]
>
>Nem todos os módulos npm para manipulação de ativos são compatíveis com o Asset Compute. Os módulos npm que dependem da existência de aplicativos como ImageMagick ou outras bibliotecas dependentes de SO não são compatíveis. É melhor limitar ao uso de módulos npm somente do JavaScript.

1. Abra a linha de comando na raiz do seu projeto Asset Compute (isso pode ser feito no Código VS por meio de __Terminal > Novo Terminal__) e execute o comando:

   ```
   $ npm install jimp
   ```

1. Importe o módulo `jimp` para o código do trabalhador para que ele possa ser usado por meio do objeto JavaScript `Jimp`.
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

Os trabalhadores do Asset Compute podem ler em parâmetros que podem ser transmitidos por meio do Processamento de perfis definidos no serviço do AEM as a Cloud Service Author. Os parâmetros são passados para o trabalhador por meio do objeto `rendition.instructions`.

Elas podem ser lidas acessando `rendition.instructions.<parameterName>` no código de trabalho.

Aqui leremos os `SIZE`, `BRIGHTNESS` e `CONTRAST` da representação configurável, fornecendo valores padrão se nenhum tiver sido fornecido por meio do Perfil de Processamento. Observe que `renditions.instructions` são passados como cadeias de caracteres quando chamados de Perfis de Processamento AEM as a Cloud Service, portanto, certifique-se de que eles sejam transformados nos tipos de dados corretos no código de trabalho.

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

Os trabalhadores da Asset Compute podem encontrar situações que resultam em erros. O Adobe Asset Compute SDK fornece [um conjunto de erros predefinidos](https://github.com/adobe/asset-compute-commons#asset-compute-errors) que podem ser gerados quando essas situações são encontradas. Se nenhum tipo de erro específico se aplicar, o `GenericError` poderá ser usado ou o `ClientErrors` personalizado específico poderá ser definido.

Antes de começar a processar a representação, verifique se todos os parâmetros são válidos e compatíveis no contexto deste trabalhador:

+ Verifique se os parâmetros de instrução de representação para `SIZE`, `CONTRAST` e `BRIGHTNESS` são válidos. Caso contrário, gerar um erro personalizado `RenditionInstructionsError`.
   + Uma classe `RenditionInstructionsError` personalizada que estende `ClientError` está definida na parte inferior deste arquivo. O uso de um erro específico e personalizado é útil ao [gravar testes](../test-debug/test.md) para o funcionário.

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

1. Crie uma nova tela `renditionImage` em dimensões quadradas especificadas pelo parâmetro `size`.
1. Criar um objeto `image` a partir do binário do ativo de origem
1. Use a biblioteca __Jimp__ para transformar a imagem:
   + Cortar a imagem original em um quadrado centralizado
   + Recortar um círculo do centro da imagem &quot;quadrada&quot;
   + Dimensionar para caber dentro das dimensões definidas pelo valor de parâmetro `SIZE`
   + Ajustar o contraste com base no valor do parâmetro `CONTRAST`
   + Ajustar o brilho com base no valor do parâmetro `BRIGHTNESS`
1. Coloque o `image` transformado no centro do `renditionImage`, que tem um plano de fundo transparente
1. Escreva o composto, `renditionImage` para `rendition.path` para que ele possa ser salvo novamente no AEM como uma representação de ativo.

Este código emprega as [APIs Jimp](https://github.com/oliver-moran/jimp#jimp) para executar essas transformações de imagem.

Os trabalhadores do Asset Compute devem terminar seu trabalho de forma síncrona, e o `rendition.path` deve ser totalmente gravado de volta antes que o `renditionCallback` do trabalhador seja concluído. Isso requer que as chamadas de funções assíncronas sejam tornadas síncronas usando o operador `await`. Se você não estiver familiarizado com as funções assíncronas do JavaScript e como executá-las de forma síncrona, familiarize-se com o [operador de espera do JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await).

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

Agora que o código do trabalhador está completo e foi registrado e configurado anteriormente no [manifest.yml](./manifest.md), ele pode ser executado usando a Ferramenta de Desenvolvimento do Asset Compute local para ver os resultados.

1. Na raiz do projeto Asset Compute
1. Executar `aio app run`
1. Aguarde a Ferramenta de desenvolvimento do Asset Compute abrir em uma nova janela
1. Na lista suspensa __Selecionar um arquivo...__, selecione uma imagem de exemplo a ser processada
   + Selecione um arquivo de imagem de amostra para usar como binário do ativo de origem
   + Se ainda não houver nenhum, toque em __(+)__ à esquerda e carregue um arquivo de [imagem de exemplo](../assets/samples/sample-file.jpg) e atualize a janela do navegador Ferramentas de Desenvolvimento
1. Atualize `"name": "rendition.png"` como este trabalhador para gerar um PNG transparente.
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

1. Toque em __Executar__ e aguarde a representação ser gerada
1. A seção __Representações__ pré-visualiza a representação gerada. Toque na pré-visualização de representação para baixar a representação completa

   ![Representação PNG padrão](./assets/worker/default-rendition.png)

### Executar o trabalhador com parâmetros

Os parâmetros, transmitidos por meio das configurações do Perfil de processamento, podem ser simulados nas Ferramentas de desenvolvimento do Asset Compute, fornecendo-os como pares de chave/valor no parâmetro de representação JSON.

>[!WARNING]
>
>Durante o desenvolvimento local, os valores podem ser passados usando vários tipos de dados, quando passados do AEM como Perfis de processamento de Cloud Service como cadeias de caracteres, portanto, verifique se os tipos de dados corretos são analisados, se necessário.
> Por exemplo, a função `crop(width, height)` de Jimp requer que seus parâmetros sejam de `int`. Se `parseInt(rendition.instructions.size)` não for analisado para um int, a chamada para `jimp.crop(SIZE, SIZE)` falhará, pois os parâmetros são do tipo &#39;String&#39; incompatível.

Nosso código aceita parâmetros para:

+ `size` define o tamanho da representação (altura e largura como números inteiros)
+ `contrast` define o ajuste do contraste, deve estar entre -1 e 1, como flutuações
+ `brightness` define o ajuste brilhante; deve estar entre -1 e 1, como flutuantes

Eles são lidos no trabalhador `index.js` via:

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

1. Toque em __Executar__ novamente
1. Toque na visualização da representação para baixar e revisar a representação gerada. Observe suas dimensões e como o contraste e o brilho foram alterados em comparação à representação padrão.

   ![Representação PNG com parâmetros](./assets/worker/parameterized-rendition.png)

1. Carregue outras imagens para a lista suspensa __Arquivo Source__ e tente executar o trabalhador com parâmetros diferentes!

## Trabalhador index.js no Github

O `index.js` final está disponível no Github em:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Resolução de problemas

+ [Representação retornada parcialmente desenhada/corrompida](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
