---
title: Solução de problemas de extensibilidade do Asset compute para AEM Assets
description: A seguir está um índice de erros e problemas comuns, juntamente com as resoluções, que podem ser encontrados ao desenvolver e implantar Asset computes personalizados para AEM Assets.
feature: asset-compute
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1146'
ht-degree: 0%

---


# Solução de problemas de extensibilidade do Asset compute

A seguir está um índice de erros e problemas comuns, juntamente com as resoluções, que podem ser encontrados ao desenvolver e implantar Asset computes personalizados para AEM Assets.

## Desenvolver{#develop}

### A representação é devolvida parcialmente desenhada/corrompida{#rendition-returned-partially-drawn-or-corrupt}

+ __Erro__: A representação é renderizada incompletamente (quando uma imagem) ou está corrompida e não pode ser aberta.

   ![A representação é parcialmente desenhada](./assets/troubleshooting/develop__await.png)

+ __Causa__: A  `renditionCallback` função do trabalhador está saindo antes que a execução possa ser completamente gravada  `rendition.path`.
+ __Resolução__: Revise o código de trabalho personalizado e verifique se todas as chamadas assíncronas são sincronizadas usando  `await`.

## Ferramenta de desenvolvimento{#development-tool}

### Recuo YAML incorreto no manifest.yml{#incorrect-yaml-indentation}

+ __Erro:__ YAMLException: recuo incorreto de uma entrada de mapeamento na linha X, coluna Y:(via padrão fora do  `aio app run` comando)
+ __Causa: os arquivos__ Yaml são sensíveis ao espaço em branco, provavelmente seu recuo está incorreto.
+ __Resolução:__ analise seu  `manifest.yml` e verifique se todo o recuo está correto.

### o limite memorySize está definido como muito baixo{#memorysize-limit-is-set-too-low}

+ __Erro:OpenWhiskError do Servidor de Desenvolvimento__  Local: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true Retornou HTTP 400 (Solicitação incorreta) —> &quot;O conteúdo da solicitação foi malformado:falha no requisito: memória 64 MB abaixo do limite permitido de 134217728 B&quot;
+ __Causa:__ um  `memorySize` limite para o trabalhador no  `manifest.yml` foi definido abaixo do limite mínimo permitido, conforme relatado pela mensagem de erro em bytes.
+ __Resolução:__  analise os  `memorySize` limites no  `manifest.yml` e verifique se eles são todos maiores que o limite mínimo permitido.

### A Ferramenta de Desenvolvimento não pode start devido à falta de private.key{#missing-private-key}

+ __Erro:__ Local Dev ServerErro: Faltando arquivos necessários em validatePrivateKeyFile.... (por padrão fora do comando `aio app run`)
+ __Causa:__ o  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valor no  `.env` arquivo não aponta para  `private.key` ou não  `private.key` é legível pelo usuário atual.
+ __Resolução:__ analise o  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valor no  `.env` arquivo e verifique se ele contém o caminho completo e absoluto para o  `private.key` em seu sistema de arquivos.

### Lista suspensa de arquivos de origem incorreta{#source-files-dropdown-incorrect}

A Ferramenta de Desenvolvimento de asset computes pode inserir um estado onde extrai dados obsoletos e é mais visível na lista suspensa __Arquivo de origem__ exibindo itens incorretos.

+ __Erro: a lista suspensa Arquivo__ de origem exibe itens incorretos.
+ __Causa: o estado do navegador em cache__ obsoleto causa a
+ __Resolução:__ no seu navegador, limpe completamente o &quot;estado do aplicativo&quot; da guia do navegador, o cache do navegador, o armazenamento local e o trabalhador do serviço.

### Parâmetro de query devToolToken ausente ou inválido{#missing-or-invalid-devtooltoken-query-parameter}

+ __Erro:__  notificação &quot;Não autorizado&quot; na ferramenta de desenvolvimento de Asset computes
+ __Causa:__ `devToolToken` está ausente ou é inválido
+ __Resolução:__ feche a janela do navegador da Ferramenta de Desenvolvimento de Asset computes, encerre todos os processos em execução da Ferramenta de Desenvolvimento iniciados por meio do  `aio app run` comando e refaça a Ferramenta de Desenvolvimento de start (usando  `aio app run`).

### Não é possível remover arquivos de origem{#unable-to-remove-source-files}

