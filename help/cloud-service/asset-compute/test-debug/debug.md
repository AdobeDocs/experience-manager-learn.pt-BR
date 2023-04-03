---
title: Depurar um trabalhador do Asset compute
description: Os trabalhadores do Asset compute podem ser depurados de várias maneiras, desde declarações de log de depuração simples, ao código VS anexado como um depurador remoto, até registros de ativação no Adobe I/O Runtime iniciados AEM as a Cloud Service.
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

Os trabalhadores do Asset compute podem ser depurados de várias maneiras, desde declarações de log de depuração simples, ao código VS anexado como um depurador remoto, até registros de ativação no Adobe I/O Runtime iniciados AEM as a Cloud Service.

## Logs

A forma mais básica de depurar os trabalhadores do Asset compute usa o modo tradicional `console.log(..)` no código do trabalhador. O `console` O objeto JavaScript é um objeto implícito e global, de modo que não é necessário importá-lo ou exigi-lo, pois está sempre presente em todos os contextos.

Essas instruções de log estão disponíveis para revisão de forma diferente com base em como o trabalhador do Asset compute é executado:

+ De `aio app run`, os logs são impressos para o padrão e a variável [Ferramentas de desenvolvimento](../develop/development-tool.md) Logs de ativação
   ![aplicativo aio executar console.log(..)](./assets/debug/console-log__aio-app-run.png)
+ De `aio app test`, logs imprimem para `/build/test-results/test-worker/test.log`
   ![aio app test console.log(..)](./assets/debug/console-log__aio-app-test.png)
+ Usando `wskdebug`, registra instruções impressas no Console de Depuração do Código VS (Exibir > Console de Depuração), padrão
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ Usando `aio app logs`, instruções de log são impressas na saída do log de ativação

## Depuração remota via depurador anexado

>[!WARNING]
>
>Use o Microsoft Visual Studio Code 1.48.0 ou superior para compatibilidade com wskdebug

O [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) O módulo npm suporta a anexação de um depurador aos trabalhadores do Asset compute, incluindo a capacidade de definir pontos de interrupção no Código VS e percorrer o código.

>[!VIDEO](https://video.tv.adobe.com/v/40383?quality=12&learn=on)

_Click-through da depuração de um trabalhador do Asset compute usando wskdebug (Sem áudio)_

1. Garantir [wskdebug](../set-up/development-environment.md#wskdebug) e [ngrok](../set-up/development-environment.md#ngork) módulos npm estão instalados
1. Garantir [Docker Desktop e as imagens Docker de suporte](../set-up/development-environment.md#docker) estão instalados e em execução
1. Feche todas as instâncias em execução ativas da Ferramenta de desenvolvimento.
1. Implante o código mais recente usando `aio app deploy`  e registre o nome da ação implantada (nome entre `[...]`). Isso é usado para atualizar a variável `launch.json` na etapa 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. Inicie uma nova instância da Ferramenta de desenvolvimento de Assets compute usando o comando `npx adobe-asset-compute devtool`
1. No Código VS, toque no ícone Depurar na navegação à esquerda
   + Se solicitado, toque em __criar um arquivo launch.json > Node.js__ para criar um novo `launch.json` arquivo.
   + Caso contrário, toque no botão __Engrenagem__ à direita do __Programa Launch__ lista suspensa para abrir o `launch.json` no editor.
1. Adicione a seguinte configuração de objeto JSON ao `configurations` array:

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
1. Toque em verde __Executar__ à esquerda de __wskdebug__ lista suspensa
1. Abrir `/actions/worker/index.js` e toque à esquerda dos números de linha para adicionar pontos de quebra 1. Navegue até a janela do navegador Web da Ferramenta de Desenvolvimento de Asset compute aberta na etapa 6
1. Toque no __Executar__ botão para executar o trabalhador
1. Navegue de volta para o Código VS, para `/actions/worker/index.js` e percorra o código
1. Para sair da Ferramenta de desenvolvimento que pode ser depurada, toque em `Ctrl-C` no terminal executado `npx adobe-asset-compute devtool` comando na etapa 6

## Acesso aos logs do Adobe I/O Runtime{#aio-app-logs}

[AEM as a Cloud Service aproveita os trabalhadores do Asset compute por meio de Perfis de processamento](../deploy/processing-profiles.md) chamando-os diretamente no Adobe I/O Runtime. Como essas invocações não envolvem o desenvolvimento local, suas execuções não podem ser depuradas usando ferramentas locais, como a Ferramenta de desenvolvimento de Assets compute ou a wskdebug. Em vez disso, a CLI do Adobe I/O pode ser usada para buscar logs do trabalhador executado em um espaço de trabalho específico no Adobe I/O Runtime.

1. Verifique se a [variáveis de ambiente específicas do workspace](../deploy/runtime.md) são definidos via `AIO_runtime_namespace` e `AIO_runtime_auth`, com base no espaço de trabalho que requer depuração.
1. Na linha de comando, execute `aio app logs`
   + Se o espaço de trabalho estiver com tráfego intenso, expanda o número de logs de ativação por meio do `--limit` sinalizador:
      `$ aio app logs --limit=25`
1. O mais recente (até o `--limit`) os logs de ativações são retornados como a saída do comando para revisão.

   ![logs do aplicativo aio](./assets/debug/aio-app-logs.png)

## Resolução de problemas

+ [O Debugger não é anexado](../troubleshooting.md#debugger-does-not-attach)
+ [Pontos de interrupção não pausados](../troubleshooting.md#breakpoints-no-pausing)
+ [Depurador de código VS não anexado](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Depurador de código VS anexado após o início da execução do trabalhador](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [O tempo limite do trabalho é excedido durante a depuração](../troubleshooting.md#worker-times-out-while-debugging)
+ [Não é possível encerrar o processo do depurador](../troubleshooting.md#cannot-terminate-debugger-process)
