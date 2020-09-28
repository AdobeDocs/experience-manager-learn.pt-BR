---
title: Introdução ao tema no Asset Share Commons
seo-title: Introdução ao tema no Asset Share Commons
description: Materiais para o entendimento funcional e técnico Ativos Compartilham Comuns
seo-description: Materiais para o entendimento funcional e técnico Ativos Compartilham Comuns
uuid: 5991a015-392a-4bb5-8332-192681505b07
discoiquuid: 08a5a394-c62b-4748-b303-33117f283612
contentOwner: dgonzale
feature: asset-share, brand-portal
topics: authoring, sharing, collaboration, search, integrations, publishing, metadata, images, renditions
audience: developer, architect, administrator
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '131'
ht-degree: 1%

---


# Introdução ao tema no Asset Share Commons {#asset-share-commons-theme}

Uma breve introdução ao tema no Asset Share Commons. O vídeo acompanha o processo de criação de um novo tema com um esquema de cores personalizado.

>[!VIDEO](https://video.tv.adobe.com/v/20572/?quality=9&learn=on)

Neste vídeo, um novo tema será criado com base no tema Escuro Compartilhamento de ativos. O esquema de cores corresponderá a um logotipo personalizado para dar ao site uma aparência consistente.

## Variáveis de tema

```java
/*******************************
         Theme Variables
*******************************/
 
/* Below is a condensed list of variables for easy updating of the theme */
 
/* Color Palette */
@primaryColor      : #560a63;
@secondaryColor    : #fc3096;
 
/* Text */
@fontName          : 'LatoWeb';
@fontSmoothing     : antialiased;
@headerFont        : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@pageFont          : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@textColor         : #D0D0D0;
 
 
/* Backgrounds */
@pageBackground      : #3C3C3C;
@sideBarBackground   : #363636;
 
/* Links */
@linkColor           : #FFFFFF;
@linkHoverColor      : @secondaryColor;
 
/* Buttons */
@buttonPrimaryColor      : #560a63; /* must be a value to prevent variable recursion*/
@buttonPrimaryColorHover : saturate(darken(@buttonPrimaryColor, 5), 10, relative);
 
/* Filters (Checkboxes,radio buttons, toggle, slider, dropdown, accordion colors)*/
@filterPrimaryColor      : @secondaryColor;
@filterPrimaryColorFocus : saturate(darken(@filterPrimaryColor, 5), 10, relative);
 
/* Label */
@labelPrimaryColor       : @primaryColor;
@labelPrimaryColorHover  : saturate(darken(@labelPrimaryColor, 5), 10, relative);
 
/* Menu */
@menuPrimaryColor        : @primaryColor;
 
/* Message */
@msgPositiveBackgroundColor : @secondaryColor;
@msgNegativeBackgroundColor : @red;
@msgInfoBackgroundColor: @midBlack;
@msgWarningBackgroundColor: @yellow;
```

Baixar o tema da biblioteca de cliente [personalizada](assets/asc-theme-demo.zip)

## Recursos adicionais{#additional-resources}

* [Downloads da versão do Asset Share Commons](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [Downloads de versões do ACS AEM Commons 3.11.0+](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)