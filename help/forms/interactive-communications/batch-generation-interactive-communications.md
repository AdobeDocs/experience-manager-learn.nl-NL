---
title: Batch-API gebruiken voor het genereren van interactieve communicatiedocumenten
description: Voorbeeldelementen voor het genereren van documenten voor afdrukkanalen met batch-API
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---


# Batch-API

U kunt de batch-API gebruiken om meerdere interactieve communicatie van een sjabloon te maken. De sjabloon is een interactieve communicatie zonder gegevens. De batch-API combineert gegevens met een sjabloon voor interactieve communicatie. De API is nuttig bij de massaproductie van interactieve communicatie. Bijvoorbeeld telefoonrekeningen, creditcardoverzichten voor meerdere klanten.

[Meer informatie over de API voor het genereren van batches](https://docs.adobe.com/content/help/en/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

Dit artikel bevat voorbeeldelementen voor het genereren van interactieve communicatiedocumenten met de Batch-API.

## Batchgeneratie met gecontroleerde map

* Importeer de [Interactieve communicatiesjabloon](assets/Beneficiaries-confirmation.zip) naar uw AEM Forms-server.
* Importeer de [gecontroleerde mapconfiguratie](assets/batch-generation-api.zip). Hierdoor wordt een map met de naam `batchAPI` in uw C-station gemaakt.

**Als u AEM Forms uitvoert op een ander besturingssysteem dan Windows, voert u de volgende drie stappen uit:**

1. [Gecontroleerde map openen](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. Selecteer BatchAPIWatchedFolder en klik op Bewerken.
3. Wijzig het pad zodat dit overeenkomt met uw besturingssysteem.

![path](assets/watched-folder-batch-api-basic.PNG)

* Download en extraheer de inhoud van [zip file](assets/jsonfile.zip). Het ZIP-bestand bevat een map met de naam `jsonfile` die `beneficiaries.json` bestand bevat. Dit bestand bevat de gegevens die moeten worden gegenereerd in 3 documenten.

* Plaats de map `jsonfile` in de invoermap van de gecontroleerde map.
* Als de map is opgepikt voor verwerking, controleert u de resultatenmap van de gecontroleerde map. 3 PDF-bestanden genereren

## Batchgeneratie met REST-verzoeken

U kunt [Batch API](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html) door REST verzoeken aanhalen. U kunt REST-eindpunten voor andere toepassingen toegankelijk maken om de API aan te roepen om documenten te genereren.
De verstrekte steekproefactiva stellen REST eindpunt voor het produceren van Interactieve Communicatie documenten bloot. De servlet accepteert de volgende parameters:

* fileName - Locatie van het gegevensbestand op het dossiersysteem.
* templatePath - IC-sjabloonpad
* saveLocation - Locatie om de geproduceerde documenten op het dossiersysteem op te slaan
* channelType - Print,Web of beide
* recordId - JSON-pad naar element om naam in te stellen voor een interactieve communicatie

De volgende schermafbeelding toont de parameters en de bijbehorende waarden
![voorbeeldverzoek](assets/generate-ic-batch-servlet.PNG)

## Stel steekproefactiva op uw server op

* Importeer [ICTemplate](assets/ICTemplate.zip) met behulp van [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* Importeren [Aangepaste verzendhandler](assets/BatchAPICustomSubmit.zip) met [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* Importeer [Adaptief formulier](assets/BatchGenerationAPIAF.zip) met de [Forms- en Document-interface](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Aangepaste OSGI-bundel](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) implementeren en starten met [Felix-webconsole](http://localhost:4502/system/console/bundles)
* [Batchgeneratie activeren door het formulier te verzenden](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
