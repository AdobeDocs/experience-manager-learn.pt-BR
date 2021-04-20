---
title: Solucionar problemas de extensibilidade do Asset Compute para o AEM Assets
description: Veja a seguir um índice de problemas e erros comuns, juntamente com as resoluções, que podem ser encontrados ao desenvolver e implantar trabalhadores do Asset Compute personalizados para o AEM Assets.
feature: Asset Compute Microservices
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1249'
ht-degree: 0%

---


# Solucionar problemas de extensibilidade do Asset Compute

Veja a seguir um índice de problemas e erros comuns, juntamente com as resoluções, que podem ser encontrados ao desenvolver e implantar trabalhadores do Asset Compute personalizados para o AEM Assets.

## Desenvolver{#develop}

### A representação é retornada parcialmente desenhada/corrompida{#rendition-returned-partially-drawn-or-corrupt}

+ __Erro__: A representação é renderizada incompletamente (quando uma imagem) ou está corrompida e não pode ser aberta.

   ![A representação é retornada parcialmente desenhada](./assets/troubleshooting/develop__await.png)

+ __Causa__: A  `renditionCallback` função do trabalhador está sendo encerrada antes que a representação possa ser completamente gravada no  `rendition.path`.
+ __Resolução__: Revise o código de trabalho personalizado e verifique se todas as chamadas assíncronas são sincronizadas usando o  `await`.

## Ferramenta de desenvolvimento{#development-tool}

### Arquivo Console.json ausente do projeto do Asset Compute{#missing-console-json}

+ __Erro:__ Erro: Arquivos necessários ausentes na validação (.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY) em async setupAssetCompute (.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY)
+ __Causa:__ O  `console.json` arquivo está ausente da raiz do projeto do Asset Compute
+ __Solução:__ Baixe um novo  `console.json` de seu projeto do Adobe I/O
   1. No console.adobe.io, abra o projeto do Adobe I/O em que o projeto do Asset Compute está configurado para usar
   1. Toque no botão __Download__ no canto superior direito
   1. Salve o arquivo baixado na raiz do projeto do Asset Compute usando o nome de arquivo `console.json`

### Recuo YAML incorreto em manifest.yml{#incorrect-yaml-indentation}

+ __Erro:__ YAMLException: indentação incorreta de uma entrada de mapeamento na linha X, coluna Y:(via padrão out from  `aio app run` command)
+ __Causa:__ Os arquivos Yaml são sensíveis ao espaço em branco, é provável que o recuo esteja incorreto.
+ __Solução:__ analise  `manifest.yml` e verifique se todo o recuo está correto.

### o limite memorySize está definido como muito baixo{#memorysize-limit-is-set-too-low}

+ __Erro:__  OpenWhiskError do Servidor de Desenvolvimento Local: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true HTTP 400 retornado (Solicitação incorreta) —> &quot;O conteúdo da solicitação foi malformado:requisito falhou: memória 64 MB abaixo do limite permitido de 134217728 B&quot;
+ __Causa:__ Um  `memorySize` limite para o trabalhador no  `manifest.yml` foi definido abaixo do limite mínimo permitido, conforme relatado pela mensagem de erro em bytes.
+ __Solução:__  analise os  `memorySize` limites na  `manifest.yml` e verifique se eles são todos maiores que o limite mínimo permitido.

### A Ferramenta de Desenvolvimento não pode ser iniciada devido à falta de private.key{#missing-private-key}

+ __Erro:__ Local Dev ServerError: Arquivos necessários ausentes em validatePrivateKeyFile.... (por padrão fora do comando `aio app run`)
+ __Causa:__ O  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valor no  `.env` arquivo não aponta para  `private.key` ou não  `private.key` é legível pelo usuário atual.
+ __Solução:__ analise o  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valor no  `.env` arquivo e verifique se ele contém o caminho completo e absoluto para o  `private.key` no seu sistema de arquivos.

### Lista suspensa de arquivos de origem incorreta{#source-files-dropdown-incorrect}

