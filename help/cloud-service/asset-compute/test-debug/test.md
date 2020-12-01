---
title: Testar um trabalhador Asset compute
description: O projeto do Asset compute define um padrão para a fácil criação e execução de testes de Asset computes.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '631'
ht-degree: 0%

---


# Testar um trabalhador Asset compute

O projeto do Asset compute define um padrão para criar e executar com facilidade [testes de Asset computes](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html).

## Anatomia de um teste de trabalhador

Os testes dos funcionários do asset compute são divididos em conjuntos de testes e, em cada conjunto de testes, um ou mais casos de teste afirmando uma condição para teste.

A estrutura dos ensaios de um projeto de Asset compute é a seguinte:

```
/actions/<worker-name>/index.js
...
/test/
  asset-compute/
    <worker-name>/           <--- Test suite for the worker, must match the yaml key for this worker in manifest.yml
        <test-case-1>/       <--- Specific test case 
            file.jpg         <--- Input file (ie. `source.path` or `source.url`)
            params.json      <--- Parameters (ie. `rendition.instructions`)
            rendition.png    <--- Expected output file (ie. `rendition.path`)
        <test-case-2>/       <--- Another specific test case for this worker
            ...
```

Cada transmissão de teste pode ter os seguintes arquivos:

+ `file.<extension>`
   + Arquivo de origem para teste (a extensão pode ser qualquer coisa exceto `.link`)
   + Obrigatório
+ `rendition.<extension>`
   + Representação esperada
   + Obrigatório, exceto para teste de erro
+ `params.json`
   + As instruções JSON de execução única
   + Opcional
+ `validate`
   + Um script que recebe os caminhos de arquivo de renderização esperados e reais como argumentos e deve retornar o código de saída 0 se o resultado estiver ok, ou um código de saída diferente de zero se a validação ou comparação falhar.
   + Opcional, o padrão é `diff` comando
   + Usar um script de shell que vincula um comando docker run para usar diferentes ferramentas de validação
+ `mock-<host-name>.json`
   + Respostas HTTP formatadas JSON para [alternar chamadas de serviço externo](https://www.mock-server.com/mock_server/creating_expectations.html).
   + Opcional, usado somente se o código de trabalho fizer solicitações HTTP de sua própria

## Gravando um caso de teste

Este caso de teste garante a entrada parametrizada (`params.json`) para o arquivo de entrada (`file.jpg`) gera a execução PNG esperada (`rendition.png`).

1. Primeiro, exclua o caso de testes `simple-worker` gerados automaticamente em `/test/asset-compute/simple-worker`, pois isso é inválido, já que nosso trabalhador não copia mais a fonte para a execução.
1. Crie uma nova pasta de caso de teste em `/test/asset-compute/worker/success-parameterized` para testar uma execução bem-sucedida do trabalhador que gera uma execução PNG.
1. Na pasta `success-parameterized`, adicione o arquivo de entrada [de teste](./assets/test/success-parameterized/file.jpg) para este caso de teste e nomeie-o `file.jpg`.
1. Na pasta `success-parameterized`, adicione um novo arquivo chamado `params.json` que defina os parâmetros de entrada do trabalhador:

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```
   Esses são os mesmos valores chave passados para a definição de perfil da ferramenta de desenvolvimento[, menos a chave ](../develop/development-tool.md).`worker`
1. Adicione o arquivo de renderização esperado [](./assets/test/success-parameterized/rendition.png) a esse caso de teste e nomeie-o `rendition.png`. Este arquivo representa a saída esperada do trabalhador para a entrada `file.jpg` fornecida.
1. Na linha de comando, execute os testes da raiz do projeto executando `aio app test`
   + Verifique se [Docker Desktop](../set-up/development-environment.md#docker) e as imagens do Docker de suporte estão instaladas e iniciadas
   + Encerrar todas as instâncias em execução da ferramenta de desenvolvimento

![Teste - Êxito  ](./assets/test/success-parameterized/result.png)

## Gravando um caso de teste de verificação de erro

Este caso de teste testa para garantir que o trabalhador jogue o erro apropriado quando o parâmetro `contrast` estiver definido como um valor inválido.

1. Crie uma nova pasta de caso de teste em `/test/asset-compute/worker/error-contrast` para testar uma execução de erro do trabalhador devido a um valor de parâmetro `contrast` inválido.
1. Na pasta `error-contrast`, adicione o arquivo de entrada [de teste](./assets/test/error-contrast/file.jpg) para este caso de teste e nomeie-o `file.jpg`. O conteúdo deste arquivo é irrelevante para este teste, ele só precisa existir para passar da verificação &quot;Fonte corrompida&quot;, para que as verificações de validade `rendition.instructions` sejam alcançadas, que esse caso de teste valida.
1. Na pasta `error-contrast`, adicione um novo arquivo chamado `params.json` que defina os parâmetros de entrada do trabalhador com o conteúdo:

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Defina os parâmetros `contrast` como `10`, um valor inválido, como o contraste deve estar entre -1 e 1, para lançar um `RenditionInstructionsError`.
   + Assegure-se de que o erro apropriado seja lançado nos testes definindo a tecla `errorReason` para o &quot;motivo&quot; associado ao erro esperado. Esse parâmetro de contraste inválido lança o [erro personalizado](../develop/worker.md#errors), `RenditionInstructionsError`, portanto, define `errorReason` para o motivo desse erro, ou`rendition_instructions_error` para afirmar que ele é lançado.

1. Como nenhuma execução deve ser gerada durante uma execução de erro, nenhum arquivo `rendition.<extension>` é necessário.
1. Execute o conjunto de testes da raiz do projeto executando o comando `aio app test`
   + Verifique se [Docker Desktop](../set-up/development-environment.md#docker) e as imagens do Docker de suporte estão instaladas e iniciadas
   + Encerrar todas as instâncias em execução da ferramenta de desenvolvimento

![Teste - Contraste do erro](./assets/test/error-contrast/result.png)

## Casos de teste no Github

Os testes finais estão disponíveis no Github em:

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Resolução de problemas

+ [Nenhuma execução gerada durante a execução do teste](../troubleshooting.md#test-no-rendition-generated)
+ [O teste gera representação incorreta](../troubleshooting.md#tests-generates-incorrect-rendition)
