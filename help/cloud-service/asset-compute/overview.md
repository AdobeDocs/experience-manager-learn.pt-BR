---
title: Extensibilidade de microsserviços do Asset Compute para o AEM as a Cloud Service
description: Este tutorial percorre a criação de um trabalhador do Asset Compute simples que cria uma representação de ativos ao cortar o ativo original para um círculo e aplica contraste e brilho configuráveis. Embora o trabalhador em si seja básico, este tutorial o usa para explorar a criação, o desenvolvimento e a implantação de um trabalhador personalizado do Asset Compute para uso com o AEM as a Cloud Service.
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1036'
ht-degree: 0%

---


# Extensibilidade de microsserviços do Asset Compute

Os microsserviços do Asset Compute do AEM as a Cloud Service são compatíveis com o desenvolvimento e a implantação de trabalhadores personalizados usados para ler e manipular dados binários de ativos armazenados no AEM, geralmente para criar representações de ativos personalizados.

Enquanto que no AEM 6.x os processos de fluxo de trabalho personalizado do AEM foram usados para ler, transformar e gravar representações de ativos de retorno, os trabalhadores do AEM as a Cloud Service Asset Compute atendem a essa necessidade.

## O que você vai fazer

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Este tutorial percorre a criação de um trabalhador do Asset Compute simples que cria uma representação de ativos ao cortar o ativo original para um círculo e aplica contraste e brilho configuráveis. Embora o trabalhador em si seja básico, este tutorial o usa para explorar a criação, o desenvolvimento e a implantação de um trabalhador personalizado do Asset Compute para uso com o AEM as a Cloud Service.

### Objetivos {#objective}

1. Provisionar e configurar as contas e os serviços necessários para criar e implantar um trabalhador do Asset Compute
1. Criar e configurar um projeto do Asset Compute
1. Desenvolver um trabalhador do Asset Compute que gera uma representação personalizada
1. Escreva testes para e saiba como depurar o trabalhador personalizado do Asset Compute
1. Implante o trabalhador do Asset Compute e integre-o ao serviço de Autor do AEM as a Cloud Service por meio de Perfis de processamento

## Configurar

Saiba como se preparar adequadamente para estender os trabalhadores do Asset Compute e entender quais serviços e contas devem ser provisionados e configurados, além de software instalado localmente para desenvolvimento.

### Provisionamento de conta e serviço{#accounts-and-services}

As contas e serviços a seguir exigem provisionamento e acesso ao para concluir o tutorial, o ambiente de desenvolvimento do AEM as a Cloud Service ou o programa Sandbox, o acesso ao Adobe Project Firefly e ao Armazenamento de blobs do Microsoft Azure.

+ [Prestar contas e serviços](./set-up/accounts-and-services.md)

### Ambiente de desenvolvimento local

O desenvolvimento local de projetos do Asset Compute requer um conjunto de ferramentas de desenvolvedor específico, diferente do desenvolvimento do AEM tradicional, incluindo: Microsoft Visual Studio Code, Docker Desktop, Node.js e módulos npm de suporte.

+ [Configurar o ambiente de desenvolvimento local](./set-up/development-environment.md)

### Adobe Project Firefly

Os projetos do Asset Compute são projetos do Adobe Project Firefly especialmente definidos e, como tal, exigem acesso ao Adobe Project Firefly no Developer Console para configurá-los e implantá-los.

+ [Configurar o Adobe Project Firefly](./set-up/firefly.md)

## Desenvolver

Saiba como criar e configurar um projeto do Asset Compute e desenvolver um trabalhador personalizado que gera uma representação de ativos personalizados.

### Criar um novo projeto do Asset Compute

Os projetos do Asset Compute, que contêm um ou mais trabalhadores do Asset Compute, são gerados usando a CLI interativa do Adobe I/O. Os projetos do Asset Compute são projetos do Adobe Project Firefly especialmente estruturados, que são, por sua vez, projetos Node.js.

+ [Criar um novo projeto do Asset Compute](./develop/project.md)

### Configurar variáveis de ambiente

As variáveis de ambiente são mantidas no arquivo `.env` para desenvolvimento local e são usadas para fornecer credenciais do Adobe I/O e credenciais de armazenamento de nuvem necessárias para o desenvolvimento local.

+ [Configurar as variáveis de ambiente](./develop/environment-variables.md)

### Configurar o manifest.yml

