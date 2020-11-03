---
title: De Verrekbaarheid van de microservices van activa voor AEM als Cloud Service
description: Deze zelfstudie doorloopt het maken van een eenvoudige worker Asset Compute die een elementuitvoering maakt door het oorspronkelijke element aan een cirkel uit te snijden en configureerbare contrast en helderheid toepast. Hoewel de worker zelf een basis is, wordt deze zelfstudie gebruikt om te verkennen hoe een aangepaste worker Asset Compute kan worden gemaakt, ontwikkeld en geïmplementeerd voor gebruik met AEM als Cloud Service.
feature: asset-compute
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1028'
ht-degree: 0%

---


# Uitbreidbaarheid van microservices voor middelenberekening

AEM als de microservices van Asset Compute van de Cloud Service ondersteunen de ontwikkeling en implementatie van aangepaste workers die worden gebruikt om binaire gegevens van elementen die zijn opgeslagen in AEM te lezen en te manipuleren, meestal om aangepaste elementuitvoeringen te maken.

Terwijl in AEM 6.x de processen van het AEMWerkschema werden gebruikt om, activa te lezen om te transformeren en terug te schrijven vertoningen, in AEM als de arbeiders van de Rekenmachine van de Activa van de Cloud Service aan deze behoefte voldoen.

## Wat u gaat doen

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Deze zelfstudie doorloopt het maken van een eenvoudige worker Asset Compute die een elementuitvoering maakt door het oorspronkelijke element aan een cirkel uit te snijden en configureerbare contrast en helderheid toepast. Hoewel de worker zelf een basis is, wordt deze zelfstudie gebruikt om te verkennen hoe een aangepaste worker Asset Compute kan worden gemaakt, ontwikkeld en geïmplementeerd voor gebruik met AEM als Cloud Service.

### Doelstellingen {#objective}

1. Verlening en opzet van de noodzakelijke rekeningen en diensten om een medewerker van de Acquute Asset te bouwen en op te stellen
1. Een project voor het berekenen van bedrijfsmiddelen maken en configureren
1. Een worker om middelen te compileren ontwikkelen die een aangepaste uitvoering genereert
1. Schrijf tests voor, en leer hoe te om de arbeider te zuiveren van het douaneElement Compute
1. Implementeer de worker Asset Compute en integreer deze AEM als een Cloud Service Author-service via het verwerken van profielen

## Instellen

Leer hoe te om behoorlijk voor te bereiden voor de uitbreiding van de arbeiders van de Compute van Activa, en te begrijpen welke diensten en rekeningen moeten worden provisioned en worden gevormd, en software plaatselijk voor ontwikkeling wordt geïnstalleerd.

### Account en service-provisioning{#accounts-and-services}

De volgende accounts en services vereisen provisioning en toegang tot Adobe Project Firefly en Microsoft Azure Blob Storage om de zelfstudie te voltooien, AEM als een Cloud Service Dev-omgeving of Sandbox-programma.

+ [Rekeningen en diensten](./set-up/accounts-and-services.md)

### Lokale ontwikkelomgeving

Voor de lokale ontwikkeling van Asset Compute-projecten is een specifieke set ontwikkelaars nodig die verschilt van de traditionele AEM-ontwikkeling, waaronder: Microsoft Visual Studio Code, de Desktop van de Dokker, Node.js en het steunen npm modules.

+ [Lokale ontwikkelomgeving instellen](./set-up/development-environment.md)

### Adobe Project Firefly

De projecten van de Compute van middelen zijn speciaal bepaalde projecten van het Project van Adobe Vuurwerk, en als dusdanig, vereisen toegang tot het Project van Adobe in de Console van de Ontwikkelaar van de Adobe om hen te vestigen en op te stellen.

+ [Adobe-project probleemloos instellen](./set-up/firefly.md)

## Ontwikkelen

Leer hoe u een project Asset Compute maakt en configureert en vervolgens een aangepaste worker ontwikkelt die een uitvoering van een op maat gemaakt element genereert.

### Een nieuw project voor het berekenen van bedrijfsmiddelen maken

De projecten van de Compute van activa, die één of meerdere arbeiders van de Rekenmachine van Activa bevatten, worden geproduceerd gebruikend interactieve Adobe I/O CLI. De projecten van de Activa Compute zijn speciaal gestructureerde projecten van het Project van de Adobe Firefly, die op hun beurt projecten Node.js zijn.

+ [Een nieuw project voor het berekenen van bedrijfsmiddelen maken](./develop/project.md)

### Omgevingsvariabelen configureren

Omgevingsvariabelen worden in het `.env` bestand bewaard voor lokale ontwikkeling en worden gebruikt om de I/O-gegevens en gegevens voor cloudopslag te verstrekken die vereist zijn voor lokale ontwikkeling.

+ [Omgevingsvariabelen configureren](./develop/environment-variables.md)

### Vorm manifest.yml

