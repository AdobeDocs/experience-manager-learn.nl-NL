---
title: Een component maken om de formuliergegevens weer te geven
description: Zelfstudie voor het maken van een overzichtscomponent voor het controleren van formuliergegevens voordat deze worden verzonden.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
duration: 33
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 0%

---

# Component maken om de formuliergegevens samen te vatten

Er is een eenvoudige component gemaakt om de formuliergegevens voor revisie weer te geven. De [ het bezoekfunctie van de gidsAPI ](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) werd gebruikt om door de vormgebieden te herhalen. De code in de clientbibliotheek die aan deze component is gekoppeld, krijgt de deelvensters/tabelcomponenten op het formulier. Uit de onderliggende elementen van dit deelvenster/de tabelcomponenten waaruit de veldtitel, waarde en de SOM-expressie worden geëxtraheerd met de API-methoden van GuidBridge. Vervolgens wordt een eenvoudige HTML-tabel samengesteld met de titel, waarde en SOM-expressie waarmee de eindgebruiker de formuliergegevens kan bekijken of bewerken voordat het formulier wordt verzonden.

Bijvoorbeeld toont het hieronder ontsproten scherm u de lijst die wordt gecreeerd om van de gebieden en zijn waarden van **een lijst te maken YourDetails**. De laatste TD in de TR wordt gebruikt om de waarde van het veld te bewerken met de velden SOM-expressie.

![ bezoek-func ](assets/visit-function.png)

## Volgende stappen

[Test de oplossing op uw lokale systeem](./deploy-on-your-system.md)
