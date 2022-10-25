---
title: Introdução ao tema no compartilhamento de ativos Commons
description: Materiais para o entendimento funcional e técnico Ativos Compartilham Commons
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
last-substantial-update: 2022-06-22T00:00:00Z
exl-id: b7d0b6b1-145a-4987-a9dc-7263efa4d9fb
source-git-commit: f37483f90f2a707c906e1e206795fdebb5f698e9
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 1%

---

# Introdução ao tema no compartilhamento de ativos Commons {#asset-share-commons-theme}

Uma breve introdução a eles no Asset Share Commons. O vídeo aborda o processo de criação de um novo tema com um esquema de cores personalizado.

>[!VIDEO](https://video.tv.adobe.com/v/20572/?quality=9&learn=on)

Neste vídeo, um novo tema é criado com base no tema Escuro do Compartilhamento de Ativos Commons. O esquema de cores corresponderá a um logotipo personalizado para dar ao site uma aparência consistente.

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

Baixar [Tema da Biblioteca de Clientes Personalizada](assets/asc-theme-demo.zip)

## Recursos adicionais{#additional-resources}

* [Downloads da versão do Asset Share Commons](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [Downloads de versões do ACS AEM Commons 3.11.0+](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)
