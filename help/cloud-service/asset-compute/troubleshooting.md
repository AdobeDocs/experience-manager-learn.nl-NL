---
title: Problemen met de uitbreidbaarheid van Asset Compute oplossen voor AEM Assets
description: Hieronder volgt een index met veelvoorkomende problemen en fouten, samen met de resoluties, die kunnen optreden bij het ontwikkelen en implementeren van aangepaste workers Asset Compute voor AEM Assets.
feature: asset-compute
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1146'
ht-degree: 0%

---


# De uitbreidbaarheid van Asset Compute oplossen

Hieronder volgt een index met veelvoorkomende problemen en fouten, samen met de resoluties, die kunnen optreden bij het ontwikkelen en implementeren van aangepaste workers Asset Compute voor AEM Assets.

## Ontwikkelen{#develop}

### Vertoning wordt gedeeltelijk getekend/beschadigd geretourneerd{#rendition-returned-partially-drawn-or-corrupt}

+ __Fout__: Uitvoering wordt onvolledig gerenderd (als een afbeelding beschadigd is) of kan niet worden geopend.

   ![Vertoning wordt gedeeltelijk getekend geretourneerd](./assets/troubleshooting/develop__await.png)

+ __Oorzaak__: De functie van de worker `renditionCallback` wordt afgesloten voordat de uitvoering volledig kan worden uitgevoerd `rendition.path`.
+ __Resolutie__: Controleer de code van de douanearbeider en zorg ervoor alle asynchrone vraag synchroon wordt gemaakt gebruikend `await`.

## Ontwikkelingsinstrument{#development-tool}

### Onjuiste YAML-inspringing in manifest.yml{#incorrect-yaml-indentation}

+ __Fout:__ YAMLException: ongeldige inspringing van een toewijzingsitem op regel X, kolom Y: (via standaard uit `aio app run` opdracht)
+ __Oorzaak:__ Bij bestanden met witruimte is de inspringing waarschijnlijk onjuist.
+ __Resolutie:__ Controleer uw `manifest.yml` en controleer of alle inspringingen correct zijn.

### memorySize limit is set to low{#memorysize-limit-is-set-too-low}

+ __Fout:__  Lokale Dev Server OpenWhiskError: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true Geretourneerde HTTP 400 (Ongeldig verzoek) —> &quot;De aanvraaginhoud is onjuist geformuleerd:vereiste is mislukt: geheugen 64 MB onder de toegestane drempel van 134217728 B&quot;
+ __Oorzaak:__ Een `memorySize` limiet voor de worker in de `manifest.yml` code is ingesteld onder de minimale toegestane drempel, zoals aangegeven in het foutbericht in bytes.
+ __Resolutie:__  Controleer de `memorySize` limieten in het `manifest.yml` en zorg ervoor dat ze allemaal groter zijn dan de minimaal toegestane drempel.

### Development Tool kan niet worden gestart omdat private.key ontbreekt{#missing-private-key}

+ __Fout:__ Lokale Dev ServerError: Vereiste bestanden ontbreken bij validatePrivateKeyFile.... (via standaard uit van `aio app run` opdracht)
+ __Oorzaak:__ De `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` waarde in het `.env` bestand verwijst niet naar `private.key` of `private.key` is niet leesbaar voor de huidige gebruiker.
+ __Resolutie:__ Controleer de `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` waarde in het `.env` bestand en controleer of het volledige, absolute pad naar het bestand `private.key` op het bestandssysteem is opgenomen.

### Vervolgkeuzelijst voor bronbestanden is onjuist{#source-files-dropdown-incorrect}

Het hulpmiddel van de Ontwikkeling van de Compute van activa kan een staat ingaan waar het stapelgegevens ophaalt, en het merkbaarst in het __Brondossier__ dropdown tonend onjuiste punten is.

+ __Fout:__ In het vervolgkeuzemenu Bronbestand worden onjuiste items weergegeven.
+ __Oorzaak:__ De browserstatus van Stale in cache veroorzaakt de
+ __Resolutie:__ In uw browser ontruimen volledig de browser staat van het lusje van de browser, het browser geheime voorgeheugen, lokale opslag en de dienstarbeider.

### Ontbrekende of ongeldige devToolToken-queryparameter{#missing-or-invalid-devtooltoken-query-parameter}

+ __Fout:__ Melding van &quot;onbevoegd&quot; in Asset Compute Development Tool
+ __Oorzaak:__ `devToolToken` ontbreekt of is ongeldig
+ __Resolutie:__ Sluit het de browser van het Hulpmiddel van de Ontwikkeling van Activa Compute, beëindigt om het even welke lopende processen van het Hulpmiddel van de Ontwikkeling die via het `aio app run` bevel worden in werking gesteld, en herstart het Hulpmiddel van de Ontwikkeling (gebruikend `aio app run`).

### Kan bronbestanden niet verwijderen{#unable-to-remove-source-files}

