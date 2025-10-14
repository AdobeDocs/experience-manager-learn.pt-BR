---
title: Testar um trabalhador do Asset Compute
description: O projeto do Asset Compute define um padrão para criar e executar facilmente testes de trabalhadores do Asset Compute.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 04992caf-b715-4701-94a8-6257e9bd300c
duration: 142
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# Testar um trabalhador do Asset Compute

O projeto do Asset Compute define um padrão para criar e executar facilmente [testes de trabalhadores do Asset Compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html?lang=pt-BR).

## Anatomia de um teste do trabalhador

Os testes dos trabalhadores da Asset Compute são divididos em conjuntos de testes e, em cada conjunto de testes, um ou mais casos de teste afirmam uma condição a ser testada.

A estrutura dos testes em um projeto do Asset Compute é a seguinte:

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
   + Arquivo Source a ser testado (a extensão pode ser qualquer coisa, exceto `.link`)
   + Obrigatório
+ `rendition.<extension>`
   + Representação esperada
   + Obrigatório, exceto para testes de erros
+ `params.json`
   + As instruções do JSON de representação única
   + Opcional
+ `validate`
   + Um script que obtém os caminhos de arquivo de representação esperados e reais como argumentos e deve retornar o código de saída 0 se o resultado for ok, ou um código de saída diferente de zero se a validação ou comparação falhar.
   + Opcional, o padrão é o comando `diff`
   + Use um script de shell que envolva um comando de execução do docker para usar ferramentas de validação diferentes
+ `mock-<host-name>.json`
   + Respostas HTTP formatadas em JSON para [chamadas de serviço externo de zombaria](https://www.mock-server.com/mock_server/creating_expectations.html).
   + Opcional, usado somente se o código do trabalhador fizer solicitações HTTP próprias

## Escrever um caso de teste

Este caso de teste declara que a entrada parametrizada (`params.json`) para o arquivo de entrada (`file.jpg`) gera a representação PNG esperada (`rendition.png`).

1. Primeiro exclua o caso de testes `simple-worker` gerado automaticamente em `/test/asset-compute/simple-worker`, pois ele é inválido, já que nosso trabalhador não copia mais simplesmente a origem para a representação.
1. Crie uma nova pasta de casos de teste em `/test/asset-compute/worker/success-parameterized` para testar uma execução bem-sucedida do operador que gera uma representação PNG.
1. Na pasta `success-parameterized`, adicione o [arquivo de entrada](./assets/test/success-parameterized/file.jpg) de teste para este caso de teste e nomeie-o como `file.jpg`.
1. Na pasta `success-parameterized`, adicione um novo arquivo chamado `params.json` que defina os parâmetros de entrada do trabalhador:

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   Esses são os mesmos valores/chaves passados para a [definição de perfil do Asset Compute da Ferramenta de Desenvolvimento](../develop/development-tool.md), menos a chave `worker`.

1. Adicione o [arquivo de representação](./assets/test/success-parameterized/rendition.png) esperado a este caso de teste e nomeie-o `rendition.png`. Este arquivo representa a saída esperada do trabalhador para a entrada especificada `file.jpg`.
1. Na linha de comando, execute os testes na raiz do projeto executando `aio app test`
   + Verifique se o [Docker Desktop](../set-up/development-environment.md#docker) e as imagens do Docker de suporte estão instalados e iniciados
   + Encerrar todas as instâncias da Ferramenta de desenvolvimento em execução

![Teste - Êxito &#x200B;](./assets/test/success-parameterized/result.png)

## Gravação de um caso de teste de verificação de erro

Este caso de teste testa para garantir que o trabalhador lança o erro apropriado quando o parâmetro `contrast` é definido para um valor inválido.

1. Crie uma nova pasta de casos de teste em `/test/asset-compute/worker/error-contrast` para testar uma execução de erro do operador devido a um valor de parâmetro `contrast` inválido.
1. Na pasta `error-contrast`, adicione o [arquivo de entrada](./assets/test/error-contrast/file.jpg) de teste para este caso de teste e nomeie-o como `file.jpg`. O conteúdo deste arquivo é irrelevante para este teste, ele só precisa existir para passar pela verificação de &quot;Origem corrompida&quot;, para alcançar as verificações de validade `rendition.instructions`, que este caso de teste valida.
1. Na pasta `error-contrast`, adicione um novo arquivo chamado `params.json` que define os parâmetros de entrada do trabalhador com o conteúdo:

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Defina `contrast` parâmetros como `10`, um valor inválido, já que o contraste deve estar entre -1 e 1, para gerar um `RenditionInstructionsError`.
   + Declarar que o erro apropriado é lançado nos testes, definindo a chave `errorReason` para o &quot;motivo&quot; associado ao erro esperado. Este parâmetro de contraste inválido gera o [erro personalizado](../develop/worker.md#errors), `RenditionInstructionsError`, portanto, defina o `errorReason` como o motivo deste erro, ou `rendition_instructions_error` para confirmar que ele foi lançado.

1. Como nenhuma representação deve ser gerada durante uma execução com erro, nenhum arquivo `rendition.<extension>` é necessário.
1. Execute o conjunto de testes na raiz do projeto executando o comando `aio app test`
   + Verifique se o [Docker Desktop](../set-up/development-environment.md#docker) e as imagens do Docker de suporte estão instalados e iniciados
   + Encerrar todas as instâncias da Ferramenta de desenvolvimento em execução

![Teste - Contraste de erros](./assets/test/error-contrast/result.png)

## Casos de teste no Github

Os casos de teste finais estão disponíveis no Github em:

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Resolução de problemas

+ [Nenhuma representação gerada durante a execução do teste](../troubleshooting.md#test-no-rendition-generated)
+ [Teste gera representação incorreta](../troubleshooting.md#tests-generates-incorrect-rendition)
