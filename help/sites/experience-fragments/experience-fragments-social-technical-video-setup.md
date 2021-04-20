---
title: Configurar publicação social com fragmentos de experiência do AEM
description: Os Fragmentos de experiência permitem que os profissionais de marketing publiquem experiências criadas no AEM em plataformas de mídia social. O vídeo abaixo detalha a configuração e a configuração necessárias para publicar Fragmentos de experiência no Facebook e Pinterest.
sub-product: sites, serviços de conteúdo
feature: Experience Fragments
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
topic: Content Management
role: Administrator, Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '175'
ht-degree: 2%

---


# Configurar a publicação do Social com fragmentos de experiência {#set-up-social-posting-with-experience-fragments}

Os Fragmentos de experiência permitem que os profissionais de marketing publiquem experiências criadas no AEM em plataformas de mídia social. O vídeo abaixo detalha a configuração e a configuração necessárias para publicar Fragmentos de experiência no Facebook e Pinterest.

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[Fragmentos de experiência]  - Configuração e configuração para publicação social no Facebook e Pinterest*

## Lista de verificação para configurar Fragmentos de experiência para publicar no Facebook e Pinterest

1. A Instância de Autor do AEM está em execução no HTTPS
2. Conta do Facebook + Aplicativo de desenvolvedor do Facebook
3. Conta Pinterest + Aplicativo para desenvolvedores do Pinterest
4. [!UICONTROL AEM Cloud ] ServicesConfiguração - Facebook
5. [!UICONTROL AEM Cloud ] ServicesConfiguração - Pinterest
6. Fragmento de experiência do AEM com serviços da nuvem para Facebook + Pinterest
7. Variação do fragmento de experiência usando modelo do Facebook
8. Variação de fragmento de experiência usando modelo Pinterest

## URI de redirecionamento do fragmento de experiência

Esse URI é usado para aplicativos do Facebook e Pinterest como parte do fluxo do Oauth.

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

