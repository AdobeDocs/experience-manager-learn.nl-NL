---
title: Uitbreidbaarheid van asset compute microservices voor AEM as a Cloud Service
description: Deze zelfstudie doorloopt het maken van een eenvoudige Asset compute-worker die een elementuitvoering maakt door het oorspronkelijke element aan een cirkel uit te snijden en configureerbare contrast en helderheid toepast. Hoewel de worker zelf van fundamenteel belang is, wordt deze zelfstudie gebruikt om het maken, ontwikkelen en implementeren van een aangepaste Asset compute-worker voor gebruik met AEM as a Cloud Service te verkennen.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
duration: 277
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '986'
ht-degree: 0%

---

# Uitbreidbaarheid van microservices voor asset compute

AEM als Asset compute microservices van de Cloud Service ondersteunen de ontwikkeling en implementatie van aangepaste workers die worden gebruikt om binaire gegevens van elementen die zijn opgeslagen in AEM te lezen en te manipuleren, meestal om aangepaste elementuitvoeringen te maken.

Terwijl in AEM 6.x de processen van het AEMWerkschema werden gebruikt om, activa te lezen om te transformeren en terug te schrijven uitvoeringen, in de Asset compute van AEM as a Cloud Service voldoen de arbeiders aan deze behoefte.

## Wat u gaat doen

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Deze zelfstudie doorloopt het maken van een eenvoudige Asset compute-worker die een elementuitvoering maakt door het oorspronkelijke element aan een cirkel uit te snijden en configureerbare contrast en helderheid toepast. Hoewel de worker zelf van fundamenteel belang is, wordt deze zelfstudie gebruikt om het maken, ontwikkelen en implementeren van een aangepaste Asset compute-worker voor gebruik met AEM as a Cloud Service te verkennen.

### Doelstellingen {#objective}

1. Het verstrekken en opzetten van de noodzakelijke rekeningen en de diensten om een werknemer van de Asset compute te bouwen en op te stellen
1. Een Asset compute-project maken en configureren
1. Een Asset compute-worker ontwikkelen die een aangepaste uitvoering genereert
1. Schrijf tests voor, en leer hoe te om de arbeider van de douane Asset compute te zuiveren
1. Implementeer de Asset compute-worker en integreer deze met de AEM as a Cloud Service Author-service via het verwerken van profielen

## Instellen

Leer hoe te om behoorlijk voor te bereiden op het uitbreiden van de arbeiders van de Asset compute, en te begrijpen welke diensten en rekeningen moeten worden provisioned en worden gevormd, en software plaatselijk voor ontwikkeling wordt geïnstalleerd.

### Account en service-provisioning{#accounts-and-services}

De volgende accounts en services vereisen provisioning en toegang tot deze services om de zelfstudie, AEM as a Cloud Service Dev-omgeving of Sandbox-programma, toegang tot App Builder en Microsoft Azure Blob Storage te voltooien.

+ [Rekeningen en diensten](./set-up/accounts-and-services.md)

### Lokale ontwikkelomgeving

De lokale ontwikkeling van de projecten van de Asset compute vereist een specifieke reeks ontwikkelaars hulpmiddel, verschillend van traditionele AEM ontwikkeling, met inbegrip van: de Code van Microsoft Visual Studio, de Desktop van de Docker, Node.js en het steunen npm modules.

+ [Lokale ontwikkelomgeving instellen](./set-up/development-environment.md)

### App Builder

Asset compute-projecten zijn speciaal gedefinieerde App Builder-projecten en hebben als zodanig toegang tot App Builder in de Adobe Developer Console nodig om ze op te zetten en te implementeren.

+ [App Builder instellen](./set-up/app-builder.md)

## Ontwikkelen

Leer om een project van de Asset compute tot stand te brengen en te vormen en dan een douanearbeider te ontwikkelen die een op maat gemaakte activavertoning produceert.

### Een nieuw Asset compute-project maken

De projecten van de asset compute, die één of meerdere arbeiders van de Asset compute bevatten, worden geproduceerd gebruikend interactieve Adobe I/O CLI. Asset compute-projecten zijn speciaal gestructureerde App Builder-projecten, die op hun beurt weer Node.js-projecten zijn.

+ [Een nieuw Asset compute-project maken](./develop/project.md)

### Omgevingsvariabelen configureren

Omgevingsvariabelen worden bijgehouden in het `.env` -bestand voor lokale ontwikkeling en worden gebruikt om gegevens voor Adobe I/O- en cloudopslag te verstrekken die vereist zijn voor lokale ontwikkeling.

+ [Omgevingsvariabelen configureren](./develop/environment-variables.md)

### Vorm manifest.yml

De projecten van de asset compute bevatten manifests die alle Asset compute werknemers binnen het project, evenals bepalen welke middelen zij wanneer opgesteld aan Adobe I/O Runtime voor uitvoering beschikbaar hebben.

