---
title: Extensibilidade de microsserviços do Asset compute para o AEM as a Cloud Service
description: Este tutorial aborda a criação de um trabalhador do Asset compute simples que cria uma representação de ativos ao recortar o ativo original para um círculo e aplica contraste e brilho configuráveis. Embora o trabalhador em si seja básico, este tutorial o usa para explorar a criação, o desenvolvimento e a implantação de um Asset compute personalizado para uso com o AEM como Cloud Service.
feature: Microsserviços Asset compute
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrações, desenvolvimento
role: Developer
level: Intermediate, Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1031'
ht-degree: 0%

---


# Extensibilidade de microsserviços do Asset compute

AEM como os microsserviços de Asset compute e desenvolvimento oferecem suporte à implantação de trabalhadores personalizados usados para ler, manipular dados binários de ativos armazenados em AEM, geralmente para criar representações de ativos personalizados.

Enquanto em AEM 6.x os processos de fluxo de trabalho de AEM personalizados foram usados para ler, transformar e gravar representações de ativos, em AEM como um Asset compute Cloud Service atende a essa necessidade.

## O que você vai fazer

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Este tutorial aborda a criação de um trabalhador do Asset compute simples que cria uma representação de ativos ao recortar o ativo original para um círculo e aplica contraste e brilho configuráveis. Embora o trabalhador em si seja básico, este tutorial o usa para explorar a criação, o desenvolvimento e a implantação de um Asset compute personalizado para uso com o AEM como Cloud Service.

### Objetivos {#objective}

1. Provisionamento e configuração das contas e serviços necessários para criar e implantar um trabalhador Asset compute
1. Criar e configurar um projeto do Asset compute
1. Desenvolver um trabalhador do Asset compute am que gera uma representação personalizada
1. Escreva testes para e saiba como depurar o trabalhador do Asset compute personalizado
1. Implante o trabalhador do Asset compute e integre-o AEM como um serviço de Autor do Cloud Service por meio de Perfis de processamento

## Configurar

Saiba como se preparar adequadamente para estender os funcionários do Asset compute, e entender quais serviços e contas devem ser provisionados e configurados, além de software instalado localmente para desenvolvimento.

### Provisionamento de conta e serviços{#accounts-and-services}

As contas e serviços a seguir exigem provisionamento e acesso ao para concluir o tutorial, AEM como um ambiente de desenvolvimento de Cloud Service ou programa de sandbox, acesso ao Adobe Project Firefly e ao Microsoft Azure Blob Storage.

+ [Prestar contas e serviços](./set-up/accounts-and-services.md)

### Ambiente de desenvolvimento local

O desenvolvimento local de projetos Assets compute requer um conjunto de ferramentas de desenvolvimento específico, diferente do desenvolvimento AEM tradicional, incluindo: Microsoft Visual Studio Code, Docker Desktop, Node.js e módulos npm de suporte.

+ [Configurar o ambiente de desenvolvimento local](./set-up/development-environment.md)

### Adobe Project Firefly

Os projetos do Asset compute são projetos do Adobe Project Firefly especialmente definidos e, como tal, exigem acesso ao Adobe Project Firefly no Adobe Developer Console para configurá-los e implantá-los.

+ [Configurar o Adobe Project Firefly](./set-up/firefly.md)

## Desenvolver

Saiba como criar e configurar um projeto do Asset compute e, em seguida, desenvolver um trabalhador personalizado que gera uma representação de ativos personalizados.

### Criar um novo projeto do Asset compute

Os projetos do Asset compute, que contêm um ou mais trabalhadores do Asset compute, são gerados usando a CLI do Adobe I/O interativo. Os projetos do Asset compute são projetos do Adobe Project Firefly especialmente estruturados, que por sua vez são projetos do Node.js.

+ [Criar um novo projeto do Asset compute](./develop/project.md)

### Configurar variáveis de ambiente

As variáveis de ambiente são mantidas no arquivo `.env` para desenvolvimento local e são usadas para fornecer credenciais de Adobe I/O e credenciais de armazenamento de nuvem necessárias para o desenvolvimento local.

+ [Configurar as variáveis de ambiente](./develop/environment-variables.md)

### Configurar o manifest.yml

