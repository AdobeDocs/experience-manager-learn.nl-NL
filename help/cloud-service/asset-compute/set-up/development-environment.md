---
title: Een lokale ontwikkelomgeving instellen voor uitbreidbaarheid van de Asset compute
description: Het ontwikkelen van de arbeiders van de Asset compute, die toepassingen Node.js JavaScript zijn, vereist specifieke ontwikkelingshulpmiddelen die van traditionele AEM ontwikkeling verschillen, die zich van Node.js en diverse npm modules aan de Desktop van de Docker en Code van Microsoft Visual Studio uitstrekken.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 0%

---


# Lokale ontwikkelomgeving instellen

De projecten van de Asset compute van Adobe kunnen niet met lokale AEM runtime worden geïntegreerd die door AEM SDK wordt verstrekt en worden ontwikkeld gebruikend hun eigen hulpmiddelketting, los van die die door AEM toepassingen wordt vereist die op het AEM Maven projectarchetype worden gebaseerd.

Als u de Asset compute-microservices wilt uitbreiden, moeten de volgende gereedschappen zijn geïnstalleerd op de lokale ontwikkelaarscomputer.

## Verkorte instructies voor het instellen

Hier volgt een korte set-upinstructies. Nadere bijzonderheden over deze ontwikkelingstools worden hieronder in afzonderlijke secties beschreven.

1. [Installeer Docker ](https://www.docker.com/products/docker-desktop) Desktop en trek de vereiste Docker-afbeeldingen:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:latest
   ```

1. [Visual Studio-code installeren](https://code.visualstudio.com/download)
1. [Node.js 10+ installeren](../../local-development-environment/development-tools.md#node-js)
1. Installeer de vereiste npm-modules en Adobe I/O CLI-plug-ins vanaf de opdrachtregel:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Lees de onderstaande secties voor meer informatie over de instructies voor een verkorte installatie.

## Visual Studio Code{#vscode} installeren

[Microsoft Visual Studio ](https://code.visualstudio.com/download) Codeis die voor het ontwikkelen en het zuiveren van de arbeiders van de Asset compute wordt gebruikt. Terwijl andere [JavaScript-compatibele winde](../../local-development-environment/development-tools.md#set-up-the-development-ide) kan worden gebruikt om de worker te ontwikkelen, kan alleen Visual Studio-code worden geïntegreerd in [debug](../test-debug/debug.md) Asset compute-worker.

_Visual Studio Code 1.48.x+ wordt vereist voor  [](#wskdebug) wskdebugto werken._

Dit leerprogramma veronderstelt het gebruik van de Code van Visual Studio aangezien het de beste ontwikkelaarervaring voor het uitbreiden van Asset compute verstrekt.

## Docker-bureaublad installeren{#docker}

Download en installeer de nieuwste, stabiele [Docker Desktop](https://www.docker.com/products/docker-desktop), aangezien dit vereist is voor [test](../test-debug/test.md) en [debug](../test-debug/debug.md) Asset compute projecten lokaal.

Nadat u Docker Desktop hebt geïnstalleerd, start u deze en installeert u de volgende Docker-afbeeldingen vanaf de opdrachtregel:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Ontwikkelaars op Windows-computers moeten ervoor zorgen dat ze Linux-containers gebruiken voor de bovenstaande afbeeldingen. De stappen om op de containers van Linux te schakelen worden beschreven in [Docker voor de documentatie van Vensters](https://docs.docker.com/docker-for-windows/).

## Node.js (en npm){#node-js} installeren

De arbeiders van de asset compute zijn [Node.js](https://nodejs.org/)-Gebaseerd, en vereisen daarom Node.js 10+ (en npm) om te ontwikkelen en te bouwen.

+ [Installeer Node.js (en npm)](../../local-development-environment/development-tools.md#node-js) op de zelfde manier zoals voor traditionele AEM ontwikkeling.

## Adobe I/O CLI{#aio} installeren

[Installeer Adobe I/O CLI](../../local-development-environment/development-tools.md#aio-cli), of  ____ aiois een bevel-lijn (CLI) npm module die het gebruik van en de interactie met de technologieën van Adobe I/O vergemakkelijkt, en voor zowel produceert als plaatselijk ontwikkelt de arbeiders van de aangepaste Asset compute gebruikt.

```
$ npm install -g @adobe/aio-cli
```

## De Adobe I/O CLI Asset compute-insteekmodule installeren{#aio-asset-compute}

De [Adobe I/O CLI Asset compute plugin](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Webfoutopsporing installeren{#wskdebug}

Download en installeer de [Apache OpenWhisk-module voor foutopsporing](https://www.npmjs.com/package/@openwhisk/wskdebug) npm om lokale foutopsporing van Asset compute-workers te vergemakkelijken.

_Visual Studio Code 1.48.x+ wordt vereist voor  [](#wskdebug) wskdebugto werken._

```
$ npm install -g @openwhisk/wskdebug
```

## Ingrot{#ngrok} installeren

Download en installeer de [ngrok](https://www.npmjs.com/package/ngrok) npm module, die het publiek toegang tot uw lokale ontwikkelingsmachine verleent, om lokaal het zuiveren van de arbeiders van de Asset compute te vergemakkelijken.

```
$ npm install -g ngrok --unsafe-perm=true
```
