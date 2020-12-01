---
title: Creeer een project van de Asset compute voor de rekbaarheid van de Asset compute
description: De projecten van de asset compute zijn projecten Node.js, die gebruikend Adobe I/O CLI worden geproduceerd, die aan een bepaalde structuur houden die hen om aan Adobe I/O Runtime toelaat worden opgesteld en met AEM als Cloud Service geïntegreerd.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 0%

---


# Een Asset compute-project maken

De projecten van de asset compute zijn projecten Node.js, die gebruikend Adobe I/O CLI worden geproduceerd, die aan een bepaalde structuur houden die hen om aan Adobe I/O Runtime toelaat worden opgesteld en met AEM als Cloud Service geïntegreerd. Één enkel project van de Asset compute kan één of meerdere arbeiders van de Asset compute bevatten, met elk die een discrete eindpuntverwijzing van HTTP van een AEM als Profiel van de Verwerking van de Cloud Service hebben.

## Een project genereren

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Doorklikken voor het genereren van een Asset compute-project (geen audio)_


Gebruik [Adobe I/O CLI Asset compute plugin](../set-up/development-environment.md#aio-cli) om een nieuw, leeg project van de Asset compute te produceren.

1. Navigeer vanaf de opdrachtregel naar de map waarin u het project wilt plaatsen.
1. Van de bevellijn, voer `aio app init` uit om met de interactieve projectgeneratie CLI te beginnen.
   + Dit kan een browser van het Web werpen die voor authentificatie aan Adobe I/O ertoe aanzet. Als het, uw geloofsbrieven van de Adobe verbonden aan [vereiste de diensten en producten van de Adobe ](../set-up/accounts-and-services.md) verstrekt. Als u zich niet kunt aanmelden, volgt u [deze instructies voor het genereren van een project](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Org selecteren__
   + Selecteer de Adobe Org die als Cloud Service heeft AEM, Project Firefly wordt geregistreerd aan
1. __Project selecteren__
   + Zoek en selecteer het project. Dit is de [Titel van project](../set-up/firefly.md) gecreeerd van het Firefly projectmalplaatje, in dit geval `WKND AEM Asset Compute`
1. __Werkruimte selecteren__
   + De werkruimte `Development` selecteren
1. __Welke Adobe I/O App-functies wilt u inschakelen voor dit project? Selecteer de onderdelen die u wilt opnemen__
   + Selecteer `Actions: Deploy runtime actions`
   + Gebruik de pijltoetsen om de selectie op te heffen en ruimte om de selectie ongedaan te maken/te selecteren en druk op Enter om de selectie te bevestigen
1. __Selecteer het type acties dat u wilt genereren__
   + Selecteer `Adobe Asset Compute Worker`
   + Gebruik de pijlen om selectie te selecteren, ruimte om selectie ongedaan te maken/te selecteren en Enter om selectie te bevestigen
1. __Hoe wilt u deze handeling een naam geven?__
   + Gebruik de standaardnaam `worker`.
   + Als uw project meerdere workers bevat die verschillende elementberekeningen uitvoeren, geeft u deze een semantische naam

## Console.json genereren

Van de wortel van het nieuwe gecreeerde project van de Asset compute, stel het volgende bevel in werking om `console.json` te produceren.

```
$ aio app use
```

Verifieer de huidige werkruimtedetails correct zijn, en behoorlijk `Y` of ingaan om `console.json` te produceren. Als `.env` en `.aio` als reeds bestaand worden ontdekt, tik `x` om hun verwezenlijking over te slaan.

## De anatomie van het project evalueren

Het geproduceerde project van de Asset compute is een project Node.js voor een gespecialiseerd project van het Project van de Adobe Firefly, is het volgende idiosyncratic aan het project van de Asset compute:

+ `/actions` bevat submappen en elke submap definieert een Asset compute-worker.
   + `/actions/<worker-name>/index.js` definieert de JavaScript die wordt uitgevoerd om het werk van deze worker uit te voeren.
      + De mapnaam `worker` is standaard en kan alles zijn, zolang deze maar is geregistreerd in `manifest.yml`.
      + Indien nodig kunnen meerdere arbeidersmappen worden gedefinieerd onder `/actions`, maar deze moeten worden geregistreerd in `manifest.yml`.
+ `/test/asset-compute` bevat de testreeksen voor elke worker. Net als in de map `/actions` kan `/test/asset-compute` meerdere submappen bevatten, die elk overeenkomen met de worker die wordt getest.
   + `/test/asset-compute/worker`, die een testsuite voor een specifieke worker vertegenwoordigt, bevat submappen die een specifiek testcase vertegenwoordigen, samen met de invoer, parameters en verwachte uitvoer van de test.
+ `/build` bevat de output, de logboeken, en de artefacten van Asset compute testcase executions.
+ `/manifest.yml` bepaalt welke Asset compute arbeiders het project verstrekt. Elke worker-implementatie moet in dit bestand worden opgesomd om deze beschikbaar te maken voor AEM als Cloud Service.
+ `/console.json` definieert Adobe I/O-configuraties
   + Dit bestand kan worden gegenereerd/bijgewerkt met de opdracht `aio app use`.
+ `/.aio` bevat configuraties die door het hulpmiddel CLI van de AIR worden gebruikt.
   + Dit bestand kan worden gegenereerd/bijgewerkt met de opdracht `aio app use`.
+ `/.env` definieert omgevingsvariabelen in een  `key=value` syntaxis en bevat geheimen die niet mogen worden gedeeld. Dit kan worden geproduceerd of om deze geheimen te beschermen, zou dit dossier NIET in Git moeten worden gecontroleerd en via het standaard `.gitignore` dossier van het project worden genegeerd.
   + Dit bestand kan worden gegenereerd/bijgewerkt met de opdracht `aio app use`.
   + Variabelen die in dit bestand worden gedefinieerd, kunnen worden overschreven door [variabelen exporteren](../deploy/runtime.md) op de opdrachtregel.

Voor meer details over de overzicht van de projectstructuur, herzie [Anatomie van een project van het Project van Adobe Firefly](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application).

Het grootste deel van de ontwikkeling vindt plaats in de map `/actions` waarin workers worden geïmplementeerd en in `/test/asset-compute` tests voor aangepaste Asset computen worden geschreven.

## asset compute van Github

Het definitieve project voor de Asset compute is beschikbaar op Github op:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github bevat de definitieve staat van het project, volledig bevolkt met de arbeider en testgevallen, maar bevat geen geloofsbrieven, d.w.z. `.env`,  `console.json` of  `.aio`._

