---
title: Capítulo 1 - Configuração e downloads do tutorial - Serviços de conteúdo
seo-title: Introdução aos serviços de conteúdo do AEM - Capítulo 1 - Configuração do tutorial
description: O Capítulo 1 do tutorial AEM Headless mostra a configuração da linha de base para a instância do AEM para o tutorial.
seo-description: O Capítulo 1 do tutorial AEM Headless mostra a configuração da linha de base para a instância do AEM para o tutorial.
feature: '"Fragmentos de conteúdo, APIs"'
topic: '"Sem Cabeça, Gerenciamento De Conteúdo"'
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---


# Configuração do tutorial

A versão mais recente dos Componentes principais do WCM no AEM e no AEM é sempre recomendada.

* AEM 6.5 ou posterior
* Componentes principais do WCM AEM 2.4.0 ou posterior
   * Incluído no [Pacote de Conteúdo do Aplicativo WKND Mobile AEM abaixo](#wknd-mobile-application-packages)

Antes de iniciar este tutorial, verifique se as seguintes instâncias do AEM estão [instaladas e em execução em sua máquina local](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **AEM** Authon  **port 4502**
* **AEM** Publishon  **port 4503**

## Pacotes de aplicativos móveis WKND{#wknd-mobile-application-packages}

Instale os seguintes pacotes de conteúdo do AEM no **AEM Author e AEM Publish, usando [!DNL AEM Package Manager].**

* [ui.apps: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Componente de proxy para componentes principais do WCM AEM
   * [!DNL WKND Mobile] CSS das páginas dos serviços de conteúdo do AEM (para estilos menores)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Estrutura do site
   * [!DNL WKND Mobile] Estrutura da pasta DAM
   * [!DNL WKND Mobile] ativos de imagem

Em [Capítulo 7](./chapter-7.md), executaremos o [!DNL WKND Mobile] Aplicativo Móvel Android usando [Android Studio](https://developer.android.com/studio) e o APK fornecido (Pacote de Aplicativo Android):

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Capítulo de pacotes de conteúdo do AEM

Esse conjunto de pacotes de conteúdo cria o conteúdo e a configuração descritos no capítulo associado e em todos os capítulos anteriores. Esses pacotes são opcionais, mas podem agilizar a criação de conteúdo.

* [Capítulo 2 Conteúdo: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 3 Conteúdo: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 4 Conteúdo: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 5 Conteúdo: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Código fonte

O código-fonte do projeto AEM e [!DNL Android Mobile App] estão disponíveis no [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). O código-fonte não precisa ser criado ou modificado para este tutorial. Ele é fornecido para permitir total transparência em como todos os aspectos do tutorial são criados.

Se você encontrar um problema com o tutorial ou o código, deixe um [problema do GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Ir para o fim

Para pular para o final do tutorial, o pacote de conteúdo [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) pode ser instalado no **Autor do AEM e Publicação do AEM.** Observe que o conteúdo e a configuração não serão exibidos como publicados no AEM Author, no entanto, devido à implantação manual, todo o conteúdo e a configuração necessários estarão disponíveis no AEM Publish, permitindo que [!DNL WKND Mobile App] acesse o conteúdo.


## Próxima etapa

* [Capítulo 2 - Definição dos modelos de fragmento do conteúdo do evento](./chapter-2.md)
