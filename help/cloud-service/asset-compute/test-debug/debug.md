---
title: Depurar um funcionário da Computação de ativos
description: Os funcionários da Asset Compute podem ser depurados de várias maneiras, desde declarações de log de depuração simples, ao código VS anexado como um depurador remoto, até registros de ativação no Adobe I/O Runtime iniciados de AEM como um Cloud Service.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---


# Depurar um funcionário da Computação de ativos

Os funcionários da Asset Compute podem ser depurados de várias maneiras, desde declarações de log de depuração simples, ao código VS anexado como um depurador remoto, até registros de ativação no Adobe I/O Runtime iniciados de AEM como um Cloud Service.

## Logs

A forma mais básica de depuração dos trabalhadores da Asset Compute usa `console.log(..)` declarações tradicionais no código de trabalho. O objeto `console` JavaScript é um objeto implícito e global, de modo que não há necessidade de importá-lo ou exigi-lo, como sempre está presente em todos os contextos.

Essas instruções de registro estão disponíveis para análise de forma diferente com base em como o trabalhador do Asset Compute é executado:

+ A partir `aio app run`, os registros são impressos para o padrão e os registros de Ativação da Ferramenta [de Desenvolvimento são](../develop/development-tool.md)
   ![aplicativo aio execute console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ De `aio app test`, os registros são impressos em `/build/test-results/test-worker/test.log`
   ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ Usando `wskdebug`, as instruções do log são impressas no Console de Depuração de Código VS (Visualização > Console de Depuração), padrão
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ Usando `aio app logs`, as declarações de log são impressas na saída do log de ativações

## Depuração remota por meio do depurador conectado

>[!WARNING]
>
>Use o Microsoft Visual Studio Code 1.48.0 ou superior para compatibilidade com wskdebug

O módulo [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm suporta anexar um depurador aos trabalhadores da Asset Compute, incluindo a capacidade de definir pontos de interrupção no Código VS e percorrer o código.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)
_Click-through de depuração de um trabalhador da Asset Compute usando wskdebug (Sem áudio)_

1. Verifique se os módulos [wskdebug](../set-up/development-environment.md#wskdebug) e [ngrok](../set-up/development-environment.md#ngork) npm estão instalados
1. Verifique se o [Docker Desktop e as imagens](../set-up/development-environment.md#docker) do Docker estão instalados e em execução
1. Feche todas as instâncias ativas em execução da ferramenta de desenvolvimento.
1. Implante o código mais recente usando `aio app deploy` e registrando o nome da ação implantada (nome entre os `[...]`). Isso será usado para atualizar o `launch.json` na etapa 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```
1. Start de uma nova instância da Ferramenta de desenvolvimento de computação de ativos usando o comando `npx adobe-asset-compute devtool`
1. No Código VS, toque no ícone Depurar no painel de navegação esquerdo
   + Se solicitado, toque em __criar um arquivo launch.json > Node.js__ para criar um novo `launch.json` arquivo.
   + Caso contrário, toque no ícone __Engrenagem__ à direita da lista suspensa __Iniciar Programa__ para abrir o existente `launch.json` no editor.
1. Adicione a seguinte configuração de objeto JSON à `configurations` matriz:

   ```json
   {
       "type": "pwa-node",
       "request": "launch",
       "name": "wskdebug",
       "attachSimplePort": 0,
       "runtimeExecutable": "wskdebug",
       "args": [
           "wkndAemAssetCompute-0.0.1/__secured_worker",  // Version must match your Asset Compute application's version
           "${workspaceFolder}/actions/worker/index.js",  // Points to your worker
           "-l",
           "--ngrok"
       ],
       "localRoot": "${workspaceFolder}",
       "remoteRoot": "/code",
       "outputCapture": "std",
       "timeout": 30000
   }
   ```

1. Selecione o novo __wskdebug__ na lista suspensa
1. Toque no botão verde __Executar__ à esquerda da __lista suspensa wskdebug__ .
1. Abra `/actions/worker/index.js` e toque à esquerda dos números de linha para adicionar pontos de quebra 1. Navegue até a janela do navegador Web da Ferramenta de Desenvolvimento de Computação de Ativo aberta na etapa 6
1. Toque no botão __Executar__ para executar o trabalhador
1. Volte para Código VS, para `/actions/worker/index.js` e navegue pelo código
1. Para sair da Ferramenta de Desenvolvimento Depurável, toque `Ctrl-C` no terminal que executou o `npx adobe-asset-compute devtool` comando na etapa 6

## Acessar registros do Adobe I/O Runtime{#aio-app-logs}

[AEM como Cloud Service utiliza os funcionários da Asset Compute por meio dos Perfis](../deploy/processing-profiles.md) de processamento, chamando-os diretamente no Adobe I/O Runtime. Como essas invocações não envolvem o desenvolvimento local, suas execuções não podem ser depuradas usando ferramentas locais, como a Ferramenta de desenvolvimento de computação de ativo ou wskdebug. Em vez disso, a CLI de E/S do Adobe pode ser usada para buscar registros do trabalhador executados em uma área de trabalho específica no Adobe I/O Runtime.

1. Verifique se as variáveis [de ambiente específicas do espaço de](../deploy/runtime.md) trabalho estão definidas via `AIO_runtime_namespace` e `AIO_runtime_auth`, com base no espaço de trabalho que requer depuração.
1. Na linha de comando, execute `aio app logs`
   + Se o espaço de trabalho estiver com tráfego intenso, expanda o número de registros de ativações por meio do `--limit` sinalizador:
      `$ aio app logs --limit=25`
1. Os registros mais recentes (até o fornecido `--limit`) do ativação serão retornados como saída do comando para revisão.

   ![registros de aplicativos do rádio](./assets/debug/aio-app-logs.png)

## Resolução de problemas

### O depurador não é anexado

+ __Erro__: Erro ao processar inicialização: Erro: Não foi possível conectar ao público alvo de depuração em...
+ __Causa__: O Docker Desktop não está sendo executado no sistema local. Verifique isso analisando o Console de Depuração de Código VS (Visualização > Console de Depuração), confirmando que esse erro é relatado.
+ __Resolução__: Desktop do Start [Docker e confirme se as imagens Docker necessárias estão instaladas](../set-up/development-environment.md#docker).

### Pontos de interrupção não pausados

+ __Erro__: Ao executar o trabalho do Asset Compute na Ferramenta de desenvolvimento depurável, o Código VS não pausará nos pontos de interrupção.

#### O depurador de código VS não está anexado

+ __Causa:__ O depurador de Código VS foi interrompido/desconectado.
+ __Resolução:__ Reinicie o depurador de Código VS e verifique se ele é anexado assistindo ao console de Saída de Depuração de Código VS (Visualização > Console de Depuração)

#### Depurador de código VS anexado após o início da execução do trabalhador

+ __Causa:__ O depurador de código VS não foi anexado antes de tocar em __Executar__ na ferramenta de desenvolvimento.
+ __Resolução:__ Certifique-se de que o depurador tenha sido anexado por meio da revisão do Console de depuração do código VS (Visualização > Console de depuração) e execute novamente o trabalhador Asset Compute na Ferramenta de desenvolvimento.

### O tempo limite do trabalho é excedido durante a depuração

+ __Erro__: O Console de depuração relata que &quot;a ação atingirá o tempo limite em -XXX milissegundos&quot; ou que a pré-visualização de execução da Ferramenta de Desenvolvimento de Computação de [Ativos](../develop/development-tool.md) gira indefinidamente ou
+ __Causa__: O tempo limite do trabalho definido no [manifest.yml](../develop/manifest.md) foi excedido durante a depuração.
+ __Resolução__: Aumente temporariamente o tempo limite do trabalhador no [manifest.yml](../develop/manifest.md) ou acelere a depuração de atividades.

### Não é possível encerrar o processo do depurador

+ __Erro__: `Ctrl-C` na linha de comando não encerra o processo do depurador (`npx adobe-asset-compute devtool`).
+ __Causa__: Um bug no `@adobe/aio-cli-plugin-asset-compute` 1.3.x resulta em `Ctrl-C` não ser reconhecido como um comando de terminação.
+ __Resolução__: Atualizar `@adobe/aio-cli-plugin-asset-compute` para a versão 1.4.1+

   ```
   $ aio update
   ```

   ![Solução de problemas - atualização do rádio](./assets/debug/troubleshooting__terminate.png)