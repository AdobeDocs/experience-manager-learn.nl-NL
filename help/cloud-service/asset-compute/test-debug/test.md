---
title: Een Asset compute-worker testen
description: Het project van de Asset compute bepaalt een patroon voor gemakkelijk het creëren van en het uitvoeren van tests van de arbeiders van de Asset compute.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 04992caf-b715-4701-94a8-6257e9bd300c
duration: 175
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# Een Asset compute-worker testen

Het project van de Asset compute bepaalt een patroon voor gemakkelijk het creëren en uitvoeren [tests van Asset compute werknemers](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html).

## Anatomie van een arbeiderstest

De tests van de werknemers van de asset compute worden opgesplitst in testreeksen, en binnen elke testreeks, één of meerdere testgevallen die een voorwaarde aan test bevestigen.

De opzet van de tests in een Asset compute is als volgt:

```
/actions/<worker-name>/index.js
...
/test/
  asset-compute/
    <worker-name>/           <--- Test suite for the worker, must match the yaml key for this worker in manifest.yml
        <test-case-1>/       <--- Specific test case 
            file.jpg         <--- Input file (ie. `source.path` or `source.url`)
            params.json      <--- Parameters (ie. `rendition.instructions`)
            rendition.png    <--- Expected output file (ie. `rendition.path`)
        <test-case-2>/       <--- Another specific test case for this worker
            ...
```

Elke testgietvorm kan de volgende dossiers hebben:

+ `file.<extension>`
   + Bronbestand dat moet worden getest (extensie kan alles behalve `.link`)
   + Vereist
+ `rendition.<extension>`
   + Uitvoering verwacht
   + Vereist, behalve voor fouttests
+ `params.json`
   + JSON-instructies voor één uitvoering
   + Optioneel
+ `validate`
   + Een script dat de verwachte en werkelijke paden van het weergavebestand als argumenten krijgt en afsluitcode 0 retourneert als het resultaat OK is, of een afsluitcode die niet gelijk is aan nul als de validatie of vergelijking mislukt.
   + Optioneel, standaard ingesteld op `diff` command
   + Een shellscript gebruiken dat een docker-uitvoeropdracht verpakt voor het gebruik van verschillende validatiegereedschappen
+ `mock-<host-name>.json`
   + HTTP-reacties met JSON-indeling voor [het controleren van externe de dienstvraag](https://www.mock-server.com/mock_server/creating_expectations.html).
   + Optioneel, alleen gebruikt als de code van de worker een eigen HTTP-aanvraag uitvoert

## Een testcase schrijven

In dit testgeval wordt de geparametereerde invoer (`params.json`) voor het invoerbestand (`file.jpg`) genereert de verwachte PNG-uitvoering (`rendition.png`).

1. Verwijder eerst de automatisch gegenereerde `simple-worker` test `/test/asset-compute/simple-worker` omdat dit ongeldig is, omdat onze worker de bron niet langer gewoon naar de uitvoering kopieert.
1. Maak een nieuwe testhoofdmap op `/test/asset-compute/worker/success-parameterized` om een geslaagde uitvoering te testen van de worker die een PNG-uitvoering genereert.
1. In de `success-parameterized` map, de test toevoegen [invoerbestand](./assets/test/success-parameterized/file.jpg) voor deze testcase en noem deze `file.jpg`.
1. In de `success-parameterized` map, voeg een nieuw bestand toe met de naam `params.json` waarmee de invoerparameters van de worker worden gedefinieerd:

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   Dit zijn dezelfde sleutel/waarden die aan de [Definitie van Asset compute-profiel van het ontwikkelingsgereedschap](../develop/development-tool.md), met minder `worker` toets.

1. Voeg de verwachte [weergavebestand](./assets/test/success-parameterized/rendition.png) naar deze testcase en noem deze `rendition.png`. Dit bestand vertegenwoordigt de verwachte uitvoer van de worker voor de opgegeven invoer `file.jpg`.
1. Van de bevellijn, stel de tests in werking de projectwortel door uit te voeren `aio app test`
   + Zorgen [Docker Desktop](../set-up/development-environment.md#docker) en ondersteunende Docker-afbeeldingen worden geïnstalleerd en gestart
   + Beëindig alle actieve instanties van het Hulpmiddel voor Ontwikkeling

![Testen - Voltooid ](./assets/test/success-parameterized/result.png)

## Een testcase voor foutcontrole schrijven

Deze testcase test om te controleren of de worker de juiste fout genereert als de `contrast` parameter is ingesteld op een ongeldige waarde.

1. Maak een nieuwe testhoofdmap op `/test/asset-compute/worker/error-contrast` om een foutieve uitvoering van de worker te testen wegens een ongeldige `contrast` parameterwaarde.
1. In de `error-contrast` map, de test toevoegen [invoerbestand](./assets/test/error-contrast/file.jpg) voor deze testcase en noem deze `file.jpg`. De inhoud van dit bestand is niet van belang voor deze test. Het bestand moet alleen bestaan om voorbij de controle &quot;Beschadigde bron&quot; te komen, zodat de `rendition.instructions` geldigheidscontroles, die door deze test worden gevalideerd.
1. In de `error-contrast` map, voeg een nieuw bestand toe met de naam `params.json` die de invoerparameters van de worker met de inhoud definieert:

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Set `contrast` parameters voor `10`, een ongeldige waarde, aangezien het contrast tussen -1 en 1 moet zijn, om een `RenditionInstructionsError`.
   + Ervan uitgaande dat de juiste fout in tests wordt gegenereerd door het instellen van de optie `errorReason` sleutel naar de &quot;reden&quot; die aan de verwachte fout is gekoppeld. Deze ongeldige contrastparameter genereert de [aangepaste fout](../develop/worker.md#errors), `RenditionInstructionsError`stelt derhalve de `errorReason` om de reden van deze fout, of`rendition_instructions_error` om aan te tonen dat het is gegenereerd.

1. Aangezien er geen uitvoering moet worden gegenereerd tijdens een foutieve uitvoering, kan `rendition.<extension>` bestand is noodzakelijk.
1. Voer de testsuite vanuit de hoofdmap van het project uit door de opdracht uit te voeren `aio app test`
   + Zorgen [Docker Desktop](../set-up/development-environment.md#docker) en ondersteunende Docker-afbeeldingen worden geïnstalleerd en gestart
   + Beëindig alle actieve instanties van het Hulpmiddel voor Ontwikkeling

![Testen - Foutcontrast](./assets/test/error-contrast/result.png)

## Testen op Github

De laatste testcase is beschikbaar op Github op:

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Problemen oplossen

+ [Geen uitvoering gegenereerd tijdens de uitvoering van de test](../troubleshooting.md#test-no-rendition-generated)
+ [Test genereert onjuiste uitvoering](../troubleshooting.md#tests-generates-incorrect-rendition)
