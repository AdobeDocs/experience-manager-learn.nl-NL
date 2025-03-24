---
title: Problemen met Asset Compute-uitbreidbaarheid voor AEM Assets oplossen
description: Hieronder volgt een index met veelvoorkomende problemen en fouten, samen met de resoluties, die kunnen optreden bij het ontwikkelen en implementeren van aangepaste Asset Compute-workers voor AEM Assets.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
duration: 260
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1218'
ht-degree: 0%

---

# Problemen met de uitbreidbaarheid van Asset Compute oplossen

Hieronder volgt een index met veelvoorkomende problemen en fouten, samen met de resoluties, die kunnen optreden bij het ontwikkelen en implementeren van aangepaste Asset Compute-workers voor AEM Assets.

## Ontwikkelen{#develop}

### Vertoning wordt gedeeltelijk getekend/beschadigd geretourneerd{#rendition-returned-partially-drawn-or-corrupt}

+ __Fout__: Vertoning geeft volledig (wanneer een beeld) terug of is corrupt en kan niet worden geopend.

  ![ de Vertoning is gedeeltelijk teruggegeven getekend ](./assets/troubleshooting/develop__await.png)

+ __Oorzaak__: De functie van de worker `renditionCallback` bestaat alvorens de vertoning volledig aan `rendition.path` kan worden geschreven.
+ __Resolutie__: Herzie de code van de douanearbeider en zorg ervoor alle asynchrone vraag synchroon wordt gemaakt gebruikend `await`.

## Ontwikkelingsinstrument{#development-tool}

### Het bestand Console.json ontbreekt in het Asset Compute-project{#missing-console-json}

+ __Fout:__ Fout: Ontbrekende vereiste dossiers bij bevestigt (`.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY`) bij async setupAssetCompute (`.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY`)
+ __Oorzaak:__ het `console.json` dossier mist van de wortel van het project van Asset Compute
+ __Resolutie:__ Download een nieuw `console.json` van uw project van Adobe I/O
   1. Open in console.adobe.io het Adobe I/O-project waarvoor het Asset Compute-project is geconfigureerd
   1. Tik de __knoop van de Download__ in het hoogste recht
   1. Sla het gedownloade bestand met de bestandsnaam op in de hoofdmap van het Asset Compute-project `console.json`

### Onjuiste YAML-inspringing in manifest.yml{#incorrect-yaml-indentation}

+ __Fout:__ YAMLException: slechte inkeping van een afbeeldingsingang bij lijn X, kolom Y: (via standaard uit `aio app run` bevel)
+ __Oorzaak:__ Yaml de dossiers zijn gevoelig wit-uit elkaar geplaatst, het waarschijnlijk dat uw inkeping onjuist is.
+ __Resolutie:__ herzie uw `manifest.yml` en zorg ervoor al inspringing correct is.

### memorySize limit is set to low{#memorysize-limit-is-set-too-low}

+ __Fout:__ Lokale Dev Server OpenWhiskError: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true Geretourneerde HTTP 400 (Slecht Verzoek) —> &quot;De verzoekinhoud werd misvormd:vereiste ontbrak: geheugen 64 MB onder toegestane drempel van 134217728 B&quot;
+ __Oorzaak:__ A `memorySize` grens voor de worker in `manifest.yml` werd geplaatst onder de minimum toegestane drempel zoals die door het foutenbericht in bytes wordt gemeld.
+ __Resolutie:__ herzie de `memorySize` grenzen in `manifest.yml` en zorg ervoor zij allen groot zijn dan de minimaal toegestane drempel.

### Development Tool kan niet worden gestart omdat private.key ontbreekt{#missing-private-key}

+ __Fout:__ Lokale Dev ServerError: Ontbrekende vereiste dossiers bij validatePrivateKeyFile... (via standaard uit `aio app run` opdracht)
+ __Oorzaak:__ de `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` waarde in `.env` dossier, richt niet aan `private.key` of `private.key` is niet leesbaar door de huidige gebruiker.
+ __Resolutie:__ herzie de `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` waarde in `.env` dossier, en zorg ervoor het de volledige, absolute weg aan `private.key` op uw dossiersysteem bevat.

### Vervolgkeuzelijst Source-bestanden is onjuist{#source-files-dropdown-incorrect}

Het Hulpmiddel van de Ontwikkeling van Asset Compute kan een staat ingaan waar het stapelgegevens trekt, en is het meest merkbaar in het __dossier van Source__ dropdown tonend onjuiste punten.

