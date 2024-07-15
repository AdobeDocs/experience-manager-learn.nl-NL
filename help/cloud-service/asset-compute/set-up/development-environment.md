---
title: Een lokale ontwikkelomgeving instellen voor uitbreidbaarheid van de Asset compute
description: Het ontwikkelen van de arbeiders van de Asset compute, die de toepassingen van Node.js JavaScript zijn, vereist specifieke ontwikkelingshulpmiddelen die van traditionele AEM ontwikkeling verschillen, die zich van Node.js en diverse npm modules aan de Desktop van de Docker en Code van Microsoft Visual Studio uitstrekken.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
duration: 96
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---

# Lokale ontwikkelomgeving instellen

De projecten van de Asset compute van de Adobe kunnen niet met lokale AEM runtime worden geïntegreerd die door AEM SDK wordt verstrekt en worden ontwikkeld gebruikend hun eigen hulpmiddelketting, los van die die die door AEM toepassingen wordt vereist die op het AEM Maven projectarchetype worden gebaseerd.

Als u de Asset compute-microservices wilt uitbreiden, moeten de volgende gereedschappen zijn geïnstalleerd op de lokale ontwikkelaarscomputer.

## Verkorte instructies voor het instellen

Hier volgt een korte set-upinstructies. Nadere bijzonderheden over deze ontwikkelingstools worden hieronder in afzonderlijke secties beschreven.

1. [ installeer de Desktop van de Dokker ](https://www.docker.com/products/docker-desktop) en trek de vereiste beelden van de Dokker:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [ installeer de Code van Visual Studio ](https://code.visualstudio.com/download)
1. [Node.js 10+ installeren](../../local-development-environment/development-tools.md#node-js)
1. Installeer de vereiste npm modules en Adobe I/O CLI stop-ins van de bevellijn:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Lees de onderstaande secties voor meer informatie over de instructies voor een verkorte installatie.

## Visual Studio-code installeren{#vscode}

[ de Code van Microsoft Visual Studio ](https://code.visualstudio.com/download) wordt gebruikt voor het ontwikkelen van en het zuiveren van de arbeiders van de Asset compute. Terwijl andere [ JavaScript-compatibele winde ](../../local-development-environment/development-tools.md#set-up-the-development-ide) kan worden gebruikt om de worker te ontwikkelen, slechts kan de Code van Visual Studio aan [ worden geïntegreerd zuiveren ](../test-debug/debug.md) de arbeider van de Asset compute.

Dit leerprogramma veronderstelt het gebruik van de Code van Visual Studio aangezien het de beste ontwikkelaarervaring voor het uitbreiden van Asset compute verstrekt.

## Docker-bureaublad installeren{#docker}

De download en installeert de recentste, stabiele [ Desktop van de Dakker ](https://www.docker.com/products/docker-desktop), aangezien dit wordt vereist om [ te testen ](../test-debug/test.md) en [ zuiver ](../test-debug/debug.md) projecten van de Asset compute plaatselijk.

Nadat u Docker Desktop hebt geïnstalleerd, start u deze en installeert u de volgende Docker-afbeeldingen vanaf de opdrachtregel:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Ontwikkelaars op Windows-computers moeten ervoor zorgen dat ze Linux-containers gebruiken voor de bovenstaande afbeeldingen. De stappen om op de containers van Linux over te schakelen worden beschreven in [ Docker voor de documentatie van Vensters ](https://docs.docker.com/docker-for-windows/).

## Node.js (en npm) installeren{#node-js}

De arbeiders van de asset compute zijn ](https://nodejs.org/) gebaseerd 0} Node.js, en vereisen daarom Node.js 10+ (en npm) om te ontwikkelen en te bouwen.[

+ [ installeer Node.js (en npm) ](../../local-development-environment/development-tools.md#node-js) op de zelfde manier zoals voor traditionele AEM ontwikkeling.

## Adobe I/O CLI installeren{#aio}

[ installeer de Adobe I/O CLI ](../../local-development-environment/development-tools.md#aio-cli), of __lucht__ is een bevel-lijn (CLI) npm module die gebruik van en interactie met de technologieën van Adobe I/O vergemakkelijkt, en voor zowel produceert als ontwikkelt plaatselijk de arbeiders van de douaneAsset compute.

```
$ npm install -g @adobe/aio-cli
```

## De Adobe I/O CLI-Asset compute-insteekmodule installeren{#aio-asset-compute}

De [ Adobe I/O CLI Asset compute stop ](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Webfoutopsporing installeren{#wskdebug}

De download en installeert [ Apache OpenWhisk zuivert ](https://www.npmjs.com/package/@openwhisk/wskdebug) npm module om lokaal het zuiveren van de arbeiders van de Asset compute te vergemakkelijken.

_Code 1.48.x+ van Visual Studio wordt vereist voor [ wskdebug ](#wskdebug) om te werken._

```
$ npm install -g @openwhisk/wskdebug
```

## Installeer de extensie{#ngrok}

Download en installeer de [ ngrok ](https://www.npmjs.com/package/ngrok) npm module, die openbare toegang tot uw lokale ontwikkelingsmachine verleent, om lokaal het zuiveren van de arbeiders van de Asset compute te vergemakkelijken.

```
$ npm install -g ngrok --unsafe-perm=true
```
