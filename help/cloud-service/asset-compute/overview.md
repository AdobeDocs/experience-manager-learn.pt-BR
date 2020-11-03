---
title: Extensibilidade de microserviços da Asset Compute para AEM como Cloud Service
description: Este tutorial percorre a criação de um trabalhador da Computação de ativos simples que cria uma representação de ativos ao recortar o ativo original em um círculo e aplica contraste e brilho configuráveis. Embora o próprio trabalhador seja básico, este tutorial o usa para explorar a criação, o desenvolvimento e a implantação de um funcionário personalizado do Asset Compute para uso com AEM como Cloud Service.
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


# Extensibilidade de microserviços da Asset Compute

AEM como os microserviços de computação de ativos Cloud Service suportam o desenvolvimento e a implantação de trabalhadores personalizados usados para ler e manipular dados binários de ativos armazenados em AEM, mais comumente, para criar execuções de ativos personalizados.

Enquanto em AEM 6.x os processos personalizados AEM Fluxo de trabalho foram usados para ler, transformar e gravar representações de ativos, em AEM como um Cloud Service Asset Compute os funcionários atendem a essa necessidade.

## O que você vai fazer

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Este tutorial percorre a criação de um trabalhador da Computação de ativos simples que cria uma representação de ativos ao recortar o ativo original em um círculo e aplica contraste e brilho configuráveis. Embora o próprio trabalhador seja básico, este tutorial o usa para explorar a criação, o desenvolvimento e a implantação de um funcionário personalizado do Asset Compute para uso com AEM como Cloud Service.

### Objetivos {#objective}

1. Provisionar e configurar as contas e os serviços necessários para criar e implantar um trabalhador da Asset Computing
1. Criar e configurar um projeto de Computação de ativos
1. Desenvolva um funcionário de Computação de ativos que gera uma representação personalizada
1. Escreva testes para e saiba como depurar o trabalhador personalizado do Asset Compute
1. Implante o funcionário da Computação de ativos e integre-o AEM como um serviço de Autor de Cloud Service através de Perfis de processamento

## Configurar

Saiba como se preparar adequadamente para estender os funcionários da Asset Compute, e entender quais serviços e contas devem ser provisionados e configurados e o software instalado localmente para o desenvolvimento.

### Provisionamento de conta e serviços{#accounts-and-services}

As contas e os serviços a seguir exigem provisionamento e acesso para concluir o tutorial, AEM como um ambiente de desenvolvedor de Cloud Service ou programa Sandbox, acesso ao Adobe Project Firefly e ao Armazenamento Blob do Microsoft Azure.

+ [Prestar contas e serviços](./set-up/accounts-and-services.md)

### Ambiente de desenvolvimento local

O desenvolvimento local de projetos de Computação de ativos requer um conjunto de ferramentas de desenvolvedor específico, diferente do desenvolvimento AEM tradicional, incluindo: Código do Microsoft Visual Studio, Docker Desktop, Node.js e módulos npm de suporte.

+ [Configurar ambiente de desenvolvimento local](./set-up/development-environment.md)

### Adobe Project Firefly

Os projetos de Computação de ativos são projetos Adobe Project Firefly especialmente definidos e, como tal, exigem acesso ao Adobe Project Firefly no Adobe Developer Console para configurá-los e implantá-los.

+ [Configurar o Adobe Project Firefly](./set-up/firefly.md)

## Desenvolver

Saiba como criar e configurar um projeto de Computação de ativos e, em seguida, desenvolver um funcionário personalizado que gera uma representação de ativos personalizada.

### Criar um novo projeto de Computação de ativos

Os projetos de Computação de ativos, que contêm um ou mais funcionários da Computação de ativos, são gerados usando a CLI de E/S do Adobe interativa. Os projetos de computação de ativos são projetos Adobe Project Firefly especialmente estruturados, que são por sua vez projetos Node.js.

+ [Criar um novo projeto de Computação de ativos](./develop/project.md)

### Configurar variáveis de ambiente

As variáveis de ambiente são mantidas no `.env` arquivo para desenvolvimento local e são usadas para fornecer credenciais de E/S de Adobe e credenciais de armazenamento de nuvem necessárias para o desenvolvimento local.

+ [Configure as variáveis de ambiente](./develop/environment-variables.md)

### Configurar o manifest.yml