+ __Fout:__ Er is geen manier om toegevoegde brondossiers uit de UI van Hulpmiddelen van de Ontwikkeling te verwijderen
+ __Oorzaak:__ Deze functionaliteit is niet geïmplementeerd
+ __Resolutie:__ Meld u aan bij uw leverancier voor cloudopslag met de referenties die zijn gedefinieerd in `.env`. Zoek de container die wordt gebruikt door de ontwikkelingsprogramma&#39;s (ook opgegeven in `.env`), navigeer naar de __bronmap__ en verwijder bronafbeeldingen. Mogelijk moet u de stappen uitvoeren die worden beschreven in het vervolgkeuzemenu [Bronbestanden onjuist](#source-files-dropdown-incorrect) als de verwijderde bronbestanden in het vervolgkeuzemenu worden weergegeven, omdat ze mogelijk lokaal in het cachegeheugen zijn opgeslagen in de &quot;toepassingsstatus&quot; van Ontwikkelingsgereedschappen.

   ![Microsoft Azure Blob-opslag](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Testen{#test}

### Geen uitvoering gegenereerd tijdens de uitvoering van de test{#test-no-rendition-generated}

+ __Fout:__ Fout: Geen uitvoering gegenereerd.
+ __Oorzaak:__ De worker kan geen uitvoering genereren vanwege een onverwachte fout, zoals een JavaScript-syntaxisfout.
+ __Resolutie:__ Controleer de uitvoering van de test `test.log` op `/build/test-results/test-worker/test.log`. Zoek de sectie in dit bestand die overeenkomt met de testcase voor mislukken en controleer of er fouten zijn opgetreden.

   ![Problemen oplossen - Geen uitvoering gegenereerd](./assets/troubleshooting/test__no-rendition-generated.png)

### Test genereert onjuiste uitvoering, waardoor de test mislukt{#tests-generates-incorrect-rendition}

+ __Fout:__ Fout: Vertoning &#39;rendition.xxx&#39; is niet zoals verwacht.
+ __Oorzaak:__ De worker voert een uitvoering uit die anders is dan de uitvoering die in het testgeval is `rendition.<extension>` opgegeven.
   + Als het verwachte `rendition.<extension>` bestand niet op dezelfde manier wordt gemaakt als de lokaal gegenereerde vertoning in het testgeval, kan de test mislukken omdat er een verschil in de bits kan zijn. Als de worker Asset Compute bijvoorbeeld het contrast wijzigt met behulp van API&#39;s en het verwachte resultaat wordt gemaakt door het contrast in Adobe Photoshop CC aan te passen, kunnen de bestanden er hetzelfde uitzien, maar kleine variaties in de bits kunnen verschillen.
+ __Resolutie:__ Bekijk de uitvoer van de vertoning van de test door naar `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`te navigeren en deze te vergelijken met het verwachte vertoningsbestand in het testgeval. Om een exact verwacht actief te maken:
   + Gebruik het Hulpmiddel van de Ontwikkeling om een vertoning te produceren, te bevestigen het correct is en dat als het verwachte vertoningsdossier te gebruiken
   + Of u valideert het bestand dat tijdens de test wordt gegenereerd bij `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, valideert het bestand en gebruikt het bestand als het verwachte vertoningsbestand

## Foutopsporing


### Foutopsporing wordt niet gekoppeld{#debugger-does-not-attach}

+ __Fout__: Fout bij verwerken start: Fout: Kan geen verbinding maken met foutopsporingsdoel op..
+ __Oorzaak__: Docker Desktop wordt niet uitgevoerd op het lokale systeem. Verifieer dit door de Console van de Foutopsporing van de Code van VS (Mening > Debug Console) te herzien, bevestigend deze fout wordt gemeld.
+ __Resolutie__: Start [Docker Desktop en bevestig dat de vereiste Docker-images zijn geïnstalleerd](./set-up/development-environment.md#docker).

### Onderbrekingspunten worden niet gepauzeerd{#breakpoints-no-pausing}

+ __Fout__: Wanneer het runnen van de worker van Asset Compute van het hulpprogramma voor foutopsporing, onderbreekt de VS-code niet bij onderbrekingspunten.

#### Foutopsporing VS-code niet gekoppeld{#vs-code-debugger-not-attached}

+ __Oorzaak:__ Foutopsporing van de Code van VS werd tegengehouden/losgemaakt.
+ __Resolutie:__ Start de VS-foutopsporing opnieuw en controleer of deze is aangesloten via de VS-console voor foutopsporing van code (Weergave > Foutopsporingsconsole)

#### Foutopsporing voor VS-code gekoppeld nadat uitvoering van worker is gestart{#vs-code-debugger-attached-after-worker-execution-began}

+ __Oorzaak:__ Foutopsporing van de Code van VS verbond niet voorafgaand aan het tikken __Looppas__ in het Hulpmiddel van de Ontwikkeling.
+ __Resolutie:__ Verzeker debugger door de Foutopsporingsconsole van de Code van VS te herzien (Mening > zuivert Console), en dan de worker van de Compute van Activa van het Hulpmiddel van de Ontwikkeling opnieuw in werking te stellen.

### Worker-time-out tijdens foutopsporing{#worker-times-out-while-debugging}

+ __Fout__: Foutopsporingsconsole rapporteert &quot;Action will timeout in -XXX milliseconds&quot; of de voorvertoning van de uitvoering van het hulpprogramma [Asset Compute Development wordt](./develop/development-tool.md) eindeloos gecentreerd of
+ __Oorzaak__: De time-out van de worker, zoals gedefinieerd in [manifest.yml](./develop/manifest.md) , wordt tijdens foutopsporing overschreden.
+ __Resolutie__: Verhoog tijdelijk de time-out van de worker in het bestand [manifest.yml](./develop/manifest.md) of versnel de foutopsporingsactiviteiten.

### Kan foutopsporingsproces niet beëindigen{#cannot-terminate-debugger-process}

+ __Fout__: `Ctrl-C` op de bevellijn beëindigt niet het debugger proces (`npx adobe-asset-compute devtool`).
+ __Oorzaak__: Een fout in `@adobe/aio-cli-plugin-asset-compute` 1.3.x, resulteert in `Ctrl-C` niet herkend als beëindigend bevel.
+ __Resolutie__: Bijwerken `@adobe/aio-cli-plugin-asset-compute` naar versie 1.4.1+

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
+ __Resolutie:__ Volg de instructies op het [zuiveren van de activering](./test-debug/debug.md#aio-app-logs) van Adobe I/O Runtime gebruikend `aio app logs`.


