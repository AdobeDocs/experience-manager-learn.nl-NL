---
title: Problemen met uitbreidbaarheid van Asset compute voor AEM Assets oplossen
description: Hieronder ziet u een index met veelvoorkomende problemen en fouten, samen met de resoluties, die kunnen optreden bij het ontwikkelen en implementeren van aangepaste Asset compute-workers voor AEM Assets.
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


# Problemen met uitbreidbaarheid van Asset compute oplossen

Hieronder ziet u een index met veelvoorkomende problemen en fouten, samen met de resoluties, die kunnen optreden bij het ontwikkelen en implementeren van aangepaste Asset compute-workers voor AEM Assets.

## Ontwikkelen{#develop}

### Vertoning wordt gedeeltelijk getekend/beschadigd{#rendition-returned-partially-drawn-or-corrupt} geretourneerd

+ __Fout__: Uitvoering wordt onvolledig gerenderd (als een afbeelding beschadigd is) of kan niet worden geopend.

   ![Vertoning wordt gedeeltelijk getekend geretourneerd](./assets/troubleshooting/develop__await.png)

+ __Oorzaak__: De  `renditionCallback` functie van de worker wordt afgesloten voordat de uitvoering volledig kan worden uitgevoerd  `rendition.path`.
+ __Resolutie__: Controleer de code van de douanearbeider en zorg ervoor alle asynchrone vraag synchroon wordt gemaakt gebruikend  `await`.

## Development Tool{#development-tool}

### Onjuiste YAML-inspringing in manifest.yml{#incorrect-yaml-indentation}

+ __Fout:__ YAMLException: ongeldige inspringing van een toewijzingsitem op regel X, kolom Y: (via standaard uit  `aio app run` opdracht)
+ __Oorzaak:__ YAML-bestanden zijn gevoelig voor witruimte, de inspringing is waarschijnlijk onjuist.
+ __Resolutie:__ controleer uw  `manifest.yml` en controleer of alle inspringingen correct zijn.

### memorySize limit is set to low{#memorysize-limit-is-set-too-low}

+ __Fout:__  Local Dev Server OpenWhiskError: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true Geretourneerde HTTP 400 (Ongeldig verzoek) —> &quot;De aanvraaginhoud is onjuist geformuleerd:vereiste is mislukt: geheugen 64 MB onder de toegestane drempel van 134217728 B&quot;
+ __Oorzaak:__ Een  `memorySize` limiet voor de worker in de werkruimte  `manifest.yml` is ingesteld onder de minimale toegestane drempel, zoals aangegeven door het foutbericht in bytes.
+ __Resolutie:__  herzie de  `memorySize` grenzen in de  `manifest.yml` en zorg ervoor zij allen groot zijn dan de minimaal toegestane drempel.

### Development Tool kan niet worden gestart omdat private.key{#missing-private-key} ontbreekt

+ __Fout:__ Lokale Dev ServerError: Vereiste bestanden ontbreken bij validatePrivateKeyFile.... (via standaard uit de opdracht `aio app run`)
+ __Oorzaak:__ de  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` waarde in  `.env` het bestand verwijst niet naar  `private.key` of  `private.key` is door de huidige gebruiker niet leesbaar.
+ __Resolutie:__ controleer de  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` waarde in het  `.env` bestand en zorg ervoor dat deze het volledige, absolute pad naar het bestand  `private.key` op uw bestandssysteem bevat.

### Vervolgkeuzelijst voor bronbestanden is onjuist{#source-files-dropdown-incorrect}

Het Hulpmiddel van de Ontwikkeling van de asset compute kan een staat ingaan waar het stapelgegevens ophaalt, en het merkbaarst in __Brondossier__ dropdown tonend onjuiste punten is.

+ __Fout:__ in de vervolgkeuzelijst Bronbestand worden onjuiste items weergegeven.
+ __Oorzaak:browserstatus in__ cache oplopend veroorzaakt de
+ __Resolutie:__ in uw browser ontruim volledig de &quot;toepassingsstaat&quot;van het browser lusje, het browser geheime voorgeheugen, lokale opslag en de dienstarbeider.

### Ontbrekende of ongeldige devToolToken-queryparameter{#missing-or-invalid-devtooltoken-query-parameter}

