---
title: Extensibilidade de microsserviços de asset compute para AEM as a Cloud Service
description: Este tutorial aborda a criação de um trabalhador do Asset compute simples que cria uma representação do ativo ao recortar o ativo original em um círculo e aplica contraste e brilho configuráveis. Embora o próprio trabalhador seja básico, este tutorial o usa para explorar a criação, o desenvolvimento e a implantação de um trabalhador de Asset compute personalizado para uso com AEM as a Cloud Service.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 0%

---

# Extensibilidade de microsserviços do Asset compute

O AEM como microsserviços de Asset compute AEM de Cloud Service são compatíveis com o desenvolvimento e a implantação de trabalhadores personalizados usados para ler e manipular dados binários de ativos armazenados no, principalmente para criar representações de ativos personalizados.

Enquanto no AEM 6.x os processos personalizados de fluxo de trabalho de AEM eram usados para ler, transformar e gravar representações de ativos, no AEM, os trabalhadores do Asset compute as a Cloud Service atendem a essa necessidade.

## O que você fará

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Este tutorial aborda a criação de um trabalhador do Asset compute simples que cria uma representação do ativo ao recortar o ativo original em um círculo e aplica contraste e brilho configuráveis. Embora o próprio trabalhador seja básico, este tutorial o usa para explorar a criação, o desenvolvimento e a implantação de um trabalhador de Asset compute personalizado para uso com AEM as a Cloud Service.

### Objetivos {#objective}

1. Provisionar e configurar as contas e os serviços necessários para criar e implantar um trabalhador do Asset compute
1. Criar e configurar um projeto do Asset compute
1. Desenvolver um trabalhador do Asset compute que gere uma representação personalizada
1. Escreva testes para o e saiba como depurar o trabalhador de Asset compute personalizado
1. Implante o trabalhador do Asset compute e integre-o ao serviço de Autor as a Cloud Service AEM por meio de Perfis de processamento

## Configurar

Saiba como se preparar adequadamente para estender os funcionários do Asset compute e entender quais serviços e contas devem ser provisionados e configurados, e o software instalado localmente para desenvolvimento.

### Provisionamento de conta e serviço{#accounts-and-services}

As contas e os serviços a seguir exigem provisionamento e acesso ao para concluir o tutorial, o ambiente de desenvolvimento as a Cloud Service do AEM ou o programa de sandbox, o acesso ao App Builder e o Armazenamento de blobs do Microsoft Azure.

+ [Provisionar contas e serviços](./set-up/accounts-and-services.md)

### Ambiente de desenvolvimento local

O desenvolvimento local de projetos do Asset compute requer um conjunto específico de ferramentas de desenvolvedor, diferente do desenvolvimento tradicional do AEM, incluindo: Microsoft Visual Studio Code, Docker Desktop, Node.js e módulos npm de suporte.

+ [Configurar ambiente de desenvolvimento local](./set-up/development-environment.md)

### Construtor de aplicativos

Os projetos do Asset compute são projetos do App Builder especialmente definidos e, como tal, exigem acesso ao App Builder no Adobe Developer Console para configurá-los e implantá-los.

+ [Configurar o Construtor de aplicativos](./set-up/app-builder.md)

## Desenvolver

Saiba como criar e configurar um projeto do Asset compute e desenvolver um trabalho personalizado que gere uma representação de ativo personalizada.

### Criar um novo projeto do Asset compute

Os projetos do Asset compute, que contêm um ou mais trabalhadores do Asset compute, são gerados usando a CLI de Adobe I/O interativa. Os projetos do Asset compute são projetos especialmente estruturados do App Builder, que por sua vez são projetos Node.js.

+ [Criar um novo projeto do Asset compute](./develop/project.md)

### Configurar variáveis de ambiente

As variáveis de ambiente são mantidas no `.env` arquivo para desenvolvimento local, e são usados para fornecer credenciais de Adobe I/O e credenciais de armazenamento em nuvem necessárias para o desenvolvimento local.

+ [Configurar as variáveis de ambiente](./develop/environment-variables.md)

### Configurar o manifest.yml

Os projetos do Asset compute contêm manifestos que definem todos os trabalhadores do Asset compute contidos no projeto, bem como quais recursos eles têm disponíveis quando implantados no Adobe I/O Runtime para execução.

