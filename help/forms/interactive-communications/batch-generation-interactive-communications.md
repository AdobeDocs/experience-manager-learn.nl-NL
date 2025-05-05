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

[ Leer meer op de Generatie API van de Partij ](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html?lang=nl-NL)

Dit artikel bevat voorbeeldelementen voor het genereren van interactieve communicatiedocumenten met de Batch-API.

## Batchgeneratie met gecontroleerde map

* Importeer het [ Interactieve Communicatie malplaatje ](assets/Beneficiaries-confirmation.zip) in uw server van AEM Forms.
* Importeer de [ gecontroleerde omslagconfiguratie ](assets/batch-generation-api.zip). Hiermee maakt u een map met de naam `batchAPI` in het station C.

**als u AEM Forms op niet-vensters werkend systeem in werking stelt, gelieve de 3 hieronder vermelde stappen te volgen:**

1. [ Open gecontroleerde omslag ](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. Selecteer BatchAPIWatchedFolder en klik uitgeven.
3. Wijzig het pad zodat dit overeenkomt met uw besturingssysteem.

![ weg ](assets/watched-folder-batch-api-basic.PNG)

* De download en haalt de inhoud van [ zip dossier ](assets/jsonfile.zip). Het ZIP-bestand bevat de map `jsonfile` die het `beneficiaries.json` -bestand bevat. Dit bestand bevat de gegevens die moeten worden gegenereerd in 3 documenten.

* Zet de map `jsonfile` neer in de invoermap van de gecontroleerde map.
* Als de map is opgepikt voor verwerking, controleert u de resultatenmap van de gecontroleerde map. Er worden 3 PDF-bestanden gegenereerd

## Batchgeneratie met REST-verzoeken

U kunt de [ Partij API ](https://helpx.adobe.com/nl/experience-manager/6-5/forms/javadocs/index.html) door REST verzoeken aanhalen. U kunt REST-eindpunten voor andere toepassingen toegankelijk maken om de API aan te roepen om documenten te genereren.
De verstrekte steekproefactiva stellen REST eindpunt voor het produceren van Interactieve Communicatie documenten bloot. De servlet accepteert de volgende parameters:

* fileName - Locatie van het gegevensbestand op het dossiersysteem.
* templatePath - IC-sjabloonpad
* saveLocation - Locatie om de geproduceerde documenten op het dossiersysteem op te slaan
* channelType - Print,Web of beide
* recordId - JSON-pad naar element om de naam van een interactieve communicatie in te stellen

De volgende schermafbeelding toont de parameters en de bijbehorende waarden
![ steekproefverzoek ](assets/generate-ic-batch-servlet.PNG)

## Stel steekproefactiva op uw server op

* De invoer [ ICTemplate ](assets/ICTemplate.zip) gebruikend [ pakketmanager ](http://localhost:4502/crx/packmgr/index.jsp)
* De invoer [ Verzenden van de Douane manager ](assets/BatchAPICustomSubmit.zip) gebruikend [ pakketmanager ](http://localhost:4502/crx/packmgr/index.jsp)
* De Invoer [ Aangepaste Vorm ](assets/BatchGenerationAPIAF.zip) gebruikend de [ interface van Forms en van het Document ](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Stel en begin [ de bundel van Aangepaste OSGI ](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) op gebruikend [ de Webconsole van de Felix ](http://localhost:4502/system/console/bundles)
* [ de Generatie van de Batch van de Trekker door de vorm ](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled) voor te leggen
