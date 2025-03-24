---
title: Solução de problemas de extensibilidade do Asset Compute para AEM Assets
description: A seguir há um índice de problemas e erros comuns, juntamente com as resoluções, que podem ser encontrados ao desenvolver e implantar trabalhadores personalizados do Asset Compute para o AEM Assets.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
duration: 260
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1218'
ht-degree: 0%

---

# Solução de problemas de extensibilidade do Asset Compute

A seguir há um índice de problemas e erros comuns, juntamente com as resoluções, que podem ser encontrados ao desenvolver e implantar trabalhadores personalizados do Asset Compute para o AEM Assets.

## Desenvolver{#develop}

### A representação é retornada parcialmente desenhada/corrompida{#rendition-returned-partially-drawn-or-corrupt}

+ __Erro__: a representação é renderizada de forma incompleta (quando uma imagem) ou está corrompida e não pode ser aberta.

  ![A representação foi retornada parcialmente desenhada](./assets/troubleshooting/develop__await.png)

+ __Causa__: a função `renditionCallback` do trabalhador está sendo encerrada antes que a representação possa ser completamente gravada em `rendition.path`.
+ __Solução__: revise o código de trabalho personalizado e verifique se todas as chamadas assíncronas foram tornadas síncronas usando `await`.

## Ferramenta de desenvolvimento{#development-tool}

### Arquivo Console.json ausente no projeto do Asset Compute{#missing-console-json}

+ __Erro:__ Erro: arquivos necessários ausentes na validação (`.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY`) em setupAssetCompute assíncrono (`.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY`)
+ __Causa:__ O arquivo `console.json` está ausente da raiz do projeto do Asset Compute
+ __Solução:__ baixe um novo `console.json` do seu projeto do Adobe I/O
   1. Em console.adobe.io, abra o projeto do Adobe I/O para o qual o projeto do Asset Compute está configurado para usar
   1. Toque no botão __Baixar__ na parte superior direita
   1. Salve o arquivo baixado na raiz do seu projeto do Asset Compute usando o nome de arquivo `console.json`

### Recuo YAML incorreto em manifest.yml{#incorrect-yaml-indentation}

+ __Erro:__ YAMLException: recuo incorreto de uma entrada de mapeamento na linha X, coluna Y:(via padrão fora do comando `aio app run`)
+ __Causa:__ os arquivos Yaml são sensíveis a espaços em branco. É provável que seu recuo esteja incorreto.
+ __Solução:__ revise `manifest.yml` e verifique se todo o recuo está correto.

### O limite memorySize está definido como muito baixo{#memorysize-limit-is-set-too-low}

+ __Erro:__ OpenWhiskError do Servidor de Desenvolvimento Local: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true Retornou HTTP 400 (Solicitação Incorreta) —> &quot;O conteúdo da solicitação estava malformado:falha de requisito: memória 64 MB abaixo do limite permitido de 134217728 B&quot;
+ __Causa:__ Um limite de `memorySize` para o trabalhador no `manifest.yml` foi definido abaixo do limite mínimo permitido, conforme relatado pela mensagem de erro em bytes.
+ __Solução:__ revise os limites de `memorySize` em `manifest.yml` e verifique se todos eles são maiores que o limite mínimo permitido.

### A Ferramenta de desenvolvimento não pode ser iniciada devido à falta de private.key{#missing-private-key}

+ __Erro:__ Servidor de Desenvolvimento LocalErro: arquivos necessários ausentes em validatePrivateKeyFile.... (pelo padrão fora do comando `aio app run`)
+ __Causa:__ O valor `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` no arquivo `.env`, não aponta para `private.key` ou `private.key` não é legível pelo usuário atual.
+ __Solução:__ revise o valor `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` no arquivo `.env` e verifique se ele contém o caminho completo e absoluto para o `private.key` no sistema de arquivos.

### Lista suspensa de arquivos do Source incorreta{#source-files-dropdown-incorrect}

A Ferramenta de Desenvolvimento do Asset Compute pode entrar em um estado em que extrai dados obsoletos, e é mais visível na lista suspensa __Arquivo Source__ exibindo itens incorretos.