A Ferramenta de Desenvolvimento do Asset Compute pode inserir um estado em que extrai dados obsoletos, e é mais notável na lista suspensa __Source file__ exibindo itens incorretos.

+ __Erro:__ a lista suspensa do arquivo de origem exibe itens incorretos.
+ __Causa:__ o estado obsoleto do navegador em cache causa o
+ __Solução:__ em seu navegador, limpe completamente o &quot;estado do aplicativo&quot; da guia do navegador, o cache do navegador, o armazenamento local e o trabalhador do serviço.

### Parâmetro de consulta devToolToken ausente ou inválido{#missing-or-invalid-devtooltoken-query-parameter}

+ __Erro:__  notificação &quot;Não autorizado&quot; na Ferramenta de desenvolvimento Asset Compute
+ __Causa:__ `devToolToken` está ausente ou é inválido
+ __Solução:__ feche a janela do navegador da Ferramenta de desenvolvimento do Asset Compute, encerre todos os processos da Ferramenta de desenvolvimento em execução iniciados pelo  `aio app run` comando e reinicie a Ferramenta de desenvolvimento (usando  `aio app run`).

### Não é possível remover arquivos de origem{#unable-to-remove-source-files}

+ __Erro:__ Não há como remover arquivos de origem adicionados da interface do usuário das Ferramentas de Desenvolvimento
+ __Causa:__ esta funcionalidade não foi implementada
+ __Solução:__ faça logon no provedor de armazenamento em nuvem usando as credenciais definidas em  `.env`. Localize o contêiner usado pelas Ferramentas de desenvolvimento (também especificado em `.env`), navegue até a pasta __source__ e exclua quaisquer imagens de origem. Talvez seja necessário executar as etapas descritas na lista suspensa [Arquivos de origem incorretos](#source-files-dropdown-incorrect) se os arquivos de origem excluídos continuarem sendo exibidos na lista suspensa, pois podem ser armazenados em cache localmente no &quot;estado do aplicativo&quot; das Ferramentas de Desenvolvimento.

   ![Armazenamento de blobs do Microsoft Azure](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Testar{#test}

### Nenhuma representação gerada durante a execução do teste{#test-no-rendition-generated}

+ __Erro:__ Falha: Nenhuma representação gerada.
+ __Causa:__ o trabalhador não conseguiu gerar uma representação devido a um erro inesperado, como um erro de sintaxe JavaScript.
+ __Solução:__ revise a execução do teste  `test.log` em  `/build/test-results/test-worker/test.log`. Localize a seção neste arquivo correspondente ao caso de teste com falha e verifique se há erros.

   ![Solução de problemas - Nenhuma renderização gerada](./assets/troubleshooting/test__no-rendition-generated.png)

### O teste gera representação incorreta, causando falha no teste{#tests-generates-incorrect-rendition}

+ __Erro:__ Falha: A representação &#39;rendition.xxx&#39; não é como esperado.
+ __Causa:__ o trabalhador gera uma representação que não é a mesma  `rendition.<extension>` fornecida no caso de teste.
   + Se o arquivo esperado `rendition.<extension>` não for criado exatamente da mesma maneira que a representação gerada localmente no caso de teste, o teste poderá falhar, pois poderá haver alguma diferença nos bits. Por exemplo, se o trabalhador do Asset Compute alterar o contraste usando APIs e o resultado esperado for criado ajustando o contraste no Adobe Photoshop CC, os arquivos poderão parecer os mesmos, mas pequenas variações nos bits poderão ser diferentes.
+ __Solução:__ analise a saída da representação do teste navegando até  `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`e compare-a com o arquivo de representação esperado no caso de teste. Para criar um ativo esperado exato, faça o seguinte:
   + Use a ferramenta de desenvolvimento para gerar uma representação, validá-la e usá-la como o arquivo de representação esperado
   + Ou valide o arquivo gerado pelo teste em `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, valide-o e use-o como o arquivo de representação esperado

## Depurar

### O depurador não anexa{#debugger-does-not-attach}

+ __Erro__: Erro ao processar inicialização: Erro: Não foi possível conectar-se ao destino de depuração em...
+ __Causa__: O Docker Desktop não está sendo executado no sistema local. Verifique isso revisando o Console de Depuração do Código VS (Exibir > Console de Depuração), confirmando que esse erro é relatado.
+ __Resolução__: Inicie o  [Docker Desktop e confirme se as imagens Docker necessárias estão instaladas](./set-up/development-environment.md#docker).

### Pontos de interrupção que não estão pausando{#breakpoints-no-pausing}

+ __Erro__: Ao executar o trabalhador do Asset Compute na Ferramenta de desenvolvimento que pode ser depurada, o Código VS não é pausado nos pontos de interrupção.

#### Depurador de código VS não anexado{#vs-code-debugger-not-attached}

+ __Causa:__ o depurador do Código VS foi interrompido/desconectado.
+ __Solução:__ Reinicie o depurador de Código VS e verifique se ele é anexado assistindo ao console Saída de Depuração de Código VS (Exibir > Console de Depuração)

#### Depurador de código VS anexado após a execução do trabalhador iniciada{#vs-code-debugger-attached-after-worker-execution-began}

+ __Causa:__ o depurador de Código VS não foi anexado antes de tocar em  ____ Executar Ferramenta de Desenvolvimento.
+ __Solução:__ certifique-se de que o depurador foi anexado revisando o Console de Depuração do Código VS (Exibir > Console de Depuração) e, em seguida, execute novamente o trabalhador do Asset Compute na Ferramenta de Desenvolvimento.

### O tempo limite do trabalhador expira ao depurar{#worker-times-out-while-debugging}

+ __Erro__: O Console de Depuração relata &quot;A ação expirará em -XXX milissegundos&quot; ou a visualização da  [representação da Ferramenta de Desenvolvimento do ](./develop/development-tool.md) Asset Compute girará indefinidamente ou
+ __Causa__: O tempo limite do trabalhador, conforme definido no  [manifest.](./develop/manifest.md) ymlis, foi excedido durante a depuração.
+ __Resolução__: Aumente temporariamente o tempo limite do trabalhador no  [manifest.](./develop/manifest.md) ymlor para acelerar as atividades de depuração.

### Não é possível encerrar o processo do depurador{#cannot-terminate-debugger-process}

+ __Erro__:  `Ctrl-C` na linha de comando não encerra o processo do depurador (`npx adobe-asset-compute devtool`).
+ __Causa__: Um bug na  `@adobe/aio-cli-plugin-asset-compute` 1.3.x  `Ctrl-C` não é reconhecido como um comando de finalização.
+ __Resolução__: Atualizar  `@adobe/aio-cli-plugin-asset-compute` para a versão 1.4.1+

   ```
   $ aio update
   ```

   ![Solução de problemas - atualização do aio](./assets/troubleshooting/debug__terminate.png)

## Implantar{#deploy}

### Representação personalizada ausente do ativo no AEM{#custom-rendition-missing-from-asset}

+ __Erro:__ processo de ativos novos e reprocessados com êxito, mas a representação personalizada está ausente

#### Perfil de processamento não aplicado à pasta ancestral

+ __Causa:__ O ativo não existe em uma pasta com o Perfil de processamento que usa o trabalhador personalizado
+ __Solução:__ aplique o perfil de processamento a uma pasta ancestral do ativo

#### Perfil de processamento substituído por Perfil de processamento mais baixo

+ __Causa:__ O ativo existe abaixo de uma pasta com o Perfil de processamento de trabalho personalizado aplicado, no entanto, um Perfil de processamento diferente que não usa o trabalhador do cliente foi aplicado entre essa pasta e o ativo.
+ __Solução:__ combine ou reconcilie de outra forma os dois perfis de processamento e remova o perfil de processamento intermediário

### O processamento de ativos falha no AEM{#asset-processing-fails}

+ __Erro:__ Selo de falha de processamento de ativos exibido no ativo
+ __Causa:__ Ocorreu um erro na execução do trabalhador personalizado
+ __Solução:__ Siga as instruções em  [depurar a ](./test-debug/debug.md#aio-app-logs) ativação do Adobe I/O Runtime usando o  `aio app logs`.