+ __Erro: não__ há como remover arquivos de origem adicionados da interface do usuário das Ferramentas de Desenvolvimento
+ __Causa:__ essa funcionalidade não foi implementada
+ __Resolução:__ faça logon no provedor de armazenamentos na nuvem usando as credenciais definidas em  `.env`. Localize o container usado pelas Ferramentas de Desenvolvimento (também especificadas em `.env`), navegue até a pasta __source__ e exclua quaisquer imagens de origem. Talvez seja necessário executar as etapas descritas na lista suspensa [Arquivos de origem incorretos](#source-files-dropdown-incorrect) se os arquivos de origem excluídos continuarem a ser exibidos na lista suspensa, pois podem ser armazenados em cache localmente no &quot;estado do aplicativo&quot; das Ferramentas de Desenvolvimento.

   ![Armazenamento Blob do Microsoft Azure](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Testar{#test}

### Nenhuma execução gerada durante a execução do teste{#test-no-rendition-generated}

+ __Erro:__ Falha: Nenhuma execução gerada.
+ __Causa:__ o trabalhador não conseguiu gerar uma execução devido a um erro inesperado, como um erro de sintaxe JavaScript.
+ __Resolução:__ Revise a execução do teste  `test.log` em  `/build/test-results/test-worker/test.log`. Localize a seção neste arquivo correspondente ao caso de teste com falha e verifique se há erros.

   ![Solução de problemas - nenhuma execução gerada](./assets/troubleshooting/test__no-rendition-generated.png)

### O teste gera renderização incorreta, fazendo com que o teste falhe{#tests-generates-incorrect-rendition}

+ __Erro:__ Falha: A execução &#39;rendition.xxx&#39; não é a esperada.
+ __Causa:__ o trabalhador gera uma execução que não é a mesma que a  `rendition.<extension>` fornecida no caso de teste.
   + Se o arquivo esperado `rendition.<extension>` não for criado exatamente da mesma maneira que a renderização gerada localmente no caso de teste, o teste poderá falhar, pois pode haver alguma diferença nos bits. Por exemplo, se o trabalhador do Asset compute alterar o contraste usando APIs e o resultado esperado for criado ao ajustar o contraste no Adobe Photoshop CC, os arquivos poderão parecer iguais, mas pequenas variações nos bits poderão ser diferentes.
+ __Resolução:__ analise a saída de execução do teste navegando até  `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`e compare-a com o arquivo de execução esperado no caso de teste. Para criar um ativo esperado exato:
   + Use a ferramenta de desenvolvimento para gerar uma representação, validar se está correta e usar esse arquivo como o arquivo de representação esperado
   + Ou valide o arquivo gerado pelo teste em `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, valide-o como correto e use-o como o arquivo de execução esperado

## Depurar


### O depurador não anexa{#debugger-does-not-attach}

+ __Erro__: Erro ao processar inicialização: Erro: Não foi possível conectar ao público alvo de depuração em...
+ __Causa__: O Docker Desktop não está sendo executado no sistema local. Verifique isso analisando o Console de Depuração de Código VS (Visualização > Console de Depuração), confirmando que esse erro é relatado.
+ __Resolução__: Desktop do Start  [Docker e confirme se as imagens Docker necessárias estão instaladas](./set-up/development-environment.md#docker).

### Pontos de interrupção que não pausam{#breakpoints-no-pausing}

+ __Erro__: Ao executar o Asset compute da Ferramenta de desenvolvimento depurável, o Código VS não pausará nos pontos de interrupção.

#### Depurador de código VS não anexado{#vs-code-debugger-not-attached}

+ __Causa:__ o depurador do Código VS foi interrompido/desconectado.
+ __Resolução:__ Reinicie o depurador de Código VS e verifique se ele é anexado assistindo ao console de Saída de Depuração de Código VS (Visualização > Console de Depuração)

#### Depurador de código VS anexado após a execução do trabalhador começar{#vs-code-debugger-attached-after-worker-execution-began}

+ __Causa:__ o depurador de Código VS não foi anexado antes de tocar em  ____ Executar a ferramenta de desenvolvimento.
+ __Resolução:__ verifique se o depurador foi anexado revisando o Console de depuração do código VS (Visualização > Console de depuração) e execute novamente o funcionário do Asset compute na Ferramenta de desenvolvimento.

### O tempo limite do trabalho é excedido durante a depuração{#worker-times-out-while-debugging}

+ __Erro__: O Console de depuração relata que &quot;a ação atingirá o tempo limite em -XXX milissegundos&quot; ou que a pré-visualização de execução da Ferramenta de Desenvolvimento de  [Asset computes ](./develop/development-tool.md) gira indefinidamente ou
+ __Causa__: O tempo limite do trabalho definido no  [manifest.](./develop/manifest.md) ymlis foi excedido durante a depuração.
+ __Resolução__: Aumente temporariamente o tempo limite do trabalhador no  [manifest.](./develop/manifest.md) ymlor para acelerar as atividades de depuração.

### Não é possível encerrar o processo do depurador{#cannot-terminate-debugger-process}

+ __Erro__:  `Ctrl-C` na linha de comando não encerra o processo do depurador (`npx adobe-asset-compute devtool`).
+ __Causa__: Um bug no  `@adobe/aio-cli-plugin-asset-compute` 1.3.x  `Ctrl-C` não é reconhecido como um comando de encerramento.
+ __Resolução__: Atualizar  `@adobe/aio-cli-plugin-asset-compute` para a versão 1.4.1+

   ```
   $ aio update
   ```

   ![Solução de problemas - atualização do rádio](./assets/troubleshooting/debug__terminate.png)

## Implantar{#deploy}

### Representação personalizada ausente do ativo em AEM{#custom-rendition-missing-from-asset}

+ __Erro: o processo de ativos__ novos e reprocessados foi bem-sucedido, mas a execução personalizada está ausente

#### Perfil de processamento não aplicado à pasta anterior

+ __Causa:__ o ativo não existe em uma pasta com o Perfil Processamento que usa o trabalhador personalizado
+ __Resolução:__ aplique o Perfil de processamento a uma pasta ancestral do ativo

#### Perfil de processamento substituído pelo Perfil de processamento mais baixo

+ __Causa:__ o ativo existe abaixo de uma pasta com o Perfil Processamento de trabalho personalizado aplicado, no entanto, um Perfil de processamento diferente que não usa o trabalhador do cliente foi aplicado entre essa pasta e o ativo.
+ __Resolução:__ Combine ou reconcilie os dois Perfis de processamento e remova o Perfil de processamento intermediário

### Falha no processamento de ativos em AEM{#asset-processing-fails}

+ __Erro: falha no processamento__ do ativo exibida no selo do ativo
+ __Causa:__ Ocorreu um erro na execução do trabalhador personalizado
+ __Resolução:__ Siga as instruções em  [depurar a ](./test-debug/debug.md#aio-app-logs) ativação do Adobe I/O Runtime  `aio app logs`.


