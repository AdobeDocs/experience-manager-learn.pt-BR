---
title: Testar um trabalhador de Asset compute
description: O projeto do Asset compute define um padrão para criar e executar facilmente testes de trabalhadores do Asset compute.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 04992caf-b715-4701-94a8-6257e9bd300c
duration: 175
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# Testar um trabalhador de Asset compute

O projeto do Asset compute define um padrão para fácil criação e execução [testes de trabalhadores Assets compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html).

## Anatomia de um teste do trabalhador

Os testes dos trabalhadores assets compute são divididos em conjuntos de testes e, em cada conjunto de testes, um ou mais casos de teste afirmam uma condição a ser testada.

A estrutura dos testes em um projeto do Asset compute é a seguinte:

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
   + Arquivo de origem a ser testado (a extensão pode ser qualquer coisa, exceto `.link`)
   + Obrigatório
+ `rendition.<extension>`
   + Representação esperada
   + Obrigatório, exceto para testes de erros
+ `params.json`
   + As instruções do JSON de representação única
   + Opcional
+ `validate`
   + Um script que obtém os caminhos de arquivo de representação esperados e reais como argumentos e deve retornar o código de saída 0 se o resultado for ok, ou um código de saída diferente de zero se a validação ou comparação falhar.
   + Opcional, o padrão é `diff` comando
   + Use um script de shell que envolva um comando de execução do docker para usar ferramentas de validação diferentes
+ `mock-<host-name>.json`
   + Respostas HTTP formatadas em JSON para [zombando de chamadas de serviço externas](https://www.mock-server.com/mock_server/creating_expectations.html).
   + Opcional, usado somente se o código do trabalhador fizer solicitações HTTP próprias

## Escrever um caso de teste

Este caso de teste declara a entrada parametrizada (`params.json`) para o arquivo de entrada (`file.jpg`) gera a representação PNG esperada (`rendition.png`).

1. Primeiro exclua os relatórios gerados automaticamente `simple-worker` testa o caso em `/test/asset-compute/simple-worker` como isso é inválido, já que nosso trabalhador não copia mais simplesmente a origem para a representação.
1. Criar uma nova pasta de casos de teste em `/test/asset-compute/worker/success-parameterized` para testar uma execução bem-sucedida do worker que gera uma representação PNG.
1. No `success-parameterized` pasta, adicione o teste [arquivo de entrada](./assets/test/success-parameterized/file.jpg) para este caso de teste e nomeie-o `file.jpg`.
1. No `success-parameterized` , adicione um novo arquivo chamado `params.json` que define os parâmetros de entrada do trabalhador:

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   Esses são os mesmos valores/chaves passados para o [Definição do perfil de Asset compute da ferramenta de desenvolvimento](../develop/development-tool.md), menos o `worker` chave.

1. Adicione o esperado [arquivo de representação](./assets/test/success-parameterized/rendition.png) para este caso de teste e nomeie-o `rendition.png`. Este arquivo representa a saída esperada do trabalhador para a entrada especificada `file.jpg`.
1. Na linha de comando, execute os testes na raiz do projeto executando `aio app test`
   + Assegurar [Desktop Docker](../set-up/development-environment.md#docker) As imagens do Docker e de suporte foram instaladas e iniciadas
   + Encerrar todas as instâncias da Ferramenta de desenvolvimento em execução

![Teste - Sucesso ](./assets/test/success-parameterized/result.png)

## Gravação de um caso de teste de verificação de erro

Esse caso de teste testa para garantir que o trabalhador acione o erro apropriado quando o `contrast` O parâmetro está definido com um valor inválido.

1. Criar uma nova pasta de casos de teste em `/test/asset-compute/worker/error-contrast` para testar uma execução incorreta do trabalhador devido a uma falha `contrast` valor do parâmetro.
1. No `error-contrast` pasta, adicione o teste [arquivo de entrada](./assets/test/error-contrast/file.jpg) para este caso de teste e nomeie-o `file.jpg`. O conteúdo deste arquivo é irrelevante para este teste, ele só precisa existir para passar pela verificação &quot;Fonte corrompida&quot;, a fim de alcançar o `rendition.instructions` verificações de validade, que este caso de teste valida.
1. No `error-contrast` , adicione um novo arquivo chamado `params.json` que define os parâmetros de entrada do trabalhador com o conteúdo:

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Definir `contrast` parâmetros para `10`, um valor inválido, pois o contraste deve estar entre -1 e 1, para gerar um `RenditionInstructionsError`.
   + Declarar que o erro apropriado é lançado nos testes definindo o `errorReason` chave para o &quot;motivo&quot; associado ao erro esperado. Este parâmetro de contraste inválido lança o [erro personalizado](../develop/worker.md#errors), `RenditionInstructionsError`, por conseguinte, defina a `errorReason` ao motivo deste erro, ou`rendition_instructions_error` para afirmar que foi lançado.

1. Como nenhuma representação deve ser gerada durante uma execução com erro, nenhuma `rendition.<extension>` arquivo é necessário.
1. Execute o conjunto de testes na raiz do projeto executando o comando `aio app test`
   + Assegurar [Desktop Docker](../set-up/development-environment.md#docker) As imagens do Docker e de suporte foram instaladas e iniciadas
   + Encerrar todas as instâncias da Ferramenta de desenvolvimento em execução

![Teste - Contraste do erro](./assets/test/error-contrast/result.png)

## Casos de teste no Github

Os casos de teste finais estão disponíveis no Github em:

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Resolução de problemas

+ [Nenhuma representação gerada durante a execução do teste](../troubleshooting.md#test-no-rendition-generated)
+ [Teste gera representação incorreta](../troubleshooting.md#tests-generates-incorrect-rendition)