+ __Fout:__ &quot;Onbevoegde&quot;bericht in het Hulpmiddel van de Ontwikkeling van de Asset compute
+ __Oorzaak:__ `devToolToken` ontbreekt of is ongeldig
+ __Resolutie:__ Sluit het browser venster van het Hulpmiddel van de Ontwikkeling van de Asset compute, eindig om het even welke lopende processen van het Hulpmiddel van de Ontwikkeling die via het  `aio app run` bevel in werking worden gesteld, en herstart het Hulpmiddel van de Ontwikkeling (gebruikend  `aio app run`).

### Kan bronbestanden niet verwijderen{#unable-to-remove-source-files}

+ __Fout:__ er is geen manier om toegevoegde bronbestanden te verwijderen uit de gebruikersinterface van de ontwikkelingsprogramma&#39;s
+ __Oorzaak:__ deze functionaliteit is niet geïmplementeerd
+ __Resolutie:__ Meld u aan bij uw cloudopslagprovider met de referenties die zijn gedefinieerd in  `.env`. Zoek de container die wordt gebruikt door de Development Tools (ook opgegeven in `.env`), navigeer naar de map __source__ en verwijder bronafbeeldingen. Mogelijk moet u de stappen uitvoeren die worden beschreven in [De dropdown van bronbestanden](#source-files-dropdown-incorrect) als de verwijderde bronbestanden in de vervolgkeuzelijst blijven weergeven omdat ze lokaal in de &quot;toepassingsstatus&quot; van de Ontwikkelingsgereedschappen kunnen worden opgeslagen.

   ![Microsoft Azure Blob-opslag](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Test{#test}

### Geen uitvoering gegenereerd tijdens de uitvoering van de test{#test-no-rendition-generated}

+ __Fout:__ fout: Geen uitvoering gegenereerd.
+ __Oorzaak:__ De worker kan geen uitvoering genereren vanwege een onverwachte fout, zoals een JavaScript-syntaxisfout.
+ __Resolutie:__ controleer de uitvoering van de test  `test.log` op  `/build/test-results/test-worker/test.log`. Zoek de sectie in dit bestand die overeenkomt met de testcase voor mislukken en controleer of er fouten zijn opgetreden.

   ![Problemen oplossen - Geen uitvoering gegenereerd](./assets/troubleshooting/test__no-rendition-generated.png)

### Test genereert een onjuiste vertoning waardoor de test mislukt{#tests-generates-incorrect-rendition}

+ __Fout:__ fout: Vertoning &#39;rendition.xxx&#39; is niet zoals verwacht.
+ __Oorzaak:__ de worker voert een uitvoering uit die anders is dan de uitvoering die in het testgeval is  `rendition.<extension>` opgegeven.
   + Als het verwachte `rendition.<extension>` dossier niet op de zelfde manier zoals de plaatselijk geproduceerde vertoning in het testgeval wordt gecreeerd, kan de test ontbreken aangezien er één of ander verschil in de beetjes kan zijn. Als de Asset compute worker bijvoorbeeld het contrast wijzigt met behulp van API&#39;s en het verwachte resultaat wordt gemaakt door het contrast in Adobe Photoshop CC aan te passen, kunnen de bestanden er hetzelfde uitzien, maar kleine variaties in de bits kunnen verschillend zijn.
+ __Resolutie:__ controleer de uitvoer van de vertoning door naar het verwachte vertoningsbestand te navigeren  `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`en dit te vergelijken met het testbestand. Om een exact verwacht actief te maken:
   + Gebruik het Hulpmiddel van de Ontwikkeling om een vertoning te produceren, te bevestigen het correct is en dat als het verwachte vertoningsdossier te gebruiken
   + U kunt ook het bestand dat tijdens de test is gegenereerd op `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>` valideren, controleren of het bestand juist is en gebruiken als het verwachte weergavebestand

## Foutopsporing


### Foutopsporing voegt geen{#debugger-does-not-attach} toe

+ __Fout__: Fout bij verwerken start: Fout: Kan geen verbinding maken met foutopsporingsdoel op..
+ __Oorzaak__: Docker Desktop wordt niet uitgevoerd op het lokale systeem. Verifieer dit door de Console van de Foutopsporing van de Code van VS (Mening > Debug Console) te herzien, bevestigend deze fout wordt gemeld.
+ __Resolutie__: Start  [Docker Desktop en bevestig dat de vereiste Docker-images zijn geïnstalleerd](./set-up/development-environment.md#docker).

### Onderbrekingspunten worden niet gepauzeerd{#breakpoints-no-pausing}

+ __Fout__: Wanneer het in werking stellen van de Asset compute worker van het foutopsporingsprogramma voor ontwikkeling, wordt de code van VS niet gepauzeerd bij onderbrekingspunten.

#### Foutopsporing VS-code niet gekoppeld{#vs-code-debugger-not-attached}

+ __Oorzaak:__ Foutopsporing van de Code VS werd tegengehouden/losgemaakt.
+ __Resolutie:__ herstart foutopsporing van VS-code en controleer of deze is gekoppeld met de VS-console voor foutopsporing van code (Weergave > Foutopsporingsconsole)

#### Foutopsporing van VS-code gekoppeld nadat uitvoering van worker is gestart{#vs-code-debugger-attached-after-worker-execution-began}

+ __Oorzaak:__ Foutopsporing van de Code VS verbond voorafgaand aan het Tikken van het Hulpmiddel van de Ontwikkeling van de  ____ Lopende niet.
+ __Resolutie:__ zorg ervoor debugger door de Foutopsporingsconsole van de Code van VS (Mening > Debug Console) te herzien heeft vastgemaakt, en dan de Asset compute worker van het Hulpmiddel van de Ontwikkeling opnieuw in werking te stellen.

### Worker-tijden uit tijdens foutopsporing{#worker-times-out-while-debugging}

+ __Fout__: Foutopsporingsconsole rapporteert &quot;Action will timeout in -XXX milliseconds&quot; of de voorvertoning van de  [Asset compute Development Tool ](./develop/development-tool.md) voor onbepaalde tijd draait of
+ __Oorzaak__: De time-out van de worker, zoals gedefinieerd in het bestand  [manifest.](./develop/manifest.md) ymlis, is tijdens foutopsporing overschreden.
+ __Resolutie__: Verhoog tijdelijk de time-out van de worker in het bestand  [manifest.](./develop/manifest.md) ymlor om de foutopsporingsactiviteiten te versnellen.

### Kan foutopsporingsproces niet beëindigen{#cannot-terminate-debugger-process}

+ __Fout__:  `Ctrl-C` op de bevellijn beëindigt niet het debugger proces (`npx adobe-asset-compute devtool`).
+ __Oorzaak__: Een fout in  `@adobe/aio-cli-plugin-asset-compute` 1.3.x, resulteert in  `Ctrl-C` niet herkend als beëindigend bevel.
+ __Resolutie__: Bijwerken  `@adobe/aio-cli-plugin-asset-compute` naar versie 1.4.1+

   ```
   $ aio update
   ```

   ![Problemen oplossen - AIR-update](./assets/troubleshooting/debug__terminate.png)

## Implementeren{#deploy}

### Aangepaste uitvoering ontbreekt in element in AEM{#custom-rendition-missing-from-asset}

+ __Fout:__ Nieuwe en opnieuw verwerkte elementen worden verwerkt, maar de aangepaste uitvoering ontbreekt

#### Profiel verwerken dat niet is toegepast op de bovenliggende map

+ __Oorzaak:__ het element bestaat niet in een map met het verwerkingsprofiel dat de aangepaste worker gebruikt
+ __Resolutie:het verwerkingsprofiel__ toepassen op een bovenliggende map van het element

#### Bezig met verwerken van profiel vervangen door lager verwerkingsprofiel

+ __Oorzaak:__ Het element bestaat onder een map waarop het aangepaste verwerkingsprofiel voor workers is toegepast. Er is echter een ander verwerkingsprofiel dat geen gebruik maakt van de worker van de klant toegepast tussen die map en het element.
+ __Resolutie:de twee verwerkingsprofielen__ combineren of op een andere manier afstemmen en het tussenliggende verwerkingsprofiel verwijderen

### Verwerking van middelen mislukt in AEM{#asset-processing-fails}

+ __Fout:__ Middelverwerking mislukt badge weergegeven op element
+ __Oorzaak:__ er is een fout opgetreden bij de uitvoering van de aangepaste worker
+ __Resolutie:__ volg de instructies over het  [opsporen van fouten in Adobe I/O Runtime-](./test-debug/debug.md#aio-app-logs) activiteiten  `aio app logs`.


