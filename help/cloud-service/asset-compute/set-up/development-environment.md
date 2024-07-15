---
title: Configurar um ambiente de desenvolvimento local para extensibilidade do Asset Compute
description: O desenvolvimento de trabalhadores do Asset Compute, que são aplicativos Node.js do JavaScript, exigem ferramentas de desenvolvimento específicas que diferem do desenvolvimento do AEM tradicional, que variam de Node.js e vários módulos npm a Docker Desktop e Microsoft Visual Studio Code.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
duration: 96
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---

# Configurar ambiente de desenvolvimento local

Os projetos do Adobe Asset Compute não podem ser integrados ao tempo de execução do AEM local fornecido pelo SDK do AEM AEM AEM e são desenvolvidos usando sua própria cadeia de ferramentas, separada daquela exigida pelos aplicativos do com base no arquétipo de projeto Maven.

Para estender os microsserviços do Asset Compute, as seguintes ferramentas devem ser instaladas na máquina do desenvolvedor local.

## Instruções de configuração abreviadas

Veja a seguir as instruções de uma configuração resumida. Detalhes sobre essas ferramentas de desenvolvimento são descritos nas seções discretas abaixo.

1. [Instale o Docker Desktop](https://www.docker.com/products/docker-desktop) e obtenha as imagens do Docker necessárias:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Instalar o Visual Studio Code](https://code.visualstudio.com/download)
1. [Instalar Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Instale os módulos npm necessários e os plug-ins da CLI do Adobe I/O na linha de comando:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Para obter mais informações sobre as instruções de instalação resumidas, leia as seções abaixo.

## Instalar o Visual Studio Code{#vscode}

O [Microsoft Visual Studio Code](https://code.visualstudio.com/download) é usado para desenvolver e depurar trabalhadores de Asset compute. Embora outro [IDE compatível com JavaScript](../../local-development-environment/development-tools.md#set-up-the-development-ide) possa ser usado para desenvolver o trabalhador, somente o Visual Studio Code pode ser integrado ao trabalhador de Asset compute [debug](../test-debug/debug.md).

Este tutorial presume o uso do Visual Studio Code, pois ele fornece a melhor experiência do desenvolvedor para estender o Asset Compute.

## Instalar o Docker Desktop{#docker}

Baixe e instale o [Docker Desktop](https://www.docker.com/products/docker-desktop) estável mais recente, pois ele é necessário para [testar](../test-debug/test.md) e [depurar](../test-debug/debug.md) projetos do Asset Compute localmente.

Depois de instalar o Docker Desktop, inicie-o e instale as seguintes imagens do Docker na linha de comando:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Os desenvolvedores em máquinas Windows devem verificar se estão usando contêineres Linux para as imagens acima. As etapas para alternar para contêineres Linux estão descritas na [Documentação do Docker para Windows](https://docs.docker.com/docker-for-windows/).

## Instalar Node.js (e npm){#node-js}

Os trabalhadores do Asset Compute são baseados em [Node.js](https://nodejs.org/) e, portanto, exigem Node.js 10+ (e npm) para serem desenvolvidos e compilados.

+ [Instale o Node.js (e o npm)](../../local-development-environment/development-tools.md#node-js) da mesma maneira que para o desenvolvimento tradicional do AEM.

## Instalar o Adobe I/O CLI{#aio}

[Instalar o Adobe I/O CLI](../../local-development-environment/development-tools.md#aio-cli) ou __aio__ é um módulo npm de linha de comando (CLI) que facilita o uso e a interação com tecnologias Adobe I/O e é usado para gerar e desenvolver localmente trabalhadores de Asset compute personalizado.

```
$ npm install -g @adobe/aio-cli
```

## Instalar o plug-in do Asset compute CLI do Adobe I/O{#aio-asset-compute}

O [plug-in de Asset compute CLI do Adobe I/O](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Instalar wskdebug{#wskdebug}

Baixe e instale o módulo [Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm para facilitar a depuração local de trabalhadores do Asset Compute.

_O Visual Studio Code 1.48.x+ é necessário para que [wskdebug](#wskdebug) funcione._

```
$ npm install -g @openwhisk/wskdebug
```

## Instalar ngrok{#ngrok}

Baixe e instale o módulo [ngrok](https://www.npmjs.com/package/ngrok) npm, que fornece acesso público à sua máquina de desenvolvimento local, para facilitar a depuração local dos trabalhadores do Asset Compute.

```
$ npm install -g ngrok --unsafe-perm=true
```
