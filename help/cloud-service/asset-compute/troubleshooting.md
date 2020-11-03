---
title: Solução de problemas de extensibilidade da Computação de ativos para AEM Assets
description: A seguir está um índice de erros e problemas comuns, juntamente com as resoluções, que podem ser encontrados ao desenvolver e implantar funcionários personalizados da Asset Compute para AEM Assets.
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


# Solução de problemas de extensibilidade da Computação de ativos

A seguir está um índice de erros e problemas comuns, juntamente com as resoluções, que podem ser encontrados ao desenvolver e implantar funcionários personalizados da Asset Compute para AEM Assets.

## Desenvolver{#develop}

### A execução é retornada parcialmente desenhada/corrompida{#rendition-returned-partially-drawn-or-corrupt}

+ __Erro__: A representação é renderizada incompletamente (quando uma imagem) ou está corrompida e não pode ser aberta.

   ![A representação é parcialmente desenhada](./assets/troubleshooting/develop__await.png)

+ __Causa__: A `renditionCallback` função do trabalhador está saindo antes que a representação possa ser completamente gravada `rendition.path`.
+ __Resolução__: Revise o código de trabalho personalizado e verifique se todas as chamadas assíncronas são sincronizadas usando `await`.

## Ferramenta de desenvolvimento{#development-tool}

### Recuo YAML incorreto no manifest.yml{#incorrect-yaml-indentation}

+ __Erro:__ YAMLException: recuo incorreto de uma entrada de mapeamento na linha X, coluna Y:(via padrão fora do `aio app run` comando)
+ __Causa:__ Os arquivos Yaml são sensíveis ao espaço em branco, provavelmente seu recuo está incorreto.
+ __Resolução:__ Revise seu `manifest.yml` recuo e verifique se ele está correto.

### o limite memorySize está definido como muito baixo{#memorysize-limit-is-set-too-low}

+ __Erro:__  OpenWhiskError do Servidor de Desenvolvimento Local: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true Retornou HTTP 400 (Solicitação incorreta) —> &quot;O conteúdo da solicitação foi malformado:falha no requisito: memória 64 MB abaixo do limite permitido de 134217728 B&quot;
+ __Causa:__ Um `memorySize` limite para o trabalhador no `manifest.yml` foi definido abaixo do limite mínimo permitido, conforme relatado pela mensagem de erro em bytes.
+ __Resolução:__  Revise os `memorySize` limites no `manifest.yml` e verifique se eles são todos maiores que o limite mínimo permitido.

### A Ferramenta de Desenvolvimento não pode start devido à falta de private.key{#missing-private-key}

+ __Erro:__ Erro do Servidor de Desenvolvimento Local: Faltando arquivos necessários em validatePrivateKeyFile.... (por padrão fora do `aio app run` comando)
+ __Causa:__ O `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valor no `.env` arquivo não aponta para `private.key` ou não `private.key` pode ser lido pelo usuário atual.
+ __Resolução:__ Revise o `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valor no `.env` arquivo e verifique se ele contém o caminho completo e absoluto para o `private.key` em seu sistema de arquivos.

### Menu suspenso de arquivos de origem incorreto{#source-files-dropdown-incorrect}

A Ferramenta de Desenvolvimento de Computação de Ativo pode inserir um estado em que extrai dados obsoletos e é mais visível na lista suspensa Arquivo ____ de origem exibindo itens incorretos.

+ __Erro:__ A lista suspensa do arquivo de origem exibe itens incorretos.
+ __Causa:__ O estado do navegador em cache obsoleto causa a
+ __Resolução:__ Em seu navegador, limpe completamente o &quot;estado do aplicativo&quot; da guia do navegador, o cache do navegador, o armazenamento local e o trabalhador do serviço.

### Parâmetro de query devToolToken ausente ou inválido{#missing-or-invalid-devtooltoken-query-parameter}

+ __Erro:__ Notificação &quot;Não autorizada&quot; na ferramenta de desenvolvimento de computação de ativos
+ __Causa:__ `devToolToken` está ausente ou inválido
+ __Resolução:__ Feche a janela do navegador Asset Compute Development Tool, encerre todos os processos em execução da Ferramenta de desenvolvimento iniciados por meio do `aio app run` comando e refaça o start Development Tool (usando `aio app run`).

### Não é possível remover arquivos de origem{#unable-to-remove-source-files}

