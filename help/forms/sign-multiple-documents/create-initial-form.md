---
title: Het eerste formulier maken om het proces te activeren
description: Maak een eerste formulier om het e-mailbericht te activeren en het ondertekeningsproces te starten.
feature: Adaptieve Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6892
thumbnail: 6892.jpg
topic: Ontwikkeling
role: Zakelijke praktiserer
level: Intermediair
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 4%

---


# Eerste formulier maken

Het oorspronkelijke formulier (Refinance Form) wordt gebruikt voor het ondertekenen van meerdere formulieren door de workflow **Meerdere Forms** AEM ondertekenen te activeren. U kunt naar keuze waarden invoeren, maar zorg dat de volgende velden aan het formulier worden toegevoegd.



| Veldtype | Naam | Doel | Verborgen | Standaardwaarde |
------------------------|---------------------------------------|--------------------|--------|-----------------
| TextField | ondertekend | De ondertekeningsstatus aangeven | J | N |
| TextField | guid | Formulier op unieke wijze identificeren | J | 3889 |
| TextField | customerName | De naam van klanten vastleggen | N |
| TextField | customerEmail | E-mailadres van klant om melding te verzenden | N |
| CheckBox | formsToSign | De items identificeren de formulieren in het pakket | N |



Het eerste formulier moet worden geconfigureerd om een AEM werkstroom met de naam **signmultipleforms** te activeren
Zorg ervoor dat het gegevenspad is ingesteld op **Data.xml**. Dit is erg belangrijk omdat de voorbeeldcode zoekt naar een bestand met de naam Data.xml in het laadproces van het verzenden van het formulier.

## Assets

Het oorspronkelijke formulier (Refinance Form) kan [hier worden gedownload](assets/refinance-form.zip)





