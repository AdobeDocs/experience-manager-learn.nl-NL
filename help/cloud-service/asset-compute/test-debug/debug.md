---
title: Fouten opsporen in een Asset compute-worker
description: De arbeiders van de asset compute kunnen op verscheidene manieren, van eenvoudige zuivert logboekverklaringen, aan de Code van VS in bijlage als verre debugger, aan trekkend logboeken voor activeringen in Adobe I/O Runtime worden in werking gesteld die van AEM als Cloud Service.
feature: asset compute microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
topic: Integratie, ontwikkeling
role: Developer
level: Tussentijdse, ervaren
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '626'
ht-degree: 0%

---


# Fouten opsporen in een Asset compute-worker

De arbeiders van de asset compute kunnen op verscheidene manieren, van eenvoudige zuivert logboekverklaringen, aan de Code van VS in bijlage als verre debugger, aan trekkend logboeken voor activeringen in Adobe I/O Runtime worden in werking gesteld die van AEM als Cloud Service.

## Logboekregistratie

De eenvoudigste vorm van het zuiveren van de arbeiders van de Asset compute gebruikt traditionele `console.log(..)` verklaringen in de arbeiderscode. Het JavaScript-object `console` is een impliciet, globaal object, zodat het niet hoeft te worden geïmporteerd of vereist, omdat het altijd in alle contexten aanwezig is.

Deze logboekverklaringen zijn beschikbaar voor overzicht verschillend gebaseerd op hoe de Asset compute arbeider wordt uitgevoerd:

+ Vanuit `aio app run` worden logboeken afgedrukt naar standaard-out en de [ontwikkelingslogboeken](../develop/development-tool.md) activeringslogboeken
   ![air app run console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ Vanuit `aio app test` worden logbestanden afgedrukt naar `/build/test-results/test-worker/test.log`
   ![air app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ Met behulp van `wskdebug` worden logboekinstructies afgedrukt naar de VS Code Debug Console (Weergave > Foutopsporingsconsole), standaard uit
   ![wskdebug console.log(..)](./assets/debug/console-log__wskdebug.png)
+ Loginstructies worden met `aio app logs` afgedrukt naar het activeringslogbestand.

## Foutopsporing op afstand via aangesloten foutopsporing

>[!WARNING]
>
>Microsoft Visual Studio Code 1.48.0 of groter van het gebruik voor verenigbaarheid met wskdebug

De [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm module, steunt het vastmaken van debugger aan de arbeiders van de Asset compute, met inbegrip van de capaciteit om breekpunten in de Code van VS te plaatsen en door de code te stappen.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_Doorklikken van foutopsporing in een Asset compute-worker met wskdebug (geen audio)_

1. Zorg ervoor dat [wskdebug](../set-up/development-environment.md#wskdebug) en [ngrok](../set-up/development-environment.md#ngork) npm-modules zijn geïnstalleerd
1. Zorg ervoor dat [Docker Desktop en de ondersteunende Docker-images](../set-up/development-environment.md#docker) zijn geïnstalleerd en actief zijn
1. Sluit alle actieve actieve uitvoeringsinstanties van Development Tool.
1. Implementeer de nieuwste code met `aio app deploy` en neem de naam van de geïmplementeerde actie op (naam tussen `[...]`). Dit wordt gebruikt om `launch.json` in stap 8 bij te werken.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```
1. Begin een nieuw geval van het Hulpmiddel van de Ontwikkeling van de Asset compute gebruikend het bevel `npx adobe-asset-compute devtool`
1. Tik in VS-code op het pictogram Foutopsporing in de linkernavigatie
   + Tik op __Maak een bestand launch.json > Node.js__ om een nieuw bestand `launch.json` te maken.
   + Tik anders op het __Gear__-pictogram rechts van het __Start Program__-vervolgkeuzemenu om het bestaande `launch.json` in de editor te openen.
1. Voeg de volgende JSON-objectconfiguratie toe aan de `configurations`-array:

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

1. Selecteer de nieuwe __wskdebug__ uit vervolgkeuzelijst
1. Tik op de groene __Run__-knop links van __wskdebug__-vervolgkeuzelijst
1. Open `/actions/worker/index.js` en tik links van de regelnummers om onderbrekingspunten 1 toe te voegen. Navigeer aan het Browser van het Web van het Hulpmiddel van de Ontwikkeling van de Asset compute die venster in stap 6 wordt geopend
1. Tik op de knop __Uitvoeren__ om de worker uit te voeren
1. Ga terug naar de Code van VS, aan `/actions/worker/index.js` en stap door de code
1. Tik `Ctrl-C` in de terminal die de opdracht `npx adobe-asset-compute devtool` in stap 6 heeft uitgevoerd om het foutopsporingsprogramma af te sluiten

## Logbestanden openen vanuit Adobe I/O Runtime{#aio-app-logs}

[AEM als Cloud Service gebruikt Asset compute werknemers via het Verwerken van ](../deploy/processing-profiles.md) Profielen door hen in Adobe I/O Runtime direct aan te halen. Omdat deze aanroepen geen lokale ontwikkeling impliceren, kunnen hun uitvoeringen niet worden gezuiverd gebruikend lokaal hulpmiddel zoals het Hulpmiddel van de Ontwikkeling van de Asset compute of wskdebug. In plaats daarvan, kan Adobe I/O CLI worden gebruikt om logboeken van de worker te halen die in een bepaalde werkruimte in Adobe I/O Runtime wordt uitgevoerd.

1. Zorg ervoor dat de [werkruimte-specifieke omgevingsvariabelen](../deploy/runtime.md) worden ingesteld via `AIO_runtime_namespace` en `AIO_runtime_auth`, op basis van de werkruimte die foutopsporing vereist.
1. Van de bevellijn, voer `aio app logs` uit
   + Als de werkruimte zwaar verkeer ondergaat, breid het aantal activeringslogboeken via de `--limit` vlag uit:
      `$ aio app logs --limit=25`
1. De meest recente (tot verstrekte `--limit`) activeringslogboeken zullen als output van het bevel voor overzicht worden teruggekeerd.

   ![aio-toepassingslogboeken](./assets/debug/aio-app-logs.png)

## Problemen oplossen

+ [Foutopsporing wordt niet gekoppeld](../troubleshooting.md#debugger-does-not-attach)
+ [Onderbrekingspunten worden niet gepauzeerd](../troubleshooting.md#breakpoints-no-pausing)
+ [Foutopsporing VS-code niet gekoppeld](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Foutopsporing voor VS-code gekoppeld nadat uitvoering van worker is gestart](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Worker-time-out tijdens foutopsporing](../troubleshooting.md#worker-times-out-while-debugging)
+ [Kan foutopsporingsproces niet beëindigen](../troubleshooting.md#cannot-terminate-debugger-process)
