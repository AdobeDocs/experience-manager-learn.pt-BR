---
title: Ambiente de desenvolvimento local para AEM como Cloud Service
description: Visão geral do ambiente de desenvolvimento local Adobe Experience Manager (AEM).
feature: null
topics: development
version: cloud-service
doc-type: article
activity: troubleshoot
audience: developer
kt: 3290
thumbnail: 32565.jpg
translation-type: tm+mt
source-git-commit: 69c1767098cc9da8ec0ae2bd83d25417d330f393
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 1%

---


# Configuração do Ambiente de desenvolvimento local

Este tutorial percorre a configuração de um ambiente de desenvolvimento local para Adobe Experience Manager (AEM) usando o AEM como um SDK Cloud Service. Inclui a ferramenta de desenvolvimento necessária para desenvolver, criar e compilar AEM Projetos, bem como os tempos de execução locais que permitem aos desenvolvedores validar rapidamente os novos recursos localmente antes de implantá-los em AEM como um Cloud Service via Gerenciador da Adobe Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM como uma pilha de tecnologia de Ambiente para desenvolvimento local Cloud Service](./assets/overview/aem-sdk-technology-stack.png)

O ambiente de desenvolvimento local para AEM pode ser dividido em três grupos lógicos:

+ O __AEM Project__ contém o código personalizado, a configuração e o conteúdo que é o aplicativo AEM personalizado.
+ O __Local AEM Runtime__ que executa uma versão local dos serviços de autor e publicação do AEM localmente.
+ O __Local Dispatcher Runtime__ que executa uma versão local do Apache HTTP Web Server e do Dispatcher.

Este tutorial mostra como instalar e configurar os itens destacados no diagrama acima, fornecendo um ambiente estável de desenvolvimento local para AEM desenvolvimento.

## Organização do sistema de arquivos

Este tutorial estabeleceu o local do AEM como artefatos SDK de Cloud Service e AEM código do projeto da seguinte maneira:

+ `~/aem-sdk` é uma pasta organizacional que contém as várias ferramentas fornecidas pelo AEM como um SDK Cloud Service
+ `~/aem-sdk/author` contém o serviço de autor de AEM
+ `~/aem-sdk/publish` contém o serviço de publicação de AEM
+ `~/aem-sdk/dispatcher` contém as Ferramentas do Dispatcher
+ `~/code/<project name>` contém o código-fonte personalizado AEM Project

Observe que `~` é abreviado para o Diretório do usuário. No Windows, isso equivale a `%HOMEPATH%`;

## Ferramentas de desenvolvimento para projetos AEM

O projeto AEM é a base de código personalizada que contém o código, a configuração e o conteúdo implantados pelo Cloud Manager para AEM como Cloud Service. A estrutura de projeto de linha de base é gerada pelo [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype).

Esta seção do tutorial mostra como:

+ Instalar [!DNL Java]
+ Instalar [!DNL Node.js] (e npm)
+ Instalar [!DNL Maven]
+ Instalar [!DNL Git]

[Configurar ferramentas de desenvolvimento para projetos AEM](./development-tools.md)

## Tempo de execução de AEM local

O AEM como um SDK Cloud Service fornece uma versão [!DNL QuickStart Jar] que executa uma versão local do AEM. O [!DNL QuickStart Jar] pode ser usado para executar o serviço de autor de AEM ou o serviço de publicação de AEM localmente. Observe que embora o [!DNL QuickStart Jar] ofereça uma experiência de desenvolvimento local, nem todos os recursos disponíveis no AEM como Cloud Service são incluídos no [!DNL QuickStart Jar].

Esta seção do tutorial mostra como:

+ Instalar [!DNL Java]
+ Baixar o SDK do AEM
+ Execute o [!DNL AEM Author Service]
+ Execute o [!DNL AEM Publish Service]

[Configurar o tempo de execução Local AEM](./aem-runtime.md)

## Tempo de [!DNL Dispatcher] execução local

AEM como as Ferramentas do Dispatcher do SDK do Cloud Service fornece tudo o que é necessário para configurar o [!DNL Dispatcher] tempo de execução local. [!DNL Dispatcher] As ferramentas são [!DNL Docker]baseadas e fornecem ferramentas de linha de comando para transformar o Servidor [!DNL Apache HTTP] Web e os arquivos de [!DNL Dispatcher] configuração em formatos compatíveis e implantá-los em [!DNL Dispatcher] execução no [!DNL Docker] container.

Esta seção do tutorial mostra como:

+ Baixar o SDK do AEM
+ Instalar [!DNL Dispatcher] ferramentas
+ Executar o [!DNL Dispatcher] tempo de execução local

[Configurar o [!DNL Dispatcher] LocalRuntime](./dispatcher-tools.md)
