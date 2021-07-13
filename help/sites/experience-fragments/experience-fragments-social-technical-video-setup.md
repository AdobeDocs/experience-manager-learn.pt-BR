---
title: Configurar a publicação no Social com AEM fragmentos de experiência
description: Os Fragmentos de experiência permitem que os profissionais de marketing publiquem experiências criadas em AEM em plataformas de mídia social. O vídeo abaixo detalha a configuração e a configuração necessárias para publicar Fragmentos de experiência no Facebook e no Pinterest.
sub-product: sites, serviços de conteúdo
feature: Fragmentos de experiência
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
topic: Gerenciamento de conteúdo
role: Admin, Developer
level: Intermediate
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 2%

---


# Configurar a publicação do Social com fragmentos de experiência {#set-up-social-posting-with-experience-fragments}

Os Fragmentos de experiência permitem que os profissionais de marketing publiquem experiências criadas em AEM em plataformas de mídia social. O vídeo abaixo detalha a configuração e a configuração necessárias para publicar Fragmentos de experiência no Facebook e no Pinterest.

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[Fragmentos de experiência]  - Configuração e configuração para publicação social no Facebook e Pinterest*

## Lista de verificação para configurar Fragmentos de experiência para publicar no Facebook e no Pinterest

1. A Instância de Autor do AEM está em execução no HTTPS
2. Conta do facebook + aplicativo para desenvolvedores do Facebook
3. Conta do pinterest + aplicativo para desenvolvedores do Pinterest
4. [!UICONTROL AEM Cloud ] ServicesConfiguração - Facebook
5. [!UICONTROL AEM Cloud ] ServicesConfiguração - Pinterest
6. AEM fragmento de experiência com Cloud Services para Facebook + Pinterest
7. Variação de fragmento de experiência usando modelo Facebook
8. Variação de fragmento de experiência usando modelo Pinterest

## URI de redirecionamento do fragmento de experiência

Esse URI é usado para aplicativos Facebook e Pinterest como parte do fluxo Oauth.

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

