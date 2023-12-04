---
title: Capítulo 1 - Configuração e downloads do tutorial - Serviços de conteúdo
description: O capítulo 1 do tutorial do AEM headless mostra a configuração da linha de base para a instância do AEM no tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
duration: 118
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---

# Configuração do tutorial

A versão mais recente dos componentes principais de WCM de AEM e AEM é sempre recomendada.

* AEM 6.5 ou posterior
* Componentes principais do WCM no AEM 2.4.0 ou posterior
   * Incluído na variável [Pacote de conteúdo do aplicativo para AEM móvel WKND abaixo](#wknd-mobile-application-packages)

Antes de iniciar este tutorial, verifique se as seguintes instâncias do AEM são [instalado e em execução em seu computador local](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **Autor do AEM** em **porta 4502**
* **Publicação no AEM** em **porta 4503**

## Pacotes de aplicativos WKND Mobile{#wknd-mobile-application-packages}

Instale os seguintes pacotes de conteúdo AEM no **ambos** AEM Author e AEM Publish, usando [!DNL AEM Package Manager].

* [ui.apps: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Componente proxy para componentes principais do WCM no AEM
   * [!DNL WKND Mobile] CSS das páginas do AEM Content Services (para estilo menor)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Estrutura do site
   * [!DNL WKND Mobile] Estrutura de pastas do DAM
   * [!DNL WKND Mobile] ativos de imagem

Entrada [Capítulo 7](./chapter-7.md) nós executaremos o [!DNL WKND Mobile] Aplicativo móvel Android usando [Android Studio](https://developer.android.com/studio) e o APK fornecido (Pacote de aplicativo Android):

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Pacotes de conteúdo do AEM do capítulo

Esse conjunto de pacotes de conteúdo cria o conteúdo e a configuração descritos no capítulo associado e em todos os capítulos anteriores. Esses pacotes são opcionais, mas podem acelerar a criação de conteúdo.

* [Conteúdo do capítulo 2: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Conteúdo do capítulo 3: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Conteúdo do capítulo 4: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Conteúdo do capítulo 5: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Código-fonte

O código-fonte para o projeto AEM e o [!DNL Android Mobile App] estão disponíveis no [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). O código-fonte não precisa ser construído ou modificado para este tutorial. Ele é fornecido para permitir total transparência em como todos os aspectos do tutorial são criados.

Se você encontrar um problema com o tutorial ou o código, deixe um [Problema do GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Pular para o final

Para ir até o fim do tutorial, a variável [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) o pacote de conteúdo pode ser instalado em **ambos** AEM Author e AEM Publish. Observe que o conteúdo e a configuração não serão exibidos como publicados no AEM Author, no entanto, devido à implantação manual, todo o conteúdo e a configuração necessários estão disponíveis no AEM Publish, permitindo que o [!DNL WKND Mobile App] para acessar o conteúdo.


## Próxima etapa

* [Capítulo 2 - Definição de modelos de fragmento de conteúdo de evento](./chapter-2.md)
