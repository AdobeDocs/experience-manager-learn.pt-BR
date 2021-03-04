---
title: Configurar um ambiente de desenvolvimento local para a extensibilidade do Asset Compute
description: O desenvolvimento de trabalhadores do Asset Compute, que são aplicativos JavaScript Node.js, requer ferramentas de desenvolvimento específicas que diferem do desenvolvimento do AEM tradicional, que vai de Node.js e vários módulos npm a Docker Desktop e Microsoft Visual Studio Code.
feature: Microserviços do Asset Compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
topic: Integrações, desenvolvimento
role: Desenvolvedor
level: Intermediário, Experienciado
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---


# Configurar o ambiente de desenvolvimento local

Os projetos do Adobe Asset Compute não podem ser integrados ao tempo de execução local do AEM fornecido pelo SDK do AEM e são desenvolvidos usando sua própria cadeia de ferramentas, separada da exigida pelos aplicativos AEM com base no arquétipo de projeto do AEM Maven.

Para estender os microsserviços do Asset Compute, as seguintes ferramentas devem ser instaladas na máquina de desenvolvedor local.

## Instruções de configuração avançadas

Veja a seguir as instruções de configuração de um abridge. Os detalhes sobre essas ferramentas de desenvolvimento são descritos nas seções discretas abaixo.

1. [Instale o Docker ](https://www.docker.com/products/docker-desktop) Desktop e retire as imagens necessárias do Docker:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:latest
   ```

1. [Instalar o Visual Studio Code](https://code.visualstudio.com/download)
1. [Instalar o Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Instale os módulos npm necessários e os plug-ins Adobe I/O CLI da linha de comando:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Para obter mais informações sobre as instruções de instalação resumidas, leia as seções abaixo.

## Instalar o código do Visual Studio{#vscode}

[O Microsoft Visual Studio ](https://code.visualstudio.com/download) Codeis usado para desenvolver e depurar trabalhadores do Asset Compute. Embora outro [IDE compatível com JavaScript](../../local-development-environment/development-tools.md#set-up-the-development-ide) possa ser usado para desenvolver o trabalhador, somente o Código do Visual Studio pode ser integrado ao [debug](../test-debug/debug.md) trabalhador do Asset Compute.

_O Visual Studio Code 1.48.x+ é necessário para que  [](#wskdebug) wskdebugfuncione._

Este tutorial assume o uso do Visual Studio Code, pois fornece a melhor experiência de desenvolvedor para estender o Asset Compute.

## Instalar o Docker Desktop{#docker}

Baixe e instale o mais recente e estável [Docker Desktop](https://www.docker.com/products/docker-desktop), pois isso é necessário para [testar](../test-debug/test.md) e [depurar](../test-debug/debug.md) projetos do Asset Compute localmente.

Depois de instalar o Docker Desktop, inicie-o e instale as seguintes imagens Docker da linha de comando:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Os desenvolvedores em máquinas Windows devem certificar-se de que estão usando contêineres Linux para as imagens acima. As etapas para alternar para contêineres Linux são descritas na [Documentação do Docker for Windows](https://docs.docker.com/docker-for-windows/).

## Instalar o Node.js (e npm){#node-js}

Os trabalhadores do Asset Compute são baseados em [Node.js](https://nodejs.org/) e, portanto, exigem que Node.js 10+ (e npm) sejam desenvolvidos e criados.

+ [Instale o Node.js (e npm)](../../local-development-environment/development-tools.md#node-js) da mesma maneira que para o desenvolvimento do AEM tradicional.

## Instalar o Adobe I/O CLI{#aio}

[Instale a CLI](../../local-development-environment/development-tools.md#aio-cli) do Adobe I/O ou  ____ um módulo de npm de linha de comando (CLI) que facilite o uso e a interação com as tecnologias de E/S da Adobe e seja usado para gerar e desenvolver localmente os trabalhadores personalizados do Asset Compute.

```
$ npm install -g @adobe/aio-cli
```

## Instale o plug-in do Adobe I/O CLI Asset Compute{#aio-asset-compute}

O plug-in [Adobe I/O CLI Asset Compute](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Instalar o wskdebug{#wskdebug}

Baixe e instale o módulo [Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm para facilitar a depuração local dos trabalhadores do Asset Compute.

_O Visual Studio Code 1.48.x+ é necessário para que  [](#wskdebug) wskdebugfuncione._

```
$ npm install -g @openwhisk/wskdebug
```

## Instalar o grupo{#ngrok}

Baixe e instale o módulo [ngrok](https://www.npmjs.com/package/ngrok) npm, que fornece acesso público à máquina de desenvolvimento local, para facilitar a depuração local dos trabalhadores do Asset Compute.

```
$ npm install -g ngrok --unsafe-perm=true
```
