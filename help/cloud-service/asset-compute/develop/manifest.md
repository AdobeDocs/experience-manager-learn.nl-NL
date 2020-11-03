---
title: Vorm manifest.yml van een project van de Verwerking van Activa
description: Met Asset Compute wordt manifest.yml van het project beschreven welke workers in dit project moeten worden geïmplementeerd.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---


# Vorm manifest.yml

In `manifest.yml`de hoofdmap van het project Asset Compute worden alle workers in dit project beschreven die moeten worden geïmplementeerd.

![manifest.yml](./assets/manifest/manifest.png)

## Standaarddefinitie van worker

Workers worden gedefinieerd als Adobe I/O Runtime-handelingangen onder `actions`en samengesteld uit een set configuraties.

Workers die andere Adobe I/O-integraties gebruiken, moeten de `annotations -> require-adobe-auth` eigenschap instellen op `true` omdat deze de Adobe I/O-gegevens [van de worker via het](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) object `params.auth` toegankelijk maakt. Dit is doorgaans vereist wanneer de worker API&#39;s van Adobe I/O aanroept, zoals de Adobe Photoshop-, Lightroom- of Sensei-API&#39;s, en deze API&#39;s kunnen per worker in- en uitschakelen.

1. Open en bekijk de automatisch gegenereerde worker `manifest.yml`. Projecten die meerdere workers voor middelenberekening bevatten, moeten een vermelding definiëren voor elke worker onder de `actions` array.

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: # the array of workers, since we have a single worker there is only one entry beneath actions
      worker: # the auto-generated worker definition
        function: actions/worker/index.js # the entry point to the worker 
        web: 'yes'  # as our worker is invoked over HTTP from AEM Author service
        runtime: 'nodejs:12' # the target nodejs runtime (only 10 and 12 are supported)
        limits:
          concurrency: 10
        annotations:
          require-adobe-auth: true # set to true, to pass through Adobe I/O access token/client id via params.auth in the worker, typically required when the worker calls out to Adobe I/O APIs such as the Adobe Photoshop, Lightroom or Sensei APIs.
```

## Limieten definiëren

Elke worker kan de [limieten](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) voor de uitvoeringscontext in Adobe I/O Runtime configureren. Deze waarden moeten zodanig worden ingesteld dat de worker een optimale grootte krijgt op basis van het volume, de snelheid en het type elementen dat de worker berekent, en het type werk dat de worker uitvoert.

Lees de [Adobe-richtlijnen](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#sizing-workers) voordat u limieten instelt. Workers van Asset Compute hebben bij het verwerken van middelen mogelijk onvoldoende geheugen, waardoor de uitvoering van Adobe I/O Runtime wordt gedood. Zo weet u zeker dat de grootte van de worker correct is aangepast aan de afhandeling van alle kandidaat-middelen.

1. Voeg een `inputs` sectie toe aan het nieuwe item voor `wknd-asset-compute` handelingen. Hierdoor kan de algemene prestaties en de toewijzing van bronnen van de worker Asset Compute worden afgesteld.

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits: # Allows for the tuning of the worker's performance
          timeout: 60000 # timeout in milliseconds (1 minute)
          memorySize: 512 # memory allocated in MB; if the worker offloads heavy computational work to other Web services this number can be reduced
          concurrency: 10 # adjust based on expected concurrent processing and timeout 
        annotations:
          require-adobe-auth: true
           
```

## Het voltooide manifest.yml

De uiteindelijke `manifest.yml` vormgeving ziet er als volgt uit:

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
```

## manifest.yml op Github

De finale `.manifest.yml` is beschikbaar op Github op:

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## manifest.yml valideren

Nadat het gegenereerde Asset Compute-programma `manifest.yml` is bijgewerkt, voert u het programma voor lokale ontwikkeling uit en zorgt u ervoor dat het programma met succes begint met de bijgewerkte `manifest.yml` instellingen.

Om het Hulpmiddel van de Ontwikkeling van de Verwerking van Activa voor het project van de Verwerking van Activa te beginnen:

1. Open een bevellijn in het Activum Compute projectwortel (in de Code van VS kan dit direct in winde via Terminal > Nieuwe Terminal) worden geopend, en voer het bevel uit:

   ```
   $ aio app run
   ```

1. Het lokale Hulpmiddel van de Ontwikkeling van het Vermogen van Activa zal in uw standaardbrowser van het Web op __http://localhost:9000__ openen.

   ![AIR-app uitgevoerd](assets/environment-variables/aio-app-run.png)

1. Bekijk de uitvoer van de opdrachtregel en de webbrowser voor foutberichten terwijl het hulpprogramma Ontwikkeling wordt geïnitialiseerd.
1. Tik in het venster dat is uitgevoerd om het proces te beëindigen `Ctrl-C` `aio app run` om het Asset Compute Development Tool te stoppen.

## Problemen oplossen

+ [Onjuiste JAML-inspringing](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize limit is set to low](../troubleshooting.md#memorysize-limit-is-set-too-low)
