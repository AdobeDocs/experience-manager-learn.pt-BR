---
title: Solução de problemas de extensibilidade do Asset compute para o AEM Assets
description: A seguir há um índice de problemas e erros comuns, juntamente com as resoluções, que podem ser encontrados ao desenvolver e implantar trabalhadores de Asset compute personalizados para o AEM Assets.
feature: Asset Compute Microservices
topics: renditions, metadata, development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1239'
ht-degree: 0%

---

# Solução de problemas de extensibilidade do Asset compute

A seguir há um índice de problemas e erros comuns, juntamente com as resoluções, que podem ser encontrados ao desenvolver e implantar trabalhadores de Asset compute personalizados para o AEM Assets.

## Desenvolver{#develop}

### A representação é retornada parcialmente desenhada/corrompida{#rendition-returned-partially-drawn-or-corrupt}

+ __Erro__: a representação é renderizada de forma incompleta (quando uma imagem) ou está corrompida e não pode ser aberta.

  ![A representação é retornada parcialmente desenhada](./assets/troubleshooting/develop__await.png)

+ __Causa__: O do trabalhador `renditionCallback` for encerrada antes que a representação possa ser completamente gravada `rendition.path`.
+ __Resolução__: revise o código de trabalho personalizado e verifique se todas as chamadas assíncronas foram tornadas síncronas usando `await`.

## Ferramenta de desenvolvimento{#development-tool}

### Arquivo Console.json ausente do projeto do Asset compute{#missing-console-json}

+ __Erro:__ Erro: arquivos necessários ausentes na validação (.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:AA) em setupAssetCompute assíncrono (.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:AA)
+ __Causa:__ A variável `console.json` arquivo está ausente na raiz do projeto do Asset compute
+ __Resolução:__ Baixar um novo `console.json` do seu projeto Adobe I/O
   1. Em console.adobe.io, abra o projeto Adobe I/O que o projeto Asset compute está configurado para usar
   1. Toque no __Baixar__ botão na parte superior direita
   1. Salve o arquivo baixado na raiz do projeto do Asset compute usando o nome de arquivo `console.json`

### Recuo YAML incorreto em manifest.yml{#incorrect-yaml-indentation}

+ __Erro:__ YAMLException: recuo incorreto de uma entrada de mapeamento na linha X, coluna Y:(via padrão fora de `aio app run` command)
+ __Causa:__ Os arquivos Yaml são sensíveis a espaços em branco, provavelmente seu recuo está incorreto.
+ __Resolução:__ Analise seu `manifest.yml` e certifique-se de que todo o recuo esteja correto.

### O limite memorySize está definido como muito baixo{#memorysize-limit-is-set-too-low}

+ __Erro:__  OpenWhiskError do Servidor de Desenvolvimento Local: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true Retornou HTTP 400 (Solicitação Incorreta) —> &quot;O conteúdo da solicitação estava malformado:requisito falhou: memória 64 MB abaixo do limite permitido de 134217728 B&quot;
+ __Causa:__ A `memorySize` limite para o trabalhador na `manifest.yml` foi definido abaixo do limite mínimo permitido, conforme relatado pela mensagem de erro em bytes.
+ __Resolução:__  Revise o `memorySize` limites no `manifest.yml` e garantir que todos sejam maiores do que o limite mínimo permitido.

### A Ferramenta de desenvolvimento não pode ser iniciada devido à falta de private.key{#missing-private-key}

+ __Erro:__ Erro do servidor de desenvolvimento local: arquivos necessários ausentes em validatePrivateKeyFile... (por padrão, fora de `aio app run` command)
+ __Causa:__ A variável `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valor em `.env` arquivo, não aponta para `private.key` ou `private.key` não é legível pelo usuário atual.
+ __Resolução:__ Revise o `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valor em `.env` e certifique-se de que ele contenha o caminho completo e absoluto para o `private.key` em seu sistema de arquivos.

### Lista suspensa de arquivos de origem incorreta{#source-files-dropdown-incorrect}

