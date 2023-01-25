---
title: Een component maken om de formuliergegevens weer te geven
description: Zelfstudie voor het maken van een overzichtscomponent voor het controleren van formuliergegevens voordat deze worden verzonden.
feature: Adaptive Forms
topics: development
doc-type: tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
source-git-commit: d3531e76d3341e0964e5ed878fc72037024a11fd
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# Component maken om de formuliergegevens samen te vatten

Er is een eenvoudige component gemaakt om de formuliergegevens voor revisie weer te geven. De [Bezoekfunctie van de API van guidebridge](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) is gebruikt om de formuliervelden te doorlopen. De code in de clientbibliotheek die aan deze component is gekoppeld, krijgt de deelvensters/tabelcomponenten op het formulier. Uit de onderliggende elementen van dit deelvenster/de tabelcomponenten waaruit de veldtitel, waarde en de SOM-expressie worden geëxtraheerd met de API-methoden van GuidBridge. Vervolgens wordt een eenvoudige HTML-tabel samengesteld met de titel, waarde en SOM-expressie waarmee de eindgebruiker de formuliergegevens kan bekijken of bewerken voordat het formulier wordt verzonden.

In de onderstaande schermopname ziet u bijvoorbeeld de tabel die is gemaakt om de velden en de waarden van de tabel weer te geven **Uw gegevens**. De laatste TD in de TR wordt gebruikt om de waarde van het veld te bewerken met de velden SOM-expressie.

![visit-func](assets/visit-function.png)

