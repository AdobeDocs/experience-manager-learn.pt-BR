---
title: Extensibilidade de microserviços de asset compute para AEM como Cloud Service
description: Este tutorial percorre a criação de um Asset compute simples que cria uma representação de ativo ao recortar o ativo original para um círculo e aplica contraste e brilho configuráveis. Embora o próprio trabalhador seja básico, este tutorial o usa para explorar a criação, o desenvolvimento e a implantação de um Asset compute personalizado para uso com AEM como Cloud Service.
feature: asset-compute
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1028'
ht-degree: 0%

---


# Extensibilidade de microserviços do asset compute

AEM como os microserviços de Asset compute oferecem suporte ao desenvolvimento de funcionários personalizados que são usados para ler, manipular dados binários de ativos armazenados em AEM, mais comumente, para criar execuções de ativos personalizados.

Enquanto em AEM 6.x os processos personalizados AEM Fluxo de trabalho foram usados para ler, transformar e gravar representações de ativos, em AEM como um Asset compute os trabalhadores atendem a essa necessidade.

## O que você vai fazer

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Este tutorial percorre a criação de um Asset compute simples que cria uma representação de ativo ao recortar o ativo original para um círculo e aplica contraste e brilho configuráveis. Embora o próprio trabalhador seja básico, este tutorial o usa para explorar a criação, o desenvolvimento e a implantação de um Asset compute personalizado para uso com AEM como Cloud Service.

### Objetivos {#objective}

1. Provisionar e configurar as contas e os serviços necessários para criar e implantar um trabalhador Asset compute
1. Criar e configurar um projeto de Asset compute
1. Desenvolva um trabalhador de um Asset compute que gera uma execução personalizada
1. Grave testes para e saiba como depurar o Asset compute personalizado
1. Implante o funcionário do Asset compute e integre-o AEM como um serviço de Autor do Cloud Service através de Perfis de processamento

## Configurar

Saiba como se preparar adequadamente para estender os funcionários do Asset compute, e entender quais serviços e contas devem ser provisionados e configurados, e o software instalado localmente para o desenvolvimento.

### Provisionamento de conta e serviço{#accounts-and-services}

As contas e os serviços a seguir exigem provisionamento e acesso para concluir o tutorial, AEM como um ambiente de desenvolvedor de Cloud Service ou programa Sandbox, acesso ao Adobe Project Firefly e ao Armazenamento Blob do Microsoft Azure.

+ [Prestar contas e serviços](./set-up/accounts-and-services.md)

### Ambiente de desenvolvimento local

O desenvolvimento local de projetos de Asset compute requer um conjunto de ferramentas de desenvolvimento específico, diferente do desenvolvimento AEM tradicional, incluindo: Código do Microsoft Visual Studio, Docker Desktop, Node.js e módulos npm de suporte.

+ [Configurar ambiente de desenvolvimento local](./set-up/development-environment.md)

### Adobe Project Firefly

Os projetos de asset compute são projetos do Adobe Project Firefly especialmente definidos e, como tal, exigem acesso ao Adobe Project Firefly no Adobe Developer Console para configurá-los e implantá-los.

+ [Configurar o Adobe Project Firefly](./set-up/firefly.md)

## Desenvolver

Saiba como criar e configurar um projeto de Asset compute e, em seguida, desenvolver um funcionário personalizado que gera uma execução de ativo personalizada.

### Criar um novo projeto de Asset compute

Os projetos de asset computes, que contêm um ou mais funcionários do Asset compute, são gerados usando a CLI interativa da Adobe I/O. Os projetos de asset compute são projetos especialmente estruturados para o Adobe Project Firefly, que por sua vez são projetos Node.js.

+ [Criar um novo projeto de Asset compute](./develop/project.md)

### Configurar variáveis de ambiente

As variáveis de ambiente são mantidas no arquivo `.env` para desenvolvimento local e são usadas para fornecer credenciais do Adobe I/O e credenciais do armazenamento de nuvem necessárias para o desenvolvimento local.

+ [Configure as variáveis de ambiente](./develop/environment-variables.md)

### Configurar o manifest.yml

