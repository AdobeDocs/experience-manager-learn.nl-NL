---
title: Een lokale ontwikkelomgeving instellen voor de uitbreidbaarheid van Asset Compute
description: Het ontwikkelen van de arbeiders van de Verwerking van Activa, die toepassingen Node.js JavaScript zijn, vereist specifieke ontwikkelingshulpmiddelen die van traditionele AEM ontwikkeling verschillen, die zich van Node.js en diverse npm modules aan de Desktop van de Docker en Code van Microsoft Visual Studio uitstrekken.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---


# Lokale ontwikkelomgeving instellen

Adobe Asset Compute-projecten kunnen niet worden geïntegreerd met de lokale AEM-runtime die wordt geleverd door de AEM SDK en worden ontwikkeld met behulp van hun eigen gereedschapsketen, apart van de toepassingen die AEM op basis van het AEM Maven-projectarchetype vereist.

Om de microservices van Asset Compute uit te breiden, moeten de volgende gereedschappen op de lokale ontwikkelaarscomputer zijn geïnstalleerd.

## Verkorte instructies voor het instellen

Hier volgt een korte set-upinstructies. Nadere bijzonderheden over deze ontwikkelingstools worden hieronder in afzonderlijke secties beschreven.

1. [Docker Desktop](https://www.docker.com/products/docker-desktop) installeren en de vereiste Docker-afbeeldingen ophalen:

   ```
   $ docker pull openwhisk/action-nodejs-v10:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Visual Studio-code installeren](https://code.visualstudio.com/download)
1. [Node.js 10+ installeren](../../local-development-environment/development-tools.md#node-js)
1. Installeer de vereiste npm-modules en Adobe I/O CLI-plug-ins vanaf de opdrachtregel:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

## Visual Studio-code installeren{#vscode}

[De Code](https://code.visualstudio.com/download) van Microsoft Visual Studio wordt gebruikt voor het ontwikkelen van en het zuiveren de arbeiders van de Verwerking van Activa. Terwijl andere [JavaScript-compatibele winde](../../local-development-environment/development-tools.md#set-up-the-development-ide) kan worden gebruikt om de arbeider te ontwikkelen, slechts kan de Code van Visual Studio worden geïntegreerd om de worker van de Verwerking van Activa te [zuiveren](../test-debug/debug.md) .

_Visual Studio Code 1.48.x+ wordt vereist voor[WebDebug](#wskdebug)om te werken._

Dit leerprogramma veronderstelt het gebruik van de Code van Visual Studio aangezien het de beste ontwikkelaarervaring voor het uitbreiden van Activa voorziet Compute.

## Docker-bureaublad installeren{#docker}

Download en installeer de nieuwste, stabiele [Docker Desktop](https://www.docker.com/products/docker-desktop), omdat dit vereist is om projecten voor het lokaal verwerken van bedrijfsmiddelen te [testen](../test-debug/test.md) en [fouten erin op te sporen](../test-debug/debug.md) .

Nadat u Docker Desktop hebt geïnstalleerd, start u deze en installeert u de volgende Docker-afbeeldingen vanaf de opdrachtregel:

```
$ docker pull openwhisk/action-nodejs-v10:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Ontwikkelaars op Windows-computers moeten ervoor zorgen dat ze Linux-containers gebruiken voor de bovenstaande afbeeldingen. De stappen om op de containers van Linux te schakelen worden beschreven in [Docker voor de documentatie](https://docs.docker.com/docker-for-windows/)van Vensters.

## Node.js (en npm) installeren{#node-js}

De arbeiders van de Compute van activa zijn op [Node.js](https://nodejs.org/)-Gebaseerd, en vereisen daarom Node.js 10+ (en npm) om te ontwikkelen en te bouwen.

+ [Installeer Node.js (en npm)](../../local-development-environment/development-tools.md#node-js) op de zelfde manier zoals voor traditionele AEM ontwikkeling.

## Adobe I/O CLI installeren{#aio}

[Installeer de Adobe I/O CLI](../../local-development-environment/development-tools.md#aio-cli), of __de lucht__ is een bevel-lijn (CLI) npm module die gebruik van en interactie met Adobe I/O technologieën vergemakkelijkt, en voor zowel produceert als plaatselijk ontwikkelt de arbeiders van de Compute van de Douane van Activa gebruikt.

```
$ npm install -g @adobe/aio-cli
```

## De plug-in Adobe I/O CLI Asset Compute installeren{#aio-asset-compute}

De plug-in [Adobe I/O CLI Asset Compute](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Webfoutopsporing installeren{#wskdebug}

Download en installeer de [Apache OpenWhisk-module voor foutopsporing](https://www.npmjs.com/package/@openwhisk/wskdebug) van Npm om lokale foutopsporing van Asset Compute-workers te vergemakkelijken.

_Visual Studio Code 1.48.x+ wordt vereist voor[WebDebug](#wskdebug)om te werken._

```
$ npm install -g @openwhisk/wskdebug
```

## Installeer de extensie{#ngrok}

Download en installeer de [npm-module Ngrok](https://www.npmjs.com/package/ngrok) , die het publiek toegang biedt tot uw lokale ontwikkelcomputer, om lokale foutopsporing van de workers Asset Compute te vergemakkelijken.

```
$ npm install -g ngrok --unsafe-perm=true
```
