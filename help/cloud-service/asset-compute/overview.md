---
title: asset compute microservices uitbreidbaarheid voor AEM als Cloud Service
description: Deze zelfstudie doorloopt het maken van een eenvoudige Asset compute-worker die een elementuitvoering maakt door het oorspronkelijke element aan een cirkel uit te snijden en configureerbare contrast en helderheid toepast. Hoewel de worker zelf een basis is, wordt deze zelfstudie gebruikt om te verkennen hoe een aangepaste Asset compute-worker voor gebruik met AEM als Cloud Service kan worden gemaakt, ontwikkeld en geïmplementeerd.
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1036'
ht-degree: 0%

---


# Uitbreidbaarheid van microservices voor asset compute

AEM als Asset compute microservices van de Cloud Service ondersteunen de ontwikkeling en implementatie van aangepaste workers die worden gebruikt om binaire gegevens van elementen die zijn opgeslagen in AEM te lezen en te manipuleren, meestal om aangepaste elementuitvoeringen te maken.

Terwijl in AEM 6.x de processen van het AEMWerkschema werden gebruikt om, activa te lezen om te transformeren en terug te schrijven uitvoeringen, in AEM aangezien de arbeiders van de Asset compute van de Cloud Service aan deze behoefte voldoen.

## Wat u gaat doen

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Deze zelfstudie doorloopt het maken van een eenvoudige Asset compute-worker die een elementuitvoering maakt door het oorspronkelijke element aan een cirkel uit te snijden en configureerbare contrast en helderheid toepast. Hoewel de worker zelf een basis is, wordt deze zelfstudie gebruikt om te verkennen hoe een aangepaste Asset compute-worker voor gebruik met AEM als Cloud Service kan worden gemaakt, ontwikkeld en geïmplementeerd.

### Doelstellingen {#objective}

1. Het verstrekken en opzetten van de noodzakelijke rekeningen en de diensten om een werknemer van de Asset compute te bouwen en op te stellen
1. Een Asset compute-project maken en configureren
1. Ontwikkelen om een Asset compute-worker te genereren die een aangepaste uitvoering genereert
1. Schrijf tests voor, en leer hoe te om de arbeider van de douane Asset compute te zuiveren
1. Implementeer de Asset compute-worker en integreer deze AEM als een Cloud Service Author-service via het verwerken van profielen

## Instellen

Leer hoe te om behoorlijk voor te bereiden op het uitbreiden van de arbeiders van de Asset compute, en te begrijpen welke diensten en rekeningen moeten worden provisioned en worden gevormd, en software plaatselijk voor ontwikkeling wordt geïnstalleerd.

### Account en serviceprovisioning{#accounts-and-services}

De volgende accounts en services vereisen provisioning en toegang tot Adobe Project Firefly en Microsoft Azure Blob Storage om de zelfstudie te voltooien, AEM als een Cloud Service Dev-omgeving of Sandbox-programma.

+ [Rekeningen en diensten](./set-up/accounts-and-services.md)

### Lokale ontwikkelomgeving

Voor de lokale ontwikkeling van Asset compute-projecten is een specifiek ontwikkelingsinstrument nodig dat verschilt van de traditionele AEM ontwikkeling, zoals: Microsoft Visual Studio Code, de Desktop van de Dokker, Node.js en het steunen npm modules.

+ [Lokale ontwikkelomgeving instellen](./set-up/development-environment.md)

### Adobe Project Firefly

De projecten van de asset compute zijn speciaal bepaalde projecten van het Project van de Adobe Vuurwerk, en als dusdanig, vereisen toegang tot het Project van Adobe in de Console van de Ontwikkelaar van de Adobe om hen te vestigen en op te stellen.

+ [Adobe-project probleemloos instellen](./set-up/firefly.md)

## Ontwikkelen

Leer om een project van de Asset compute tot stand te brengen en te vormen en dan een douanearbeider te ontwikkelen die een op maat gemaakte activavertoning produceert.

### Een nieuw Asset compute-project maken

De projecten van de asset compute, die één of meerdere arbeiders van de Asset compute bevatten, worden geproduceerd gebruikend interactieve Adobe I/O CLI. asset compute-projecten zijn speciaal gestructureerde projecten van de Adobe, die op hun beurt weer Node.js-projecten zijn.

+ [Een nieuw Asset compute-project maken](./develop/project.md)

### Omgevingsvariabelen configureren

Omgevingsvariabelen worden in het `.env`-bestand onderhouden voor lokale ontwikkeling en worden gebruikt om gegevens over Adobe I/O en cloudopslag te verstrekken die vereist zijn voor lokale ontwikkeling.

