---
title: Configurar um ambiente de desenvolvimento local para extensibilidade do Asset compute
description: O desenvolvimento de trabalhadores do Asset compute, que são aplicativos JavaScript Node.js, exigem ferramentas de desenvolvimento específicas que diferem do desenvolvimento de AEM tradicional, que vai de Node.js e vários módulos npm a Docker Desktop e Microsoft Visual Studio Code.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
duration: 119
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---

# Configurar ambiente de desenvolvimento local

Os projetos do Adobe Asset compute não podem ser integrados ao tempo de execução do AEM local fornecido pelo SDK do AEM AEM AEM e são desenvolvidos usando sua própria cadeia de ferramentas, separada daquela exigida pelos aplicativos do com base no arquétipo de projeto Maven.

Para estender os microsserviços do Asset compute, as seguintes ferramentas devem ser instaladas na máquina do desenvolvedor local.

## Instruções de configuração abreviadas

Veja a seguir as instruções de uma configuração resumida. Detalhes sobre essas ferramentas de desenvolvimento são descritos nas seções discretas abaixo.

1. [Instalar o Docker Desktop](https://www.docker.com/products/docker-desktop) e obtenha as imagens necessárias do Docker:

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

[Código do Microsoft Visual Studio](https://code.visualstudio.com/download) O é usado para desenvolver e depurar trabalhadores do Asset compute. Embora outras [IDE compatível com JavaScript](../../local-development-environment/development-tools.md#set-up-the-development-ide) pode ser usado para desenvolver o trabalhador, somente o Visual Studio Code pode ser integrado [depurar](../test-debug/debug.md) Trabalhador de asset compute.

Este tutorial presume o uso do Visual Studio Code, pois ele fornece a melhor experiência do desenvolvedor para estender o Asset compute.

## Instalar o Docker Desktop{#docker}

Baixe e instale o mais recente e estável [Desktop Docker](https://www.docker.com/products/docker-desktop), pois isso é necessário para [test](../test-debug/test.md) e [depurar](../test-debug/debug.md) Asset compute projetos localmente.

Depois de instalar o Docker Desktop, inicie-o e instale as seguintes imagens do Docker na linha de comando:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Os desenvolvedores em máquinas Windows devem verificar se estão usando contêineres Linux para as imagens acima. As etapas para alternar para contêineres Linux estão descritas em [Documentação do Docker para Windows](https://docs.docker.com/docker-for-windows/).

## Instalar Node.js (e npm){#node-js}

Os trabalhadores assets compute são [Node.js](https://nodejs.org/)com base em e, portanto, exigem que o Node.js 10+ (e npm) seja desenvolvido e criado.

+ [Instalar Node.js (e npm)](../../local-development-environment/development-tools.md#node-js) da mesma forma que para o desenvolvimento tradicional do AEM.

## Instalar o Adobe I/O CLI{#aio}

[Instale o Adobe I/O CLI](../../local-development-environment/development-tools.md#aio-cli)ou __aio__ é um módulo npm de linha de comando (CLI) que facilita o uso e a interação com tecnologias Adobe I/O e é usado para trabalhadores de Assets compute personalizados gerados e desenvolvidos localmente.

```
$ npm install -g @adobe/aio-cli
```

## Instalar o plug-in do Asset compute CLI do Adobe I/O{#aio-asset-compute}

A variável [Plug-in de Asset compute do Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Instalar wskdebug{#wskdebug}

Baixe e instale o [Depuração do Apache OpenWhisk](https://www.npmjs.com/package/@openwhisk/wskdebug) módulo npm para facilitar a depuração local de trabalhadores do Asset compute.

_O Visual Studio Code 1.48.x+ é necessário para [wskdebug](#wskdebug) para trabalhar._

```
$ npm install -g @openwhisk/wskdebug
```

## Instalar ngrok{#ngrok}

Baixe e instale o [grok](https://www.npmjs.com/package/ngrok) módulo npm, que fornece acesso público à máquina de desenvolvimento local, para facilitar a depuração local de trabalhadores do Asset compute.

```
$ npm install -g ngrok --unsafe-perm=true
```