+ __Erro:__ a lista suspensa de arquivos do Source exibe itens incorretos.
+ __Causa:__ O estado obsoleto do navegador em cache causa
+ __Solução:__ no navegador, limpe completamente o &quot;estado do aplicativo&quot; da guia do navegador, o cache do navegador, o armazenamento local e o service worker.

### Parâmetro de consulta devToolToken ausente ou inválido{#missing-or-invalid-devtooltoken-query-parameter}

+ __Erro:__ Notificação &quot;Não Autorizada&quot; na Ferramenta de Desenvolvimento do Asset Compute
+ __Causa:__ `devToolToken` está ausente ou é inválido
+ __Solução:__ feche a janela do navegador da Ferramenta de Desenvolvimento da Asset Compute, encerre todos os processos da Ferramenta de Desenvolvimento iniciados pelo comando `aio app run` e reinicie a Ferramenta de Desenvolvimento (usando `aio app run`).

### Não foi possível remover os arquivos de origem{#unable-to-remove-source-files}

+ __Erro:__ Não há como remover arquivos de origem adicionados da interface de Ferramentas de Desenvolvimento
+ __Causa:__ esta funcionalidade não foi implementada
+ __Solução:__ faça logon no seu provedor de armazenamento na nuvem usando as credenciais definidas em `.env`. Localize o contêiner usado pelas Ferramentas de Desenvolvimento (também especificado em `.env`), navegue até a pasta __origem__ e exclua todas as imagens de origem. Talvez seja necessário executar as etapas descritas na [lista suspensa de arquivos do Source incorreta](#source-files-dropdown-incorrect) se os arquivos de origem excluídos continuarem a ser exibidos na lista suspensa, pois eles podem ser armazenados em cache localmente no &quot;estado do aplicativo&quot; das Ferramentas de Desenvolvimento.

  ![Armazenamento de Blobs do Microsoft Azure](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Teste{#test}

### Nenhuma representação gerada durante a execução do teste{#test-no-rendition-generated}

+ __Erro:__ Falha: nenhuma representação gerada.
+ __Causa:__ o trabalhador falhou ao gerar uma representação devido a um erro inesperado, como um erro de sintaxe do JavaScript.
+ __Solução:__ Revise o `test.log` da execução de teste em `/build/test-results/test-worker/test.log`. Localize a seção nesse arquivo correspondente ao caso de teste com falha e verifique se há erros.

  ![Solução de problemas - Nenhuma representação gerada](./assets/troubleshooting/test__no-rendition-generated.png)

### O teste gera representação incorreta, causando falha no teste{#tests-generates-incorrect-rendition}

+ __Erro:__ Falha: a representação &#39;rendition.xxx&#39; não foi a esperada.
+ __Causa:__ o trabalhador gerou uma representação que não era a mesma que a `rendition.<extension>` fornecida no caso de teste.
   + Se o arquivo esperado `rendition.<extension>` não for criado exatamente da mesma maneira que a representação gerada localmente no caso de teste, o teste pode falhar, pois pode haver alguma diferença nos bits. Por exemplo, se o trabalhador do Asset Compute alterar o contraste usando APIs e o resultado esperado for criado ajustando o contraste no Adobe Photoshop CC, os arquivos poderão ter a mesma aparência, mas pequenas variações nos bits poderão ser diferentes.
+ __Solução:__ revise a saída da representação do teste navegando até `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, e compare-a com o arquivo de representação esperado no caso de teste. Para criar um ativo esperado exato:
   + Use a Ferramenta de desenvolvimento para gerar uma representação, validar se ela está correta e usá-la como o arquivo de representação esperado
   + Ou valide o arquivo gerado pelo teste em `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, confirme se ele está correto e use-o como o arquivo de representação esperado

## Depurar

### O depurador não anexa{#debugger-does-not-attach}

+ __Erro__: Erro ao processar lançamento: Erro: Não foi possível conectar ao destino de depuração em...
+ __Causa__: o Docker Desktop não está sendo executado no sistema local. Verifique isso revisando o Console de depuração do código VS (Exibir > Console de depuração), confirmando que esse erro foi relatado.
+ __Solução__: inicie o [Docker Desktop e confirme se as imagens do Docker necessárias estão instaladas](./set-up/development-environment.md#docker).

### Os pontos de interrupção não estão pausando{#breakpoints-no-pausing}

+ __Erro__: ao executar o trabalho do Asset Compute a partir da Ferramenta de Desenvolvimento que pode ser depurada, o Código VS não pausa nos pontos de interrupção.

#### Depurador de código VS não anexado{#vs-code-debugger-not-attached}

+ __Causa:__ o depurador de código do VS foi interrompido/desconectado.
+ __Solução:__ reinicie o depurador de código do VS e verifique se ele se anexa observando o console Saída de depuração do código do VS (Exibir > Console de depuração)

#### Depurador de código VS anexado após o início da execução do trabalho{#vs-code-debugger-attached-after-worker-execution-began}

+ __Causa:__ o depurador de código do VS não foi anexado antes de tocar em __Executar__ na Ferramenta de desenvolvimento.
+ __Solução:__ certifique-se de que o depurador foi anexado revisando o Console de Depuração do Código VS (Exibir > Console de Depuração) e, em seguida, execute novamente o Asset Compute Worker a partir da Ferramenta de Desenvolvimento.

### O trabalhador atinge o tempo limite durante a depuração{#worker-times-out-while-debugging}

+ __Erro__: o Console de Depuração relata &quot;O tempo limite da ação será de -XXX milissegundos&quot; ou a pré-visualização da representação ](./develop/development-tool.md) da Ferramenta de Desenvolvimento do Asset Compute gira indefinidamente ou[
+ __Causa__: o tempo limite do trabalhador conforme definido em [manifest.yml](./develop/manifest.md) foi excedido durante a depuração.
+ __Solução__: aumente temporariamente o tempo limite do trabalhador no [manifest.yml](./develop/manifest.md) ou acelere as atividades de depuração.

### Não é possível encerrar o processo do depurador{#cannot-terminate-debugger-process}

+ __Erro__: `Ctrl-C` na linha de comando não encerra o processo do depurador (`npx adobe-asset-compute devtool`).
+ __Causa__: um erro em `@adobe/aio-cli-plugin-asset-compute` 1.3.x, resulta no `Ctrl-C` não ser reconhecido como um comando de terminação.
+ __Solução__: atualize `@adobe/aio-cli-plugin-asset-compute` para a versão 1.4.1+

  ```
  $ aio update
  ```

  ![Solução de problemas - atualização da aio](./assets/troubleshooting/debug__terminate.png)

## Implantar{#deploy}

### Representação personalizada ausente do ativo no AEM{#custom-rendition-missing-from-asset}

+ __Erro:__ os ativos novos e reprocessados foram processados com êxito, mas a representação personalizada está ausente

#### Perfil de processamento não aplicado à pasta antecessora

+ __Causa:__ o ativo não existe em uma pasta com o Perfil de Processamento que usa o trabalhador personalizado
+ __Solução:__ aplique o perfil de processamento a uma pasta ancestral do ativo

#### Perfil de processamento substituído por perfil de processamento inferior

+ __Causa:__ o ativo existe abaixo de uma pasta com o Perfil de Processamento do trabalhador personalizado aplicado, no entanto, um Perfil de Processamento diferente que não usa o trabalhador do cliente foi aplicado entre essa pasta e o ativo.
+ __Solução:__ Combine ou reconcilie os dois Perfis de Processamento e remova o Perfil de Processamento intermediário

### O processamento de ativos falha no AEM{#asset-processing-fails}

+ __Erro:__ Selo de falha de processamento de ativo exibido no ativo
+ __Causa:__ Erro na execução do trabalho personalizado
+ __Solução:__ siga as instruções em [depurando ativações do Adobe I/O Runtime](./test-debug/debug.md#aio-app-logs) usando `aio app logs`.
