---
title: Ambiente de desenvolvimento local para o AEM as a Cloud Service
description: Visão geral do ambiente de desenvolvimento local do Adobe Experience Manager (AEM).
feature: Developer Tools
version: Cloud Service
doc-type: article
kt: 3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
source-git-commit: 0737cd2410b48dbaa9b6dfaaa27b854d44536f15
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 1%

---


# Configuração do Ambiente de Desenvolvimento Local

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Visão geral"
>abstract="A configuração do ambiente de desenvolvimento local para o AEM as a Cloud Service inclui ferramentas de desenvolvimento necessárias para desenvolver, criar e compilar AEM projetos, bem como tempos de execução locais que permitem aos desenvolvedores validar rapidamente os novos recursos localmente antes de implantá-los no AEM como Cloud Service pelo Adobe Cloud Manager."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="Diretrizes de desenvolvimento"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="Noções básicas de desenvolvimento"

Este tutorial aborda a configuração de um ambiente de desenvolvimento local para o Adobe Experience Manager (AEM) usando o AEM como um SDK do Cloud Service. Inclui as ferramentas de desenvolvimento necessárias para desenvolver, criar e compilar AEM projetos, bem como tempos de execução locais que permitem aos desenvolvedores validar rapidamente novos recursos localmente antes de implantá-los no AEM como Cloud Service via Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM como uma pilha de tecnologia de ambiente de desenvolvimento local do Cloud Service](./assets/overview/aem-sdk-technology-stack.png)

O ambiente de desenvolvimento local para AEM pode ser dividido em três grupos lógicos:

+ O __AEM Projeto__ contém o código, a configuração e o conteúdo personalizados que são o aplicativo de AEM personalizado.
+ O __Local AEM Runtime__ que executa uma versão local dos serviços de Autor e Publicação do AEM localmente.
+ O __Local Dispatcher Runtime__ que executa uma versão local do Apache HTTP Web Server e Dispatcher.

Este tutorial aborda como instalar e configurar os itens destacados no diagrama acima, fornecendo um ambiente de desenvolvimento local estável para desenvolvimento AEM.

## Organização do Sistema de Arquivos

Este tutorial estabeleceu a localização do AEM como artefatos do SDK do Cloud Service e AEM código do projeto da seguinte maneira:

+ `~/aem-sdk` é uma pasta organizacional que contém as várias ferramentas fornecidas pelo AEM como um SDK do Cloud Service
+ `~/aem-sdk/author` contém o serviço de autor do AEM
+ `~/aem-sdk/publish` contém o AEM Publish Service
+ `~/aem-sdk/dispatcher` contém as Ferramentas do Dispatcher
+ `~/code/<project name>` contém o código-fonte personalizado do AEM Project

Observe que `~` é abreviado para o Diretório do usuário. No Windows, isso é equivalente a `%HOMEPATH%`;

## Ferramentas de desenvolvimento para projetos de AEM

O AEM projeto é a base de código personalizada que contém o código, a configuração e o conteúdo implantados pelo Cloud Manager para AEM como Cloud Service. A estrutura do projeto da linha de base é gerada por meio do [Arquétipo de Maven do Projeto AEM](https://github.com/adobe/aem-project-archetype).

Esta seção do tutorial mostra como:

+ Instalar [!DNL Java]
+ Instalar [!DNL Node.js] (e npm)
+ Instalar [!DNL Maven]
+ Instalar [!DNL Git]

[Configurar ferramentas de desenvolvimento para projetos do AEM](./development-tools.md)

## Tempo de Execução do AEM Local

O AEM como SDK do Cloud Service fornece um [!DNL QuickStart Jar] que executa uma versão local do AEM. O [!DNL QuickStart Jar] pode ser usado para executar o AEM Author Service ou o AEM Publish Service localmente. Observe que, embora o [!DNL QuickStart Jar] forneça uma experiência de desenvolvimento local, nem todos os recursos disponíveis no AEM como um Cloud Service estão incluídos no [!DNL QuickStart Jar].

Esta seção do tutorial mostra como:

+ Instalar [!DNL Java]
+ Baixar o SDK do AEM
+ Execute o [!DNL AEM Author Service]
+ Execute o [!DNL AEM Publish Service]

[Configurar o tempo de execução do AEM local](./aem-runtime.md)

## Tempo de execução local [!DNL Dispatcher]

AEM as a Cloud Service SDK&#39;s Dispatcher Tools fornece tudo o que é necessário para configurar o tempo de execução local [!DNL Dispatcher]. [!DNL Dispatcher] As ferramentas são  [!DNL Docker]baseadas em e fornecem ferramentas de linha de comando para transpor o Servidor  [!DNL Apache HTTP] Web e os arquivos  [!DNL Dispatcher] de configuração em formatos compatíveis e implantá-los em  [!DNL Dispatcher] execução no  [!DNL Docker] contêiner.

Esta seção do tutorial mostra como:

+ Baixar o SDK do AEM
+ Instalar ferramentas do [!DNL Dispatcher]
+ Execute o tempo de execução local [!DNL Dispatcher]

[Configurar o Tempo de Execução Local [!DNL Dispatcher] ](./dispatcher-tools.md)
