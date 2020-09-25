---
title: Fouten opsporen in een worker voor het berekenen van bedrijfsmiddelen
description: De arbeiders van de Compute van activa kunnen op verscheidene manieren worden gezuiverd, van eenvoudige zuivert logboekverklaringen, aan de Code van in bijlage VS als verre debugger, aan trekkend logboeken voor activeringen in Adobe I/O Runtime die van AEM als Cloud Service worden in werking gesteld.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---


# Fouten opsporen in een worker voor het berekenen van bedrijfsmiddelen

De arbeiders van de Compute van activa kunnen op verscheidene manieren worden gezuiverd, van eenvoudige zuivert logboekverklaringen, aan de Code van in bijlage VS als verre debugger, aan trekkend logboeken voor activeringen in Adobe I/O Runtime die van AEM als Cloud Service worden in werking gesteld.

## Logboekregistratie

De eenvoudigste vorm van foutopsporing voor de workers Asset Compute gebruikt traditionele `console.log(..)` instructies in de code van de worker. Het `console` JavaScript-object is een impliciet, globaal object, zodat het niet hoeft te worden geïmporteerd of vereist, omdat het altijd in alle contexten voorkomt.

Deze logboekverklaringen zijn beschikbaar voor overzicht verschillend gebaseerd op hoe de worker Asset Compute wordt uitgevoerd:

