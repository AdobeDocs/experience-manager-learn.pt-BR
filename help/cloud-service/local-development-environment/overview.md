---
title: Ambiente de desenvolvimento local do AEM as a Cloud Service
description: Visão geral do ambiente de desenvolvimento local do Adobe Experience Manager (AEM).
feature: Developer Tools
topics: development
version: cloud-service
doc-type: article
activity: troubleshoot
audience: developer
kt: 3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 2%

---


# Configuração do Ambiente de Desenvolvimento Local

Este tutorial aborda a configuração de um ambiente de desenvolvimento local para o Adobe Experience Manager (AEM) usando o SDK do AEM as a Cloud Service. Inclui as ferramentas de desenvolvimento necessárias para desenvolver, criar e compilar projetos do AEM, bem como tempos de execução locais que permitem aos desenvolvedores validar rapidamente novos recursos localmente antes de implantá-los no AEM as a Cloud Service por meio do Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![Pilha de tecnologia de ambiente de desenvolvimento local do AEM as a Cloud Service](./assets/overview/aem-sdk-technology-stack.png)

O ambiente de desenvolvimento local do AEM pode ser dividido em três grupos lógicos:

+ O __Projeto AEM__ contém o código, a configuração e o conteúdo personalizados que são o aplicativo AEM personalizado.
+ O __AEM Runtime local__ que executa uma versão local dos serviços de Autor e Publicação do AEM localmente.
+ O __Local Dispatcher Runtime__ que executa uma versão local do Apache HTTP Web Server e Dispatcher.

Este tutorial aborda como instalar e configurar os itens destacados no diagrama acima, fornecendo um ambiente de desenvolvimento local estável para o desenvolvimento do AEM.

## Organização do Sistema de Arquivos

Este tutorial estabeleceu a localização dos artefatos do SDK do AEM as a Cloud Service e do código do Projeto AEM da seguinte maneira:

+ `~/aem-sdk` é uma pasta organizacional que contém as várias ferramentas fornecidas pelo SDK do AEM as a Cloud Service
+ `~/aem-sdk/author` contém o serviço de autor do AEM
+ `~/aem-sdk/publish` contém o AEM Publish Service
+ `~/aem-sdk/dispatcher` contém as Ferramentas do Dispatcher
+ `~/code/<project name>` contém o código fonte personalizado do Projeto AEM

Observe que `~` é abreviado para o Diretório do usuário. No Windows, isso é equivalente a `%HOMEPATH%`;

## Ferramentas de desenvolvimento para Projetos AEM

O projeto do AEM é a base de código personalizada que contém o código, a configuração e o conteúdo implantados pelo Cloud Manager no AEM as a Cloud Service. A estrutura do projeto da linha de base é gerada por meio do [Arquétipo de Maven do Projeto AEM](https://github.com/adobe/aem-project-archetype).

Esta seção do tutorial mostra como:

+ Instalar [!DNL Java]
+ Instalar [!DNL Node.js] (e npm)
+ Instalar [!DNL Maven]
+ Instalar [!DNL Git]

[Configurar ferramentas de desenvolvimento para Projetos AEM](./development-tools.md)

## Tempo de execução local do AEM

O SDK do AEM as a Cloud Service fornece um [!DNL QuickStart Jar] que executa uma versão local do AEM. O [!DNL QuickStart Jar] pode ser usado para executar o AEM Author Service ou o AEM Publish Service localmente. Observe que, embora o [!DNL QuickStart Jar] forneça uma experiência de desenvolvimento local, nem todos os recursos disponíveis no AEM as a Cloud Service estão incluídos no [!DNL QuickStart Jar].

Esta seção do tutorial mostra como:

+ Instalar [!DNL Java]
+ Baixar o SDK do AEM
+ Execute o [!DNL AEM Author Service]
+ Execute o [!DNL AEM Publish Service]

[Configurar o tempo de execução local do AEM](./aem-runtime.md)

## Tempo de execução local [!DNL Dispatcher]

As Ferramentas do Dispatcher do SDK do AEM as a Cloud Service fornecem tudo o que é necessário para configurar o tempo de execução local [!DNL Dispatcher]. [!DNL Dispatcher] As ferramentas são  [!DNL Docker]baseadas em e fornecem ferramentas de linha de comando para transpor o Servidor  [!DNL Apache HTTP] Web e os arquivos  [!DNL Dispatcher] de configuração em formatos compatíveis e implantá-los em  [!DNL Dispatcher] execução no  [!DNL Docker] contêiner.

Esta seção do tutorial mostra como:

+ Baixar o SDK do AEM
+ Instalar ferramentas do [!DNL Dispatcher]
+ Execute o tempo de execução local [!DNL Dispatcher]

[Configurar o  [!DNL Dispatcher] LocalRuntime](./dispatcher-tools.md)
