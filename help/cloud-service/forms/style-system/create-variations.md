---
title: Stijlsysteem gebruiken in AEM Forms
description: CSS-klassen definiëren voor de variaties
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
exl-id: 68030c25-1674-4a64-9f5f-c1a74a38e3ca
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Variaties maken voor de knopcomponent

Nadat het thema is gekloond, open het project gebruikend visuele studiocode. Een vergelijkbare weergave is vereist
in visuele studiocode
![ projectontdekkingsreiziger ](assets/easel-theme.png)

Open het bestand src->components->button->_button.scss. Wij zullen onze douanevariaties in dit dossier bepalen.

## Bedrijfsvariatie

```css
.cmp-adaptiveform-button-corporate {
  @include container;
  .cmp-adaptiveform-button {
    &__widget {
      @include primary-button;
      background: $brand-red;
      text-transform: uppercase;
      border-radius: 0px;
      color: yellow;
    }
  }
}
```

## Toelichting

* **cmp-adaptiveform-collectieve**: Dit is de belangrijkste omslag of containerklasse voor de &quot;cmp-adaptiveform-knoop-collectieve&quot;component.
Stijlen of mixen in dit blok zijn van toepassing op elementen in deze klasse.
* **@include container**: Dit gebruikt een mengeling genoemd container, die in _mixins.scss wordt bepaald. De mixincontainer past typisch lay-outgerelateerde stijlen toe zoals vestiging marges, het opvullen, of andere structurele stijlen om het gedrag van de container constant te verzekeren.
* **.cmp-adaptiveform-button**: Binnen het collectief-stijl-knoop blok, richt u het kindelement met de klasse .cmp-adaptiveform-button.
* **&amp;_widget**: &amp; symbool verwijst naar de ouderselecteur, die in dit geval .cmp-adaptiveform-knoop is.
Dit betekent dat de uiteindelijke doelklasse .cmp-adaptiveform-button_widget is, een BEM-stijlklasse (Block Element Modifier) die een subcomponent (het element __widget) binnen het blok .cmp-adaptiveform-button vertegenwoordigt.
* **@include primair-button**: dit omvat een mixin voor primaire knoppen, dat wordt gedefinieerd in _mixin.scss en stijlen toevoegt die betrekking hebben op de knop (zoals opvulling, kleuren, aanwijseffecten, enz.). De eigenschappen background,text-transform,border-radius,color die zijn gedefinieerd in de mixin primary-button, worden overschreven.

Het bestand _mixins.scss wordt gedefinieerd onder de src->site, zoals hieronder in de schermafbeelding wordt getoond

![ mixin.scss ](assets/mixins.png)

## Handelswijziging

```css
.cmp-adaptiveform-button--marketing {
  
  @include container;
  .cmp-adaptiveform-button {
  &__widget {
    @include primary-button;
    background-color: #3498db;
    color: white;
    font-weight: bold;
    border: none;
    border-radius: 50px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    transition: all 0.3s ease;
    outline: none;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    &:hover:not([disabled]) {
      position: relative;
      scale: 102%;
      transition: box-shadow 0.1s ease-out, transform 0.1s ease-out;
      background-color: #2980b9;
      box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2);
      transform: translateY(-3px);
    }
  }
}
  
}
```

## Volgende stappen

[Variaties testen](./build.md)
