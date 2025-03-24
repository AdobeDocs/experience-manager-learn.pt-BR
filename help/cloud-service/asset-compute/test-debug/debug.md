---
title: Depurar um trabalhador do Asset Compute
description: Os trabalhadores do Asset Compute podem ser depurados de várias maneiras, desde declarações de log de depuração simples, ao código VS anexado como um depurador remoto, até logs de ativação no Adobe I/O Runtime iniciados pelo AEM as a Cloud Service.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
duration: 229
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---

# Depurar um trabalhador do Asset Compute

Os trabalhadores do Asset Compute podem ser depurados de várias maneiras, desde declarações de log de depuração simples, ao código VS anexado como um depurador remoto, até logs de ativação no Adobe I/O Runtime iniciados pelo AEM as a Cloud Service.

## Logs

A forma mais básica de depuração de trabalhadores do Asset Compute usa instruções `console.log(..)` tradicionais no código do trabalhador. O objeto JavaScript `console` é um objeto global implícito, portanto não há necessidade de importá-lo ou solicitá-lo, pois ele está sempre presente em todos os contextos.

Estas instruções de log estão disponíveis para revisão de forma diferente com base em como o worker do Asset Compute é executado:

+ De `aio app run`, os logs são impressos no padrão e os ](../develop/development-tool.md) Logs de Ativação da [Ferramenta de Desenvolvimento
  ![aio app run console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ De `aio app test`, logs são impressos em `/build/test-results/test-worker/test.log`
  ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ Usando o `wskdebug`, as instruções de logs são impressas no Console de Depuração do Código VS (Exibir > Console de Depuração), padrão
  ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ Usando `aio app logs`, as instruções de log são impressas na saída do log de ativação

## Depuração remota via depurador anexado

>[!WARNING]
>
>Use o Microsoft Visual Studio Code 1.48.0 ou posterior para compatibilidade com wskdebug

O módulo [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm oferece suporte à anexação de um depurador aos trabalhadores do Asset Compute, incluindo a capacidade de definir pontos de interrupção no Código VS e percorrer o código.

>[!VIDEO](https://video.tv.adobe.com/v/40383?quality=12&learn=on)

_Click-through de depuração de um trabalhador do Asset Compute usando wskdebug (Sem áudio)_

1. Verifique se os módulos [wskdebug](../set-up/development-environment.md#wskdebug) e [ngrok](../set-up/development-environment.md#ngork) npm estão instalados
1. Verifique se o [Docker Desktop e as imagens do Docker de suporte](../set-up/development-environment.md#docker) estão instalados e em execução
1. Feche todas as instâncias ativas em execução da Ferramenta de desenvolvimento.
1. Implante o código mais recente usando `aio app deploy` e registre o nome da ação implantada (nome entre o `[...]`). Isto é usado para atualizar o `launch.json` na etapa 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. Inicie uma nova instância da Ferramenta de Desenvolvimento do Asset Compute usando o comando `npx adobe-asset-compute devtool`
1. No Código VS, toque no ícone Depurar na navegação à esquerda
   + Se solicitado, toque em __criar um arquivo launch.json > Node.js__ para criar um novo arquivo `launch.json`.
   + Senão, toque no ícone de __engrenagem__ à direita da lista suspensa __Iniciar programa__ para abrir o `launch.json` existente no editor.
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
1. Abra `/actions/worker/index.js` e toque à esquerda dos números de linha para adicionar pontos de interrupção 1. Navegue até a janela do navegador da Web da Ferramenta de desenvolvimento do Asset Compute aberta na etapa 6
1. Toque no botão __Executar__ para executar o trabalhador
1. Navegue de volta para o Código VS, para `/actions/worker/index.js` e passe pelo código
1. Para sair da Ferramenta de Desenvolvimento depurável, toque em `Ctrl-C` no terminal que executou o comando `npx adobe-asset-compute devtool` na etapa 6

## Acessar logs do Adobe I/O Runtime{#aio-app-logs}

[O AEM as a Cloud Service aproveita os trabalhadores do Asset Compute por meio do Processamento de Perfis](../deploy/processing-profiles.md), invocando-os diretamente no Adobe I/O Runtime. Como essas invocações não envolvem desenvolvimento local, suas execuções não podem ser depuradas usando ferramentas locais, como a Ferramenta de desenvolvimento do Asset Compute ou wskdebug. Em vez disso, a CLI do Adobe I/O pode ser usada para buscar registros do trabalhador executado em um espaço de trabalho específico no Adobe I/O Runtime.

1. Verifique se as [variáveis de ambiente específicas do espaço de trabalho](../deploy/runtime.md) estão definidas via `AIO_runtime_namespace` e `AIO_runtime_auth`, com base no espaço de trabalho que requer depuração.
1. Na linha de comando, execute `aio app logs`
   + Se o espaço de trabalho estiver com tráfego intenso, expanda o número de logs de ativação por meio do sinalizador `--limit`:
     `$ aio app logs --limit=25`
1. Os logs de ativações mais recentes (até o `--limit` fornecido) são retornados como a saída do comando para revisão.

   ![logs do aplicativo aio](./assets/debug/aio-app-logs.png)

## Resolução de problemas

+ [O depurador não anexa](../troubleshooting.md#debugger-does-not-attach)
+ [Os pontos de interrupção não estão pausando](../troubleshooting.md#breakpoints-no-pausing)
+ [Depurador de código VS não anexado](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Depurador de código VS anexado após o início da execução do trabalho](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [O trabalhador atinge o tempo limite durante a depuração](../troubleshooting.md#worker-times-out-while-debugging)
+ [Não é possível encerrar o processo do depurador](../troubleshooting.md#cannot-terminate-debugger-process)
