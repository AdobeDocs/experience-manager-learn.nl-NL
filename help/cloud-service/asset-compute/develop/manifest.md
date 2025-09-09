---
title: Vorm manifest.yml van een project van Asset Compute
description: Met manifest.yml van het Asset Compute-project worden alle workers in dit project beschreven die moeten worden geïmplementeerd.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 766bfaff-ade0-41c8-a395-e79dfb4b3d76
duration: 115
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 0%

---

# Vorm manifest.yml

In `manifest.yml`, dat zich in de hoofdmap van het Asset Compute-project bevindt, worden alle workers in dit project beschreven die moeten worden geïmplementeerd.

![ manifest.yml ](./assets/manifest/manifest.png)

## Standaarddefinitie van worker

Workers worden gedefinieerd als Adobe I/O Runtime-handelingangen onder `actions` en bestaan uit een set configuraties.

De arbeiders die tot andere integratie van Adobe I/O toegang hebben moeten het `annotations -> require-adobe-auth` bezit aan `true` plaatsen aangezien dit [ de geloofsbrieven van Adobe I/O van de worker ](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) via het `params.auth` voorwerp blootstelt. Dit is doorgaans verplicht wanneer de worker API&#39;s van Adobe I/O, zoals de Adobe Photoshop- of Lightroom-API&#39;s, aanroept en per worker in- en uitschakelen kan worden uitgevoerd.

1. Open en bekijk de automatisch gegenereerde worker `manifest.yml` . Voor projecten die meerdere Asset Compute-workers bevatten, moet een vermelding voor elke worker onder de array `actions` worden gedefinieerd.

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
          require-adobe-auth: true # set to true, to pass through Adobe I/O access token/client id via params.auth in the worker, typically required when the worker calls out to Adobe I/O APIs such as the Adobe Photoshop, or Lightroom.
```

## Limieten definiëren

Elke worker kan de [ grenzen ](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) voor zijn uitvoeringscontext in Adobe I/O Runtime vormen. Deze waarden moeten zodanig worden ingesteld dat de worker een optimale grootte krijgt op basis van het volume, de snelheid en het type elementen dat de worker berekent, en het type werk dat de worker uitvoert.

Herzie [ Adobe rangschikkend begeleiding ](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#sizing-workers) alvorens grenzen te plaatsen. Asset Compute-workers hebben onvoldoende geheugen tijdens het verwerken van middelen, wat tot gevolg heeft dat de Adobe I/O Runtime-executie wordt gedood. Zo weet u zeker dat de grootte van de worker correct is aangepast aan de afhandeling van alle kandidaatmiddelen.

1. Voeg een sectie `inputs` toe aan het nieuwe item voor `wknd-asset-compute` -handelingen. Hierdoor kunnen de algemene prestaties en de toewijzing van bronnen van de Asset Compute-worker worden afgestemd.

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

De uiteindelijke `manifest.yml` ziet er als volgt uit:

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

De laatste `.manifest.yml` is beschikbaar op Github op:

+ [ aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## manifest.yml valideren

Zodra de gegenereerde Asset Compute `manifest.yml` is bijgewerkt, voert u het programma voor lokale ontwikkeling uit en zorgt u ervoor dat de toepassing begint met de bijgewerkte `manifest.yml` -instellingen.

Asset Compute Development Tool starten voor het Asset Compute-project:

1. Open een bevellijn in de het projectwortel van Asset Compute (in VS Code kan dit direct in winde via Terminal > Nieuwe Terminal worden geopend), en voer het bevel uit:

   ```
   $ aio app run
   ```

1. Het lokale Hulpmiddel van de Ontwikkeling van Asset Compute zal in uw standaardbrowser van het Web bij __http://localhost :9000__openen.

   ![ de looppas van de audio app ](assets/environment-variables/aio-app-run.png)

1. Bekijk de uitvoer van de opdrachtregel en de webbrowser voor foutberichten terwijl het hulpprogramma Ontwikkeling wordt geïnitialiseerd.
1. Tik op `Ctrl-C` in het venster dat `aio app run` heeft uitgevoerd om het Asset Compute Development Tool te stoppen.

## Problemen oplossen

+ [Onjuiste JAML-inspringing](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize limit is set to low](../troubleshooting.md#memorysize-limit-is-set-too-low)
