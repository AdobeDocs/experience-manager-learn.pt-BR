---
title: Configurar publicação social com AEM fragmentos de experiência
description: Fragmentos de experiência permitem que os profissionais de marketing publiquem experiências criadas em AEM em plataformas de mídia social. O vídeo abaixo detalha a configuração necessária para publicar Fragmentos de experiência no Facebook e Pinterest.
sub-product: sites, serviços de conteúdo
feature: experience-fragments
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---


# Configuração de publicação social com fragmentos de experiência {#set-up-social-posting-with-experience-fragments}

Fragmentos de experiência permitem que os profissionais de marketing publiquem experiências criadas em AEM em plataformas de mídia social. O vídeo abaixo detalha a configuração necessária para publicar Fragmentos de experiência no Facebook e Pinterest.

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[Fragmentos]  de experiência - Configuração e configuração para publicação social no Facebook e Pinterest*

## Lista de verificação para configurar Fragmentos de experiência para postar no Facebook e no Pinterest

1. A instância do autor de AEM está sendo executada em HTTPS
2. Conta do Facebook + Aplicativo de desenvolvedor do Facebook
3. Conta Pinterest + Aplicativo para desenvolvedores do Pinterest
4. [!UICONTROL AEM Cloud ] ServicesConfiguração - Facebook
5. [!UICONTROL AEM Cloud ] ServicesConfiguração - Pinterest
6. Fragmento de experiência AEM com Cloud Services para Facebook + Pinterest
7. Variação de fragmento de experiência usando modelo do Facebook
8. Variação do fragmento de experiência usando o modelo Pinterest

## URI de redirecionamento do fragmento de experiência

Este URI é usado para aplicativos do Facebook e Pinterest como parte do fluxo Oauth.

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