+ [Configurar o manifest.yml](./develop/manifest.md)

### Desenvolver um trabalhador

O desenvolvimento de um trabalhador do Asset compute é o núcleo da extensão dos microsserviços do Asset compute, pois o trabalhador contém o código personalizado que gera ou coordena a geração da representação do ativo resultante.

+ [Desenvolver um trabalhador do Asset compute](./develop/worker.md)

### Usar a Ferramenta de desenvolvimento de Assets compute

A Ferramenta de desenvolvimento de Assets compute fornece um recurso da Web local para implantar, executar e pré-visualizar representações geradas pelo trabalhador, oferecendo suporte ao desenvolvimento rápido e iterativo do trabalhador de Assets compute.

+ [Usar a Ferramenta de desenvolvimento de Assets compute](./develop/development-tool.md)

## Teste e depuração

Saiba como testar trabalhadores de Assets compute personalizados para serem confiantes em sua operação e depurar trabalhadores de Assets compute para entender e solucionar problemas de como o código personalizado é executado.

### Testar um trabalhador

O Asset compute fornece uma estrutura de teste para a criação de conjuntos de testes para trabalhadores, facilitando a definição de testes que garantam o comportamento adequado.

+ [Testar um trabalhador](./test-debug/test.md)

### Depurar um trabalhador

Os trabalhadores do Asset compute fornecem vários níveis de depuração do `console.log(..)` saída, para integrações com __Código VS__ e  __wskdebug__, permitindo que os desenvolvedores analisem o código do trabalhador à medida que ele é executado em tempo real.

+ [Depurar um trabalhador](./test-debug/debug.md)

## Implantar

Saiba como integrar trabalhadores de Assets compute personalizados com o AEM as a Cloud Service, implantando-os primeiro no Adobe I/O Runtime AEM as a Cloud Service e, em seguida, invocando o Author por meio dos Perfis de processamento do AEM Assets.

### Implantar no Adobe I/O Runtime

Os funcionários do Asset compute devem ser implantados no Adobe I/O Runtime para serem usados com o AEM as a Cloud Service.

+ [Uso de perfis de processamento](./deploy/runtime.md)

### Integrar trabalhadores por meio de perfis de processamento AEM

Depois de implantados no Adobe I/O Runtime, os funcionários Assets compute podem ser registrados no AEM por meio do as a Cloud Service [Perfis de processamento de ativos](../../assets/configuring/processing-profiles.md). Os perfis de processamento são, por sua vez, aplicados às pastas de ativos que se aplicam aos ativos contidos neles.

+ [Integrar a perfis de processamento AEM](./deploy/processing-profiles.md)

## Avançado 

Esses tutoriais resumidos abordam casos de uso mais avançados com base em aprendizados fundamentais estabelecidos nos capítulos anteriores.

+ [Desenvolver um trabalhador de metadados do Asset compute](./advanced/metadata.md) que podem gravar metadados no

## Codebase no Github

A base de código do tutorial está disponível no Github em:

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) ramificação mestre @

O código-fonte não contém os caracteres necessários `.env` ou `config.json` arquivos. Eles devem ser adicionados e configurados usando o [contas e serviços](#accounts-and-services) informações.

## Recursos adicionais

A seguir estão vários recursos de Adobe que fornecem mais informações e APIs e SDKs úteis para o desenvolvimento de funcionários de Assets compute.

### Documentação

+ [Documentação do Asset compute Service](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html)
+ [readme da Ferramenta de desenvolvimento de assets compute](https://github.com/adobe/asset-compute-devtool)
+ [Trabalhadores de exemplo de asset compute](https://github.com/adobe/asset-compute-example-workers)

### APIs e SDKs

+ [ASSET COMPUTE SDK](https://github.com/adobe/asset-compute-sdk)
   + [Asset compute Commons](https://github.com/adobe/asset-compute-commons)
   + [XMP asset compute](https://github.com/adobe/asset-compute-xmp#readme)
+ [Biblioteca Wrapper do Adobe Cloud Blobstore](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Biblioteca Adobe Node Fetch Retry](https://github.com/adobe/node-fetch-retry)
+ [Trabalhadores de exemplo de asset compute](https://github.com/adobe/asset-compute-example-workers)
