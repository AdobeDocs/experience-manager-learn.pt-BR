---
title: Testar um trabalhador da Computação de ativos
description: O projeto Asset Compute define um padrão para criar e executar facilmente testes de funcionários da Asset Compute.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
translation-type: tm+mt
source-git-commit: 06632b90e5cdaf80b9343e5a69ab9c735d4a70f1
workflow-type: tm+mt
source-wordcount: '792'
ht-degree: 0%

---


# Testar um trabalhador da Computação de ativos

O projeto Asset Compute define um padrão para criar e executar facilmente [testes de funcionários](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html)da Asset Compute.

## Anatomia de um teste de trabalhador

Os testes dos funcionários da Computação de ativos são divididos em conjuntos de testes e, em cada conjunto de testes, um ou mais casos de teste que afirmam uma condição para teste.

A estrutura dos testes em um projeto do Asset Compute é a seguinte:

```
/actions/<worker-name>/index.js
...
/test/
  asset-compute/
    <worker-name>/           <--- Test suite for the worker
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
   + Opcional, o padrão é o `diff` comando
   + Usar um script de shell que vincula um comando docker run para usar diferentes ferramentas de validação
+ `mock-<host-name>.json`
   + Respostas HTTP formatadas JSON para [monitorar chamadas](https://www.mock-server.com/mock_server/creating_expectations.html)de serviço externas.
   + Opcional, usado somente se o código de trabalho fizer solicitações HTTP de sua própria

## Gravando um caso de teste

Este caso de teste declara a entrada parametrizada (`params.json`) para o arquivo de entrada (`file.jpg`) gera a renderização PNG esperada (`rendition.png`).

1. Primeiro, exclua o caso de `simple-worker` testes gerados automaticamente em `/test/asset-compute/simple-worker` caso de inválido, já que nosso trabalhador não copia mais a fonte para a execução.
1. Crie uma nova pasta de caso de teste em `/test/asset-compute/worker/success-parameterized` para testar uma execução bem-sucedida do trabalhador que gera uma execução PNG.
1. Na `success-parameterized` pasta, adicione o arquivo [de](./assets/test/success-parameterized/file.jpg) entrada de teste para este caso de teste e nomeie-o `file.jpg`.
1. Na `success-parameterized` pasta, adicione um novo arquivo chamado `params.json` que define os parâmetros de entrada do trabalhador:

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```
   Esses são os mesmos valores-chave passados para a definição [de perfil da ferramenta de](../develop/development-tool.md)desenvolvimento Asset Compute, menos a `worker` chave.
1. Adicione o arquivo [de](./assets/test/success-parameterized/rendition.png) execução esperado a esse caso de teste e nomeie-o `rendition.png`. Esse arquivo representa a saída esperada do trabalhador para a entrada em questão `file.jpg`.
1. Na linha de comando, execute os testes na raiz do projeto executando `aio app test`
   + Verifique se o [Docker Desktop](../set-up/development-environment.md#docker) e as imagens Docker de suporte estão instalados e iniciados
   + Encerrar todas as instâncias em execução da ferramenta de desenvolvimento

![Teste - Êxito ](./assets/test/success-parameterized/result.png)

## Gravando um caso de teste de verificação de erro

Este caso de teste testa para garantir que o trabalhador jogue o erro apropriado quando o `contrast` parâmetro estiver definido como um valor inválido.

1. Crie uma nova pasta de caso de teste em `/test/asset-compute/worker/error-contrast` para testar uma execução de erro do trabalhador devido a um valor de `contrast` parâmetro inválido.
1. Na `error-contrast` pasta, adicione o arquivo [de](./assets/test/error-contrast/file.jpg) entrada de teste para este caso de teste e nomeie-o `file.jpg`. O conteúdo deste arquivo é irrelevante para este teste, ele só precisa existir para passar da verificação &quot;Fonte corrompida&quot;, para chegar às verificações de `rendition.instructions` validade que esse caso de teste valida.
1. Na `error-contrast` pasta, adicione um novo arquivo chamado `params.json` que define os parâmetros de entrada do trabalhador com o conteúdo:

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Defina `contrast` os parâmetros como `10`, um valor inválido, pois o contraste deve estar entre -1 e 1, para lançar um `RenditionInstructionsError`.
   + Assegure-se de que o erro apropriado seja lançado nos testes definindo a `errorReason` chave para o &quot;motivo&quot; associado ao erro esperado. Esse parâmetro de contraste inválido lança o erro [](../develop/worker.md#errors)personalizado, `RenditionInstructionsError`portanto, define o `errorReason` para o motivo desse erro ou`rendition_instructions_error` para afirmar que ele é lançado.

1. Como nenhuma execução deve ser gerada durante uma execução de erro, nenhum `rendition.<extension>` arquivo é necessário.
1. Execute o conjunto de testes da raiz do projeto executando o comando `aio app test`
   + Verifique se o [Docker Desktop](../set-up/development-environment.md#docker) e as imagens Docker de suporte estão instalados e iniciados
   + Encerrar todas as instâncias em execução da ferramenta de desenvolvimento

![Teste - Contraste do erro](./assets/test/error-contrast/result.png)

## Resolução de problemas

### Nenhuma execução gerada

O caso de teste falha sem gerar uma execução.

+ __Erro:__ Falha: Nenhuma execução gerada.
+ __Causa:__ O trabalhador não conseguiu gerar uma execução devido a um erro inesperado, como um erro de sintaxe JavaScript.
+ __Resolução:__ Verifique se a execução do teste está `test.log` em `/build/test-results/test-worker/test.log`. Localize a seção neste arquivo correspondente ao caso de teste com falha e verifique se há erros.

   ![Solução de problemas - nenhuma execução gerada](./assets/test/troubleshooting__no-rendition-generated.png)

### O teste gera representação incorreta

O caso de teste falha ao gerar uma execução incorreta.

+ __Erro:__ Falha: A execução &#39;rendition.xxx&#39; não é a esperada.
+ __Causa:__ O trabalhador gera uma execução que não é a mesma que a `rendition.<extension>` fornecida no caso de teste.
   + Se o arquivo esperado não for criado exatamente da mesma maneira que a execução gerada localmente no caso de teste, o teste poderá falhar, pois pode haver alguma diferença nos bits. `rendition.<extension>` Se a renderização esperada no caso de teste for salva da Ferramenta de desenvolvimento, o que significa gerada no Adobe I/O Runtime, os bits poderão ser tecnicamente diferentes, causando a falha do teste, mesmo se, de uma perspectiva humana, os arquivos de renderização esperados e reais forem idênticos.
+ __Resolução:__ Revise a saída de representação do teste navegando até `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`o arquivo de representação esperado no caso de teste e compare-o com ele.
