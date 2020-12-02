---
title: Capítulo 1 - Configuração e downloads do tutorial - Serviços de conteúdo
seo-title: Introdução ao AEM Content Services - Capítulo 1 - Configuração do tutorial
description: O Capítulo 1 do tutorial sem cabeçalho AEM a configuração da linha de base para a instância AEM do tutorial.
seo-description: O Capítulo 1 do tutorial sem cabeçalho AEM a configuração da linha de base para a instância AEM do tutorial.
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 0%

---


# Configuração do tutorial

A versão mais recente dos componentes principais AEM e AEM WCM é sempre recomendada.

* AEM 6.5 ou posterior
* Componentes principais do AEM WCM 2.4.0 ou posterior
   * Incluído no [Pacote de Conteúdo do Aplicativo do AEM do WKND Mobile abaixo](#wknd-mobile-application-packages)

Antes de iniciar este tutorial, verifique se as seguintes instâncias AEM estão [instaladas e em execução em sua máquina local](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **AEM** Authon  **port 4502**
* **AEM** Publishon  **port 4503**

## Pacotes de aplicativos móveis WKND{#wknd-mobile-application-packages}

Instale os seguintes pacotes de conteúdo AEM em **Autor do AEM e Publicar do AEM, usando [!DNL AEM Package Manager].**

* [ui.apps: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Componente proxy para componentes principais AEM WCM
   * [!DNL WKND Mobile] CSS das páginas do AEM Content Services (para estilização secundária)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Estrutura do site
   * [!DNL WKND Mobile] Estrutura da pasta DAM
   * [!DNL WKND Mobile] ativos de imagem

Em [Capítulo 7](./chapter-7.md), executaremos o [!DNL WKND Mobile] aplicativo Android Mobile usando [Android Studio](https://developer.android.com/studio) e o APK fornecido (Pacote de aplicativos Android):

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Capítulo AEM Pacotes de conteúdo

Esse conjunto de pacotes de conteúdo cria o conteúdo e a configuração descritos no capítulo associado e em todos os capítulos anteriores. Esses pacotes são opcionais, mas podem agilizar a criação de conteúdo.

* [Capítulo 2 Conteúdo: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 3 Conteúdo: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 4 Conteúdo: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 5 Conteúdo: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Código fonte

O código fonte do projeto AEM e [!DNL Android Mobile App] estão disponíveis em [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). O código-fonte não precisa ser criado ou modificado para este tutorial, ele é fornecido para permitir total transparência em como todos os aspectos do tutorial são criados.

Se você encontrar um problema com o tutorial ou o código, deixe um [problema do GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Pular para o fim

Para pular para o final do tutorial, o pacote de conteúdo [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) pode ser instalado no **autor do AEM e no editor do AEM.** Observe que o conteúdo e a configuração não serão exibidos como publicados no AEM Author, no entanto, devido à implantação manual, todo o conteúdo e a configuração necessários estarão disponíveis no AEM Publish, permitindo que o [!DNL WKND Mobile App] acesse o conteúdo.


## Próxima etapa

* [Capítulo 2 - Definição de modelos de fragmento do conteúdo do Evento](./chapter-2.md)