Os projetos do Asset Compute contêm manifestos que definem todos os trabalhadores do Asset Compute contidos no projeto, bem como quais recursos eles têm disponíveis quando implantados no Adobe I/O Runtime para execução.

+ [Configurar o manifest.yml](./develop/manifest.md)

### Desenvolver um trabalhador

O desenvolvimento de um trabalhador do Asset Compute é o núcleo da extensão dos microsserviços do Asset Compute, já que o trabalhador contém o código personalizado que gera, ou orquestra, a geração da representação de ativos resultante.

+ [Desenvolver um trabalhador do Asset Compute](./develop/worker.md)

### Usar a ferramenta de desenvolvimento Asset Compute

A Ferramenta de desenvolvimento Asset Compute fornece um recurso da Web local para implantar, executar e visualizar representações geradas por funcionários, com suporte ao desenvolvimento rápido e iterativo de funcionários do Asset Compute.

+ [Usar a ferramenta de desenvolvimento Asset Compute](./develop/development-tool.md)

## Testar e depurar

Saiba como testar os trabalhadores personalizados do Asset Compute para terem confiança em sua operação e depurar os trabalhadores do Asset Compute para entender e solucionar problemas de como o código personalizado é executado.

### Testar um trabalhador

O Asset Compute fornece uma estrutura de teste para criar conjuntos de teste para trabalhadores, tornando fácil a definição de testes que garantem que o comportamento adequado seja fácil.

+ [Testar um trabalhador](./test-debug/test.md)

### Depurar um trabalhador

Os trabalhadores do Asset Compute fornecem vários níveis de depuração da saída tradicional `console.log(..)`, para integrações com __Código VS__ e __wskdebug__, permitindo que os desenvolvedores naveguem pelo código do trabalhador, pois ele é executado em tempo real.

+ [Depurar um trabalhador](./test-debug/debug.md)

## Implantar

Saiba como integrar trabalhadores do Asset Compute personalizados ao AEM as a Cloud Service, primeiro implantando-os no Adobe I/O Runtime e, em seguida, chamando do autor do AEM as a Cloud Service por meio dos Perfis de processamento do AEM Assets.

### Implantar no Adobe I/O Runtime

Os trabalhadores do Asset Compute devem ser implantados no Adobe I/O Runtime para serem usados com o AEM as a Cloud Service.

+ [Uso de perfis de processamento](./deploy/runtime.md)

### Integre trabalhadores por meio de perfis de processamento do AEM

Depois de implantados no Adobe I/O Runtime, os trabalhadores do Asset Compute podem ser registrados no AEM as a Cloud Service por meio de [Perfis de processamento de ativos](../../assets/configuring/processing-profiles.md). Os Perfis de processamento são, por sua vez, aplicados às pastas de ativos que se aplicam aos ativos neles contidos.

+ [Integrar a Perfis de processamento AEM](./deploy/processing-profiles.md)

## Avançado 

Esses tutoriais resumidos abordam casos de uso mais avançados com base em aprendizagens fundamentais estabelecidas nos capítulos anteriores.

+ [Desenvolver um ](./advanced/metadata.md) trabalho de metadados do Asset Compute que possa gravar metadados de volta no

## Base de código no Github

A base de código do tutorial está disponível no Github em:

+ [ramificação mestre adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @

O código-fonte não contém os arquivos `.env` ou `config.json` necessários. Eles devem ser adicionados e configurados usando as informações [accounts and services](#accounts-and-services).

## Recursos adicionais

A seguir estão vários recursos da Adobe que fornecem mais informações e APIs úteis e SDKs para desenvolver trabalhadores do Asset Compute.

### Documentação

+ [Documentação do Asset Compute Service](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [Leia-me da ferramenta de desenvolvimento Asset Compute](https://github.com/adobe/asset-compute-devtool)
+ [Exemplos de trabalhadores do Asset Compute](https://github.com/adobe/asset-compute-example-workers)

### APIs e SDKs

+ [SDK do Asset Compute](https://github.com/adobe/asset-compute-sdk)
   + [Asset Compute Commons](https://github.com/adobe/asset-compute-commons)
   + [Asset Compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Biblioteca do Wrapper da Adobe Cloud Blobstore](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Biblioteca de tentativas de busca de nós da Adobe](https://github.com/adobe/node-fetch-retry)
+ [Exemplos de trabalhadores do Asset Compute](https://github.com/adobe/asset-compute-example-workers)
