---
title: Een lokale ontwikkelomgeving instellen voor uitbreidbaarheid van de Asset compute
description: Het ontwikkelen van de arbeiders van de Asset compute, die toepassingen Node.js JavaScript zijn, vereist specifieke ontwikkelingshulpmiddelen die van traditionele AEM ontwikkeling verschillen, die zich van Node.js en diverse npm modules aan de Desktop van de Docker en Code van Microsoft Visual Studio uitstrekken.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 0%

---

# Lokale ontwikkelomgeving instellen

De projecten van de Asset compute van Adobe kunnen niet met lokale AEM runtime worden geïntegreerd die door AEM SDK wordt verstrekt en worden ontwikkeld gebruikend hun eigen hulpmiddelketting, los van die die door AEM toepassingen wordt vereist die op het AEM Maven projectarchetype worden gebaseerd.

Als u de Asset compute-microservices wilt uitbreiden, moeten de volgende gereedschappen zijn geïnstalleerd op de lokale ontwikkelaarscomputer.

## Verkorte instructies voor het instellen

Hier volgt een korte set-upinstructies. Nadere bijzonderheden over deze ontwikkelingstools worden hieronder in afzonderlijke secties beschreven.

1. [Docker-bureaublad installeren](https://www.docker.com/products/docker-desktop) en trekt u de vereiste Docker-afbeeldingen:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Visual Studio-code installeren](https://code.visualstudio.com/download)
1. [Node.js 10+ installeren](../../local-development-environment/development-tools.md#node-js)
1. Installeer de vereiste npm modules en Adobe I/O CLI stop-ins van de bevellijn:

   ```
   $ npm i -g @adobe/aio-cli@7.1.0 @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Lees de onderstaande secties voor meer informatie over de instructies voor een verkorte installatie.

## Visual Studio-code installeren{#vscode}

[Microsoft Visual Studio-code](https://code.visualstudio.com/download) wordt gebruikt voor het ontwikkelen van en het zuiveren van de arbeiders van de Asset compute. Terwijl andere [JavaScript-compatibele IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide) kan worden gebruikt om de arbeider te ontwikkelen, slechts kan de Code van Visual Studio aan worden geïntegreerd [foutopsporing](../test-debug/debug.md) asset compute worker.

Dit leerprogramma veronderstelt het gebruik van de Code van Visual Studio aangezien het de beste ontwikkelaarervaring voor het uitbreiden van Asset compute verstrekt.

## Docker-bureaublad installeren{#docker}

Download en installeer de nieuwste, stabiele [Docker-bureaublad](https://www.docker.com/products/docker-desktop), aangezien dit vereist is [test](../test-debug/test.md) en [foutopsporing](../test-debug/debug.md) asset compute projecten lokaal.

Nadat u Docker Desktop hebt geïnstalleerd, start u deze en installeert u de volgende Docker-afbeeldingen vanaf de opdrachtregel:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Ontwikkelaars op Windows-computers moeten ervoor zorgen dat ze Linux-containers gebruiken voor de bovenstaande afbeeldingen. De stappen voor het overschakelen naar Linux-containers worden beschreven in het dialoogvenster [Docker voor Windows-documentatie](https://docs.docker.com/docker-for-windows/).

## Node.js (en npm) installeren{#node-js}

asset compute werknemers zijn [Node.js](https://nodejs.org/)-based, en dus vereist Node.js 10+ (en npm) om te ontwikkelen en te bouwen.

+ [Node.js (en npm) installeren](../../local-development-environment/development-tools.md#node-js) op dezelfde wijze als voor de traditionele AEM.

## Adobe I/O CLI installeren{#aio}

[De Adobe I/O CLI installeren](../../local-development-environment/development-tools.md#aio-cli), of __aio__ is een bevel-lijn (CLI) npm module die gebruik van en interactie met de technologieën van Adobe I/O vergemakkelijkt, en voor zowel produceert als plaatselijk gebruikt ontwikkelt de arbeiders van de douaneAsset compute.

```
$ npm install -g @adobe/aio-cli@7.1.0
```

_Adobe I/O CLI versie 7.1.0 is vereist. Later versies van Adobe I/O CLI worden op dit moment niet ondersteund._


## De Adobe I/O CLI-Asset compute-insteekmodule installeren{#aio-asset-compute}

De [Adobe I/O CLI Asset compute-insteekmodule](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Webfoutopsporing installeren{#wskdebug}

Download en installeer de [Apache OpenWhisk-foutopsporing](https://www.npmjs.com/package/@openwhisk/wskdebug) npm module om lokale foutopsporing van Asset compute-workers te vergemakkelijken.

_Visual Studio Code 1.48.x+ wordt vereist voor [wskdebug](#wskdebug) om te werken._

```
$ npm install -g @openwhisk/wskdebug
```

## Installeer de extensie{#ngrok}

Download en installeer de [ngrot](https://www.npmjs.com/package/ngrok) npm module, die het publiek toegang tot uw lokale ontwikkelingsmachine verleent, om lokaal het zuiveren van de arbeiders van de Asset compute te vergemakkelijken.

```
$ npm install -g ngrok --unsafe-perm=true
```
