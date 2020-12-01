---
title: Configurar um ambiente de desenvolvimento local para extensibilidade ao Asset compute
description: Os trabalhadores do Asset compute em desenvolvimento, que são aplicativos JavaScript Node.js, exigem ferramentas de desenvolvimento específicas que diferem do desenvolvimento tradicional de AEM, que variam de Node.js e vários módulos npm a Docker Desktop e o Código do Microsoft Visual Studio.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 0%

---


# Configurar ambiente de desenvolvimento local

Os projetos de Asset computes Adobe não podem ser integrados ao tempo de execução local AEM fornecido pelo SDK AEM e são desenvolvidos usando sua própria cadeia de ferramentas, separada da exigida por AEM aplicativos baseados no arquétipo de projeto AEM Maven.

Para estender os microserviços do Asset compute, as seguintes ferramentas devem ser instaladas no computador local do desenvolvedor.

## Instruções de configuração avançadas

Veja a seguir as instruções para a configuração do abridge. Detalhes sobre essas ferramentas de desenvolvimento são descritos nas seções distintas abaixo.

1. [Instale o Docker ](https://www.docker.com/products/docker-desktop) Desktop e extraia as imagens necessárias do Docker:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:latest
   ```

1. [Instalar código do Visual Studio](https://code.visualstudio.com/download)
1. [Instalar Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Instale os módulos npm necessários e os plug-ins Adobe I/O CLI da linha de comando:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Para obter mais informações sobre as instruções de instalação resumidas, leia as seções abaixo.

## Instalar código do Visual Studio{#vscode}

[O Microsoft Visual Studio ](https://code.visualstudio.com/download) Codeis é usado para desenvolver e depurar trabalhadores do Asset compute. Embora outros [IDE compatível com JavaScript](../../local-development-environment/development-tools.md#set-up-the-development-ide) possam ser usados para desenvolver o trabalhador, somente o Código do Visual Studio pode ser integrado ao trabalho do Asset compute [debug](../test-debug/debug.md).

_Visual Studio Code 1.48.x+ é necessário para  [](#wskdebug) wskdebugto trabalhar._

Este tutorial assume o uso do código do Visual Studio, pois fornece a melhor experiência do desenvolvedor para estender o Asset compute.

## Instalar o Docker Desktop{#docker}

Baixe e instale a área de trabalho mais recente e estável [Docker](https://www.docker.com/products/docker-desktop), pois isso é necessário para [testar](../test-debug/test.md) e [debug](../test-debug/debug.md) projetos de Asset computes localmente.

Depois de instalar o Docker Desktop, start-o e instale as seguintes imagens do Docker na linha de comando:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Os desenvolvedores em máquinas Windows devem certificar-se de que estão usando container Linux para as imagens acima. As etapas para alternar para container Linux estão descritas na [documentação do Docker for Windows](https://docs.docker.com/docker-for-windows/).

## Instalar o Node.js (e npm){#node-js}

Os funcionários do asset compute têm como base [Node.js](https://nodejs.org/) e, portanto, exigem que Node.js 10+ (e npm) seja desenvolvido e construído.

+ [Instale o Node.js (e npm) ](../../local-development-environment/development-tools.md#node-js) da mesma maneira que para o desenvolvimento AEM tradicional.

## Instalar o Adobe I/O CLI{#aio}

[Instale o Adobe I/O CLI](../../local-development-environment/development-tools.md#aio-cli) ou  ____ aiois um módulo de linha de comando (CLI) npm que facilite o uso e a interação com as tecnologias Adobe I/O e seja usado para gerar e desenvolver localmente Asset computes personalizados.

```
$ npm install -g @adobe/aio-cli
```

## Instale o plug-in do Asset compute Adobe I/O CLI{#aio-asset-compute}

O [plugin do Asset compute Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Instalar wskdebug{#wskdebug}

Baixe e instale o módulo [Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm para facilitar a depuração local dos trabalhadores do Asset compute.

_Visual Studio Code 1.48.x+ é necessário para  [](#wskdebug) wskdebugto trabalhar._

```
$ npm install -g @openwhisk/wskdebug
```

## Instalar o ngrok{#ngrok}

Baixe e instale o módulo [ngrok](https://www.npmjs.com/package/ngrok) npm, que fornece acesso público ao seu computador de desenvolvimento local, para facilitar a depuração local dos trabalhadores do Asset compute.

```
$ npm install -g ngrok --unsafe-perm=true
```