A Ferramenta de desenvolvimento de assets compute pode entrar em um estado em que extrai dados obsoletos e é mais visível na __Arquivo de origem__ lista suspensa exibindo itens incorretos.

+ __Erro:__ A lista suspensa de arquivos de origem exibe itens incorretos.
+ __Causa:__ O estado obsoleto do navegador em cache causa
+ __Resolução:__ No navegador, limpe completamente o &quot;estado do aplicativo&quot; da guia do navegador, o cache do navegador, o armazenamento local e o service worker.

### Parâmetro de consulta devToolToken ausente ou inválido{#missing-or-invalid-devtooltoken-query-parameter}

+ __Erro:__ Notificação &quot;Não autorizado&quot; na Ferramenta de desenvolvimento do Asset compute
+ __Causa:__ `devToolToken` está ausente ou é inválido
+ __Resolução:__ Feche a janela do navegador da Ferramenta de desenvolvimento do Asset compute e encerre todos os processos da Ferramenta de desenvolvimento iniciados por meio da `aio app run` e reinicie a Ferramenta de desenvolvimento (usando `aio app run`).

### Não foi possível remover os arquivos de origem{#unable-to-remove-source-files}

+ __Erro:__ Não há como remover arquivos de origem adicionados da interface das Ferramentas de desenvolvimento
+ __Causa:__ Esta funcionalidade não foi implementada
+ __Resolução:__ Faça logon no provedor de armazenamento na nuvem usando as credenciais definidas no `.env`. Localize o contêiner usado pelas Ferramentas de desenvolvimento (também especificado em `.env`), navegue até o __origem__ e exclua todas as imagens de origem. Talvez seja necessário executar as etapas descritas em [Lista suspensa de arquivos de origem incorreta](#source-files-dropdown-incorrect) se os arquivos de origem excluídos continuarem a ser exibidos na lista suspensa, pois podem ser armazenados em cache localmente no &quot;estado do aplicativo&quot; das Ferramentas de desenvolvimento.

  ![Armazenamento de blobs do Microsoft Azure](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Testar{#test}

### Nenhuma representação gerada durante a execução do teste{#test-no-rendition-generated}

+ __Erro:__ Falha: nenhuma representação gerada.
+ __Causa:__ O trabalhador falhou ao gerar uma representação devido a um erro inesperado, como um erro de sintaxe JavaScript.
+ __Resolução:__ Revisar o da execução de teste `test.log` em `/build/test-results/test-worker/test.log`. Localize a seção nesse arquivo correspondente ao caso de teste com falha e verifique se há erros.

  ![Solução de problemas - Nenhuma representação gerada](./assets/troubleshooting/test__no-rendition-generated.png)

### O teste gera representação incorreta, causando falha no teste{#tests-generates-incorrect-rendition}

+ __Erro:__ Falha: a representação &quot;rendition.xxx&quot; não foi executada conforme esperado.
+ __Causa:__ O trabalhador gerou uma representação que não era a mesma que a `rendition.<extension>` fornecido no caso de teste.
   + Se o esperado `rendition.<extension>` o arquivo não é criado exatamente da mesma maneira que a representação gerada localmente no caso de teste, o teste pode falhar, pois pode haver alguma diferença nos bits. Por exemplo, se o trabalhador do Asset compute alterar o contraste usando APIs e o resultado esperado for criado ajustando o contraste no Adobe Photoshop CC, os arquivos poderão parecer iguais, mas variações secundárias nos bits poderão ser diferentes.
+ __Resolução:__ Revise a saída da representação do teste navegando até `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`e compare-o ao arquivo de representação esperado no caso de teste. Para criar um ativo esperado exato:
   + Use a Ferramenta de desenvolvimento para gerar uma representação, validar se ela está correta e usá-la como o arquivo de representação esperado
   + Ou valide o arquivo gerado pelo teste em `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, confirme se está correto e use-o como o arquivo de representação esperado

## Depurar

### O depurador não anexa{#debugger-does-not-attach}

+ __Erro__: Erro ao processar lançamento: Erro: Não foi possível conectar ao destino de depuração em...
+ __Causa__: o Docker Desktop não está sendo executado no sistema local. Verifique isso revisando o Console de depuração do código VS (Exibir > Console de depuração), confirmando que esse erro foi relatado.
+ __Resolução__: Início [Docker Desktop e confirme se as imagens do Docker necessárias estão instaladas](./set-up/development-environment.md#docker).

### Os pontos de interrupção não estão pausando{#breakpoints-no-pausing}

+ __Erro__: ao executar o Asset compute worker a partir da Ferramenta de desenvolvimento depurável, o Código VS não pausa nos pontos de interrupção.

#### Depurador de código VS não anexado{#vs-code-debugger-not-attached}

+ __Causa:__ O depurador de código do VS foi interrompido/desconectado.
+ __Resolução:__ Reinicie o depurador de código do VS e verifique se ele se anexa observando o console Saída de depuração do código do VS (Exibir > Console de depuração)

#### Depurador de código VS anexado após o início da execução do trabalho{#vs-code-debugger-attached-after-worker-execution-began}

+ __Causa:__ O depurador de código do VS não foi anexado antes de tocar __Executar__ em Ferramenta de desenvolvimento.
+ __Resolução:__ Verifique se o depurador foi anexado revisando o Console de depuração do código VS (Exibir > Console de depuração) e, em seguida, execute novamente o trabalhador do Asset compute na Ferramenta de desenvolvimento.

### O trabalhador atinge o tempo limite durante a depuração{#worker-times-out-while-debugging}

+ __Erro__: o Console de depuração relata &quot;O tempo limite da ação será de -XXX milissegundos&quot; ou [Ferramentas de desenvolvimento do Asset compute](./develop/development-tool.md) a visualização de representação gira indefinidamente ou
+ __Causa__: O tempo limite do worker conforme definido na variável [manifest.yml](./develop/manifest.md) é excedido durante a depuração.
+ __Resolução__: aumente temporariamente o tempo limite do trabalhador no [manifest.yml](./develop/manifest.md) ou acelere as atividades de depuração.

### Não é possível encerrar o processo do depurador{#cannot-terminate-debugger-process}

+ __Erro__: `Ctrl-C` na linha de comando não encerra o processo do depurador (`npx adobe-asset-compute devtool`).
+ __Causa__: um erro no `@adobe/aio-cli-plugin-asset-compute` 1.3.x, resulta em `Ctrl-C` não é reconhecido como um comando de terminação.
+ __Resolução__: Atualizar `@adobe/aio-cli-plugin-asset-compute` para a versão 1.4.1+

  ```
  $ aio update
  ```

  ![Solução de problemas - atualização do aio](./assets/troubleshooting/debug__terminate.png)

## Implantar{#deploy}

### Representação personalizada ausente do ativo no AEM{#custom-rendition-missing-from-asset}

+ __Erro:__ Os ativos novos e reprocessados são processados com sucesso, mas a representação personalizada está ausente

#### Perfil de processamento não aplicado à pasta antecessora

+ __Causa:__ O ativo não existe em uma pasta com o Perfil de processamento que usa o trabalhador personalizado
+ __Resolução:__ Aplicar o perfil de processamento a uma pasta ancestral do ativo

#### Perfil de processamento substituído por perfil de processamento inferior

+ __Causa:__ O ativo existe abaixo de uma pasta com o Perfil de processamento do trabalhador personalizado aplicado, no entanto, um Perfil de processamento diferente que não usa o trabalhador do cliente foi aplicado entre essa pasta e o ativo.
+ __Resolução:__ Combine ou reconcilie os dois perfis de processamento e remova o perfil de processamento intermediário

### O processamento de ativos falha no AEM{#asset-processing-fails}

+ __Erro:__ Selo Falha de processamento de ativo exibido no ativo
+ __Causa:__ Ocorreu um erro na execução do trabalho personalizado
+ __Resolução:__ Siga as instruções em [depurar ativações do Adobe I/O Runtime](./test-debug/debug.md#aio-app-logs) usar `aio app logs`.
