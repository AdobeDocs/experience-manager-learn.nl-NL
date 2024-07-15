---
title: Fouten opsporen in een Asset compute-worker
description: De arbeiders van de asset compute kunnen op verscheidene manieren, van eenvoudige zuivert logboekverklaringen, aan bijlage de Code van VS als verre debugger, aan trekkend logboeken voor activeringen in Adobe I/O Runtime worden gezeten die van AEM as a Cloud Service in werking worden gesteld.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
duration: 229
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---

# Fouten opsporen in een Asset compute-worker

De arbeiders van de asset compute kunnen op verscheidene manieren, van eenvoudige zuivert logboekverklaringen, aan bijlage de Code van VS als verre debugger, aan trekkend logboeken voor activeringen in Adobe I/O Runtime worden gezeten die van AEM as a Cloud Service in werking worden gesteld.

## Logboekregistratie

De meest elementaire vorm van foutopsporing voor Asset compute-workers gebruikt traditionele `console.log(..)` -instructies in de code van de worker. Het JavaScript-object `console` is een impliciet, globaal object, zodat het niet hoeft te worden geïmporteerd of vereist, omdat het altijd in alle contexten voorkomt.

Deze logboekverklaringen zijn beschikbaar voor overzicht verschillend gebaseerd op hoe de Asset compute arbeider wordt uitgevoerd:

+ Van `aio app run`, drukken de logboeken aan standaard uit en de ](../develop/development-tool.md) Logboeken van de Activering van het 1} Hulpmiddel van de Ontwikkeling {[
  ![ de looppasconsole.log van de AIR app (...) ](./assets/debug/console-log__aio-app-run.png)
+ Vanuit `aio app test` worden logbestanden afgedrukt naar `/build/test-results/test-worker/test.log`
  ![ Ao app test console.log (...) ](./assets/debug/console-log__aio-app-test.png)
+ Met `wskdebug` worden loginstructies afgedrukt naar de VS-console voor foutopsporing van code (Weergave > Foutopsporingsconsole), standaard uit
  ![ wskdebug console.log (...) ](./assets/debug/console-log__wskdebug.png)
+ Loginstructies worden met `aio app logs` afgedrukt naar het activeringslogbestand.

## Foutopsporing op afstand via aangesloten foutopsporing

>[!WARNING]
>
>Microsoft Visual Studio Code 1.48.0 of groter van het gebruik voor verenigbaarheid met wskdebug

[ wskdebug ](https://www.npmjs.com/package/@openwhisk/wskdebug) npm module, steunt het vastmaken van debugger aan de arbeiders van de Asset compute, met inbegrip van de capaciteit om breekpunten in de Code van VS te plaatsen en door de code te stappen.

>[!VIDEO](https://video.tv.adobe.com/v/40383?quality=12&learn=on)

_klik-door van het zuiveren van een arbeider van de Asset compute gebruikend wskdebug (Geen audio)_

1. Verzeker [ wskdebug ](../set-up/development-environment.md#wskdebug) en [ ngrok ](../set-up/development-environment.md#ngork) npm modules worden geïnstalleerd
1. Verzeker {de Desktop van 0} Docker en de ondersteunende beelden van Docker ](../set-up/development-environment.md#docker) geïnstalleerd en lopend zijn[
1. Sluit alle actieve actieve actieve uitvoeringsinstanties van Development Tool.
1. Implementeer de nieuwste code met `aio app deploy` en neem de naam van de geïmplementeerde actie op (naam tussen de `[...]` ). Hiermee werkt u de `launch.json` in stap 8 bij.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. Een nieuwe instantie van het gereedschap Asset compute ontwikkelen starten met de opdracht `npx adobe-asset-compute devtool`
1. Tik in VS-code op het pictogram Foutopsporing in de linkernavigatie
   + Indien ertoe aangezet, creeer de aanraking __een launch.json- dossier > Node.js__ om een nieuw `launch.json` dossier tot stand te brengen.
   + Elders, tik het __pictogram van het Gear__ rechts van __Programma van de Lancering__ dropdown om het bestaan `launch.json` in de redacteur te openen.
1. Voeg de volgende JSON-objectconfiguratie toe aan de array `configurations` :

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

1. Selecteer nieuwe __wskdebug__ van dropdown
1. Tik de groene __knoop van de Looppas__ links van __wskdebug__ dropdown
1. Open `/actions/worker/index.js` en tik links van de regelnummers om onderbrekingspunten 1 toe te voegen. Navigeer aan het Browser van het Web van het Hulpmiddel van de Ontwikkeling van de Asset compute die venster in stap 6 wordt geopend
1. Tik de __Looppas__ knoop om de worker uit te voeren
1. Navigeer terug naar de VS-code, naar `/actions/worker/index.js` en doorloop de code
1. Tik op `Ctrl-C` in de terminal die de opdracht `npx adobe-asset-compute devtool` in stap 6 heeft uitgevoerd om het hulpprogramma voor foutopsporing af te sluiten

## Logbestanden openen vanuit Adobe I/O Runtime{#aio-app-logs}

[ AEM as a Cloud Service hefboomwerkingen de arbeiders van de Asset compute via de Profielen van de Verwerking ](../deploy/processing-profiles.md) door hen in Adobe I/O Runtime direct aan te halen. Omdat deze aanroepen geen lokale ontwikkeling impliceren, kunnen hun uitvoeringen niet worden gezuiverd gebruikend lokaal hulpmiddel zoals het Hulpmiddel van de Ontwikkeling van de Asset compute of wskdebug. In plaats daarvan, kan Adobe I/O CLI worden gebruikt om logboeken van de worker te halen die in een bepaalde werkruimte in Adobe I/O Runtime wordt uitgevoerd.

1. Verzeker de [ werkruimte-specifieke milieuvariabelen ](../deploy/runtime.md) via `AIO_runtime_namespace` en `AIO_runtime_auth` worden geplaatst, die op de werkruimte wordt gebaseerd die het zuiveren vereisen.
1. Uitvoeren vanaf de opdrachtregel `aio app logs`
   + Als er veel verkeer is in de werkruimte, vouwt u het aantal activeringslogboeken uit via de markering `--limit` :
     `$ aio app logs --limit=25`
1. De meest recente (tot en met de meegeleverde `--limit` ) activeringslogboeken worden geretourneerd als uitvoer van de opdracht ter controle.

   ![ de logboeken van de ao app ](./assets/debug/aio-app-logs.png)

## Problemen oplossen

+ [Foutopsporing wordt niet gekoppeld](../troubleshooting.md#debugger-does-not-attach)
+ [Onderbrekingspunten worden niet gepauzeerd](../troubleshooting.md#breakpoints-no-pausing)
+ [Foutopsporing VS-code niet gekoppeld](../troubleshooting.md#vs-code-debugger-not-attached)
+ [Foutopsporing voor VS-code gekoppeld nadat uitvoering van worker is gestart](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Worker-time-out tijdens foutopsporing](../troubleshooting.md#worker-times-out-while-debugging)
+ [Kan foutopsporingsproces niet beëindigen](../troubleshooting.md#cannot-terminate-debugger-process)
