---
title: Het eerste formulier maken om het proces te activeren
description: Maak een eerste formulier om het e-mailbericht te activeren en het ondertekeningsproces te starten.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: User
level: Intermediate
jira: KT-6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
duration: 35
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 1%

---

# Oorspronkelijk formulier maken

De aanvankelijke vorm (de Vorm van de Verfijning) wordt gebruikt voor het ondertekenen van veelvoudige vormen door het **Teken Veelvoudige werkschema van Forms** AEM in werking te stellen. U kunt naar keuze waarden invoeren, maar zorg dat de volgende velden aan het formulier worden toegevoegd.

| Veldtype | Naam | Doel | Verborgen | Standaardwaarde |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| TextField | ondertekend | De ondertekeningsstatus aangeven | Y | N |
| TextField | guid | Formulier op unieke wijze identificeren | Y | 3889 |
| TextField | customerName | De naam van klanten vastleggen | N |
| TextField | customerEmail | E-mailadres van klant om melding te verzenden | N |
| CheckBox | formsToSign | De items identificeren de formulieren in het pakket | N |

De aanvankelijke vorm moet worden gevormd om een werkschema van AEM te teweegbrengen genoemd **signmultipleforms**
Zorg ervoor de Weg van het Dossier van Gegevens aan **Data.xml** wordt geplaatst. Dit is erg belangrijk omdat de voorbeeldcode zoekt naar een bestand met de naam Data.xml tijdens het laden van het formulier.

## Assets

De aanvankelijke vorm (de Vorm van de Verfijning) kan [&#x200B; van hier worden gedownload &#x200B;](assets/refinance-form.zip)

## Volgende stappen

[Formulieren maken voor ondertekening](./create-forms-for-signing.md)
