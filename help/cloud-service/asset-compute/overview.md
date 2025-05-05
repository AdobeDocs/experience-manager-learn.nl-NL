---
title: Uitbreidbaarheid van Asset Compute-microservices voor AEM as a Cloud Service
description: Deze zelfstudie doorloopt het maken van een eenvoudige Asset Compute-worker die een elementuitvoering maakt door het oorspronkelijke element uit te snijden op een cirkel en past configureerbare contrast en helderheid toe. Hoewel de worker zelf van fundamenteel belang is, wordt deze zelfstudie gebruikt om het maken, ontwikkelen en implementeren van een aangepaste Asset Compute-worker voor gebruik met AEM as a Cloud Service te verkennen.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
duration: 277
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '986'
ht-degree: 0%

---

# Uitbreidbaarheid van Asset Compute-microservices

AEM als Cloud Service Asset Compute-microservices ondersteunen de ontwikkeling en implementatie van aangepaste workers die worden gebruikt om binaire gegevens van in AEM opgeslagen elementen te lezen en te manipuleren, meestal om aangepaste elementuitvoeringen te maken.

Terwijl in AEM 6.x aangepaste AEM Workflow-processen werden gebruikt om asset-uitvoeringen te lezen, te transformeren en terug te schrijven, voorzien AEM as a Cloud Service Asset Compute-workers in deze behoefte.

## Wat u gaat doen

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Deze zelfstudie doorloopt het maken van een eenvoudige Asset Compute-worker die een elementuitvoering maakt door het oorspronkelijke element uit te snijden op een cirkel en past configureerbare contrast en helderheid toe. Hoewel de worker zelf van fundamenteel belang is, wordt deze zelfstudie gebruikt om het maken, ontwikkelen en implementeren van een aangepaste Asset Compute-worker voor gebruik met AEM as a Cloud Service te verkennen.

### Doelstellingen {#objective}

1. Het verstrekken en opzetten van de noodzakelijke rekeningen en de diensten om een werknemer van Asset Compute te bouwen en op te stellen
1. Een Asset Compute-project maken en configureren
1. Een Asset Compute-worker ontwikkelen die een aangepaste uitvoering genereert
1. Schrijf tests voor, en leer hoe te om de arbeider van de douaneAsset Compute te zuiveren
1. Implementeer de Asset Compute-worker en integreer deze met de AEM as a Cloud Service Author-service via het verwerken van profielen

## Instellen

Leer hoe u zich op de juiste wijze voorbereidt voor het uitbreiden van Asset Compute-workers en begrijpt welke services en accounts moeten worden geleverd en geconfigureerd en welke software lokaal moet worden geïnstalleerd voor ontwikkeling.

### Account en service-provisioning{#accounts-and-services}

De volgende accounts en services vereisen provisioning en toegang tot deze services om de zelfstudie, AEM as a Cloud Service Dev-omgeving of Sandbox-programma, toegang tot App Builder en Microsoft Azure Blob Storage te voltooien.

+ [Rekeningen en diensten](./set-up/accounts-and-services.md)

### Lokale ontwikkelomgeving

Voor de lokale ontwikkeling van Asset Compute-projecten is een specifieke set ontwikkelaars nodig die verschilt van de traditionele AEM-ontwikkeling, waaronder Microsoft Visual Studio Code, Docker Desktop, Node.js en ondersteunende npm-modules.

+ [Lokale ontwikkelomgeving instellen](./set-up/development-environment.md)

### App Builder

Asset Compute-projecten zijn speciaal gedefinieerde App Builder-projecten en hebben als zodanig toegang tot App Builder in de Adobe Developer Console nodig om ze op te zetten en te implementeren.

+ [App Builder instellen](./set-up/app-builder.md)

## Ontwikkelen

Leer hoe u een Asset Compute-project maakt en configureert en vervolgens een aangepaste worker ontwikkelt die een uitvoering voor op maat gemaakte middelen genereert.

### Een nieuw Asset Compute-project maken

Asset Compute-projecten, die een of meer Asset Compute-workers bevatten, worden gegenereerd met behulp van de interactieve Adobe I/O CLI. Asset Compute-projecten zijn speciaal gestructureerde App Builder-projecten, die op hun beurt weer Node.js-projecten zijn.

+ [Een nieuw Asset Compute-project maken](./develop/project.md)

### Omgevingsvariabelen configureren

Omgevingsvariabelen worden bijgehouden in het `.env` -bestand voor lokale ontwikkeling en worden gebruikt om Adobe I/O-referenties en gegevens voor cloudopslag te verstrekken die vereist zijn voor lokale ontwikkeling.

+ [Omgevingsvariabelen configureren](./develop/environment-variables.md)

### Vorm manifest.yml

Asset Compute-projecten bevatten manifesten waarin alle Asset Compute-medewerkers in het project worden gedefinieerd, en welke middelen zij ter uitvoering hebben wanneer zij naar Adobe I/O Runtime worden ingezet.

