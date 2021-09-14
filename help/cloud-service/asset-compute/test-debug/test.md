---
title: Testar um trabalhador do Asset compute
description: O projeto do Asset compute define um padrão para criar e executar facilmente testes de trabalhadores do Asset compute.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 04992caf-b715-4701-94a8-6257e9bd300c
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '629'
ht-degree: 0%

---

# Testar um trabalhador do Asset compute

O projeto do Asset compute define um padrão para criar e executar facilmente [testes de trabalhadores do Asset compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html).

## Anatomia de um teste de trabalhador

Os testes dos trabalhadores do Asset compute são divididos em conjuntos de testes e, em cada conjunto de testes, um ou mais casos de teste que afirmam uma condição para testar.

A estrutura dos ensaios de um projeto Asset compute é a seguinte:

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

Cada conversão de teste pode ter os seguintes arquivos:

+ `file.<extension>`
   + Arquivo de origem a ser testado (a extensão pode ser qualquer item exceto `.link`)
   + Obrigatório
+ `rendition.<extension>`
   + Representação esperada
   + Obrigatório, exceto para teste de erro
+ `params.json`
   + As instruções JSON de representação única
   + Opcional
+ `validate`
   + Um script que obtém caminhos de arquivo de renderização esperados e reais como argumentos e deve retornar o código de saída 0 se o resultado for ok, ou um código de saída diferente de zero se a validação ou comparação falhar.
   + Opcional, o padrão é o comando `diff`
   + Use um script de shell que envolva um comando docker run para usar ferramentas de validação diferentes
+ `mock-<host-name>.json`
   + Respostas HTTP JSON formatadas para [zombando de chamadas de serviço externas](https://www.mock-server.com/mock_server/creating_expectations.html).
   + Opcional, somente usado se o código de trabalho fizer solicitações HTTP próprias

## Gravando um caso de teste

Esse caso de teste afirma a entrada parametrizada (`params.json`) para o arquivo de entrada (`file.jpg`) que gera a renderização PNG esperada (`rendition.png`).

1. Primeiro, exclua o caso de testes `simple-worker` gerado automaticamente em `/test/asset-compute/simple-worker`, pois isso é inválido, pois nosso trabalhador não copia mais simplesmente a fonte para a representação.
1. Crie uma nova pasta de caso de teste em `/test/asset-compute/worker/success-parameterized` para testar uma execução bem-sucedida do trabalhador que gera uma representação PNG.
1. Na pasta `success-parameterized`, adicione o arquivo de entrada [de teste](./assets/test/success-parameterized/file.jpg) para esse caso de teste e o nomeie como `file.jpg`.
1. Na pasta `success-parameterized`, adicione um novo arquivo chamado `params.json` que defina os parâmetros de entrada do trabalhador:

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   Esses são os mesmos valores/chaves passados para a definição de perfil de Asset compute da [Ferramenta de desenvolvimento](../develop/development-tool.md), menos a chave `worker`.

1. Adicione o [arquivo de representação esperado](./assets/test/success-parameterized/rendition.png) a esse caso de teste e o nomeie como `rendition.png`. Este arquivo representa a saída esperada do trabalhador para a entrada `file.jpg` fornecida.
1. Na linha de comando, execute os testes na raiz do projeto executando `aio app test`
   + Certifique-se de que o [Docker Desktop](../set-up/development-environment.md#docker) e as imagens Docker de suporte estão instaladas e iniciadas
   + Encerrar qualquer instância da Ferramenta de desenvolvimento em execução

![Teste - Sucesso  ](./assets/test/success-parameterized/result.png)

## Gravando um caso de teste de verificação de erro

Este caso de teste testa para garantir que o trabalhador jogue o erro apropriado quando o parâmetro `contrast` estiver definido como um valor inválido.

1. Crie uma nova pasta de caso de teste em `/test/asset-compute/worker/error-contrast` para testar uma execução incorreta do trabalhador devido a um valor de parâmetro `contrast` inválido.
1. Na pasta `error-contrast`, adicione o arquivo de entrada [de teste](./assets/test/error-contrast/file.jpg) para esse caso de teste e o nomeie como `file.jpg`. O conteúdo desse arquivo é irrelevante para esse teste, basta existir para passar da verificação &quot;Origem corrompida&quot;, para alcançar as `rendition.instructions` verificações de validade, que esse caso de teste valida.
1. Na pasta `error-contrast`, adicione um novo arquivo chamado `params.json` que defina os parâmetros de entrada do trabalhador com o conteúdo:

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Defina os parâmetros `contrast` como `10`, um valor inválido, como o contraste deve estar entre -1 e 1, para acionar um `RenditionInstructionsError`.
   + Repare que o erro apropriado é lançado nos testes definindo a chave `errorReason` para o &quot;motivo&quot; associado ao erro esperado. Esse parâmetro de contraste inválido lança o [erro personalizado](../develop/worker.md#errors), `RenditionInstructionsError`, portanto, defina o `errorReason` para o motivo deste erro, ou`rendition_instructions_error` para afirmar que ele é lançado.

1. Como nenhuma representação deve ser gerada durante uma execução de erro, nenhum arquivo `rendition.<extension>` é necessário.
1. Execute o conjunto de teste a partir da raiz do projeto executando o comando `aio app test`
   + Certifique-se de que o [Docker Desktop](../set-up/development-environment.md#docker) e as imagens Docker de suporte estão instaladas e iniciadas
   + Encerrar qualquer instância da Ferramenta de desenvolvimento em execução

![Teste - Contraste de erros](./assets/test/error-contrast/result.png)

## Testar casos no Github

Os casos finais de teste estão disponíveis no Github em:

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Resolução de problemas

+ [Nenhuma representação gerada durante a execução do teste](../troubleshooting.md#test-no-rendition-generated)
+ [O teste gera representação incorreta](../troubleshooting.md#tests-generates-incorrect-rendition)
