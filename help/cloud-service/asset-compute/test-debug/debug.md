---
title: Depurar um trabalhador do Asset compute
description: Os trabalhadores do Asset compute podem ser depurados de várias maneiras, desde declarações de log de depuração simples, ao Código VS anexado como um depurador remoto, até logs de ativação no Adobe I/O Runtime iniciados a partir do AEM as a Cloud Service.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---

# Depurar um trabalhador do Asset compute

Os trabalhadores do Asset compute podem ser depurados de várias maneiras, desde declarações de log de depuração simples, ao Código VS anexado como um depurador remoto, até logs de ativação no Adobe I/O Runtime iniciados a partir do AEM as a Cloud Service.

## Logs

A forma mais básica de depuração de trabalhadores de Asset compute usa o modelo `console.log(..)` demonstrativos no código do trabalhador. A variável `console` O objeto JavaScript é um objeto global implícito, portanto, não há necessidade de importá-lo ou solicitá-lo, pois está sempre presente em todos os contextos.

Estas instruções de log estão disponíveis para revisão de forma diferente com base em como o worker do Asset compute é executado:

+ De `aio app run`, registra a impressão em uma saída padrão e o [Ferramentas de desenvolvimento](../develop/development-tool.md) Logs de ativação
   ![aplicativo aio executar console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ De `aio app test`, registros imprimir em `/build/test-results/test-worker/test.log`
   ![aplicativo aio test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ Usar `wskdebug`, as instruções de registro são impressas no Console de depuração do código VS (Exibir > Console de depuração), padrão
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ Usar `aio app logs`, instruções de log são impressas na saída do log de ativação

## Depuração remota via depurador anexado

>[!WARNING]
>
>Use o Microsoft Visual Studio Code 1.48.0 ou posterior para compatibilidade com wskdebug

A variável [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) O módulo npm suporta a anexação de um depurador a trabalhadores do Asset compute, incluindo a capacidade de definir pontos de interrupção no Código VS e percorrer o código.

>[!VIDEO](https://video.tv.adobe.com/v/40383?quality=12&learn=on)

_Click-through de depuração de um funcionário do Asset compute usando wskdebug (sem áudio)_

1. Assegurar [wskdebug](../set-up/development-environment.md#wskdebug) e [grok](../set-up/development-environment.md#ngork) módulos npm instalados
1. Assegurar [Desktop Docker e as imagens Docker de suporte](../set-up/development-environment.md#docker) estão instalados e em execução
1. Feche todas as instâncias ativas em execução da Ferramenta de desenvolvimento.
1. Implante o código mais recente usando o `aio app deploy`  e registre o nome da ação implantada (nome entre a variável `[...]`). Isso é usado para atualizar o `launch.json` na etapa 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. Inicie uma nova instância da Ferramenta de desenvolvimento do Asset compute usando o comando `npx adobe-asset-compute devtool`
1. No Código VS, toque no ícone Depurar na navegação à esquerda
   + Se solicitado, toque em __criar um arquivo launch.json > Node.js__ para criar um novo `launch.json` arquivo.
   + Senão, toque no __Engrenagem__ ícone à direita do __Iniciar programa__ lista suspensa para abrir o existente `launch.json` no editor.
1. Adicione a seguinte configuração de objeto JSON à `configurations` matriz:

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
1. Toque no verde __Executar__ botão à esquerda de __wskdebug__ lista suspensa
1. Abertura `/actions/worker/index.js` e toque à esquerda dos números de linha para adicionar pontos de interrupção 1. Navegue até a janela do navegador da Web da Ferramenta de desenvolvimento do Asset compute aberta na etapa 6
1. Toque no __Executar__ botão para executar o trabalhador
1. Volte para o Código VS, para `/actions/worker/index.js` e passe pelo código
1. Para sair da Ferramenta de desenvolvimento depurável, toque em `Ctrl-C` no terminal que executou `npx adobe-asset-compute devtool` comando na etapa 6

## Acessar logs do Adobe I/O Runtime{#aio-app-logs}

[O AEM as a Cloud Service aproveita os trabalhadores do Asset compute por meio de perfis de processamento](../deploy/processing-profiles.md) chamando-os diretamente no Adobe I/O Runtime. Como essas invocações não envolvem desenvolvimento local, suas execuções não podem ser depuradas usando ferramentas locais, como Asset compute Development Tool ou wskdebug. Em vez disso, a CLI do Adobe I/O pode ser usada para buscar registros do operador executado em um espaço de trabalho específico no Adobe I/O Runtime.

1. Assegure a [variáveis de ambiente específicas do espaço de trabalho](../deploy/runtime.md) são definidos via `AIO_runtime_namespace` e `AIO_runtime_auth`, com base no espaço de trabalho que requer depuração.
1. Na linha de comando, execute `aio app logs`
   + Se o espaço de trabalho estiver com tráfego intenso, expanda o número de logs de ativação por meio da `--limit` sinalizador:
      `$ aio app logs --limit=25`
1. O mais recente (até o fornecido `--limit`) os logs de ativação são retornados como a saída do comando para revisão.

   ![logs do aplicativo aio](./assets/debug/aio-app-logs.png)

## Resolução de problemas

+ [O depurador não anexa](../troubleshooting.md#debugger-does-not-attach)
+ [Os pontos de interrupção não estão pausando](../troubleshooting.md#breakpoints-no-pausing)
+ [Depurador de código VS não anexado](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Depurador de código VS anexado após o início da execução do trabalho](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [O trabalhador atinge o tempo limite durante a depuração](../troubleshooting.md#worker-times-out-while-debugging)
+ [Não é possível encerrar o processo do depurador](../troubleshooting.md#cannot-terminate-debugger-process)
