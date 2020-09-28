---
title: Configure um ambiente de desenvolvimento local para a extensibilidade da Computação de ativos
description: Desenvolvendo os trabalhadores da Asset Compute, que são aplicativos JavaScript Node.js, exigem ferramentas de desenvolvimento específicas que diferem do desenvolvimento tradicional de AEM, desde Node.js e vários módulos npm até o Docker Desktop e o Código do Microsoft Visual Studio.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
translation-type: tm+mt
source-git-commit: 53e4235c55d890765e9f13ffeb37a2c805fb307b
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---


# Configurar ambiente de desenvolvimento local

Os aplicativos Adobe Asset Compute não podem ser integrados com o tempo de execução local AEM fornecido pelo SDK AEM e são desenvolvidos usando sua própria cadeia de ferramentas, diferente da exigida pelos aplicativos AEM com base no arquétipo de projeto AEM Maven.

Para estender os microserviços da Asset Compute, as seguintes ferramentas devem ser instaladas na máquina de desenvolvedor local.

## Instruções de configuração avançadas

Veja a seguir as instruções para a configuração do abridge. Detalhes sobre essas ferramentas de desenvolvimento são descritos nas seções distintas abaixo.

1. [Instale o Docker Desktop](https://www.docker.com/products/docker-desktop) e puxe as imagens Docker necessárias:

   ```
   $ docker pull openwhisk/action-nodejs-v10:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Instalar código do Visual Studio](https://code.visualstudio.com/download)
1. [Instalar Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Instale os módulos npm necessários e os plug-ins CLI de E/S de Adobe da linha de comando:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

## Instalar código do Visual Studio{#vscode}

[O código](https://code.visualstudio.com/download) do Microsoft Visual Studio é usado para desenvolver e depurar aplicativos do Asset Compute. Embora outros IDE [compatíveis com](../../local-development-environment/development-tools.md#set-up-the-development-ide) JavaScript possam ser usados para desenvolver o aplicativo, somente o Código do Visual Studio pode ser integrado para [depurar](../test-debug/debug.md) aplicativos do Asset Compute.

_Visual Studio Code 1.48.x+ é necessário para que[wskdebug](#wskdebug)funcione._

Este tutorial assume o uso do código do Visual Studio, pois fornece a melhor experiência do desenvolvedor para estender o Asset Compute.

## Instalar o Docker Desktop{#docker}

Baixe e instale a versão mais recente e estável da [Docker Desktop](https://www.docker.com/products/docker-desktop), pois isso é necessário para [testar](../test-debug/test.md) e [depurar](../test-debug/debug.md) projetos da Asset Compute localmente.

Depois de instalar o Docker Desktop, start-o e instale as seguintes imagens do Docker na linha de comando:

```
$ docker pull openwhisk/action-nodejs-v10:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Os desenvolvedores em máquinas Windows devem certificar-se de que estão usando container Linux para as imagens acima. As etapas para alternar para container Linux estão descritas na documentação [do](https://docs.docker.com/docker-for-windows/)Docker for Windows.

## Instalar Node.js (e npm){#node-js}

Os funcionários da Asset Compute são aplicativos [Node.js](https://nodejs.org/) e, portanto, exigem que Node.js 10+ (e npm) seja desenvolvido e construído.

+ [Instale o Node.js (e npm)](../../local-development-environment/development-tools.md#node-js) da mesma maneira que para o desenvolvimento AEM tradicional.

## Instale a CLI de E/S do Adobe{#aio}

[Instale a CLI](../../local-development-environment/development-tools.md#aio-cli)de E/S do Adobe, ou o __aio__ é um módulo npm da linha de comando (CLI) que facilita o uso e a interação com as tecnologias de E/S do Adobe, e é usado tanto para gerar quanto para desenvolver localmente os funcionários personalizados da Asset Compute.

```
$ npm install -g @adobe/aio-cli
```

## Instale o plug-in Adobe I/O CLI Asset Compute{#aio-asset-compute}

O plug-in [Adobe I/O CLI Asset Compute](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Instalar wskdebug{#wskdebug}

Baixe e instale o módulo [Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm para facilitar a depuração local dos aplicativos Asset Compute.

_Visual Studio Code 1.48.x+ é necessário para que[wskdebug](#wskdebug)funcione._

```
$ npm install -g @openwhisk/wskdebug
```

## Instalar o ngrok{#ngrok}

Baixe e instale o módulo [ngrok](https://www.npmjs.com/package/ngrok) npm, que fornece acesso público à sua máquina de desenvolvimento local, para facilitar a depuração local dos aplicativos Asset Compute.

```
$ npm install -g ngrok --unsafe-perm=true
```
