---
title: Fouten opsporen in een Asset compute-worker
description: De arbeiders van de asset compute kunnen op verscheidene manieren, van eenvoudige zuivert logboekverklaringen, aan de Code van VS in bijlage als verre debugger, aan trekkend logboeken voor activeringen in Adobe I/O Runtime worden in werking gesteld van AEM as a Cloud Service.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
duration: 268
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---

# Fouten opsporen in een Asset compute-worker

De arbeiders van de asset compute kunnen op verscheidene manieren, van eenvoudige zuivert logboekverklaringen, aan de Code van VS in bijlage als verre debugger, aan trekkend logboeken voor activeringen in Adobe I/O Runtime worden in werking gesteld van AEM as a Cloud Service.

## Logboekregistratie

De eenvoudigste vorm van het zuiveren van de arbeiders van de Asset compute gebruikt traditionele `console.log(..)` instructies in de code van de worker. De `console` JavaScript-object is een impliciet, globaal object, zodat het niet hoeft te worden geïmporteerd of vereist, omdat het altijd in alle contexten voorkomt.

Deze logboekverklaringen zijn beschikbaar voor overzicht verschillend gebaseerd op hoe de Asset compute arbeider wordt uitgevoerd:

+ Van `aio app run`, worden logbestanden standaard afgedrukt en worden de [Ontwikkelingsinstrumenten](../develop/development-tool.md) Activeringslogboeken
  ![air app run console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ Van `aio app test`, logbestanden afdrukken naar `/build/test-results/test-worker/test.log`
  ![air app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ Gebruiken `wskdebug`, worden logboekinstructies afgedrukt naar de VS-console voor foutopsporing van code (Weergave > Foutopsporingsconsole), standaard uit
  ![wskdebug console.log(..)](./assets/debug/console-log__wskdebug.png)
+ Gebruiken `aio app logs`, loginstructies worden afgedrukt naar het activeringslogbestand

## Foutopsporing op afstand via aangesloten foutopsporing

>[!WARNING]
>
>Microsoft Visual Studio Code 1.48.0 of groter van het gebruik voor verenigbaarheid met wskdebug

De [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm module, steunt het vastmaken van debugger aan de arbeiders van de Asset compute, met inbegrip van de capaciteit om breekpunten in de Code van VS te plaatsen en door de code te stappen.

>[!VIDEO](https://video.tv.adobe.com/v/40383?quality=12&learn=on)

_Doorklikken van foutopsporing in een Asset compute-worker met wskdebug (geen audio)_

1. Zorgen [wskdebug](../set-up/development-environment.md#wskdebug) en [ngrot](../set-up/development-environment.md#ngork) npm-modules worden geïnstalleerd
1. Zorgen [Docker Desktop en de ondersteunende Docker-afbeeldingen](../set-up/development-environment.md#docker) worden geïnstalleerd en uitgevoerd
1. Sluit alle actieve actieve actieve uitvoeringsinstanties van Development Tool.
1. De nieuwste code implementeren met `aio app deploy`  en registreer de opgestelde actienaam (naam tussen `[...]`). Hiermee werkt u het dialoogvenster `launch.json` in stap 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. Begin een nieuw geval van het Hulpmiddel van de Ontwikkeling van de Asset compute gebruikend het bevel `npx adobe-asset-compute devtool`
1. Tik in VS-code op het pictogram Foutopsporing in de linkernavigatie
   + Tik indien nodig op __Een bestand launch.json maken > Node.js__ om een nieuwe `launch.json` bestand.
   + Tik anders op de knop __Tandwiel__ pictogram rechts van __Programma starten__ vervolgkeuzelijst voor het openen van de bestaande `launch.json` in de editor.
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

1. Selecteer de nieuwe __wskdebug__ uit de vervolgkeuzelijst
1. Tik op groen __Uitvoeren__ links van __wskdebug__ druppel
1. Openen `/actions/worker/index.js` en tik links van de regelnummers om onderbrekingspunten 1 toe te voegen. Navigeer aan het Browser van het Web van het Hulpmiddel van de Ontwikkeling van de Asset compute die venster in stap 6 wordt geopend
1. Tik op de knop __Uitvoeren__ knop om de worker uit te voeren
1. Ga terug naar de Code van VS, aan `/actions/worker/index.js` en doorlopen de code
1. Tik op `Ctrl-C` in de terminal die `npx adobe-asset-compute devtool` opdracht in stap 6

## Logbestanden openen vanuit Adobe I/O Runtime{#aio-app-logs}

[AEM as a Cloud Service gebruikt Asset compute-workers via verwerkingsprofielen](../deploy/processing-profiles.md) door ze rechtstreeks in Adobe I/O Runtime aan te roepen. Omdat deze aanroepen geen lokale ontwikkeling impliceren, kunnen hun uitvoeringen niet worden gezuiverd gebruikend lokaal hulpmiddel zoals het Hulpmiddel van de Ontwikkeling van de Asset compute of wskdebug. In plaats daarvan, kan Adobe I/O CLI worden gebruikt om logboeken van de worker te halen die in een bepaalde werkruimte in Adobe I/O Runtime wordt uitgevoerd.

1. Zorg ervoor dat [specifieke omgevingsvariabelen voor de werkruimte](../deploy/runtime.md) worden ingesteld via `AIO_runtime_namespace` en `AIO_runtime_auth`, op basis van de werkruimte die foutopsporing vereist.
1. Vanuit de opdrachtregel uitvoeren `aio app logs`
   + Als de werkruimte zwaar verkeer ondergaat, breid het aantal activeringslogboeken via uit `--limit` markering:
     `$ aio app logs --limit=25`
1. De meest recente (tot en met de geleverde `--limit`) de activeringslogboeken zijn teruggekeerd als output van het bevel voor overzicht.

   ![aio-toepassingslogboeken](./assets/debug/aio-app-logs.png)

## Problemen oplossen

+ [Foutopsporing wordt niet gekoppeld](../troubleshooting.md#debugger-does-not-attach)
+ [Onderbrekingspunten worden niet gepauzeerd](../troubleshooting.md#breakpoints-no-pausing)
+ [Foutopsporing VS-code niet gekoppeld](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Foutopsporing voor VS-code gekoppeld nadat uitvoering van worker is gestart](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Worker-time-out tijdens foutopsporing](../troubleshooting.md#worker-times-out-while-debugging)
+ [Kan foutopsporingsproces niet beëindigen](../troubleshooting.md#cannot-terminate-debugger-process)
