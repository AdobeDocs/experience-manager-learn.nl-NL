---
title: Problemen met uitbreidbaarheid van Asset compute voor AEM Assets oplossen
description: Hieronder ziet u een index met veelvoorkomende problemen en fouten, samen met de resoluties, die kunnen optreden bij het ontwikkelen en implementeren van aangepaste Asset compute-workers voor AEM Assets.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
duration: 355
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1230'
ht-degree: 0%

---

# Problemen met uitbreidbaarheid van Asset compute oplossen

Hieronder ziet u een index met veelvoorkomende problemen en fouten, samen met de resoluties, die kunnen optreden bij het ontwikkelen en implementeren van aangepaste Asset compute-workers voor AEM Assets.

## Ontwikkelen{#develop}

### Vertoning wordt gedeeltelijk getekend/beschadigd geretourneerd{#rendition-returned-partially-drawn-or-corrupt}

+ __Fout__: Vertoning wordt onvolledig gerenderd (als een afbeelding beschadigd is) of kan niet worden geopend.

  ![Vertoning wordt gedeeltelijk getekend geretourneerd](./assets/troubleshooting/develop__await.png)

+ __Oorzaak__: De `renditionCallback` functie wordt afgesloten voordat de vertoning volledig kan worden geschreven naar `rendition.path`.
+ __Resolutie__: Controleer de code van de douanearbeider en zorg ervoor alle asynchrone vraag synchroon wordt gemaakt gebruikend `await`.

## Ontwikkelingsinstrument{#development-tool}

### Het bestand Console.json ontbreekt in het project Asset compute{#missing-console-json}

+ __Fout:__ Fout: ontbrekende vereiste bestanden bij validate (../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY) bij async setupAssetCompute (.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:JJ)
+ __Oorzaak:__ De `console.json` bestand ontbreekt in de hoofdmap van het Asset compute-project
+ __Resolutie:__ Een nieuwe download `console.json` vorm uw Adobe I/O-project
   1. In console.adobe.io, open het project van Adobe I/O het project van de Asset compute wordt gevormd om te gebruiken
   1. Tik op de knop __Downloaden__ knop rechtsboven
   1. Sla het gedownloade bestand met de bestandsnaam op in de hoofdmap van het Asset compute-project `console.json`

### Onjuiste YAML-inspringing in manifest.yml{#incorrect-yaml-indentation}

