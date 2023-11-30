---
title: Integrações as a Cloud Service do AEM com o Adobe Experience Cloud
description: Saiba mais sobre as integrações compatíveis do AEM as a Cloud Service com outros produtos da Adobe Experience Cloud.
version: Cloud Service
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
jira: KT-10718
thumbnail: KT-10718.png
last-substantial-update: 2022-11-17T00:00:00Z
mini-toc-levels: 1
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM as a Cloud Service" before-title="false"
exl-id: 9e856dcc-f042-4e9d-bf97-dd4f72e837e3
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '928'
ht-degree: 18%

---

# Integrações as a Cloud Service do AEM com o Adobe Experience Cloud

Saiba mais sobre as integrações compatíveis do AEM as a Cloud Service com outros produtos da Adobe Experience Cloud.
Clique no produto Experience Cloud para obter a documentação sobre como configurar e usar as integrações.

|                                                                   | AEM Sites | AEM Assets | AEM Forms |
|-------------------------------------------------------------------|:---------:|:----------:|:---------:|
| [Acrobat Sign](#adobe-acrobat-sign) |           |            | ✔ |
| Publicidade |           |            |          |
| [Analytics](#adobe-analytics) | ✔ | ✔ | ✔ |
| Gerenciador de público |           |            |          |
| Campaign Classic |           |            |          |
| Campaign Standard |           |            |          |
| [Commerce](#adobe-commerce) | ✔ | ✔ |          |
| Customer Journey Analytics |           |            |          |
| [tags Experience Platform](#adobe-experience-platform-tags) | ✔ |            | ✔ |
| [Journey Optimizer](#adobe-journey-optimizer) |           | ✔ |          |
| [Learning Manager](#adobe-learning-manager) | ✔ |            |          |
| Marketo Engage |           |            |          |
| Real-time CDP |           |            |          |
| [Sensei](#adobe-sensei) | ✔ | ✔ | ✔ |
| [Target](#adobe-target) | ✔ |            |          |
| [Workfront](#adobe-workfront) |           | ✔ |          |


## Adobe Acrobat Sign

O Adobe Acrobat Sign (antigo Acrobat Sign) permite fluxos de trabalho de assinatura eletrônica para formulários adaptáveis do AEM Forms, melhorando os fluxos de trabalho para processar documentos para áreas jurídicas, de vendas, de folha de pagamento, de RH e outras.

### AEM Forms

+ [Configurar a integração do Adobe Acrobat Sign](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adobe-sign-integration-adaptive-forms.html)
+ [Tutorial do AEM Forms e do Adobe Acrobat Sign](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/forms-and-sign/introduction.html)

## Adobe Analytics

A integração do Adobe Analytics com o AEM as a Cloud Service permite rastrear a atividade de conteúdo e analisar dados de qualquer lugar na jornada do cliente. Além disso, obtenha relatórios versáteis, inteligência preditiva e muito mais.

### AEM Sites

+ [Configurar a integração do Adobe Analytics](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-analytics.html)
+ [Tutorial do AEM Sites e do Analytics](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/analytics/collect-data-analytics.html?lang=pt-BR)
+ Camada de dados de clientes Adobe (ACDL)

   + [Estender o ACDL nos componentes principais do WCM no AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html)
   + [Integrar o ACDL com os componentes principais do WCM no AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/integrations.html)
   + [Manipulação de dados orientada por eventos com o ACDL](https://experienceleague.adobe.com/docs/adobe-developers-live-events/events/2021/oct2021/adobe-client-data-layer.html)
   + [Tutorial da Camada de dados do cliente Adobe (ACDL)](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=pt-BR)

### AEM Assets

+ [Visão geral do Assets Insights](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html)
+ [Configurar insights do Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html#configure-asset-insights)
+ [Tutorial do Assets Insights](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/advanced/asset-insights-launch-tutorial.html)

### AEM Forms

+ [Configurar a integração do Adobe Analytics](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate-aem-forms-with-adobe-analytics.html)

### AEM Sites

+ [Integração com o Adobe Campaign Classic](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-campaign-classic.html#configure-user)
+ [Criação de boletim informativo no Adobe Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/creating-newsletter.html)
+ [Documentação dos Componentes principais de email do AEM](https://github.com/adobe/aem-core-email-components#aem-email-core-components)

## Adobe Commerce

A integração do Adobe Commerce com o AEM as a Cloud Service permite que as marcas possam dimensionar e inovar com mais rapidez para diferenciar experiências de comércio e capturar gastos online acelerados. O AEM com o Commerce combina as experiências imersivas, omnicanal e personalizadas no Experience Manager com qualquer número de soluções comerciais para trazer experiências diferenciadas para todas as partes da jornada de compras, reduzindo o tempo de retorno do investimento e impulsionando uma conversão mais alta.

### AEM Sites

+ [Guia do usuário de Conteúdo e Comércio de AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/content-and-commerce/home.html?lang=pt-BR)


## Tags do Adobe Experience Platform

As tags da Adobe Experience Platform (antigo Adobe Launch, DTM) se integram perfeitamente ao AEM, oferecendo uma forma simples de implantar e gerenciar [analytics](#adobe-analytics), [direcionamento](#adobe-target)tags de marketing e publicidade necessárias para envolver as experiências do cliente.

### AEM Sites

+ [guia do usuário de tags Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Tutorial de tags do Experience Platform](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=pt-BR)

### AEM Forms

+ [guia do usuário de tags Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Tutorial de tags do Experience Platform](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=pt-BR)

## Adobe Journey Optimizer

O Adobe Journey Optimizer ajuda você a programar campanhas omnicanal e momentos personalizados com milhões de clientes por meio de um único aplicativo, e toda a jornada é otimizada com decisões e insights inteligentes.

### AEM Assets

+ [Integrar o AEM Assets Essentials ao Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer-learn/tutorials/create-messages/create-email-content-with-the-message-editor.html?lang=pt-BR)

## Adobe Learning Manager

O Adobe Learning Manager (antigo Adobe Captivate Prime) oferece aprendizado personalizado a clientes e funcionários.

### AEM Sites

+ [Integrar o AEM Sites com o Adobe Learning Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-learning-manager.html)

## Adobe Sensei

O Adobe Sensei fornece IA e tecnologia de aprendizado de máquina para transformar o processo de gerenciamento de conteúdo por meio de Tags inteligentes, Recorte inteligente, Pesquisa visual e muito mais.

### AEM Sites

+ [Resumir texto em Fragmentos de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-variations.html#summarizing-text)

### AEM Assets

+ [Tags inteligentes para imagens](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/image-smart-tags.html)
+ [Tags inteligentes personalizadas para imagens](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/custom-smart-tags.html)
+ [Tags inteligentes para vídeos](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/video-smart-tags.html)
+ [Corte inteligente](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/smart-crop-feature-video-use.html?lang=pt-BR)
+ [Pesquisa visual](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/search-and-discovery/search.html)

### AEM Forms

+ [Serviço de conversão automática de formulários](https://experienceleague.adobe.com/docs/aem-forms-automated-conversion-service/using/configure-service.html?lang=pt-br)


## Adobe Target

O Adobe Target integra-se ao AEM as a Cloud Service para fornecer experiência otimizada na Web para cada usuário final, tudo isso alimentado por conteúdo do AEM.

### AEM Sites

+ [Configurar a integração do Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)
+ Fragmentos de experiência para o Target

   + [Publicar fragmentos de experiência no Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)
   + [Publicar fragmentos de experiência como JSON no Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)

+ [Usar o AEM Context Hub com o Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/audiences.html#creating-an-adobe-target-audience-using-the-audience-console)
+ [Tutorial do AEM Sites e do Target](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/target/overview.html)

## Adobe Workfront

As integrações da Adobe Workfront com o AEM s a Cloud Service simplificam o processo de criação de ativos digitais, colaboração e gerenciamento do ciclo de vida.

### AEM Assets

+ [Configurar o conector aprimorado do Workfront](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html?lang=pt-BR)
+ [Vídeos do conector aprimorado do Workfront](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/workfront/enhanced-connector/basics.html)
+ AEM Assets Essentials

   + [Guia do usuário do Adobe Workfront para Assets Essentials](https://one.workfront.com/s/document-item?bundleId=the-new-workfront-experience&amp;topicId=Content%2FDocuments%2FAdobe_Workfront_for_Experience_Manager_Assets_Essentials%2F_workfront-for-aem-asset-essentials.htm)
   + [Vídeos do Adobe Workfront e do Assets Essentials](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html?lang=pt-BR)