+ [Vorm manifest.yml](./develop/manifest.md)

### Een worker ontwikkelen

Het ontwikkelen van een Asset compute arbeider is de kern van het uitbreiden van de microdiensten van de Asset compute, aangezien de arbeider de douanecode bevat die, of orkest, de generatie van de resulterende activauitvoering produceert.

+ [Een Asset compute-worker ontwikkelen](./develop/worker.md)

### Het Asset compute-ontwikkelingsgereedschap gebruiken

Het Hulpmiddel van de Ontwikkeling van de Asset compute verstrekt een lokaal bezit van het Web voor het opstellen van, het uitvoeren van en het voorvertonen van worker-geproduceerde vertoningen, het steunen van snelle en iteratieve de arbeidersontwikkeling van de Asset compute.

+ [Het Asset compute-ontwikkelingsgereedschap gebruiken](./develop/development-tool.md)

## Testen en fouten opsporen

Leer hoe te om de arbeiders van de douaneAsset compute te testen om in hun verrichting vertrouwd te zijn, en zuivert de arbeiders van de Asset compute om te begrijpen en problemen op te lossen hoe de douanecode wordt uitgevoerd.

### Een worker testen

Asset compute biedt een testkader voor het maken van testreeksen voor werknemers, waarbij het definiëren van tests die ervoor zorgen dat goed gedrag eenvoudig is.

+ [Een worker testen](./test-debug/test.md)

### Fouten opsporen in een worker

De arbeiders van de asset compute verstrekken diverse niveaus van het zuiveren van traditionele `console.log(..)` output, aan integratie met __VS Code__ en __wskdebug__, toestaand ontwikkelaars stap door arbeiderscode aangezien het in echt - tijd uitvoert.

+ [Fouten opsporen in een worker](./test-debug/debug.md)

## Implementeren

Leer hoe u medewerkers van aangepaste Asset computen kunt integreren met AEM as a Cloud Service door deze eerst te implementeren in Adobe I/O Runtime en vervolgens een beroep te doen op AEM as a Cloud Service Author via AEM Assets&#39; Processing Profiles.

### Distribueren naar Adobe I/O Runtime

Asset compute werknemers moeten in Adobe I/O Runtime worden ingezet om met AEM as a Cloud Service te worden gebruikt.

+ [Verwerkingsprofielen gebruiken](./deploy/runtime.md)

### Workers integreren via AEM verwerkingsprofielen

Zodra opgesteld aan Adobe I/O Runtime, kunnen de arbeiders van de Asset compute in AEM as a Cloud Service via [ Assets Verwerkingsprofielen ](../../assets/configuring/processing-profiles.md) worden geregistreerd. Verwerkingsprofielen worden op hun beurt toegepast op de mappen met elementen die op de elementen ervan van toepassing zijn.

+ [Integreren met AEM verwerkingsprofielen](./deploy/processing-profiles.md)

## Geavanceerd

Deze verkorte zelfstudies behandelen geavanceerdere gebruiksgevallen, voortbouwend op basiskennis die in de voorgaande hoofdstukken is vastgelegd.

+ [ ontwikkelt een worker van de meta-gegevens van de Asset compute ](./advanced/metadata.md) die meta-gegevens terug naar kan schrijven

## Codebase op Github

De zelfstudie is beschikbaar op Github op:

+ [ adobe/aem-guides-wknd-asset-compute ](https://github.com/adobe/aem-guides-wknd-asset-compute) @ hoofdtak

De broncode bevat niet de vereiste `.env` - of `config.json` -bestanden. Deze moeten worden toegevoegd en worden gevormd gebruikend uw [ rekeningen en de diensten ](#accounts-and-services) informatie.

## Aanvullende bronnen

Hieronder vindt u diverse bronnen voor Adobe die aanvullende informatie en nuttige API&#39;s en SDK&#39;s bieden voor het ontwikkelen van workers in de Asset compute.

### Documentatie

+ [ documentatie van de Dienst van de Asset compute ](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html)
+ [ Rereadme van het Hulpmiddel van de Ontwikkeling van de Asset compute ](https://github.com/adobe/asset-compute-devtool)
+ [ de voorbeeldarbeiders van de Asset compute ](https://github.com/adobe/asset-compute-example-workers)

### API&#39;s en SDK&#39;s

+ [ Asset compute SDK ](https://github.com/adobe/asset-compute-sdk)
   + [ Commons van de Asset compute ](https://github.com/adobe/asset-compute-commons)
   + [ Asset compute XMP ](https://github.com/adobe/asset-compute-xmp#readme)
+ [ Adobe Cloud Blobstore Wrapper library ](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [ de Vetch van de Knoop van de Adobe probeert bibliotheek ](https://github.com/adobe/node-fetch-retry) opnieuw
+ [ de voorbeeldarbeiders van de Asset compute ](https://github.com/adobe/asset-compute-example-workers)
