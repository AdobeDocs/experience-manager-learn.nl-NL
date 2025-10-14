---
title: Een lokale ontwikkelomgeving instellen voor uitbreidbaarheid met Asset Compute
description: Het ontwikkelen van de arbeiders van Asset Compute, die de toepassingen van Node.js JavaScript zijn, vereist specifieke ontwikkelingshulpmiddelen die van traditionele ontwikkeling van AEM verschillen, die zich van Node.js en diverse npm modules aan de Desktop van de Docker en Code van Microsoft Visual Studio uitstrekken.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
duration: 96
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---

# Lokale ontwikkelomgeving instellen

Adobe Asset Compute-projecten kunnen niet worden geïntegreerd met de lokale AEM-runtime die door de AEM SDK wordt geleverd en worden ontwikkeld met behulp van hun eigen gereedschapsketen, los van die welke door AEM-toepassingen wordt vereist op basis van het AEM Maven-projectarchetype.

Als u de Asset Compute-microservices wilt uitbreiden, moeten de volgende gereedschappen zijn geïnstalleerd op de lokale ontwikkelaarscomputer.

## Verkorte instructies voor het instellen

Hier volgt een korte set-upinstructies. Nadere bijzonderheden over deze ontwikkelingstools worden hieronder in afzonderlijke secties beschreven.

1. [&#x200B; installeer de Desktop van de Dokker &#x200B;](https://www.docker.com/products/docker-desktop) en trek de vereiste beelden van de Dokker:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [&#x200B; installeer de Code van Visual Studio &#x200B;](https://code.visualstudio.com/download)
1. [Node.js 10+ installeren](../../local-development-environment/development-tools.md#node-js)
1. Installeer de vereiste npm-modules en Adobe I/O CLI-plug-ins vanaf de opdrachtregel:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Lees de onderstaande secties voor meer informatie over de instructies voor een verkorte installatie.

## Visual Studio-code installeren{#vscode}

[&#x200B; de Code van Microsoft Visual Studio &#x200B;](https://code.visualstudio.com/download) wordt gebruikt voor het ontwikkelen van en het zuiveren van de arbeiders van Asset Compute. Terwijl andere [&#x200B; JavaScript-compatibele winde &#x200B;](../../local-development-environment/development-tools.md#set-up-the-development-ide) kan worden gebruikt om de worker te ontwikkelen, slechts kan de Code van Visual Studio aan [&#x200B; worden geïntegreerd zuiveren &#x200B;](../test-debug/debug.md) de arbeider van Asset Compute.

Dit leerprogramma veronderstelt het gebruik van de Code van Visual Studio aangezien het de beste ontwikkelaarervaring voor het uitbreiden van Asset Compute verstrekt.

## Docker-bureaublad installeren{#docker}

De download en installeert de recentste, stabiele [&#x200B; Desktop van de Dakker &#x200B;](https://www.docker.com/products/docker-desktop), aangezien dit wordt vereist om [&#x200B; te testen &#x200B;](../test-debug/test.md) en [&#x200B; zuiver &#x200B;](../test-debug/debug.md) projecten van Asset Compute plaatselijk.

Nadat u Docker Desktop hebt geïnstalleerd, start u deze en installeert u de volgende Docker-afbeeldingen vanaf de opdrachtregel:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Ontwikkelaars op Windows-computers moeten ervoor zorgen dat ze Linux-containers gebruiken voor de bovenstaande afbeeldingen. De stappen om op de containers van Linux over te schakelen worden beschreven in [&#x200B; Docker voor de documentatie van Vensters &#x200B;](https://docs.docker.com/docker-for-windows/).

## Node.js (en npm) installeren{#node-js}

De arbeiders van Asset Compute zijn [&#x200B; Node.js &#x200B;](https://nodejs.org/)-Gebaseerd, en vereisen daarom Node.js 10+ (en npm) om te ontwikkelen en te bouwen.

+ [&#x200B; installeer Node.js (en npm) &#x200B;](../../local-development-environment/development-tools.md#node-js) op de zelfde manier zoals voor traditionele ontwikkeling van AEM.

## Adobe I/O CLI installeren{#aio}

[&#x200B; installeer Adobe I/O CLI &#x200B;](../../local-development-environment/development-tools.md#aio-cli), of __lucht__ is een bevel-lijn (CLI) npm module die gebruik van en interactie met de technologieën van Adobe I/O vergemakkelijkt, en voor zowel produceert als ontwikkelt plaatselijk de arbeiders van douaneAsset Compute.

```
$ npm install -g @adobe/aio-cli
```

## De Adobe I/O CLI Asset Compute-insteekmodule installeren{#aio-asset-compute}

De [&#x200B; Adobe I/O CLI Asset Compute stop &#x200B;](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Webfoutopsporing installeren{#wskdebug}

De download en installeert [&#x200B; Apache OpenWhisk zuivert &#x200B;](https://www.npmjs.com/package/@openwhisk/wskdebug) npm module om lokaal het zuiveren van de arbeiders van Asset Compute te vergemakkelijken.

_Code 1.48.x+ van Visual Studio wordt vereist voor [&#x200B; wskdebug &#x200B;](#wskdebug) om te werken._

```
$ npm install -g @openwhisk/wskdebug
```

## Installeer de extensie{#ngrok}

Download en installeer de [&#x200B; ngrok &#x200B;](https://www.npmjs.com/package/ngrok) npm module, die openbare toegang tot uw lokale ontwikkelingsmachine verleent, om lokaal het zuiveren van de arbeiders van Asset Compute te vergemakkelijken.

```
$ npm install -g ngrok --unsafe-perm=true
```
