---
title: Introdução ao tema no compartilhamento de ativos Commons
description: Materiais para o entendimento funcional e técnico Ativos Compartilham Commons
version: 6.3, 6.4, 6.5
topic: Gerenciamento de conteúdo
role: Desenvolvedor
level: Intermediário
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 4%

---


# Introdução ao tema no compartilhamento de ativos Commons {#asset-share-commons-theme}

Uma breve introdução a eles no Asset Share Commons. O vídeo aborda o processo de criação de um novo tema com um esquema de cores personalizado.

>[!VIDEO](https://video.tv.adobe.com/v/20572/?quality=9&learn=on)

Neste vídeo, um novo tema será criado com base no tema Escuro do Compartilhamento de Ativos Commons. O esquema de cores corresponderá a um logotipo personalizado para dar ao site uma aparência consistente.

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

Baixar [Tema Personalizado da Biblioteca de Clientes](assets/asc-theme-demo.zip)

## Recursos adicionais{#additional-resources}

* [Downloads da versão do Asset Share Commons](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [Downloads de versões do ACS AEM Commons 3.11.0+](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)