+ Van `aio app run`, drukken de logboeken aan standaard uit en de Logboeken van de Activering van het Hulpmiddel van de [Ontwikkeling](../develop/development-tool.md)
   ![air app run console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ Van `aio app test`logbestanden afdrukken naar `/build/test-results/test-worker/test.log`
   ![air app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ Met behulp `wskdebug`van logboeken worden instructies afgedrukt naar de VS Code Debug Console (Weergave > Foutopsporingsconsole), standaard uit
   ![wskdebug console.log(..)](./assets/debug/console-log__wskdebug.png)
+ Loginstructies worden met behulp `aio app logs`van loginstructies afgedrukt naar het activeringslogbestand.

## Foutopsporing op afstand via aangesloten foutopsporing

>[!WARNING]
>
>Microsoft Visual Studio Code 1.48.0 of groter van het gebruik voor verenigbaarheid met wskdebug

De [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm module, steunt het vastmaken van debugger aan de arbeiders van de Compute van Activa, met inbegrip van de capaciteit om breekpunten in de Code van VS te plaatsen en door de code te stappen.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)
_Doorklikken voor foutopsporing in een worker voor het berekenen van bedrijfsmiddelen met behulp van wskdebug (geen audio)_

1. Zorg ervoor dat [wskdebug](../set-up/development-environment.md#wskdebug) - en [ngrok](../set-up/development-environment.md#ngork) -npm-modules zijn geïnstalleerd
1. Zorg ervoor dat [Docker Desktop en de ondersteunende Docker-images](../set-up/development-environment.md#docker) zijn geïnstalleerd en uitgevoerd
1. Sluit alle actieve actieve uitvoeringsinstanties van Development Tool.
1. Stel de recentste code op gebruikend en registreer de opgestelde actienaam (naam tussen `aio app deploy` `[...]`). Hiermee wordt de update voor `launch.json` stap 8 uitgevoerd.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```
1. Start een nieuwe instantie van het hulpprogramma voor het berekenen van bedrijfsmiddelen met de opdracht `npx adobe-asset-compute devtool`
1. Tik in VS-code op het pictogram Foutopsporing in de linkernavigatie
   + Tik desgevraagd op __Create a launch.json file > Node.js__ om een nieuw `launch.json` bestand te maken.
   + Tik anders op het __tandwielpictogram__ rechts van het vervolgkeuzemenu __Start Program__ (Programma `launch.json` starten) om het bestaande pictogramin de editor te openen.
1. Voeg de volgende JSON-objectconfiguratie toe aan de `configurations` array:

   ```json
   {
       "type": "pwa-node",
       "request": "launch",
       "name": "wskdebug",
       "attachSimplePort": 0,
       "runtimeExecutable": "wskdebug",
       "args": [
           "wkndAemAssetCompute-0.0.1/__secured_worker",  // Version must match your Asset Compute application's version
           "${workspaceFolder}/actions/worker/index.js",  // Points to your worker
           "-l",
           "--ngrok"
       ],
       "localRoot": "${workspaceFolder}",
       "remoteRoot": "/code",
       "outputCapture": "std",
       "timeout": 30000
   }
   ```

1. Selecteer de nieuwe __foutopsporing__ in het vervolgkeuzemenu
1. Tik op de groene knop __Uitvoeren__ links van de __vervolgkeuzelijst voor__ foutopsporing
1. Open `/actions/worker/index.js` en tik links van de regelnummers om onderbrekingspunten 1 toe te voegen. Navigeer naar het browservenster van het Web van het Hulpmiddel voor computergebruik dat in stap 6 is geopend
1. Tik op de knop __Uitvoeren__ om de worker uit te voeren
1. Ga terug naar de Code van VS, aan `/actions/worker/index.js` en stap door de code
1. Om het zuivert-able Hulpmiddel van de Ontwikkeling weg te gaan, tik `Ctrl-C` in de terminal die `npx adobe-asset-compute devtool` bevel in stap 6 in werking stelde

## Logbestanden openen vanuit Adobe I/O Runtime{#aio-app-logs}

[AEM als Cloud Service maakt gebruik van Asset Compute-workers via het verwerken van profielen](../deploy/processing-profiles.md) door deze rechtstreeks aan te roepen in Adobe I/O Runtime. Omdat deze aanroepen geen lokale ontwikkeling impliceren, kunnen hun uitvoeringen niet worden gezuiverd gebruikend lokale tooling zoals het Hulpmiddel van de Ontwikkeling van de Compute van Activa of wskdebug. In plaats daarvan, kan Adobe I/O CLI worden gebruikt om logboeken van de worker te halen die in een bepaalde werkruimte in Adobe I/O Runtime wordt uitgevoerd.

1. Zorg ervoor dat de [werkruimte-specifieke omgevingsvariabelen](../deploy/runtime.md) via `AIO_runtime_namespace` en `AIO_runtime_auth`worden ingesteld op basis van de werkruimte die foutopsporing vereist.
1. Vanuit de opdrachtregel uitvoeren `aio app logs`
   + Als de werkruimte zwaar verkeer ondergaat, breid het aantal activeringslogboeken via de `--limit` vlag uit:
      `$ aio app logs --limit=25`
1. De meest recente (tot verstrekte `--limit`) activeringslogboeken zullen als output van het bevel voor overzicht zijn teruggekeerd.

   ![aio-toepassingslogboeken](./assets/debug/aio-app-logs.png)

## Problemen oplossen

### Foutopsporing wordt niet gekoppeld

+ __Fout__: Fout bij verwerken start: Fout: Kan geen verbinding maken met foutopsporingsdoel op..
+ __Oorzaak__: Docker Desktop wordt niet uitgevoerd op het lokale systeem. Verifieer dit door de Console van de Foutopsporing van de Code van VS (Mening > Debug Console) te herzien, bevestigend deze fout wordt gemeld.
+ __Resolutie__: Start [Docker Desktop en bevestig dat de vereiste Docker-images zijn geïnstalleerd](../set-up/development-environment.md#docker).

### Onderbrekingspunten worden niet gepauzeerd

+ __Fout__: Wanneer het runnen van de worker van Asset Compute van het hulpprogramma voor foutopsporing, onderbreekt de VS-code niet bij onderbrekingspunten.

#### Foutopsporing VS-code is niet gekoppeld

+ __Oorzaak:__ Foutopsporing van de Code van VS werd tegengehouden/losgemaakt.
+ __Resolutie:__ Start de VS-foutopsporing opnieuw en controleer of deze is aangesloten via de VS Code Debug Output Console (Weergave > Foutopsporingsconsole)

#### Foutopsporing voor VS-code gekoppeld nadat uitvoering van worker is gestart

+ __Oorzaak:__ Foutopsporing van de Code van VS verbond niet voorafgaand aan het tikken __Looppas__ in het Hulpmiddel van de Ontwikkeling.
+ __Resolutie:__ Verzeker debugger door de Foutopsporingsconsole van de Code van VS te herzien (Mening > zuivert Console), en dan de worker van de Compute van Activa van het Hulpmiddel van de Ontwikkeling opnieuw in werking te stellen.

### Worker-time-out tijdens foutopsporing

+ __Fout__: Foutopsporingsconsole rapporteert &quot;Action will timeout in -XXX milliseconds&quot; of de voorvertoning van de uitvoering van het hulpprogramma [Asset Compute Development wordt](../develop/development-tool.md) eindeloos gecentreerd of
+ __Oorzaak__: De time-out van de worker, zoals gedefinieerd in [manifest.yml](../develop/manifest.md) , wordt tijdens foutopsporing overschreden.
+ __Resolutie__: Verhoog tijdelijk de time-out van de worker in het bestand [manifest.yml](../develop/manifest.md) of versnel de foutopsporingsactiviteiten.

### Kan foutopsporingsproces niet beëindigen

+ __Fout__: `Ctrl-C` op de bevellijn beëindigt niet het debugger proces (`npx adobe-asset-compute devtool`).
+ __Oorzaak__: Een fout in `@adobe/aio-cli-plugin-asset-compute` 1.3.x, resulteert in `Ctrl-C` niet herkend als beëindigend bevel.
+ __Resolutie__: Bijwerken `@adobe/aio-cli-plugin-asset-compute` naar versie 1.4.1+

   ```
   $ aio update
   ```

   ![Problemen oplossen - AIR-update](./assets/debug/troubleshooting__terminate.png)