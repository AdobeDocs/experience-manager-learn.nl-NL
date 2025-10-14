---
title: Batch-API gebruiken voor het genereren van interactieve communicatiedocumenten
description: Voorbeeldelementen voor het genereren van documenten voor afdrukkanalen met batch-API
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 2cdf37e6-42ad-469a-a6e4-a693ab2ca908
last-substantial-update: 2019-07-07T00:00:00Z
duration: 77
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 0%

---

# Batch-API

U kunt de batch-API gebruiken om meerdere interactieve communicatie van een sjabloon te maken. De sjabloon is een interactieve communicatie zonder gegevens. De batch-API combineert gegevens met een sjabloon voor interactieve communicatie. De API is nuttig bij de massaproductie van interactieve communicatie. Bijvoorbeeld telefoonrekeningen, creditcardoverzichten voor meerdere klanten.

[&#x200B; Leer meer op de Generatie API van de Partij &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html?lang=nl-NL)

Dit artikel bevat voorbeeldelementen voor het genereren van interactieve communicatiedocumenten met de Batch-API.

## Batchgeneratie met gecontroleerde map

* Importeer het [&#x200B; Interactieve Communicatie malplaatje &#x200B;](assets/Beneficiaries-confirmation.zip) in uw server van AEM Forms.
* Importeer de [&#x200B; gecontroleerde omslagconfiguratie &#x200B;](assets/batch-generation-api.zip). Hiermee maakt u een map met de naam `batchAPI` in het station C.

**als u AEM Forms op niet-vensters werkend systeem in werking stelt, gelieve de 3 hieronder vermelde stappen te volgen:**

1. [&#x200B; Open gecontroleerde omslag &#x200B;](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. Selecteer BatchAPIWatchedFolder en klik uitgeven.
3. Wijzig het pad zodat dit overeenkomt met uw besturingssysteem.

![&#x200B; weg &#x200B;](assets/watched-folder-batch-api-basic.PNG)

* De download en haalt de inhoud van [&#x200B; zip dossier &#x200B;](assets/jsonfile.zip). Het ZIP-bestand bevat de map `jsonfile` die het `beneficiaries.json` -bestand bevat. Dit bestand bevat de gegevens die moeten worden gegenereerd in 3 documenten.

* Zet de map `jsonfile` neer in de invoermap van de gecontroleerde map.
* Als de map is opgepikt voor verwerking, controleert u de resultatenmap van de gecontroleerde map. Er worden 3 PDF-bestanden gegenereerd

## Batchgeneratie met REST-verzoeken

U kunt de [&#x200B; Partij API &#x200B;](https://helpx.adobe.com/nl/experience-manager/6-5/forms/javadocs/index.html) door REST verzoeken aanhalen. U kunt REST-eindpunten voor andere toepassingen toegankelijk maken om de API aan te roepen om documenten te genereren.
De verstrekte steekproefactiva stellen REST eindpunt voor het produceren van Interactieve Communicatie documenten bloot. De servlet accepteert de volgende parameters:

* fileName - Locatie van het gegevensbestand op het dossiersysteem.
* templatePath - IC-sjabloonpad
* saveLocation - Locatie om de geproduceerde documenten op het dossiersysteem op te slaan
* channelType - Print,Web of beide
* recordId - JSON-pad naar element om de naam van een interactieve communicatie in te stellen

De volgende schermafbeelding toont de parameters en de bijbehorende waarden
![&#x200B; steekproefverzoek &#x200B;](assets/generate-ic-batch-servlet.PNG)

## Stel steekproefactiva op uw server op

* De invoer [&#x200B; ICTemplate &#x200B;](assets/ICTemplate.zip) gebruikend [&#x200B; pakketmanager &#x200B;](http://localhost:4502/crx/packmgr/index.jsp)
* De invoer [&#x200B; Verzenden van de Douane manager &#x200B;](assets/BatchAPICustomSubmit.zip) gebruikend [&#x200B; pakketmanager &#x200B;](http://localhost:4502/crx/packmgr/index.jsp)
* De Invoer [&#x200B; Aangepaste Vorm &#x200B;](assets/BatchGenerationAPIAF.zip) gebruikend de [&#x200B; interface van Forms en van het Document &#x200B;](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Stel en begin [&#x200B; de bundel van Aangepaste OSGI &#x200B;](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) op gebruikend [&#x200B; de Webconsole van de Felix &#x200B;](http://localhost:4502/system/console/bundles)
* [&#x200B; de Generatie van de Batch van de Trekker door de vorm &#x200B;](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled) voor te leggen
