---
title: Het eerste formulier maken om het proces te activeren
description: Maak een eerste formulier om het e-mailbericht te activeren en het ondertekeningsproces te starten.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
kt: 6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 1%

---

# Eerste formulier maken

Het oorspronkelijke formulier (Refinance Form) wordt gebruikt voor het ondertekenen van meerdere formulieren door het activeren van de **Meerdere Forms ondertekenen** AEM workflow. U kunt naar keuze waarden invoeren, maar zorg dat de volgende velden aan het formulier worden toegevoegd.

| Veldtype | Naam | Doel | Verborgen | Standaardwaarde |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| TextField | ondertekend | De ondertekeningsstatus aangeven | Y | N |
| TextField | guid | Formulier op unieke wijze identificeren | Y | 3889 |
| TextField | customerName | De naam van klanten vastleggen | N |
| TextField | customerEmail | E-mailadres van klant om melding te verzenden | N |
| CheckBox | formsToSign | De items identificeren de formulieren in het pakket | N |

Het eerste formulier moet worden geconfigureerd om een AEM, genaamd **multipleforms**
Zorg ervoor dat het pad naar het gegevensbestand is ingesteld op **Data.xml**. Dit is erg belangrijk omdat de voorbeeldcode zoekt naar een bestand met de naam Data.xml in het laadproces van het verzenden van het formulier.

## Assets

Het oorspronkelijke formulier (Refinantie-formulier) kan [hier gedownload](assets/refinance-form.zip)

## Volgende stappen

[Formulieren maken voor ondertekening](./create-forms-for-signing.md)
