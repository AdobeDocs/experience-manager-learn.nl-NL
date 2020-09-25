---
title: Batch-API gebruiken voor het genereren van interactieve communicatiedocumenten
description: Voorbeeldelementen voor het genereren van documenten voor afdrukkanalen met batch-API
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
translation-type: tm+mt
source-git-commit: 3dc1bd3f2f7b6324c53640f01a263fa0728d439c
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 0%

---


# Batch-API

U kunt de batch-API gebruiken om meerdere interactieve communicatie van een sjabloon te maken. De sjabloon is een interactieve communicatie zonder gegevens. De batch-API combineert gegevens met een sjabloon voor interactieve communicatie. De API is nuttig bij de massaproductie van interactieve communicatie. Bijvoorbeeld telefoonrekeningen, creditcardoverzichten voor meerdere klanten.

[Meer informatie over de API voor het genereren van batches](https://docs.adobe.com/content/help/en/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

Dit artikel bevat voorbeeldelementen voor het genereren van interactieve communicatiedocumenten met de Batch-API.

## Batchgeneratie met gecontroleerde map

* Importeer de sjabloon [](assets/Beneficiaries-confirmation.zip) Interactieve communicatie naar uw AEM Forms-server.
* Importeer de [controlemapconfiguratie](assets/batch-generation-api.zip). Hiermee wordt een map gemaakt die `batchAPI` in het station C wordt aangeroepen.

**Als u AEM Forms uitvoert op een ander besturingssysteem dan Windows, voert u de volgende drie stappen uit:**

1. [Gecontroleerde map openen](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. Selecteer BatchAPIWatchedFolder en klik op Bewerken.
3. Wijzig het pad zodat dit overeenkomt met uw besturingssysteem.

![path](assets/watched-folder-batch-api-basic.PNG)

* Download en extraheer de inhoud van het [ZIP-bestand](assets/jsonfile.zip). Het ZIP-bestand bevat een map met de naam `jsonfile` die het `beneficiaries.json` bestand bevat. Dit bestand bevat de gegevens die moeten worden gegenereerd in 3 documenten.

* Zet de `jsonfile` map neer in de invoermap van de gecontroleerde map.
* Als de map is opgepikt voor verwerking, controleert u de resultatenmap van de gecontroleerde map. 3 PDF-bestanden genereren

## Batchgeneratie met REST-verzoeken

U kunt de [Batch-API](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html) activeren via REST-aanvragen. U kunt REST-eindpunten voor andere toepassingen toegankelijk maken om de API aan te roepen om documenten te genereren.
De verstrekte steekproefactiva stellen REST eindpunt voor het produceren van Interactieve Communicatie documenten bloot. De servlet accepteert de volgende parameters:

* fileName - Locatie van het gegevensbestand op het dossiersysteem.
* templatePath - IC-sjabloonpad
* saveLocation - Locatie om de geproduceerde documenten op het dossiersysteem op te slaan
* channelType - Print,Web of beide
* recordId - JSON-pad naar element om naam in te stellen voor een interactieve communicatie

De volgende schermafbeelding toont de parameters en de voorbeeldaanvraag voor de waarden![ervan](assets/generate-ic-batch-servlet.PNG)

## Stel steekproefactiva op uw server op

* ICT- [sjabloon](assets/ICTemplate.zip) importeren met [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* Handler [Aangepast verzenden](assets/BatchAPICustomSubmit.zip) importeren met [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* Adaptief [formulier](assets/BatchGenerationAPIAF.zip) importeren met de interface [Forms en Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Aangepaste OSGI-bundel [implementeren en starten met](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) [Felix-webconsole](http://localhost:4502/system/console/bundles)
* [Batchgeneratie activeren door het formulier te verzenden](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