+ __Fout:__ YAMLException: bad indentation of a mapping entry at line X, column Y: (via standard out from `aio app run` (opdracht)
+ __Oorzaak:__ Bij bestanden met witruimte is de inspringing waarschijnlijk onjuist.
+ __Resolutie:__ Controleer uw `manifest.yml` en zorgt u ervoor dat alle inspringingen correct zijn.

### memorySize limit is set to low{#memorysize-limit-is-set-too-low}

+ __Fout:__  Lokale Dev Server OpenWhiskError: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true Geretourneerde HTTP 400 (Ongeldig verzoek) —> &quot;De aanvraaginhoud is onjuist geformuleerd:vereiste is mislukt: geheugen 64 MB onder de toegestane drempel van 134217728 B&quot;
+ __Oorzaak:__ A `memorySize` limiet voor de werknemer in de `manifest.yml` is ingesteld onder de minimaal toegestane drempel die wordt gerapporteerd door het foutbericht in bytes.
+ __Resolutie:__  Controleer de `memorySize` grenswaarden in de `manifest.yml` en ervoor te zorgen dat ze allemaal groter zijn dan de minimaal toegestane drempel.

### Development Tool kan niet worden gestart omdat private.key ontbreekt{#missing-private-key}

+ __Fout:__ Local Dev ServerError: Missing required files at validatePrivateKeyFile... (via standaard uit van `aio app run` (opdracht)
+ __Oorzaak:__ De `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` waarde in `.env` bestand, verwijst niet naar `private.key` of `private.key` kan niet worden gelezen door de huidige gebruiker.
+ __Resolutie:__ Controleer de `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` waarde in `.env` en zorg ervoor dat het het volledige, absolute pad naar het `private.key` op uw bestandssysteem.

### Vervolgkeuzelijst voor bronbestanden is onjuist{#source-files-dropdown-incorrect}

Het Hulpmiddel van de Ontwikkeling van de asset compute kan een staat ingaan waar het stapelgegevens trekt, en het merkbaarst in het __Bronbestand__ vervolgkeuzelijst met onjuiste items.

+ __Fout:__ In het vervolgkeuzemenu Bronbestand worden onjuiste items weergegeven.
+ __Oorzaak:__ De browserstatus van Stale in cache veroorzaakt de
+ __Resolutie:__ In uw browser ontruimen volledig de browser staat van het lusje van de browser, het browser geheime voorgeheugen, lokale opslag en de dienstarbeider.

### Ontbrekende of ongeldige devToolToken-queryparameter{#missing-or-invalid-devtooltoken-query-parameter}

+ __Fout:__ Melding &quot;Niet-geautoriseerd&quot; in Asset compute Development Tool
+ __Oorzaak:__ `devToolToken` ontbreekt of is ongeldig
+ __Resolutie:__ Sluit het browser venster van het Hulpmiddel van de Ontwikkeling van de Asset compute, beëindigt om het even welke lopende processen van het Hulpmiddel van de Ontwikkeling die via worden in werking gesteld `aio app run` en begin het Hulpmiddel van de Ontwikkeling opnieuw (gebruikend `aio app run`).

### Kan bronbestanden niet verwijderen{#unable-to-remove-source-files}

+ __Fout:__ Er is geen manier om toegevoegde brondossiers uit de UI van Hulpmiddelen van de Ontwikkeling te verwijderen
+ __Oorzaak:__ Deze functionaliteit is niet geïmplementeerd
+ __Resolutie:__ Meld u aan bij uw leverancier voor cloudopslag met de referenties die zijn gedefinieerd in `.env`. Zoek de container die wordt gebruikt door de Development Tools (ook opgegeven in `.env`), navigeert u naar de __bron__ en verwijder bronafbeeldingen. U moet mogelijk de stappen uitvoeren die worden beschreven in [Vervolgkeuzelijst voor bronbestanden is onjuist](#source-files-dropdown-incorrect) als de verwijderde bronbestanden in het vervolgkeuzemenu blijven weergeven omdat ze lokaal in de cache kunnen worden geplaatst in de &#39;toepassingsstatus&#39; van de ontwikkelingsprogramma&#39;s.

  ![Microsoft Azure Blob-opslag](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Testen{#test}

### Geen uitvoering gegenereerd tijdens de uitvoering van de test{#test-no-rendition-generated}

+ __Fout:__ Fout: er is geen uitvoering gegenereerd.
+ __Oorzaak:__ De worker kan geen uitvoering genereren vanwege een onverwachte fout, zoals een JavaScript-syntaxisfout.
+ __Resolutie:__ De uitvoering van de test controleren `test.log` om `/build/test-results/test-worker/test.log`. Zoek de sectie in dit bestand die overeenkomt met de testcase voor mislukken en controleer of er fouten zijn opgetreden.

  ![Problemen oplossen - Geen uitvoering gegenereerd](./assets/troubleshooting/test__no-rendition-generated.png)

### Test genereert onjuiste uitvoering, waardoor de test mislukt{#tests-generates-incorrect-rendition}

+ __Fout:__ Fout: vertoning &#39;rendition.xxx&#39; is niet zoals verwacht.
+ __Oorzaak:__ De worker voert een andere uitvoering uit dan de `rendition.<extension>` in het testgeval worden verstrekt.
   + Als de verwachte `rendition.<extension>` bestand wordt niet op exact dezelfde manier gemaakt als de lokaal gegenereerde vertoning in het testgeval, de test kan mislukken omdat er een verschil in de bits kan zijn. Als de Asset compute worker bijvoorbeeld het contrast wijzigt met behulp van API&#39;s en het verwachte resultaat wordt gemaakt door het contrast in Adobe Photoshop CC aan te passen, kunnen de bestanden er hetzelfde uitzien, maar kleine variaties in de bits kunnen verschillend zijn.
+ __Resolutie:__ De uitvoer van de vertoning van de vertoning controleren door naar te navigeren `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`en vergelijk het bestand met het verwachte vertoningsbestand in het testgeval. Om een exact verwacht actief te maken:
   + Gebruik het gereedschap Ontwikkeling om een vertoning te genereren, te valideren dat deze correct is en te gebruiken als het verwachte vertoningsbestand
   + U kunt het bestand dat tijdens de test wordt gegenereerd ook valideren op `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, valideer het correct is en gebruik dat als het verwachte vertoningsbestand

## Foutopsporing

### Foutopsporing wordt niet gekoppeld{#debugger-does-not-attach}

+ __Fout__: Fout bij het verwerken van de toepassing: Fout: kan geen verbinding maken met het foutopsporingsdoel op..
+ __Oorzaak__: Docker Desktop wordt niet uitgevoerd op het lokale systeem. Verifieer dit door de Console van de Foutopsporing van de Code van VS (Mening > Debug Console) te herzien, bevestigend deze fout wordt gemeld.
+ __Resolutie__: Start [De Desktop van de Docker en bevestigen de vereiste beelden van de Docker geïnstalleerd](./set-up/development-environment.md#docker).

### Onderbrekingspunten worden niet gepauzeerd{#breakpoints-no-pausing}

+ __Fout__: Wanneer het runnen van de Asset compute worker van het foutopsporingsprogramma voor ontwikkeling, wordt de code van VS niet gepauzeerd bij onderbrekingspunten.

#### Foutopsporing VS-code niet gekoppeld{#vs-code-debugger-not-attached}

+ __Oorzaak:__ Foutopsporing van de Code van VS werd tegengehouden/losgemaakt.
+ __Resolutie:__ Start de VS-foutopsporing opnieuw en controleer of deze is aangesloten via de VS Code Debug Output Console (Weergave > Foutopsporingsconsole)

#### Foutopsporing voor VS-code gekoppeld nadat uitvoering van worker is gestart{#vs-code-debugger-attached-after-worker-execution-began}

+ __Oorzaak:__ Foutopsporing van de Code VS heeft voorafgaand aan het tikken niet vastgemaakt __Uitvoeren__ in Development Tool.
+ __Resolutie:__ Verzeker debugger door de Foutopsporingsconsole van de Code van VS te herzien (Mening > zuivert Console), en dan de Asset compute worker van het Hulpmiddel van de Ontwikkeling opnieuw in werking te stellen.

### Worker-time-out tijdens foutopsporing{#worker-times-out-while-debugging}

+ __Fout__: Foutopsporingsconsole meldt &quot;Action will timeout in -XXX milliseconds&quot; of [Asset compute Development Tool](./develop/development-tool.md) renditievoorvertoning draait oneindig of
+ __Oorzaak__: De time-out van de worker zoals gedefinieerd in het dialoogvenster [manifest.yml](./develop/manifest.md) wordt tijdens foutopsporing overschreden.
+ __Resolutie__: Verhoog tijdelijk de time-out van de worker in het dialoogvenster [manifest.yml](./develop/manifest.md) of versnelt de foutopsporingsactiviteiten.

### Kan foutopsporingsproces niet beëindigen{#cannot-terminate-debugger-process}

+ __Fout__: `Ctrl-C` op de bevellijn beëindigt niet het debugger proces (`npx adobe-asset-compute devtool`).
+ __Oorzaak__: Een fout in `@adobe/aio-cli-plugin-asset-compute` 1.3.x, resulteert in `Ctrl-C` wordt niet herkend als een afsluitende opdracht.
+ __Resolutie__: Update `@adobe/aio-cli-plugin-asset-compute` naar versie 1.4.1+

  ```
  $ aio update
  ```

  ![Problemen oplossen - AIR-update](./assets/troubleshooting/debug__terminate.png)

## Implementeren{#deploy}

### Aangepaste uitvoering ontbreekt in element in AEM{#custom-rendition-missing-from-asset}

+ __Fout:__ Nieuwe en opnieuw verwerkte elementen worden verwerkt, maar de aangepaste uitvoering ontbreekt

#### Profiel verwerken dat niet is toegepast op de bovenliggende map

+ __Oorzaak:__ Het element bestaat niet in een map met het verwerkingsprofiel dat de aangepaste worker gebruikt
+ __Resolutie:__ Pas het verwerkingsprofiel toe op een bovenliggende map van het element

#### Bezig met verwerken van profiel vervangen door lager verwerkingsprofiel

+ __Oorzaak:__ Het middel bestaat onder een omslag met het toegepaste Profiel van de de arbeidersverwerking van de douane, nochtans een verschillend Profiel van de Verwerking dat niet de klantenarbeider gebruikt is toegepast tussen die omslag en het middel.
+ __Resolutie:__ De twee verwerkingsprofielen combineren of op een andere manier afstemmen en het tussentijdse verwerkingsprofiel verwijderen

### Verwerking van middelen mislukt in AEM{#asset-processing-fails}

+ __Fout:__ Asset Processing Failed badge displayed on asset
+ __Oorzaak:__ Er is een fout opgetreden bij de uitvoering van de aangepaste worker
+ __Resolutie:__ Volg de instructies op [foutopsporing in Adobe I/O Runtime](./test-debug/debug.md#aio-app-logs) gebruiken `aio app logs`.
