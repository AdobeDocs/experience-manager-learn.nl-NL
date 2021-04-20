---
title: Inleiding tot Thema's in Commons voor het Aandeel van Activa
description: Materialen voor zowel de functionele als de technische overeenkomst Activa Share Commons
version: 6.3, 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 2%

---


# Inleiding tot Thema in Commons van het Aandeel van Activa {#asset-share-commons-theme}

Een korte inleiding op het thema in Asset Share Commons. De video doorloopt het proces voor het maken van een nieuw thema met een aangepast kleurenschema.

>[!VIDEO](https://video.tv.adobe.com/v/20572/?quality=9&learn=on)

In deze video wordt een nieuw thema gemaakt op basis van het donkere thema Commons voor delen van bedrijfsmiddelen. Het kleurenschema komt overeen met een aangepast logo zodat de site er consistent uitziet.

## Themavariabelen

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

[Aangepast clientbibliotheekthema](assets/asc-theme-demo.zip) downloaden

## Aanvullende bronnen{#additional-resources}

* [Downloads voor de release van Asset Share Commons](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [ACS AEM Commons 3.11.0+ versiedownloads](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)