---
title: Capítulo 1 - Configuração e downloads do tutorial - Serviços de conteúdo
seo-title: Getting Started with AEM Content Services - Chapter 1 -  Tutorial Set up
description: Capítulo 1 do tutorial Sem cabeçalho do AEM a configuração da linha de base da instância do AEM para o tutorial.
seo-description: Chapter 1 of the AEM Headless tutorial the baseline setup for the AEM instance for the tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 1%

---

# Configuração do tutorial

A versão mais recente dos Componentes principais do AEM e AEM WCM é sempre recomendada.

* AEM 6.5 ou posterior
* AEM Componentes principais do WCM 2.4.0 ou posterior
   * Incluído na [Pacote de conteúdo do aplicativo AEM móvel WKND abaixo](#wknd-mobile-application-packages)

Antes de iniciar este tutorial, verifique se as seguintes instâncias de AEM são [instalado e em execução no computador local](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **Autor do AEM** on **porta 4502**
* **Publicação do AEM** on **porta 4503**

## Pacotes de aplicativos móveis WKND{#wknd-mobile-application-packages}

Instale os seguintes pacotes de conteúdo AEM em **both** Autor do AEM e publicação do AEM, usando [!DNL AEM Package Manager].

* [ui.apps: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Componente proxy para componentes principais AEM WCM
   * [!DNL WKND Mobile] CSS das páginas dos Serviços de conteúdo AEM (para estilos menores)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Estrutura do site
   * [!DNL WKND Mobile] Estrutura da pasta DAM
   * [!DNL WKND Mobile] ativos de imagem

Em [Capítulo 7](./chapter-7.md) vamos executar o [!DNL WKND Mobile] Aplicativo móvel do Android usando [Android Studio](https://developer.android.com/studio) e o APK fornecido (Pacote de aplicativo Android):

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Capítulo AEM pacotes de conteúdo

Esse conjunto de pacotes de conteúdo cria o conteúdo e a configuração descritos no capítulo associado e em todos os capítulos anteriores. Esses pacotes são opcionais, mas podem agilizar a criação de conteúdo.

* [Capítulo 2 Conteúdo: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 3 Conteúdo: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 4 Conteúdo: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 5 Conteúdo: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Código fonte

O código-fonte do projeto do AEM e do [!DNL Android Mobile App] estão disponíveis no [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). O código-fonte não precisa ser criado ou modificado para este tutorial. Ele é fornecido para permitir total transparência em como todos os aspectos do tutorial são criados.

Se você encontrar um problema com o tutorial ou o código, deixe um [Problema do GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Ir para o fim

Para pular para o final do tutorial, a variável [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) o pacote de conteúdo pode ser instalado em **both** Autor do AEM e publicação do AEM. Observe que o conteúdo e a configuração não serão exibidos como publicados no AEM Author, no entanto, devido à implantação manual, todo o conteúdo e a configuração necessários estão disponíveis no AEM Publish, permitindo que a variável [!DNL WKND Mobile App] para acessar o conteúdo.


## Próxima etapa

* [Capítulo 2 - Definição dos modelos de fragmento do conteúdo do evento](./chapter-2.md)