Os projetos do Asset compute contêm manifestos que definem todos os trabalhadores do Asset compute contidos no projeto, bem como quais recursos eles têm disponíveis quando implantados no Adobe I/O Runtime para execução.

+ [Configurar o manifest.yml](./develop/manifest.md)

### Desenvolver um trabalhador

O desenvolvimento de um trabalhador do Asset compute é o núcleo da extensão de microsserviços do Asset compute, já que o trabalhador contém o código personalizado que gera, ou orquestra, a geração da representação de ativos resultante.

+ [Desenvolver um trabalhador do Asset compute](./develop/worker.md)

### Usar a ferramenta de desenvolvimento de Assets compute

A Ferramenta de desenvolvimento de Assets compute fornece um recurso da Web local para implantar, executar e visualizar representações geradas por funcionários, com suporte ao desenvolvimento rápido e iterativo de funcionários do Asset compute.

+ [Usar a ferramenta de desenvolvimento de Assets compute](./develop/development-tool.md)

## Testar e depurar

Saiba como testar trabalhadores personalizados do Asset compute para terem confiança em sua operação e depurar os trabalhadores do Asset compute para entender e solucionar problemas de como o código personalizado é executado.

### Testar um trabalhador

O Asset compute fornece uma estrutura de teste para criar conjuntos de teste para trabalhadores, tornando fácil a definição de testes que garantam que o comportamento adequado seja fácil.

+ [Testar um trabalhador](./test-debug/test.md)

### Depurar um trabalhador

Os trabalhadores do Asset compute fornecem vários níveis de depuração da saída `console.log(..)` tradicional, para integrações com __Código VS__ e __wskdebug__, permitindo que os desenvolvedores passem pelo código do trabalhador, pois ele é executado em tempo real.

+ [Depurar um trabalhador](./test-debug/debug.md)

## Implantar

Saiba como integrar trabalhadores personalizados do Asset compute com o AEM as a Cloud Service, primeiro implantando-os no Adobe I/O Runtime e, em seguida, chamando do AEM como um autor do Cloud Service por meio dos Perfis de processamento do AEM Assets.

### Implantar no Adobe I/O Runtime

Os trabalhadores do Asset compute devem ser implantados no Adobe I/O Runtime para serem usados com o AEM como um Cloud Service.

+ [Uso de perfis de processamento](./deploy/runtime.md)

### Integrar trabalhadores por meio de Perfis de processamento de AEM

Depois de implantados no Adobe I/O Runtime, os trabalhadores do Asset compute podem ser registrados no AEM como um Cloud Service via [Assets Processing Profiles](../../assets/configuring/processing-profiles.md). Os Perfis de processamento são, por sua vez, aplicados às pastas de ativos que se aplicam aos ativos neles contidos.

+ [Integrar a Perfis de processamento de AEM](./deploy/processing-profiles.md)

## Avançado 

Esses tutoriais resumidos abordam casos de uso mais avançados com base em aprendizagens fundamentais estabelecidas nos capítulos anteriores.

+ [Desenvolver um ](./advanced/metadata.md) trabalho de metadados de Asset compute que possa gravar metadados de volta no

## Base de código no Github

A base de código do tutorial está disponível no Github em:

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ ramificação principal

O código-fonte não contém os arquivos `.env` ou `config.json` necessários. Eles devem ser adicionados e configurados usando as informações [accounts and services](#accounts-and-services).

## Recursos adicionais

Veja a seguir vários recursos do Adobe que fornecem mais informações e APIs úteis e SDKs para desenvolver funcionários do Asset compute.

### Documentação

+ [Documentação do Asset compute Service](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html)
+ [Leia-me da ferramenta de desenvolvimento de assets compute](https://github.com/adobe/asset-compute-devtool)
+ [Exemplos de trabalhadores do Asset compute](https://github.com/adobe/asset-compute-example-workers)

### APIs e SDKs

+ [SDK do Asset compute](https://github.com/adobe/asset-compute-sdk)
   + [asset compute Commons](https://github.com/adobe/asset-compute-commons)
   + [asset compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Biblioteca do Wrapper da Adobe Cloud Blobstore](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Biblioteca de tentativas de busca de nó Adobe](https://github.com/adobe/node-fetch-retry)
+ [Exemplos de trabalhadores do Asset compute](https://github.com/adobe/asset-compute-example-workers)
