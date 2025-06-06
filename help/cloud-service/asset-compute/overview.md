---
title: Extensibilidade de microsserviços do Asset Compute para o AEM as a Cloud Service
description: Este tutorial aborda a criação de um trabalhador simples do Asset Compute que cria uma representação de um ativo, recortando o ativo original em um círculo e aplicando opções de contraste e brilho configuráveis. Embora esse trabalhador seja básico, este tutorial usa-o para explorar a criação, o desenvolvimento e a implantação de um trabalhador personalizado do Asset Compute para uso com o AEM as a Cloud Service.
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
workflow-type: ht
source-wordcount: '986'
ht-degree: 100%

---

# Extensibilidade de microsserviços do Asset Compute

Os microsserviços do Asset Compute do AEM as a Cloud Service permitem o desenvolvimento e a implantação de trabalhadores personalizados usados para ler e tratar dados binários de ativos armazenados no AEM, geralmente para criar representações de ativos personalizados.

Enquanto os processos personalizados de fluxo de trabalho do AEM eram usados para ler, transformar e gravar representações de ativos no AEM 6.x, no AEM as a Cloud Service, os trabalhadores do Asset Compute suprem essa necessidade.

## O que você fará

>[!VIDEO](https://video.tv.adobe.com/v/3417757?quality=12&learn=on&captions=por_br)

Este tutorial aborda a criação de um trabalhador simples do Asset Compute que cria uma representação de um ativo, recortando o ativo original em um círculo e aplicando opções de contraste e brilho configuráveis. Embora esse trabalhador seja básico, este tutorial usa-o para explorar a criação, o desenvolvimento e a implantação de um trabalhador personalizado do Asset Compute para uso com o AEM as a Cloud Service.

### Objetivos {#objective}

1. Provisionar e configurar as contas e os serviços necessários para criar e implantar um trabalhador do Asset Compute
1. Criar e configurar um projeto do Asset Compute
1. Desenvolver um trabalhador do Asset Compute que gere uma representação personalizada
1. Gerar testes para o trabalhador personalizado do Asset Compute e aprender a depurá-lo
1. Implantar o trabalhador do Asset Compute e integrá-lo ao serviço de criação do AEM as a Cloud Service por meio de perfis de processamento

## Configurar

Saiba como preparar-se corretamente para estender os funcionários do Asset Compute e entender quais serviços e contas devem ser provisionados e configurados, bem como o software instalado localmente para desenvolvimento.

### Provisionamento de contas e serviços{#accounts-and-services}

As contas e os serviços a seguir exigem provisionamento e acesso para concluir o tutorial, o ambiente de desenvolvimento do AEM as a Cloud Service ou o programa de sandbox, bem como acesso ao App Builder e ao armazenamento de blobs do Microsoft Azure.

+ [Provisionar contas e serviços](./set-up/accounts-and-services.md)

### Ambiente de desenvolvimento local

Diferentemente do desenvolvimento tradicional do AEM, o desenvolvimento local de projetos do Asset Compute requer um conjunto específico de ferramentas de desenvolvedor, incluindo: Microsoft Visual Studio Code, Docker Desktop, Node.js e módulos de npm de suporte.

+ [Configurar ambiente de desenvolvimento local](./set-up/development-environment.md)

### App Builder

Os projetos do Asset Compute são projetos do App Builder definidos especialmente e, por esse motivo, exigem acesso ao App Builder no Adobe Developer Console para serem configurados e implantados.

+ [Configurar o App Builder](./set-up/app-builder.md)

## Desenvolver

Saiba como criar e configurar um projeto do Asset Compute e desenvolver um trabalhador personalizado que gere uma representação de ativo personalizada.

### Criar um novo projeto do Asset Compute

Os projetos do Asset Compute que contêm um ou mais trabalhadores do Asset Compute são gerados por meio da CLI interativa do Adobe I/O. Os projetos do Asset Compute são projetos do App Builder com uma estrutura especial, os quais, por sua vez, são projetos de Node.js.

+ [Criar um novo projeto do Asset Compute](./develop/project.md)

### Configurar variáveis de ambiente

As variáveis de ambiente são mantidas no arquivo `.env` para desenvolvimento local e usadas para fornecer credenciais do Adobe I/O e credenciais de armazenamento na nuvem necessárias para o desenvolvimento local.

+ [Configurar as variáveis de ambiente](./develop/environment-variables.md)

### Configurar o manifest.yml

Os projetos do Asset Compute contêm manifestos que definem todos os trabalhadores do Asset Compute contidos no projeto, bem como quais recursos estarão disponíveis quando eles forem implantados no Adobe I/O Runtime para execução.

+ [Configurar o manifest.yml](./develop/manifest.md)

### Desenvolver um trabalhador

O desenvolvimento de um trabalhador do Asset Compute é o núcleo da extensão de microsserviços do Asset Compute, pois o trabalhador contém o código personalizado que gera ou coordena a geração da representação de ativos resultante.

+ [Desenvolver um trabalhador do Asset Compute](./develop/worker.md)

### Usar a ferramenta de desenvolvimento do Asset Compute

A ferramenta de desenvolvimento do Asset Compute fornece um recurso da web local para implantar, executar e visualizar representações geradas pelo trabalhador, permitindo um desenvolvimento rápido e iterativo do trabalhador do Asset Compute.

+ [Usar a ferramenta de desenvolvimento do Asset Compute](./develop/development-tool.md)

## Testar e depurar

Saiba como testar trabalhadores personalizados do Asset Compute para uma operação confiável e como depurar trabalhadores do Asset Compute para entender e solucionar problemas de execução do código personalizado.

### Testar um trabalhador

O Asset Compute fornece uma estrutura de teste para criar conjuntos de testes para trabalhadores, facilitando a definição de testes a fim de garantir o comportamento adequado.

+ [Testar um trabalhador](./test-debug/test.md)

### Depurar um trabalhador

Os trabalhadores do Asset Compute oferecem vários níveis de depuração, desde a saída tradicional do `console.log(..)` até integrações com o __Código VS__ e o __wskdebug__, permitindo que os desenvolvedores passem o código do trabalhador, à medida que ele é executado em tempo real.

+ [Depurar um trabalhador](./test-debug/debug.md)

## Implantar

Saiba como integrar trabalhadores personalizados do Asset Compute ao AEM as a Cloud Service, implantando-os primeiro no Adobe I/O Runtime e, em seguida, invocando-os a partir do AEM as a Cloud Service Author por meio dos perfis de processamento do AEM Assets.

### Implantar no Adobe I/O Runtime

Os trabalhadores do Asset Compute devem ser implantados no Adobe I/O Runtime para ser usados com o AEM as a Cloud Service.

+ [Usar perfis de processamento](./deploy/runtime.md)

### Integrar trabalhadores por meio de perfis de processamento do AEM

Após a implantação no Adobe I/O Runtime, os trabalhadores do Asset Compute podem ser registrados no AEM as a Cloud Service por meio de [Perfis de processamento de ativos](../../assets/configuring/processing-profiles.md). Os perfis de processamento, por sua vez, são aplicados às pastas de ativos e aos ativos contidos nelas.

+ [Integrar a perfis de processamento do AEM](./deploy/processing-profiles.md)

## Avançado 

Estes tutoriais resumidos abordam casos de uso mais avançados com base em lições fundamentais estabelecidas nos capítulos anteriores.

+ [Desenvolver um trabalhador de metadados do Asset Compute](./advanced/metadata.md) que possa gravar metadados de volta na

## Base de código no Github

A base de código do tutorial está disponível no Github, em:

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute), na ramificação mestre