Os projetos de Computação de ativos contêm manifestos que definem todos os funcionários da Computação de ativos contidos no projeto, bem como quais recursos eles têm disponíveis quando implantados na Adobe I/O Runtime para execução.

+ [Configurar o manifest.yml](./develop/manifest.md)

### Desenvolver um trabalhador

Desenvolver um trabalhador da Computação de ativos é o núcleo da extensão dos microserviços da Computação de ativos, já que o trabalhador contém o código personalizado que gera, ou orquestra, a geração da representação de ativos resultante.

+ [Desenvolver um funcionário da Asset Computing](./develop/worker.md)

### Use a ferramenta de desenvolvimento Asset Compute

A Ferramenta de desenvolvimento de computação de ativos oferece um recurso Web local para implantar, executar e visualizar representações geradas por funcionários, com suporte ao desenvolvimento rápido e interativo de funcionários da Computação de ativos.

+ [Use a ferramenta de desenvolvimento Asset Compute](./develop/development-tool.md)

## Testar e depurar

Saiba como testar os funcionários personalizados da Asset Compute para terem confiança em sua operação e depurar os funcionários da Asset Compute para entender e solucionar problemas de como o código personalizado é executado.

### Testar um trabalhador

A Asset Compute fornece uma estrutura de teste para a criação de conjuntos de testes para trabalhadores, tornando fácil a definição de testes que asseguram o comportamento adequado.

+ [Testar um trabalhador](./test-debug/test.md)

### Depurar um trabalhador

Os funcionários da Asset Compute fornecem vários níveis de depuração, desde a saída tradicional, até integrações com o Código `console.log(..)` __VS e__ wskdebug ____, permitindo que os desenvolvedores avancem pelo código de trabalho conforme ele é executado em tempo real.

+ [Depurar um trabalhador](./test-debug/debug.md)

## Implantar

Saiba como integrar os funcionários personalizados da Asset Compute com AEM como Cloud Service, implantando-os primeiro na Adobe I/O Runtime e chamando-os de AEM como um autor Cloud Service por meio dos Perfis de processamento dos ativos AEM.

### Implantar no Adobe I/O Runtime

Os funcionários da Asset Compute devem ser implantados na Adobe I/O Runtime para serem usados com AEM como Cloud Service.

+ [Uso de Perfis de processamento](./deploy/runtime.md)

### Integrar trabalhadores por meio de Perfis de processamento AEM

Depois de implantados na Adobe I/O Runtime, os funcionários da Asset Compute podem ser registrados em AEM como Cloud Service através dos Perfis [de processamento de](../../assets/configuring/processing-profiles.md)ativos. Os Perfis de processamento são, por sua vez, aplicados às pastas de ativos que se aplicam aos ativos neles contidos.

+ [Integrar a Perfis de processamento AEM](./deploy/processing-profiles.md)

## Avançado 

Esses tutoriais resumidos tratam de casos de uso mais avançados baseados em aprendizados fundamentais estabelecidos nos capítulos anteriores.

+ [Desenvolver um trabalhador](./advanced/metadata.md) de metadados do Asset Compute que possa gravar metadados de volta no

## Base de código do Github

A base de códigos do tutorial está disponível no Github em:

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ ramo principal

O código-fonte não contém os arquivos necessários `.env` ou `config.json` . Eles devem ser adicionados e configurados usando suas [contas e informações de serviços](#accounts-and-services) .

## Recursos adicionais

A seguir estão vários recursos de Adobe que fornecem mais informações e APIs e SDKs úteis para o desenvolvimento de funcionários da Asset Compute.

### Documentação

+ [Documentação do Asset Compute Service](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [Leia-me da ferramenta de desenvolvimento de computação de ativos](https://github.com/adobe/asset-compute-devtool)
+ [Funcionários de exemplo da Asset Compute](https://github.com/adobe/asset-compute-example-workers)

### APIs e SDKs

+ [SDK de computação de ativos](https://github.com/adobe/asset-compute-sdk)
   + [Pacotes de computação de ativos](https://github.com/adobe/asset-compute-commons)
   + [XMP de computação de ativos](https://github.com/adobe/asset-compute-xmp#readme)
+ [Biblioteca do Wrapper da Adobe Cloud Blobstore](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Biblioteca de tentativas de busca de nó de Adobe](https://github.com/adobe/node-fetch-retry)
+ [Funcionários de exemplo da Asset Compute](https://github.com/adobe/asset-compute-example-workers)
