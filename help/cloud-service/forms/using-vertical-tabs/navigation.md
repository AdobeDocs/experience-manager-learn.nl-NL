---
title: Verticale tabbladen gebruiken in AEM Forms as a Cloud Service
description: Een adaptief formulier maken met verticale tabbladen
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16023
exl-id: c5bbd35e-fd15-4293-901e-c81faf6025f9
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '111'
ht-degree: 0%

---

# Navigeren tussen de tabbladen

U kunt tussen de tabbladen navigeren door op de afzonderlijke tabbladen te klikken of door de knoppen Vorige en Volgende op het formulier te gebruiken.
Als u met knoppen wilt navigeren, voegt u twee knoppen toe aan het formulier en geeft u deze de namen Vorige en Volgende. Koppel de volgende aangepaste functie aan de gebeurtenis click van de knop om tussen de tabbladen te navigeren.

Hier volgt de aangepaste functie waarmee u tussen de tabbladen kunt navigeren.



```javascript
/**
 * Navigate in panel with focusOption
 * @name navigateInPanelWithFocusOption
 * @param {object} panelField
 * @param {string} focusOption - values can be 'nextItem'/'previousItem'
 * @param {scope} globals
 */
function navigateInPanelWithFocusOption(panelField, focusOption, globals)
{
    globals.functions.setFocus(panelField, focusOption);
}
```

Het volgende is de regelredacteur voor de Volgende en Vorige knopen

**Volgende Knoop**

![&#x200B; next-button &#x200B;](assets/next-button.png)

**Vorige Knoop**

![&#x200B; prev-button &#x200B;](assets/prev-button.png)
