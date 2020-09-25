---
title: Inleiding tot Thema's in Commons voor het Aandeel van Activa
seo-title: Inleiding tot Thema's in Commons voor het Aandeel van Activa
description: Materialen voor zowel de functionele als de technische overeenkomst Activa Share Commons
seo-description: Materialen voor zowel de functionele als de technische overeenkomst Activa Share Commons
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


# Inleiding tot Thema&#39;s in Commons voor het Aandeel van Activa {#asset-share-commons-theme}

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

Thema [aangepaste clientbibliotheek downloaden](assets/asc-theme-demo.zip)

## Aanvullende bronnen{#additional-resources}

* [Downloads voor de release van Asset Share Commons](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [ACS AEM Commons 3.11.0+ versiedownloads](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)