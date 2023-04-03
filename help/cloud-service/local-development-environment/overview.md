---
title: Ambiente de desenvolvimento local para AEM as a Cloud Service
description: Visão geral do ambiente de desenvolvimento local do Adobe Experience Manager (AEM).
feature: Developer Tools
version: Cloud Service
doc-type: article
kt: 3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 5%

---

# Configuração do Ambiente de Desenvolvimento Local {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Visão geral"
>abstract="A configuração do ambiente de desenvolvimento local para AEM as a Cloud Service inclui ferramentas de desenvolvimento necessárias para desenvolver, criar e compilar AEM Projetos, bem como tempos de execução locais que permitem aos desenvolvedores validar rapidamente os novos recursos localmente antes de implantá-los em AEM as a Cloud Service por meio do Adobe Cloud Manager."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=pt-BR" text="Diretrizes de desenvolvimento"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=pt-BR" text="Conceitos básicos de desenvolvimento"

Este tutorial aborda a configuração de um ambiente de desenvolvimento local para o Adobe Experience Manager (AEM) usando o SDK as a Cloud Service AEM. Inclui as ferramentas de desenvolvimento necessárias para desenvolver, criar e compilar AEM projetos, bem como tempos de execução locais que permitem aos desenvolvedores validar rapidamente novos recursos localmente antes de implantá-los em AEM as a Cloud Service por meio do Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![AEM pilha de tecnologia de ambiente de desenvolvimento local as a Cloud Service](./assets/overview/aem-sdk-technology-stack.png)

O ambiente de desenvolvimento local para AEM pode ser dividido em três grupos lógicos:

+ O __Projeto AEM__ contém o código personalizado, a configuração e o conteúdo que é o aplicativo de AEM personalizado.
+ O __Tempo de Execução do AEM Local__ que executa uma versão local dos serviços de Autor e Publicação do AEM localmente.
+ O __Tempo de Execução Local do Dispatcher__ que executa uma versão local do Apache HTTP Web Server e Dispatcher.

Este tutorial aborda como instalar e configurar os itens destacados no diagrama acima, fornecendo um ambiente de desenvolvimento local estável para desenvolvimento AEM.

## Organização do Sistema de Arquivos

Este tutorial estabeleceu a localização dos artefatos AEM as a Cloud Service do SDK e AEM código do projeto da seguinte maneira:

+ `~/aem-sdk` é uma pasta organizacional que contém as várias ferramentas fornecidas pelo AEM as a Cloud Service SDK
+ `~/aem-sdk/author` contém o serviço de autor do AEM
+ `~/aem-sdk/publish` contém o AEM Publish Service
+ `~/aem-sdk/dispatcher` contém as Ferramentas do Dispatcher
+ `~/code/<project name>` contém o código-fonte personalizado do AEM Project

Observe que `~` é abreviado para o Diretório do usuário. No Windows, isso é equivalente a `%HOMEPATH%`;

## Ferramentas de desenvolvimento para projetos de AEM

O projeto do AEM é a base de código personalizada que contém o código, a configuração e o conteúdo implantados pelo Cloud Manager para AEM as a Cloud Service. A estrutura do projeto de base é gerada por meio do [Arquétipo de Maven do Projeto AEM](https://github.com/adobe/aem-project-archetype).

Esta seção do tutorial mostra como:

+ Instalar [!DNL Java]
+ Instalar [!DNL Node.js] (e npm)
+ Instalar [!DNL Maven]
+ Instalar [!DNL Git]

[Configurar ferramentas de desenvolvimento para projetos do AEM](./development-tools.md)

## Tempo de Execução do AEM Local

O AEM SDK as a Cloud Service fornece um [!DNL QuickStart Jar] que executa uma versão local do AEM. O [!DNL QuickStart Jar] pode ser usado para executar o AEM Author Service ou o AEM Publish Service localmente. Observe que, embora a variável [!DNL QuickStart Jar] O fornece uma experiência de desenvolvimento local, nem todos os recursos disponíveis AEM as a Cloud Service estão incluídos no [!DNL QuickStart Jar].

Esta seção do tutorial mostra como:

+ Instalar [!DNL Java]
+ Baixar o SDK do AEM
+ Execute o [!DNL AEM Author Service]
+ Execute o [!DNL AEM Publish Service]

[Configurar o tempo de execução do AEM local](./aem-runtime.md)

## Local [!DNL Dispatcher] Tempo de execução

AEM Ferramentas do Dispatcher do SDK as a Cloud Service fornece tudo o que é necessário para configurar o [!DNL Dispatcher] tempo de execução. [!DNL Dispatcher] As ferramentas são [!DNL Docker]baseado em e fornece ferramentas de linha de comando para transpilação [!DNL Apache HTTP] Servidor Web e [!DNL Dispatcher] arquivos de configuração em formatos compatíveis e implantá-los em [!DNL Dispatcher] em execução no [!DNL Docker] contêiner.

Esta seção do tutorial mostra como:

+ Baixar o SDK do AEM
+ Instalar [!DNL Dispatcher] Ferramentas
+ Executar o local [!DNL Dispatcher] tempo de execução

[Configure o [!DNL Dispatcher] Tempo de execução](./dispatcher-tools.md)
