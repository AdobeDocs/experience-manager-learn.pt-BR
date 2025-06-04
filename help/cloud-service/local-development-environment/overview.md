---
title: Ambiente de desenvolvimento local para o AEM as a Cloud Service
description: Visão geral do ambiente de desenvolvimento local do Adobe Experience Manager (AEM).
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: article
jira: KT-3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
duration: 835
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '530'
ht-degree: 100%

---

# Configuração de ambiente de desenvolvimento local {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Visão geral"
>abstract="A configuração do ambiente de desenvolvimento local do AEM as a Cloud Service inclui ferramentas de desenvolvimento necessárias para desenvolver, criar e compilar projetos do AEM, bem como runtimes locais que permitem aos desenvolvedores validar rapidamente os novos recursos localmente antes de implantá-los no AEM as a Cloud Service por meio do Adobe Cloud Manager."
>additional-url="https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines" text="Diretrizes de desenvolvimento"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=pt-BR" text="Noções básicas de desenvolvimento"

Este tutorial explica como configurar um ambiente de desenvolvimento local para o Adobe Experience Manager (AEM) usando o SDK do AEM as a Cloud Service. Estão incluídas as ferramentas de desenvolvimento necessárias para desenvolver, construir e compilar projetos AEM, bem como tempos de execução locais que permitem aos desenvolvedores validar rapidamente novos recursos localmente antes de implantá-los no AEM as a Cloud Service por meio do Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/36531?quality=12&learn=on&captions=por_br)

![Pilha de tecnologias do ambiente de desenvolvimento local do AEM as a Cloud Service](./assets/overview/aem-sdk-technology-stack.png)

O ambiente de desenvolvimento local do AEM pode ser dividido em três grupos lógicos:

+ O __Projeto AEM__ contém o código personalizado, a configuração e o conteúdo que é o aplicativo AEM personalizado.
+ O __Tempo de execução local do AEM__ que executa uma versão local dos serviços de Autor e Publicação do AEM localmente.
+ O __Tempo de execução local do Dispatcher__ que executa uma versão local do Apache HTTP Web Server e do Dispatcher.

Este tutorial mostra como instalar e configurar os itens destacados no diagrama acima, fornecendo um ambiente de desenvolvimento local estável para o desenvolvimento do AEM.

## Organização do sistema de arquivos

Este tutorial estabeleceu a localização dos artefatos do SDK do AEM as a Cloud Service e o código do Projeto do AEM da seguinte maneira:

+ O `~/aem-sdk` é uma pasta organizacional contendo as várias ferramentas fornecidas pelo SDK do AEM as a Cloud Service
+ O `~/aem-sdk/author` contém o Serviço de autor do AEM
+ O `~/aem-sdk/publish` contém o Serviço de Publicação do AEM
+ O `~/aem-sdk/dispatcher` contém as Ferramentas do Dispatcher
+ O `~/code/<project name>` contém o código-fonte personalizado do projeto do AEM

Observe que `~` é a abreviação de Diretório do usuário. No Windows, é equivalente a `%HOMEPATH%`;

## Ferramentas de desenvolvimento para Projetos do AEM

O projeto do AEM é a base de código personalizada que contém o código, a configuração e o conteúdo implantados via Cloud Manager no AEM as a Cloud Service. A estrutura do projeto de linha de base é gerada por meio do [Arquétipo Maven do projeto do AEM](https://github.com/adobe/aem-project-archetype).

Esta seção do tutorial mostra como:

+ Instalar o [!DNL Java]
+ Instalar o [!DNL Node.js] (e npm)
+ Instalar o [!DNL Maven]
+ Instalar o [!DNL Git]

[Configurar ferramentas de desenvolvimento para projetos do AEM](./development-tools.md)

## Tempo de execução local do AEM local

O SDK do AEM as a Cloud Service fornece um [!DNL QuickStart Jar] que executa uma versão local do AEM. O [!DNL QuickStart Jar] pode ser usado para executar o Serviço de AEM Author ou o Serviço de Publicação do AEM localmente. Observe que embora o [!DNL QuickStart Jar] forneça uma experiência de desenvolvimento local, nem todos os recursos disponíveis no AEM as a Cloud Service estão incluídos no [!DNL QuickStart Jar].

Esta seção do tutorial mostra como:

+ Instalar o [!DNL Java]
+ Baixar o SDK do AEM
+ Executar o [!DNL AEM Author Service]
+ Executar o [!DNL AEM Publish Service]

[Configurar o tempo de execução local do AEM](./aem-runtime.md)

## Tempo de execução local do [!DNL Dispatcher]

As Ferramentas do Dispatcher do SDK do AEM as a Cloud Service fornecem tudo o que é necessário para configurar o tempo de execução local do [!DNL Dispatcher]. As Ferramentas do [!DNL Dispatcher] são baseadas no [!DNL Docker] e fornecem ferramentas de linha de comando para transpilar o Servidor Web do [!DNL Apache HTTP] e os arquivos de configuração do [!DNL Dispatcher] em formatos compatíveis e implantá-los no [!DNL Dispatcher] em execução no container do [!DNL Docker].

Esta seção do tutorial mostra como:

+ Baixar o SDK do AEM
+ Instalar as ferramentas [!DNL Dispatcher]
+ Executar o tempo de execução local [!DNL Dispatcher]

[Configurar o tempo de execução local [!DNL Dispatcher] ](./dispatcher-tools.md)