De projecten van de Compute van activa bevatten manifests die alle arbeiders bepalen van de Verwerking van Activa binnen het project, evenals welke middelen zij wanneer opgesteld aan Adobe I/O Runtime voor uitvoering beschikbaar hebben.

+ [Vorm manifest.yml](./develop/manifest.md)

### Een worker ontwikkelen

Het ontwikkelen van een worker Asset Compute is de kern van het uitbreiden van Asset Compute-microservices, aangezien de worker de aangepaste code bevat die het genereren van de resulterende asset-uitvoering genereert of ordent.

+ [Een worker Asset Compute ontwikkelen](./develop/worker.md)

### Het hulpprogramma voor het ontwikkelen van bedrijfsmiddelen gebruiken

Het hulpmiddel van de Ontwikkeling van de Verwerking van Activa verstrekt een lokaal Web-toestel voor het opstellen, uitvoeren en voorvertonen van worker-geproduceerde vertoningen, ondersteunend snelle en iteratieve de arbeidersontwikkeling van Activa Compute.

+ [Het hulpprogramma voor het ontwikkelen van bedrijfsmiddelen gebruiken](./develop/development-tool.md)

## Testen en fouten opsporen

Leer hoe te om de arbeiders van de Compute van het douaneMiddelen te testen om in hun verrichting zeker te zijn, en zuivert de arbeiders van de Compute van Activa om te begrijpen en problemen op te lossen hoe de douanecode wordt uitgevoerd.

### Een worker testen

Asset Compute biedt een testkader voor het maken van testreeksen voor workers, waarbij u tests definieert die ervoor zorgen dat correct gedrag eenvoudig is.

+ [Een worker testen](./test-debug/test.md)

### Fouten opsporen in een worker

De arbeiders van de Compute van activa verstrekken diverse niveaus van het zuiveren van traditionele `console.log(..)` output, aan integratie met de Code __van__ VS en __wskdebug__, toestaand ontwikkelaars stap door arbeiderscode aangezien het in echt - tijd uitvoert.

+ [Fouten opsporen in een worker](./test-debug/debug.md)

## Implementeren

Leer hoe u workers voor het samenstellen van aangepaste middelen als Cloud Service integreert met AEM, door deze eerst te implementeren in Adobe I/O Runtime en vervolgens bij AEM aan te roepen als Cloud Service Auteur via de verwerkingsprofielen van AEM Assets.

### Distribueren naar Adobe I/O Runtime

De arbeiders van de Compute van activa moeten aan Adobe I/O Runtime worden opgesteld om met AEM als Cloud Service te worden gebruikt.

+ [Verwerkingsprofielen gebruiken](./deploy/runtime.md)

### Workers integreren via AEM verwerkingsprofielen

Zodra de medewerkers van Asset Compute zijn geïmplementeerd in Adobe I/O Runtime, kunnen deze in AEM worden geregistreerd als Cloud Service via [middelenverwerkingsprofielen](../../assets/configuring/processing-profiles.md). Verwerkingsprofielen worden op hun beurt toegepast op de mappen met elementen die op de elementen ervan van toepassing zijn.

+ [Integreren met AEM verwerkingsprofielen](./deploy/processing-profiles.md)

## Geavanceerd

Deze verkorte zelfstudies behandelen geavanceerdere gebruiksgevallen, voortbouwend op basiskennis die in de voorgaande hoofdstukken is vastgelegd.

+ [Ontwikkelen van een worker](./advanced/metadata.md) voor het berekenen van metagegevens van bedrijfsmiddelen die metagegevens naar de

## Codebase op Github

De zelfstudie is beschikbaar op Github op:

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ master vertakking

De broncode bevat niet de vereiste `.env` of `config.json` bestanden. Deze moeten worden toegevoegd en gevormd gebruikend uw [rekeningen en de dienstinformatie](#accounts-and-services) .

## Aanvullende bronnen

Hieronder volgen verschillende bronnen van Adobe die meer informatie en nuttige API&#39;s en SDK&#39;s bieden voor de ontwikkeling van workers voor het berekenen van bedrijfsmiddelen.

### Documentatie

+ [Asset Compute Service-documentatie](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [Leesmij van het Hulpmiddel van de Ontwikkeling van de Verwerking van de Activa](https://github.com/adobe/asset-compute-devtool)
+ [Voorbeeldworkers van Asset Compute](https://github.com/adobe/asset-compute-example-workers)

### API&#39;s en SDK&#39;s

+ [Asset Compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [Commentaar voor het berekenen van bedrijfsmiddelen](https://github.com/adobe/asset-compute-commons)
   + [XMP voor middelenboekhouding](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe Cloud Blobstore Wrapper library](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe Node Fetch, bibliotheek opnieuw proberen](https://github.com/adobe/node-fetch-retry)
+ [Voorbeeldworkers van Asset Compute](https://github.com/adobe/asset-compute-example-workers)