Os projetos de asset compute contêm manifestos que definem todos os trabalhadores do Asset compute contidos no projeto, bem como quais recursos eles têm disponíveis quando implantados na Adobe I/O Runtime para execução.

+ [Configurar o manifest.yml](./develop/manifest.md)

### Desenvolver um trabalhador

O desenvolvimento de um trabalhador de Asset compute é o núcleo da extensão de microserviços de Asset compute, já que o trabalhador contém o código personalizado que gera, ou orquestra, a geração da execução de ativos resultante.

+ [Desenvolver um Asset compute](./develop/worker.md)

### Use a ferramenta de desenvolvimento de Asset computes

A Ferramenta de Desenvolvimento de Asset computes oferece um recurso Web local para implantar, executar e visualizar execuções geradas por funcionários, com suporte ao desenvolvimento rápido e interativo de funcionários do Asset compute.

+ [Use a ferramenta de desenvolvimento de Asset computes](./develop/development-tool.md)

## Testar e depurar

Saiba mais sobre como testar Asset computes personalizados para terem confiança em sua operação e como depurar Asset computes para entender e solucionar problemas de como o código personalizado é executado.

### Testar um trabalhador

O Asset compute fornece uma estrutura de teste para a criação de conjuntos de testes para trabalhadores, tornando fácil a definição de testes que asseguram o comportamento adequado.

+ [Testar um trabalhador](./test-debug/test.md)

### Depurar um trabalhador

Os funcionários do asset compute fornecem vários níveis de depuração da saída `console.log(..)` tradicional, para integrações com __Código VS__ e __wskdebug__, permitindo que os desenvolvedores avancem pelo código de trabalho conforme ele é executado em tempo real.

+ [Depurar um trabalhador](./test-debug/debug.md)

## Implantar

Saiba como integrar Asset computes personalizados com AEM como Cloud Service, implantando-os primeiro na Adobe I/O Runtime e chamando-os de AEM como um autor de Cloud Service por meio dos Perfis de processamento dos ativos AEM.

### Implantar no Adobe I/O Runtime

Os funcionários do asset compute devem ser implantados na Adobe I/O Runtime para serem usados com AEM como Cloud Service.

+ [Uso de Perfis de processamento](./deploy/runtime.md)

### Integrar trabalhadores por meio de Perfis de processamento AEM

Depois de implantados na Adobe I/O Runtime, os funcionários do Asset compute podem ser registrados em AEM como Cloud Service via [Perfis de processamento de ativos](../../assets/configuring/processing-profiles.md). Os Perfis de processamento são, por sua vez, aplicados às pastas de ativos que se aplicam aos ativos neles contidos.

+ [Integrar a Perfis de processamento AEM](./deploy/processing-profiles.md)

## Avançado 

Esses tutoriais resumidos tratam de casos de uso mais avançados baseados em aprendizados fundamentais estabelecidos nos capítulos anteriores.

+ [Desenvolva um ](./advanced/metadata.md) trabalho de metadados de Asset compute que possa gravar os metadados de volta no

## Base de código do Github

A base de códigos do tutorial está disponível no Github em:

+ [ramo principal adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @

O código-fonte não contém os arquivos `.env` ou `config.json` necessários. Eles devem ser adicionados e configurados usando suas informações de [contas e serviços](#accounts-and-services).

## Recursos adicionais

A seguir estão vários recursos de Adobe que fornecem mais informações e APIs e SDKs úteis para o desenvolvimento de funcionários de Asset computes.

### Documentação

+ [Documentação do Serviço de asset compute](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [Leia-me da Ferramenta de Desenvolvimento de asset computes](https://github.com/adobe/asset-compute-devtool)
+ [trabalhadores de exemplo de asset compute](https://github.com/adobe/asset-compute-example-workers)

### APIs e SDKs

+ [SDK do asset compute](https://github.com/adobe/asset-compute-sdk)
   + [asset compute comum](https://github.com/adobe/asset-compute-commons)
   + [asset compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Biblioteca do Wrapper da Adobe Cloud Blobstore](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Biblioteca de tentativas de busca de nó de Adobe](https://github.com/adobe/node-fetch-retry)
+ [trabalhadores de exemplo de asset compute](https://github.com/adobe/asset-compute-example-workers)
