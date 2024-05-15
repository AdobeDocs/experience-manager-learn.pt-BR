---
title: Ambiente de desenvolvimento local para o AEM as a Cloud Service
description: Visão geral do ambiente de desenvolvimento local do Adobe Experience Manager (AEM).
feature: Developer Tools
version: Cloud Service
doc-type: article
jira: KT-3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
duration: 835
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 12%

---

# Configuração de ambiente de desenvolvimento local {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Visão geral"
>abstract="A configuração do ambiente de desenvolvimento local do AEM as a Cloud Service inclui ferramentas de desenvolvimento necessárias para desenvolver, criar e compilar projetos do AEM, bem como runtimes locais que permitem aos desenvolvedores validar rapidamente os novos recursos localmente antes de implantá-los no AEM as a Cloud Service por meio do Adobe Cloud Manager."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=pt-BR" text="Diretrizes de desenvolvimento"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=pt-BR" text="Noções básicas de desenvolvimento"

Este tutorial aborda a configuração de um ambiente de desenvolvimento local para o Adobe Experience Manager (AEM) usando o SDK as a Cloud Service do AEM. Estão incluídas as ferramentas de desenvolvimento necessárias para desenvolver, construir e compilar projetos AEM, bem como os tempos de execução locais que permitem aos desenvolvedores validar rapidamente novos recursos localmente antes de implantá-los no AEM as a Cloud Service via Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![Pilha de tecnologia de ambiente de desenvolvimento local as a Cloud Service do AEM](./assets/overview/aem-sdk-technology-stack.png)

O ambiente de desenvolvimento local do AEM pode ser dividido em três grupos lógicos:

+ A variável __Projeto AEM__ contém o código personalizado, a configuração e o conteúdo que são o aplicativo AEM personalizado.
+ A variável __Tempo de execução local do AEM__ que executa uma versão local dos serviços AEM Author e Publish localmente.
+ A variável __Tempo de execução local do Dispatcher__ que executa uma versão local do Apache HTTP Web Server e do Dispatcher.

Este tutorial mostra como instalar e configurar os itens destacados no diagrama acima, fornecendo um ambiente de desenvolvimento local estável para o desenvolvimento do AEM.

## Organização do sistema de arquivos

Este tutorial estabeleceu a localização dos artefatos do SDK as a Cloud Service do AEM e o código do projeto do AEM da seguinte maneira:

+ `~/aem-sdk` é uma pasta organizacional que contém as várias ferramentas fornecidas pelo SDK do AEM as a Cloud Service
+ `~/aem-sdk/author` contém o serviço de autor AEM
+ `~/aem-sdk/publish` contém o serviço de publicação do AEM
+ `~/aem-sdk/dispatcher` contém as Ferramentas do Dispatcher
+ `~/code/<project name>` contém o código fonte personalizado do projeto AEM

Observe que `~` é uma abreviação para o Diretório do usuário. No Windows, é equivalente a `%HOMEPATH%`;

## Ferramentas de desenvolvimento para projetos AEM

O projeto AEM é a base de código personalizado que contém o código, a configuração e o conteúdo implantados por meio do Cloud Manager no AEM as a Cloud Service. A estrutura do projeto de linha de base é gerada por meio da [Arquétipo Maven do projeto AEM](https://github.com/adobe/aem-project-archetype).

Esta seção do tutorial mostra como:

+ Instalar [!DNL Java]
+ Instalar [!DNL Node.js] (e npm)
+ Instalar [!DNL Maven]
+ Instalar [!DNL Git]

[Configurar ferramentas de desenvolvimento para projetos AEM](./development-tools.md)

## AEM Runtime local

O SDK as a Cloud Service do AEM fornece uma [!DNL QuickStart Jar] que executa uma versão local do AEM. A variável [!DNL QuickStart Jar] O pode ser usado para executar o Serviço de autoria do AEM ou o Serviço de publicação do AEM localmente. Observe que, embora a variável [!DNL QuickStart Jar] fornece uma experiência de desenvolvimento local, nem todos os recursos disponíveis no AEM as a Cloud Service estão incluídos no [!DNL QuickStart Jar].

Esta seção do tutorial mostra como:

+ Instalar [!DNL Java]
+ Baixar o SDK do AEM
+ Execute o [!DNL AEM Author Service]
+ Execute o [!DNL AEM Publish Service]

[Configurar o tempo de execução local do AEM](./aem-runtime.md)

## Local [!DNL Dispatcher] Tempo de execução

As ferramentas do Dispatcher do SDK as a Cloud Service do AEM fornecem tudo o que é necessário para configurar o [!DNL Dispatcher] tempo de execução. [!DNL Dispatcher] As ferramentas são [!DNL Docker]com base em e fornece ferramentas de linha de comando para transcompilar [!DNL Apache HTTP] Servidor da Web e [!DNL Dispatcher] arquivos de configuração em formatos compatíveis e implantá-los em [!DNL Dispatcher] executando no [!DNL Docker] recipiente.

Esta seção do tutorial mostra como:

+ Baixar o SDK do AEM
+ Instalar [!DNL Dispatcher] Ferramentas
+ Executar o local [!DNL Dispatcher] tempo de execução

[Configurar o Local [!DNL Dispatcher] Tempo de execução](./dispatcher-tools.md)