+ [Omgevingsvariabelen configureren](./develop/environment-variables.md)

### Vorm manifest.yml

De projecten van de asset compute bevatten manifests die alle Asset compute werknemers binnen het project, evenals bepalen welke middelen zij wanneer opgesteld aan Adobe I/O Runtime voor uitvoering beschikbaar hebben.

+ [Vorm manifest.yml](./develop/manifest.md)

### Een worker ontwikkelen

Het ontwikkelen van een Asset compute arbeider is de kern van het uitbreiden van de microdiensten van de Asset compute, aangezien de arbeider de douanecode bevat die, of orkest, de generatie van de resulterende activauitvoering produceert.

+ [Een Asset compute-worker ontwikkelen](./develop/worker.md)

### Het Asset compute Development Tool gebruiken

Het Hulpmiddel van de Ontwikkeling van de Asset compute verstrekt een lokaal bezit van het Web voor het opstellen van, het uitvoeren van en het voorvertonen van worker-geproduceerde vertoningen, het steunen van snelle en iteratieve de arbeidersontwikkeling van de Asset compute.

+ [Het Asset compute Development Tool gebruiken](./develop/development-tool.md)

## Testen en fouten opsporen

Leer hoe te om de arbeiders van de douaneAsset compute te testen om in hun verrichting vertrouwd te zijn, en zuivert de arbeiders van de Asset compute om te begrijpen en problemen op te lossen hoe de douanecode wordt uitgevoerd.

### Een worker testen

asset compute biedt een testkader voor het maken van testreeksen voor werknemers, waarbij het definiëren van tests die ervoor zorgen dat goed gedrag eenvoudig is.

+ [Een worker testen](./test-debug/test.md)

### Fouten opsporen in een worker

De arbeiders van de asset compute verstrekken diverse niveaus van het zuiveren van traditionele `console.log(..)` output, aan integratie met __VS Code__ en __wskdebug__, toestaand ontwikkelaars stap door arbeiderscode aangezien het in echt - tijd uitvoert.

+ [Fouten opsporen in een worker](./test-debug/debug.md)

## Implementeren

Leer hoe u medewerkers van aangepaste Asset computen als Cloud Service kunt integreren met AEM door deze eerst te implementeren in Adobe I/O Runtime en vervolgens bij AEM aan te roepen als Cloud Service Auteur via de verwerkingsprofielen van AEM Assets.

### Distribueren naar Adobe I/O Runtime

asset compute werknemers moeten in Adobe I/O Runtime worden ingezet om met AEM als Cloud Service te worden gebruikt.

+ [Verwerkingsprofielen gebruiken](./deploy/runtime.md)

### Workers integreren via AEM verwerkingsprofielen

Zodra de arbeiders van de Asset compute aan Adobe I/O Runtime worden opgesteld, kunnen in AEM als Cloud Service via [Profielen van de Verwerking van Activa](../../assets/configuring/processing-profiles.md) worden geregistreerd. Verwerkingsprofielen worden op hun beurt toegepast op de mappen met elementen die op de elementen ervan van toepassing zijn.

+ [Integreren met AEM verwerkingsprofielen](./deploy/processing-profiles.md)

## Geavanceerd

Deze verkorte zelfstudies behandelen geavanceerdere gebruiksgevallen, voortbouwend op basiskennis die in de voorgaande hoofdstukken is vastgelegd.

+ [Ontwikkelen van een ](./advanced/metadata.md) werkruimte met metagegevens voor Asset computen die metagegevens naar de

## Codebase op Github

De zelfstudie is beschikbaar op Github op:

+ [master vertakking adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @

De broncode bevat niet de vereiste `.env`- of `config.json`-bestanden. Deze moeten worden toegevoegd en gevormd gebruikend uw [rekeningen en de diensten](#accounts-and-services) informatie.

## Aanvullende bronnen

Hieronder volgen verschillende bronnen van Adobe die meer informatie en nuttige API&#39;s en SDK&#39;s bieden voor het ontwikkelen van workers in de Asset compute.

### Documentatie

+ [asset compute Service-documentatie](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [Gereedschap asset compute ontwikkelen Lees mij](https://github.com/adobe/asset-compute-devtool)
+ [asset compute voorbeeldworkers](https://github.com/adobe/asset-compute-example-workers)

### API&#39;s en SDK&#39;s

+ [asset compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [asset compute Commons](https://github.com/adobe/asset-compute-commons)
   + [asset compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe Cloud Blobstore Wrapper library](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe Node Fetch, bibliotheek opnieuw proberen](https://github.com/adobe/node-fetch-retry)
+ [asset compute voorbeeldworkers](https://github.com/adobe/asset-compute-example-workers)
