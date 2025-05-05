---
title: Extensibilidade de microsserviços da Asset Compute para o AEM as a Cloud Service
description: Este tutorial aborda a criação de um simples trabalhador do Asset Compute que cria uma representação do ativo ao recortar o ativo original em um círculo e aplica contraste e brilho configuráveis. Embora o próprio trabalhador seja básico, este tutorial o usa para explorar a criação, o desenvolvimento e a implantação de um trabalhador personalizado do Asset Compute para uso com o AEM as a Cloud Service.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
duration: 277
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '986'
ht-degree: 0%

---

# Extensibilidade de microsserviços do Asset Compute

Os microsserviços do Asset Compute do AEM as a Cloud Service são compatíveis com o desenvolvimento e a implantação de trabalhadores personalizados usados para ler e manipular dados binários de ativos armazenados no AEM, geralmente para criar representações de ativos personalizados.

Enquanto no AEM 6.x os processos personalizados de fluxo de trabalho do AEM eram usados para ler, transformar e gravar representações de ativos, no AEM as a Cloud Service os trabalhadores da Asset Compute atendem a essa necessidade.

## O que você fará

>[!VIDEO](https://video.tv.adobe.com/v/3417757?quality=12&learn=on&captions=por_br)

Este tutorial aborda a criação de um simples trabalhador do Asset Compute que cria uma representação do ativo ao recortar o ativo original em um círculo e aplica contraste e brilho configuráveis. Embora o próprio trabalhador seja básico, este tutorial o usa para explorar a criação, o desenvolvimento e a implantação de um trabalhador personalizado do Asset Compute para uso com o AEM as a Cloud Service.

### Objetivos {#objective}

1. Provisionar e configurar as contas e os serviços necessários para criar e implantar um trabalhador do Asset Compute
1. Criar e configurar um projeto do Asset Compute
1. Desenvolver um trabalhador do Asset Compute que gere uma representação personalizada
1. Escreva testes para o e saiba como depurar o trabalhador personalizado do Asset Compute
1. Implante o trabalhador do Asset Compute e integre-o ao serviço do AEM as a Cloud Service Author por meio de Perfis de processamento

## Configurar

Saiba como se preparar adequadamente para estender os funcionários da Asset Compute e entender quais serviços e contas devem ser provisionados e configurados, e o software instalado localmente para desenvolvimento.

### Provisionamento de conta e serviço{#accounts-and-services}

As contas e os serviços a seguir exigem provisionamento e acesso ao para concluir o tutorial, o ambiente de desenvolvimento do AEM as a Cloud Service ou o programa de sandbox, além de acesso ao App Builder e ao Armazenamento de blobs do Microsoft Azure.

+ [Provisionar contas e serviços](./set-up/accounts-and-services.md)

### Ambiente de desenvolvimento local

O desenvolvimento local de projetos do Asset Compute requer um conjunto específico de ferramentas de desenvolvedor, diferente do desenvolvimento tradicional do AEM, incluindo: Microsoft Visual Studio Code, Docker Desktop, Node.js e módulos npm de suporte.

+ [Configurar ambiente de desenvolvimento local](./set-up/development-environment.md)

### App Builder

Os projetos do Asset Compute são projetos do App Builder especialmente definidos e, como tal, exigem acesso ao App Builder na Adobe Developer Console para configurá-los e implantá-los.

+ [Configurar o App Builder](./set-up/app-builder.md)

## Desenvolver

Saiba como criar e configurar um projeto do Asset Compute e desenvolver um trabalho personalizado que gere uma representação de ativo personalizada.

### Criar um novo projeto do Asset Compute

Os projetos do Asset Compute, que contêm um ou mais trabalhadores do Asset Compute, são gerados usando a CLI interativa do Adobe I/O. Os projetos do Asset Compute são projetos App Builder especialmente estruturados que, por sua vez, são projetos Node.js.

+ [Criar um novo projeto do Asset Compute](./develop/project.md)

### Configurar variáveis de ambiente

As variáveis de ambiente são mantidas no arquivo `.env` para desenvolvimento local e são usadas para fornecer credenciais do Adobe I/O e credenciais de armazenamento de nuvem necessárias para o desenvolvimento local.

+ [Configurar as variáveis de ambiente](./develop/environment-variables.md)

### Configurar o manifest.yml

Os projetos do Asset Compute contêm manifestos que definem todos os trabalhadores do Asset Compute contidos no projeto, bem como quais recursos eles têm disponíveis quando implantados no Adobe I/O Runtime para execução.

+ [Configurar o manifest.yml](./develop/manifest.md)

### Desenvolver um trabalhador

O desenvolvimento de um trabalhador do Asset Compute é o núcleo da extensão dos microsserviços do Asset Compute, pois o trabalhador contém o código personalizado que gera ou coordena a geração da representação de ativos resultante.

+ [Desenvolver um trabalhador do Asset Compute](./develop/worker.md)

### Usar a Ferramenta de desenvolvimento do Asset Compute

A Ferramenta de desenvolvimento do Asset Compute fornece um recurso da Web local para implantar, executar e pré-visualizar representações geradas pelo trabalhador, oferecendo suporte ao desenvolvimento rápido e iterativo do trabalhador do Asset Compute.

+ [Usar a Ferramenta de desenvolvimento do Asset Compute](./develop/development-tool.md)

## Teste e depuração

Saiba como testar trabalhadores personalizados do Asset Compute para terem confiança em sua operação e depurar trabalhadores do Asset Compute para entender e solucionar problemas de como o código personalizado é executado.

### Testar um trabalhador

O Asset Compute fornece uma estrutura de teste para criar conjuntos de testes para trabalhadores, facilitando a definição de testes que garantam o comportamento adequado.

+ [Testar um trabalhador](./test-debug/test.md)

### Depurar um trabalhador

Os trabalhadores do Asset Compute fornecem vários níveis de depuração, desde a saída tradicional do `console.log(..)` até integrações com o __Código VS__ e o __wskdebug__, permitindo que os desenvolvedores passem pelo código do trabalhador à medida que ele é executado em tempo real.

+ [Depurar um trabalhador](./test-debug/debug.md)

## Implantar

Saiba como integrar trabalhadores personalizados do Asset Compute ao AEM as a Cloud Service, primeiro implantando-os no Adobe I/O Runtime e depois chamando do AEM as a Cloud Service Author por meio dos Perfis de processamento do AEM Assets.

### Implantar no Adobe I/O Runtime

Os funcionários da Asset Compute devem ser implantados no Adobe I/O Runtime para serem usados com o AEM as a Cloud Service.

+ [Uso de perfis de processamento](./deploy/runtime.md)

### Integrar trabalhadores por meio de perfis de processamento do AEM

Após a implantação no Adobe I/O Runtime, os trabalhadores da Asset Compute podem ser registrados no AEM as a Cloud Service através de [Perfis de Processamento do Assets](../../assets/configuring/processing-profiles.md). Os perfis de processamento são, por sua vez, aplicados às pastas de ativos que se aplicam aos ativos contidos neles.

+ [Integrar a perfis de processamento do AEM](./deploy/processing-profiles.md)

## Avançado 

Esses tutoriais resumidos abordam casos de uso mais avançados com base em aprendizados fundamentais estabelecidos nos capítulos anteriores.

+ [Desenvolver um trabalhador de metadados do Asset Compute](./advanced/metadata.md) que possa gravar metadados de volta no

## Codebase no Github

A base de código do tutorial está disponível no Github em:

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) na ramificação mestre

O código-fonte não contém os arquivos `.env` ou `config.json` necessários. Eles devem ser adicionados e configurados usando suas informações de [contas e serviços](#accounts-and-services).

## Recursos adicionais

A seguir estão vários recursos do Adobe que fornecem mais informações e APIs e SDKs úteis para o desenvolvimento de trabalhadores do Asset Compute.

### Documentação

+ [Documentação de serviço do Asset Compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html?lang=pt-BR)
+ [readme da Ferramenta de desenvolvimento do Asset Compute](https://github.com/adobe/asset-compute-devtool)
+ [exemplos de trabalhadores do Asset Compute](https://github.com/adobe/asset-compute-example-workers)

### APIs e SDKs

+ [Asset Compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [Asset Compute Commons](https://github.com/adobe/asset-compute-commons)
   + [Asset Compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Biblioteca de Wrapper de Blobstore da Adobe Cloud](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Biblioteca de repetição de busca de nós do Adobe](https://github.com/adobe/node-fetch-retry)
+ [exemplos de trabalhadores do Asset Compute](https://github.com/adobe/asset-compute-example-workers)
