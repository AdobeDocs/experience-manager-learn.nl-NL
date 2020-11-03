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
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '618'
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
           "wkndAemAssetCompute-0.0.1/__secured_worker",  // Version must match your Asset Compute worker's version
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

+ [Foutopsporing wordt niet gekoppeld](../troubleshooting.md#debugger-does-not-attach)
+ [Onderbrekingspunten worden niet gepauzeerd](../troubleshooting.md#breakpoints-no-pausing)
+ [Foutopsporing VS-code niet gekoppeld](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Foutopsporing voor VS-code gekoppeld nadat uitvoering van worker is gestart](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Worker-time-out tijdens foutopsporing](../troubleshooting.md#worker-times-out-while-debugging)
+ [Kan foutopsporingsproces niet beëindigen](../troubleshooting.md#cannot-terminate-debugger-process)
