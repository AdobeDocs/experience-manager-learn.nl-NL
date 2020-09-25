---
title: AEM maken
description: AEM workflowmodel maken met AEM Forms-workflowcomponenten om verzonden gegevens te controleren.
sub-product: formulieren
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 4271
thumbnail: kt-4271.jpg
translation-type: tm+mt
source-git-commit: 738e356c4e72e0c3518bb5fd4ad6a076522e9f5c
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 0%

---


# Workflow maken om verzonden gegevens te controleren

De werkschema&#39;s worden typisch gebruikt om voorgelegde gegevens voor overzicht en goedkeuring te leiden. Workflows worden gemaakt met de werkstroomeditor in AEM. De workflows kunnen worden geactiveerd bij het verzenden van een adaptief formulier. De volgende stappen zullen u door het proces leiden om uw eerste werkschema tot stand te brengen.

## Vereiste

Controleer of je een werkende versie van AEM Forms hebt. Volg de [installatiehandleiding](https://docs.adobe.com/content/help/en/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) om AEM Forms te installeren en configureren


## Workflowmodel maken

* [Workflowmodellen openen](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
* Klik op Maken -> Model maken
* Geef een betekenisvolle titel en naam op, zoals Gegevens die zijn verzonden door _Revisie_.
* Tik zachtjes op de nieuwe workflow en klik op het pictogram _Bewerken_ .
* De workflow wordt geopend in de bewerkingsmodus. De workflow heeft standaard één component met de naam _Step1_. Selecteer deze component en klik op het pictogram Verwijderen om de component te verwijderen.
* Links ziet u de verschillende workflowcomponenten waarmee u uw workflow kunt samenstellen. U kunt de componenten filteren op type _Forms Workflow_ .

## Variabele maken

* Klik op het pictogram van de variabele om nieuwe variabelen tot stand te brengen. De variabelen worden gebruikt om waarden op te slaan. AEM Forms biedt een aantal typen variabelen die kunnen worden gemaakt. Vandaag zullen wij een variabele van type XML tot stand brengen om de verzonden gegevens van het Aangepaste Vorm te houden. Maak een nieuwe variabele met de naam _submitData_ van het type XML, zoals in de onderstaande afbeelding wordt getoond.

   >[!NOTE]
Als het formulier is gebaseerd op het formuliergegevensmodel, hebben de verzonden gegevens de JSON-indeling. In dat geval maakt u een variabele van het type JSON die de verzonden gegevens bevat.

![ingediende gegevensvariabele](assets/submitted-data-variable.PNG)

* Klik op het pictogram met de _stappen_ links om de verschillende workflowcomponenten weer te geven. Sleep de component Variabele __ instellen naar de workflow rechts in het scherm. Plaats de component Variabele __ instellen onder het begin van de stroom.
   * Klik op de component _Variabele_ instellen en klik vervolgens op het pictogram _Sleutel_ om het eigenschapblad van de component te openen.
   * Klik op het tabblad Toewijzing ->Toewijzing toevoegen->Variabele toewijzen. Stel de waarden in zoals weergegeven in de onderstaande schermafbeelding.
      ![variabele maken](assets/set-variable.PNG)

## Workflowcomponenten toevoegen

* Sleep de component _Taak_ toewijzen naar de rechterkant onder de component _Variabele_ instellen.
   * Klik op de _Assign component van de Taak_ en klik dan op het pictogram van de _Sleutel_ om het bezitsblad te openen.
   * Verstrek betekenende Titel aan de Assign component van de Taak.
   * Klik op het tabblad Forms en Documenten en stel de volgende eigenschappen in, zoals weergegeven in de schermafbeelding
      ![Tabblad Forms-documenten](assets/forms-documents.PNG)

   * _1 Als u deze optie selecteert, wordt de workflow niet gekoppeld aan een specifiek adaptief formulier._
   * _2 De workflowengine zoekt naar het bestand Data.xml in verhouding tot de lading in de repository_

   * Klik op het tabblad Toegewezen. Hier kunt u de taak aan een gebruiker in uw organisatie toewijzen. Voor dit gebruik zullen wij de taak aan admin gebruiker zoals aangetoond in het hieronder ontsproten scherm toewijzen.
      ![Tab van ontvanger](assets/assignee-tab.PNG)
   * Sla uw wijzigingen op door op het pictogram _Gereed_ van de component te klikken
* Klik op _Synchroniseren_ om het runtimemodel van de workflow te genereren.
Uw workflowmodel is nu gereed en kan worden gekoppeld aan de verzendactie van het Adaptief formulier.