O código-fonte não contém os arquivos `.env` ou `config.json` necessários. Eles devem ser adicionados e configurados com as informações das suas [contas e serviços](#accounts-and-services).

## Recursos adicionais

Confira abaixo vários recursos da Adobe que fornecem mais informações, bem como APIs e SDKs úteis, para o desenvolvimento de trabalhadores do Asset Compute.

### Documentação

+ [Documentação do serviço do Asset Compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html?lang=pt-BR)
+ [readme da ferramenta de desenvolvimento do Asset Compute](https://github.com/adobe/asset-compute-devtool)
+ [Exemplos de trabalhadores do Asset Compute](https://github.com/adobe/asset-compute-example-workers)

### APIs e SDKs

+ [SDK do Asset Compute](https://github.com/adobe/asset-compute-sdk)
   + [Elementos comuns do Asset Compute](https://github.com/adobe/asset-compute-commons)
   + [XMP do Asset Compute](https://github.com/adobe/asset-compute-xmp#readme)
+ [Biblioteca de wrappers da Adobe Cloud Blobstore](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Biblioteca de novas tentativas de obter nós da Adobe](https://github.com/adobe/node-fetch-retry)
+ [Exemplos de trabalhadores do Asset Compute](https://github.com/adobe/asset-compute-example-workers)
