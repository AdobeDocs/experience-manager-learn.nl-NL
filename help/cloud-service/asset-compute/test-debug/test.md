---
title: Een worker voor middelenberekening testen
description: Het project Asset Compute definieert een patroon waarmee u eenvoudig tests van Asset Compute-workers kunt maken en uitvoeren.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '631'
ht-degree: 0%

---


# Een worker voor middelenberekening testen

Het project Asset Compute definieert een patroon voor het eenvoudig maken en uitvoeren van [tests van Asset Compute Workers](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html).

## Anatomie van een arbeiderstest

De tests van de arbeiders van de Verwerking van activa worden gebroken in testreeksen, en binnen elke testreeks, één of meerdere testgevallen die een voorwaarde aan test bevestigen.

De structuur van tests in een project Asset Compute is als volgt:

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
   + Optioneel, wordt standaard de `diff` opdracht
   + Een shellscript gebruiken dat een docker-uitvoeropdracht verpakt voor het gebruik van verschillende validatiegereedschappen
+ `mock-<host-name>.json`
   + JSON heeft HTTP-reacties opgemaakt voor het [activeren van externe serviceaanroepen](https://www.mock-server.com/mock_server/creating_expectations.html).
   + Optioneel, alleen gebruikt als de code van de worker een eigen HTTP-aanvraag uitvoert

## Een testcase schrijven

Deze testcase stelt de parameterized input (`params.json`) voor het inputdossier (`file.jpg`)) toe produceert de verwachte PNG vertoning (`rendition.png`).

1. Verwijder eerst de automatisch gegenereerde `simple-worker` testcase op `/test/asset-compute/simple-worker` omdat deze ongeldig is, omdat onze worker de bron niet langer gewoon naar de uitvoering kopieert.
1. Maak een nieuwe testhoofdmap op `/test/asset-compute/worker/success-parameterized` om een geslaagde uitvoering te testen van de worker die een PNG-uitvoering genereert.
1. Voeg in de `success-parameterized` map het testinvoerbestand [voor deze testcase](./assets/test/success-parameterized/file.jpg) toe en noem deze `file.jpg`.
1. Voeg in de `success-parameterized` map een nieuw bestand toe met de naam `params.json` waarmee de invoerparameters van de worker worden gedefinieerd:

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```
   Dit zijn dezelfde sleutel/waarden die worden doorgegeven aan de definitie [van het Asset Compute-profiel van het](../develop/development-tool.md)Development Tool, min de `worker` sleutel.
1. Voeg het verwachte [vertoningsbestand](./assets/test/success-parameterized/rendition.png) toe aan deze testcase en noem deze `rendition.png`. Dit bestand vertegenwoordigt de verwachte uitvoer van de worker voor de opgegeven invoer `file.jpg`.
1. Van de bevellijn, stel de tests in werking de projectwortel door uit te voeren `aio app test`
   + Zorg ervoor dat [Docker Desktop](../set-up/development-environment.md#docker) en ondersteunende Docker-images zijn geïnstalleerd en gestart
   + Beëindig alle actieve instanties van het Hulpmiddel voor Ontwikkeling

![Testen - Voltooid ](./assets/test/success-parameterized/result.png)

## Een testcase voor foutcontrole schrijven

Deze testcase test om te controleren of de worker de juiste fout genereert wanneer de `contrast` parameter op een ongeldige waarde is ingesteld.

1. Maak een nieuwe testhoofdmap bij `/test/asset-compute/worker/error-contrast` om een foutuitvoering van de worker te testen vanwege een ongeldige `contrast` parameterwaarde.
1. Voeg in de `error-contrast` map het testinvoerbestand [voor deze testcase](./assets/test/error-contrast/file.jpg) toe en noem deze `file.jpg`. De inhoud van dit bestand is niet van belang voor deze test. Het bestand moet alleen bestaan om voorbij de controle &quot;Corrupt source&quot; te komen, zodat de `rendition.instructions` geldigheidscontroles kunnen worden uitgevoerd die door dit testgeval worden gevalideerd.
1. Voeg in de `error-contrast` map een nieuw bestand toe met de naam `params.json` dat de invoerparameters van de worker met de inhoud definieert:

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Stel `contrast` parameters in op `10`, een ongeldige waarde als contrast tussen -1 en 1 moet zijn om een `RenditionInstructionsError`waarde te genereren.
   + Bevestig dat de juiste fout in tests wordt gegenereerd door de `errorReason` sleutel in te stellen op de &quot;reden&quot; die aan de verwachte fout is gekoppeld. Deze ongeldige contrastparameter genereert de [aangepaste fout](../develop/worker.md#errors), `RenditionInstructionsError`stelt de fout daarom in `errorReason` op de reden van deze fout of`rendition_instructions_error` beweert dat deze is gegenereerd.

1. Aangezien er geen uitvoering moet worden gegenereerd tijdens een foutieve uitvoering, is er geen `rendition.<extension>` bestand nodig.
1. Voer de testsuite vanuit de hoofdmap van het project uit door de opdracht uit te voeren `aio app test`
   + Zorg ervoor dat [Docker Desktop](../set-up/development-environment.md#docker) en ondersteunende Docker-images zijn geïnstalleerd en gestart
   + Beëindig alle actieve instanties van het Hulpmiddel voor Ontwikkeling

![Testen - Foutcontrast](./assets/test/error-contrast/result.png)

## Testen op Github

De laatste testcase is beschikbaar op Github op:

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Problemen oplossen

+ [Geen uitvoering gegenereerd tijdens de uitvoering van de test](../troubleshooting.md#test-no-rendition-generated)
+ [Test genereert onjuiste uitvoering](../troubleshooting.md#tests-generates-incorrect-rendition)
