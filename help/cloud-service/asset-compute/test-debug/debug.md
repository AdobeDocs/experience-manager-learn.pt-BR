---
title: Depurar um funcionário do Asset compute
description: Os funcionários do asset compute podem ser depurados de várias maneiras, desde simples declarações de log de depuração, ao código VS anexado como um depurador remoto, até registros de ativações no Adobe I/O Runtime iniciados de AEM como Cloud Service.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---


# Depurar um funcionário do Asset compute

Os funcionários do asset compute podem ser depurados de várias maneiras, desde simples declarações de log de depuração, ao código VS anexado como um depurador remoto, até registros de ativações no Adobe I/O Runtime iniciados de AEM como Cloud Service.

## Logs

A forma mais básica de depurar trabalhadores de Asset computes usa as declarações `console.log(..)` tradicionais no código de trabalho. O objeto JavaScript `console` é um objeto implícito e global, portanto não há necessidade de importá-lo ou exigi-lo, pois ele está sempre presente em todos os contextos.

Essas instruções de registro estão disponíveis para análise de forma diferente com base em como o trabalhador do Asset compute é executado:

+ De `aio app run`, os registros são impressos para saída padrão e os registros de Ativações [das Ferramentas de Desenvolvimento](../develop/development-tool.md)
   ![aplicativo aio execute console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ De `aio app test`, os registros imprimem para `/build/test-results/test-worker/test.log`
   ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ Usando `wskdebug`, as instruções de registro são impressas no Console de Depuração de Código VS (Visualização > Console de Depuração), padrão
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ Usando `aio app logs`, as declarações de log são impressas na saída do log de ativações

## Depuração remota por meio do depurador conectado

>[!WARNING]
>
>Use o Microsoft Visual Studio Code 1.48.0 ou superior para compatibilidade com wskdebug

O módulo [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm oferece suporte à anexação de um depurador aos trabalhadores do Asset compute, incluindo a capacidade de definir pontos de interrupção no Código VS e percorrer o código.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_Click-through da depuração de um Asset compute do trabalho usando wskdebug (Sem áudio)_

1. Verifique se os módulos [wskdebug](../set-up/development-environment.md#wskdebug) e [ngrok](../set-up/development-environment.md#ngork) npm estão instalados
1. Verifique se [O Docker Desktop e as imagens do Docker](../set-up/development-environment.md#docker) estão instalados e em execução
1. Feche todas as instâncias ativas em execução da ferramenta de desenvolvimento.
1. Implante o código mais recente usando `aio app deploy` e registre o nome da ação implantada (nome entre `[...]`). Isso será usado para atualizar o `launch.json` na etapa 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```
1. Start de uma nova instância da Ferramenta de Desenvolvimento de Asset computes usando o comando `npx adobe-asset-compute devtool`
1. No Código VS, toque no ícone Depurar no painel de navegação esquerdo
   + Se solicitado, toque em __criar um arquivo launch.json > Node.js__ para criar um novo arquivo `launch.json`.
   + Caso contrário, toque no ícone __Gear__ à direita da lista suspensa __Iniciar Programa__ para abrir o `launch.json` existente no editor.
1. Adicione a seguinte configuração de objeto JSON à matriz `configurations`:

   ```json
   {
       "type": "pwa-node",
       "request": "launch",
       "name": "wskdebug",
       "attachSimplePort": 0,
       "runtimeExecutable": "wskdebug",
       "args": [
           "wkndAemAssetCompute-0.0.1/__secured_worker",  // Version must match your Asset Compute worker's version
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
1. Toque no botão verde __Executar__ à esquerda da lista suspensa __wskdebug__
1. Abra `/actions/worker/index.js` e toque à esquerda dos números de linha para adicionar pontos de quebra 1. Navegue até a janela do navegador da Web da Ferramenta de Desenvolvimento de Asset computes aberta na etapa 6
1. Toque no botão __Executar__ para executar o trabalhador
1. Navegue de volta ao Código VS, até `/actions/worker/index.js` e percorra o código
1. Para sair da Ferramenta de Desenvolvimento Depurável, toque em `Ctrl-C` no terminal que executou o comando `npx adobe-asset-compute devtool` na etapa 6

## Acessar registros do Adobe I/O Runtime{#aio-app-logs}

[AEM como um Cloud Service potencializa os funcionários do Asset compute por meio dos ](../deploy/processing-profiles.md) Perfis de processamento, chamando-os diretamente no Adobe I/O Runtime. Como essas invocações não envolvem o desenvolvimento local, suas execuções não podem ser depuradas usando ferramentas locais, como a Ferramenta de desenvolvimento de Asset computes ou wskdebug. Em vez disso, a CLI do Adobe I/O pode ser usada para buscar registros do trabalhador executado em uma área de trabalho específica no Adobe I/O Runtime.

1. Certifique-se de que [variáveis de ambiente específicas do espaço de trabalho](../deploy/runtime.md) estejam definidas por `AIO_runtime_namespace` e `AIO_runtime_auth`, com base no espaço de trabalho que requer depuração.
1. Na linha de comando, execute `aio app logs`
   + Se o espaço de trabalho estiver com tráfego intenso, expanda o número de logs de ativação pelo sinalizador `--limit`:
      `$ aio app logs --limit=25`
1. Os registros mais recentes (até o `--limit`) do ativação serão retornados como saída do comando para revisão.

   ![registros de aplicativos do rádio](./assets/debug/aio-app-logs.png)

## Resolução de problemas

+ [O depurador não é anexado](../troubleshooting.md#debugger-does-not-attach)
+ [Pontos de interrupção não pausados](../troubleshooting.md#breakpoints-no-pausing)
+ [Depurador de código VS não anexado](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Depurador de código VS anexado após o início da execução do trabalhador](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [O tempo limite do trabalho é excedido durante a depuração](../troubleshooting.md#worker-times-out-while-debugging)
+ [Não é possível encerrar o processo do depurador](../troubleshooting.md#cannot-terminate-debugger-process)
