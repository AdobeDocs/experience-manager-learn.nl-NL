---
title: Gegevens verzenden naar SharePoint-lijst met behulp van workflowstap
description: Gegevens invoegen in lijst met deelpunten met behulp van FDM-workflowstap aanroepen
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-15126
exl-id: b369ed05-ba25-4b0e-aa3b-e7fc1621067d
duration: 52
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Gegevens in SharePoint-lijst invoegen met FDM-workflowstap aanroepen


In dit artikel worden de stappen beschreven die nodig zijn om gegevens in te voegen in de SharePoint-lijst met de FDM-stap opvragen in AEM workflow.

In dit artikel wordt ervan uitgegaan dat u [adaptief formulier is geconfigureerd voor het verzenden van gegevens naar de SharePoint-lijst.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=en#connect-af-sharepoint-list)


## Een formuliergegevensmodel maken op basis van de gegevensbron in de SharePoint-lijst

* Maak een nieuw formuliergegevensmodel op basis van de gegevensbron van de SharePoint-lijst.
* Voeg het juiste model toe en ontvang de service van het formuliergegevensmodel.
* Vorm de tussenvoegseldienst om het hoogste niveaumodelvoorwerp op te nemen.
* Test de tussenvoegseldienst.


## Een workflow maken

* Maak een eenvoudige workflow met een FDM-stap aanroepen.
* Vorm aanhalen FDM stap om het model van vormgegevens te gebruiken dat in de vorige stap wordt gecreeerd.
* ![associatief-fdm](assets/fdm-insert-1.png)

* ![map-input-parameters](assets/fdm-insert-2.png)
* Let op het gebruik van JSON-puntnotatie. De verzonden gegevens hebben de onderstaande indeling en we extraheren het object ContactUS uit de verzonden gegevens.

```json
{
  "ContactUS": {
    "Title": "Mr",
    "Products": "Photoshop",
    "HighNetWorth": "1",
    "SubmitterName": "John Does"
  }
}
```



## Adaptief formulier configureren om AEM workflow te activeren

* Maak een adaptief formulier met het formuliergegevensmodel dat u eerder hebt gemaakt.
* Sleep enkele velden van de gegevensbron naar het formulier.
* De verzendactie van het formulier configureren, zoals hieronder wordt weergegeven
* ![voorlegging](assets/configure-af.png)



## Het formulier testen

Bekijk een voorbeeld van het formulier dat u in de vorige stap hebt gemaakt. Vul het formulier in en verzend het. De gegevens uit het formulier moeten worden ingevoegd in de SharePoint-lijst.
