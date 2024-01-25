---
title: Schema maken
description: Een schema maken op basis van de gegevens die moeten worden geïmporteerd in het adaptieve formulier
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: b286c3e9-70df-46e8-b0bc-21599ab1ec06
duration: 46
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# Inleiding

De eerste stap bestaat uit het maken van een schema op basis van de gegevens die worden gebruikt om het aangepaste formulier in te vullen.

## XFA is gebaseerd op een schema

Het schema gebruiken om een adaptief formulier te maken

## XFA is niet gebaseerd op een schema

* Open XDP in de ontwerper van AEM Forms.
* Klikken op bestand | Formuliereigenschappen | Voorvertoning.
* Klik op Voorbeeldgegevens genereren.
* Klik op Genereren.
* Geef een betekenisvolle bestandsnaam op, bijvoorbeeld `form-data.xml`

U kunt alle gratis onlinegereedschappen gebruiken om [XSD genereren](https://www.freeformatter.com/xsd-generator.html) uit de XML-gegevens die in de vorige stap zijn gegenereerd.

Maak een adaptief formulier op basis van het schema van de vorige stap.

>[!NOTE]
>Het wordt altijd aangeraden de gegevens te bekijken die worden gegenereerd bij het verzenden van het adaptieve formulier. Zo krijgt u een goed idee van de XML-indeling van de gegevens die moeten worden samengevoegd met het aangepaste formulier.

Uit het adaptieve formulier verstrekte gegevens
![ingediende gegevens](./assets/af-submitted-data.png)

Gegevens geëxporteerd uit de PDF
![geëxporteerde gegevens](./assets/exported-data.png)

Uit de geëxporteerde gegevens moet u de **_topmostSubform_** -knooppunten met de juiste naamruimten behouden zodat gegevens met het aangepaste formulier kunnen worden samengevoegd.

## Volgende stappen

[OSGi-service maken](./create-osgi-service.md)