+ __Fout:__ het dossierdropdown van Source toont onjuiste punten.
+ __Oorzaak:__ de de browser van de Stale in het voorgeheugen ondergebrachte staat veroorzaakt
+ __Resolutie:__ in uw browser ontruimen volledig de browser &quot;toepassingsstaat van het lusje&quot;, het browser geheime voorgeheugen, lokale opslag en de dienstarbeider.

### Ontbrekende of ongeldige devToolToken-queryparameter{#missing-or-invalid-devtooltoken-query-parameter}

+ __Fout:__ &quot;Onbevoegd&quot;bericht in het Hulpmiddel van de Ontwikkeling van Asset Compute
+ __Oorzaak:__ `devToolToken` mist of ongeldig
+ __Resolutie:__ sluit het browser venster van het Hulpmiddel van de Ontwikkeling van Asset Compute, beëindigt om het even welke lopende processen van het Hulpmiddel van de Ontwikkeling die via het `aio app run` bevel worden in werking gesteld, en herstart het Hulpmiddel van de Ontwikkeling (gebruikend `aio app run`).

### Kan bronbestanden niet verwijderen{#unable-to-remove-source-files}

+ __Fout:__ Er is geen manier om toegevoegde brondossiers uit de UI van Hulpmiddelen van de Ontwikkeling te verwijderen
+ __Oorzaak:__ Deze functionaliteit is niet uitgevoerd
+ __Resolutie:__ Login uw leverancier van de wolkenopslag gebruikend de geloofsbrieven die in `.env` worden bepaald. Bepaal de plaats van de container die door de Hulpmiddelen van de Ontwikkeling (ook in `.env` wordt gespecificeerd) wordt gebruikt, navigeer in de __bron__ omslag, en schrap om het even welke bronbeelden die. U kunt de stappen moeten uitvoeren die in [ worden geschetst Source dossiers dropdown onjuist ](#source-files-dropdown-incorrect) als de geschrapte brondossiers in dropdown blijven tonen aangezien zij plaatselijk in de &quot;toepassingsstaat van de Ontwikkeling hulpmiddelen&quot;kunnen worden in het voorgeheugen ondergebracht.

  ![ Microsoft Azure Blob Storage ](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Testen{#test}

### Geen uitvoering gegenereerd tijdens de uitvoering van de test{#test-no-rendition-generated}

+ __Fout:__ Mislukking: Geen geproduceerde vertoning.
+ __Oorzaak:__ de worker slaagde erin om een vertoning te produceren wegens een onverwachte fout zoals een de syntaxisfout van JavaScript.
+ __Resolutie:__ herzie de 2} van de testuitvoering bij `/build/test-results/test-worker/test.log`. `test.log` Zoek de sectie in dit bestand die overeenkomt met de testcase voor mislukken en controleer of er fouten zijn opgetreden.

  ![ het Oplossen van problemen - Geen vertoning produceerde ](./assets/troubleshooting/test__no-rendition-generated.png)

### Test genereert onjuiste uitvoering, waardoor de test mislukt{#tests-generates-incorrect-rendition}

+ __Fout:__ Mislukking: Vertoning &quot;rendition.xxx&quot;niet zoals verwacht.
+ __Oorzaak:__ de arbeidersoutput een vertoning die niet het zelfde als `rendition.<extension>` in het testgeval verstrekte was.
   + Als het verwachte `rendition.<extension>` -bestand niet op dezelfde manier wordt gemaakt als de lokaal gegenereerde uitvoering in het testgeval, kan de test mislukken omdat er een verschil in de bits kan zijn. Als de Asset Compute-worker bijvoorbeeld het contrast wijzigt met behulp van API&#39;s en het verwachte resultaat wordt bereikt door het contrast aan te passen in Adobe Photoshop CC, kunnen de bestanden er hetzelfde uitzien, maar kleine variaties in de bits kunnen verschillen.
+ __Resolutie:__ de uitvoeringen van de vertoning van de Overzicht van de test door aan `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>` te navigeren, en het te vergelijken met het verwachte vertoningsdossier in het testgeval. Om een exact verwacht actief te maken:
   + Gebruik het gereedschap Ontwikkeling om een vertoning te genereren, te valideren dat deze correct is en te gebruiken als het verwachte vertoningsbestand
   + U kunt ook het bestand dat tijdens de test is gegenereerd op `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>` valideren, controleren of het bestand juist is en gebruiken als het verwachte renderingsbestand

## Foutopsporing

### Foutopsporing wordt niet gekoppeld{#debugger-does-not-attach}

+ __Fout__: De verwerkingslancering van de fout: Fout: Kon niet verbinden met zuiveren doel bij..
+ __Oorzaak__: De Desktop van de Docker loopt niet op het lokale systeem. Verifieer dit door de Console van de Foutopsporing van de Code van VS (Mening > Debug Console) te herzien, bevestigend deze fout wordt gemeld.
+ __Resolutie__: De Desktop van het Begin [ van de Dokker en bevestigt de vereiste beelden van de Dokker worden geïnstalleerd ](./set-up/development-environment.md#docker).

### Onderbrekingspunten worden niet gepauzeerd{#breakpoints-no-pausing}

+ __Fout__: Wanneer het runnen van de worker van Asset Compute van het zuivert-able Hulpmiddel van de Ontwikkeling, pauzeert de Code van VS niet bij breekpunten.

#### Foutopsporing VS-code niet gekoppeld{#vs-code-debugger-not-attached}

+ __Oorzaak:__ debugger van de Code van VS werd tegengehouden/losgemaakt.
+ __Resolutie:__ herstart foutopsporing van de Code van VS, en verifieert het door de console van de Output van de Output van de Code van VS te bekijken zuivert (Mening > zuivert Console)

#### Foutopsporing voor VS-code gekoppeld nadat uitvoering van worker is gestart{#vs-code-debugger-attached-after-worker-execution-began}

+ __Oorzaak:__ de debugger van de Code van VS maakte niet vast voorafgaand aan het tappen __Looppas__ in het Hulpmiddel van de Ontwikkeling.
+ __Resolutie:__ verzeker debugger door de Foutopsporingsconsole van de Code van VS te herzien (Mening > zuivert Console), en dan de worker van Asset Compute van het Hulpmiddel van de Ontwikkeling opnieuw in werking te stellen.

### Worker-time-out tijdens foutopsporing{#worker-times-out-while-debugging}

+ __Fout__: Zuiver de rapporten van de Console &quot;Actie zal onderbreking in -XXX milliseconden&quot;of ](./develop/development-tool.md) de vertoningsvoorproef van de vertoningen van het Hulpmiddel van de Ontwikkeling van Asset Compute [ voor onbepaalde tijd of
+ __Oorzaak__: De arbeidersonderbreking zoals bepaald in [ manifest.yml ](./develop/manifest.md) wordt overschreden tijdens het zuiveren.
+ __Resolutie__: Verhoog tijdelijk de onderbreking van de worker in [ manifest.yml ](./develop/manifest.md) of versnelt het zuiveren activiteiten.

### Kan foutopsporingsproces niet beëindigen{#cannot-terminate-debugger-process}

+ __Fout__: `Ctrl-C` op de bevellijn beëindigt niet het debugger proces (`npx adobe-asset-compute devtool`).
+ __Oorzaak__: Een insect in `@adobe/aio-cli-plugin-asset-compute` 1.3.x, resulteert in `Ctrl-C` die niet als beëindigend bevel wordt erkend.
+ __Resolutie__: Update `@adobe/aio-cli-plugin-asset-compute` aan versie 1.4.1+

  ```
  $ aio update
  ```

  ![ het Oplossen van problemen - de update van het a-gebouw ](./assets/troubleshooting/debug__terminate.png)

## Implementeren{#deploy}

### Aangepaste uitvoering ontbreekt in element in AEM{#custom-rendition-missing-from-asset}

+ __Fout:__ Nieuwe en opnieuw verwerkte activa verwerken met succes, maar missen de douanevertoning

#### Profiel verwerken dat niet is toegepast op de bovenliggende map

+ __Oorzaak:__ het element bestaat niet onder een omslag met het Profiel van de Verwerking dat de douanearbeider gebruikt
+ __Resolutie:__ pas het Profiel van de Verwerking op een voorouderomslag van de activa toe

#### Bezig met verwerken van profiel vervangen door lager verwerkingsprofiel

+ __Oorzaak:__ het middel bestaat onder een omslag met het toegepaste Profiel van de douanearbeidersverwerking, nochtans een verschillend Profiel van de Verwerking dat niet de klantenarbeider gebruikt is toegepast tussen die omslag en het middel.
+ __Resolutie:__ combineer, of op een andere manier, de twee Profielen van de Verwerking en verwijder het middenProfiel van de Verwerking

### Verwerking van middelen mislukt in AEM{#asset-processing-fails}

+ __Fout:__ Ontbroken symbool van de Verwerking van Activa dat op activa wordt getoond
+ __Oorzaak:__ een fout kwam in de uitvoering van de douanearbeider voor
+ __Resolutie:__ volg de instructies op [ het zuiveren Adobe I/O Runtime actities ](./test-debug/debug.md#aio-app-logs) gebruikend `aio app logs`.