+ __Erro:__ Não há como remover arquivos de origem adicionados da interface do usuário das Ferramentas de Desenvolvimento
+ __Causa:__ Esta funcionalidade não foi implementada
+ __Resolução:__ Faça logon no provedor de armazenamentos na nuvem usando as credenciais definidas em `.env`. Localize o container usado pelas Ferramentas de desenvolvimento (também especificadas em `.env`), navegue até a pasta __de origem__ e exclua as imagens de origem. Talvez seja necessário executar as etapas descritas na lista suspensa Arquivos [de origem incorretas](#source-files-dropdown-incorrect) se os arquivos de origem excluídos continuarem a ser exibidos na lista suspensa, pois podem ser armazenados em cache localmente no &quot;estado do aplicativo&quot; das Ferramentas de desenvolvimento.

   ![Armazenamento Blob do Microsoft Azure](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Testar{#test}

### Nenhuma execução gerada durante a execução do teste{#test-no-rendition-generated}

+ __Erro:__ Falha: Nenhuma execução gerada.
+ __Causa:__ O trabalhador não conseguiu gerar uma execução devido a um erro inesperado, como um erro de sintaxe JavaScript.
+ __Resolução:__ Verifique se a execução do teste está `test.log` em `/build/test-results/test-worker/test.log`. Localize a seção neste arquivo correspondente ao caso de teste com falha e verifique se há erros.

   ![Solução de problemas - nenhuma execução gerada](./assets/troubleshooting/test__no-rendition-generated.png)

### O teste gera representação incorreta, fazendo com que o teste falhe{#tests-generates-incorrect-rendition}

+ __Erro:__ Falha: A execução &#39;rendition.xxx&#39; não é a esperada.
+ __Causa:__ O trabalhador gera uma execução que não é a mesma que a `rendition.<extension>` fornecida no caso de teste.
   + Se o arquivo esperado não for criado exatamente da mesma maneira que a execução gerada localmente no caso de teste, o teste poderá falhar, pois pode haver alguma diferença nos bits. `rendition.<extension>` Por exemplo, se o funcionário da Computação de ativos alterar o contraste usando APIs e o resultado esperado for criado ao ajustar o contraste no Adobe Photoshop CC, os arquivos poderão parecer iguais, mas pequenas variações nos bits poderão ser diferentes.
+ __Resolução:__ Revise a saída de representação do teste navegando até `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`o arquivo de representação esperado no caso de teste e compare-o com ele. Para criar um ativo esperado exato:
   + Use a ferramenta de desenvolvimento para gerar uma representação, validar se está correta e usar esse arquivo como o arquivo de representação esperado
   + Ou valide o arquivo gerado pelo teste em `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, valide-o como correto e use-o como o arquivo de execução esperado

## Depurar


### O depurador não é anexado{#debugger-does-not-attach}

+ __Erro__: Erro ao processar inicialização: Erro: Não foi possível conectar ao público alvo de depuração em...
+ __Causa__: O Docker Desktop não está sendo executado no sistema local. Verifique isso analisando o Console de Depuração de Código VS (Visualização > Console de Depuração), confirmando que esse erro é relatado.
+ __Resolução__: Desktop do Start [Docker e confirme se as imagens Docker necessárias estão instaladas](./set-up/development-environment.md#docker).

### Pontos de interrupção não pausados{#breakpoints-no-pausing}

+ __Erro__: Ao executar o trabalho do Asset Compute na Ferramenta de desenvolvimento depurável, o Código VS não pausará nos pontos de interrupção.

#### Depurador de código VS não anexado{#vs-code-debugger-not-attached}

+ __Causa:__ O depurador de Código VS foi interrompido/desconectado.
+ __Resolução:__ Reinicie o depurador de Código VS e verifique se ele é anexado assistindo ao console de Saída de Depuração de Código VS (Visualização > Console de Depuração)

#### Depurador de código VS anexado após o início da execução do trabalhador{#vs-code-debugger-attached-after-worker-execution-began}

+ __Causa:__ O depurador de código VS não foi anexado antes de tocar em __Executar__ na ferramenta de desenvolvimento.
+ __Resolução:__ Certifique-se de que o depurador tenha sido anexado por meio da revisão do Console de depuração do código VS (Visualização > Console de depuração) e execute novamente o trabalhador Asset Compute na Ferramenta de desenvolvimento.

### O tempo limite do trabalho é excedido durante a depuração{#worker-times-out-while-debugging}

+ __Erro__: O Console de depuração relata que &quot;a ação atingirá o tempo limite em -XXX milissegundos&quot; ou que a pré-visualização de execução da Ferramenta de Desenvolvimento de Computação de [Ativos](./develop/development-tool.md) gira indefinidamente ou
+ __Causa__: O tempo limite do trabalho definido no [manifest.yml](./develop/manifest.md) foi excedido durante a depuração.
+ __Resolução__: Aumente temporariamente o tempo limite do trabalhador no [manifest.yml](./develop/manifest.md) ou acelere a depuração de atividades.

### Não é possível encerrar o processo do depurador{#cannot-terminate-debugger-process}

+ __Erro__: `Ctrl-C` na linha de comando não encerra o processo do depurador (`npx adobe-asset-compute devtool`).
+ __Causa__: Um bug no `@adobe/aio-cli-plugin-asset-compute` 1.3.x resulta em `Ctrl-C` não ser reconhecido como um comando de terminação.
+ __Resolução__: Atualizar `@adobe/aio-cli-plugin-asset-compute` para a versão 1.4.1+

   ```
   $ aio update
   ```

   ![Solução de problemas - atualização do rádio](./assets/troubleshooting/debug__terminate.png)

## Implantar{#deploy}

### Representação personalizada ausente do ativo no AEM{#custom-rendition-missing-from-asset}

+ __Erro:__ O processo de ativos novos e reprocessados foi bem-sucedido, mas a execução personalizada está ausente

#### Perfil de processamento não aplicado à pasta anterior

+ __Causa:__ O ativo não existe em uma pasta com o Perfil Processamento que usa o trabalhador personalizado
+ __Resolução:__ Aplicar o Perfil de processamento a uma pasta ancestral do ativo

#### Perfil de processamento substituído pelo Perfil de processamento mais baixo

+ __Causa:__ O ativo existe abaixo de uma pasta com o Perfil Processamento de trabalho personalizado aplicado, no entanto, um Perfil de processamento diferente que não usa o trabalhador do cliente foi aplicado entre essa pasta e o ativo.
+ __Resolução:__ Combine ou reconcilie os dois Perfis de processamento e remova o Perfil de processamento intermediário

### Falha no processamento de ativos em AEM{#asset-processing-fails}

+ __Erro:__ Processamento de ativos Falha ao exibir o selo no ativo
+ __Causa:__ Ocorreu um erro na execução do trabalhador personalizado
+ __Resolução:__ Siga as instruções em [depurar o Adobe I/O Runtime ativação](./test-debug/debug.md#aio-app-logs) usando `aio app logs`.