+ [Vorm manifest.yml](./develop/manifest.md)

### Een worker ontwikkelen

Het ontwikkelen van een Asset Compute-worker vormt de kern van het uitbreiden van Asset Compute-microservices, aangezien de worker de aangepaste code bevat die het genereren van de resulterende asset-uitvoering genereert of ordent.

+ [Een Asset Compute-worker ontwikkelen](./develop/worker.md)

### Asset Compute-ontwikkelingsprogramma gebruiken

Het Asset Compute Development Tool biedt een lokale webhardware voor het implementeren, uitvoeren en voorvertonen van door workers gegenereerde uitvoeringen, en ondersteunt snelle en iteratieve ontwikkeling van Asset Compute-workers.

+ [Asset Compute-ontwikkelingsprogramma gebruiken](./develop/development-tool.md)

## Testen en fouten opsporen

Leer hoe u aangepaste Asset Compute-workers kunt testen om vertrouwd te zijn met hun bewerking en hoe u fouten kunt opsporen in Asset Compute-workers om te begrijpen en problemen op te lossen hoe de aangepaste code wordt uitgevoerd.

### Een worker testen

Asset Compute biedt een testkader voor het maken van testreeksen voor werknemers, waarbij het definiëren van tests die ervoor zorgen dat goed gedrag eenvoudig is.

+ [Een worker testen](./test-debug/test.md)

### Fouten opsporen in een worker

De arbeiders van Asset Compute verstrekken diverse niveaus van het zuiveren van traditionele `console.log(..)` output, aan integratie met __VS Code__ en __wskdebug__, toestaand ontwikkelaars stap door arbeiderscode aangezien het in echt - tijd uitvoert.

+ [Fouten opsporen in een worker](./test-debug/debug.md)

## Implementeren

Leer hoe u aangepaste Asset Compute-workers met AEM as a Cloud Service kunt integreren door ze eerst naar Adobe I/O Runtime te implementeren en vervolgens een beroep te doen op AEM as a Cloud Service Author via AEM Assets&#39; Processing Profiles.

### Distribueren naar Adobe I/O Runtime

Asset Compute-werknemers moeten in Adobe I/O Runtime worden ingezet om met AEM as a Cloud Service te worden gebruikt.

+ [Verwerkingsprofielen gebruiken](./deploy/runtime.md)

### Workers integreren via AEM-verwerkingsprofielen

Zodra opgesteld aan Adobe I/O Runtime, kunnen de arbeiders van Asset Compute in AEM as a Cloud Service via [ de Profielen van de Verwerking van Assets ](../../assets/configuring/processing-profiles.md) worden geregistreerd. Verwerkingsprofielen worden op hun beurt toegepast op de mappen met elementen die op de elementen ervan van toepassing zijn.

+ [Integreren met AEM-verwerkingsprofielen](./deploy/processing-profiles.md)

## Geavanceerd

Deze verkorte zelfstudies behandelen geavanceerdere gebruiksgevallen, voortbouwend op basiskennis die in de voorgaande hoofdstukken is vastgelegd.

+ [ ontwikkelt een de meta-gegevensarbeider van Asset Compute ](./advanced/metadata.md) die meta-gegevens terug naar kan schrijven

## Codebase op Github

De zelfstudie is beschikbaar op Github op:

+ [ adobe/aem-guides-wknd-asset-compute ](https://github.com/adobe/aem-guides-wknd-asset-compute) @ hoofdtak

De broncode bevat niet de vereiste `.env` - of `config.json` -bestanden. Deze moeten worden toegevoegd en worden gevormd gebruikend uw [ rekeningen en de diensten ](#accounts-and-services) informatie.

## Aanvullende bronnen

Hieronder vindt u een aantal Adobe-bronnen die aanvullende informatie en nuttige API&#39;s en SDK&#39;s bieden voor de ontwikkeling van Asset Compute-workers.

### Documentatie

+ [ de documentatie van de Dienst van Asset Compute ](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html?lang=nl-NL)
+ [ het Hulpmiddel van de Ontwikkeling van Asset Compute readme ](https://github.com/adobe/asset-compute-devtool)
+ [ Asset Compute voorbeeldarbeiders ](https://github.com/adobe/asset-compute-example-workers)

### API&#39;s en SDK&#39;s

+ [ Asset Compute SDK ](https://github.com/adobe/asset-compute-sdk)
   + [ Asset Compute Commons ](https://github.com/adobe/asset-compute-commons)
   + [ Asset Compute XMP ](https://github.com/adobe/asset-compute-xmp#readme)
+ [ Adobe Cloud Blobstore Wrapper library ](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [ de Vetch van de Knoop van Adobe probeert opnieuw bibliotheek ](https://github.com/adobe/node-fetch-retry)
+ [ Asset Compute voorbeeldarbeiders ](https://github.com/adobe/asset-compute-example-workers)
