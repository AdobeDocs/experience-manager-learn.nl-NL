---
title: Een project voor het berekenen van bedrijfsmiddelen maken voor de rekbaarheid van bedrijfsmiddelen
description: De toepassingen van de Compute van activa zijn projecten Node.js, die gebruikend Adobe I/O CLI worden geproduceerd, die aan een bepaalde structuur houden die hen om aan Adobe I/O Runtime toelaat worden opgesteld en met AEM als Cloud Service worden geïntegreerd.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: a71c61304bbc9d54490086b3313c823225fbe2e0
workflow-type: tm+mt
source-wordcount: '630'
ht-degree: 0%

---


# Een project voor het berekenen van bedrijfsmiddelen maken

De toepassingen van de Compute van activa zijn projecten Node.js, die gebruikend Adobe I/O CLI worden geproduceerd, die aan een bepaalde structuur hangen die hen om aan Adobe I/O Runtime toelaat worden opgesteld en met AEM als Cloud Service worden geïntegreerd. Één enkel project van de Verwerking van Activa kan één of meerdere arbeiders van de Verwerking van Activa bevatten, met elk die een discrete eindpunt van HTTP van een AEM als Profiel van de Verwerking van de Cloud Service van verwijzingen voorzien.

## Een project genereren

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)
_Doorklikken voor het genereren van een project Asset Compute (geen audio)_


Gebruik de [Adobe I/O CLI-insteekmodule](../set-up/development-environment.md#aio-cli) Asset Compute om een nieuw, leeg project Asset Compute te genereren.

1. Navigeer vanaf de opdrachtregel naar de map waarin u het project wilt plaatsen.
1. Van de bevellijn, voer uit `aio app init` om met de interactieve projectgeneratie CLI te beginnen.
   + Dit kan browser van het Web die voor authentificatie aan Adobe I/O ertoe aanzetten. Als het, uw geloofsbrieven van de Adobe verbonden aan de [vereiste diensten en de producten](../set-up/accounts-and-services.md)van de Adobe verstrekt. Als u zich niet kunt aanmelden, volgt u deze instructies voor het genereren van een project.
1. __Org selecteren__
   + Selecteer de Adobe Org die als Cloud Service heeft AEM, Project Firefly wordt geregistreerd aan
1. __Project selecteren__
   + Zoek en selecteer het project. Dit is de titel [van het](../set-up/firefly.md) Project die van het Firefly projectmalplaatje wordt gecreeerd, in dit geval `WKND AEM Asset Compute`
1. __Werkruimte selecteren__
   + De `Development` werkruimte selecteren
1. __Welke I/O App-functies van Adobe wilt u inschakelen voor dit project? Te gebruiken componenten selecteren__
   + Selecteer `Actions: Deploy runtime actions`
   + Gebruik de pijltoetsen om de selectie op te heffen en ruimte om de selectie ongedaan te maken/te selecteren en druk op Enter om de selectie te bevestigen
1. __Selecteer het type acties dat u wilt genereren__
   + Selecteer `Adobe Asset Compute Worker`
   + Gebruik de pijlen om selectie te selecteren, ruimte om selectie ongedaan te maken/te selecteren en Enter om selectie te bevestigen
1. __Hoe wilt u deze handeling een naam geven?__
   + Gebruik de standaardnaam `worker`.
   + Als uw project meerdere workers bevat die verschillende elementberekeningen uitvoeren, geeft u deze een semantische naam

## De anatomie van het project evalueren

Het geproduceerde project van de Verwerking van Activa is een project Node.js voor een gespecialiseerde toepassing van het Project van Adobe Frefly, is het volgende idiosyncratic aan het project van de Verwerking van Activa:

+ `/actions` bevat submappen en elke submap definieert een worker Asset Compute.
   + `/actions/<worker-name>/index.js` definieert de JavaScript die wordt uitgevoerd om het werk van deze worker uit te voeren.
      + De mapnaam `worker` is standaard en kan alles zijn, zolang deze maar is geregistreerd in het `manifest.yml`bestand.
      + Er kunnen meerdere arbeidersmappen worden gedefinieerd onder `/actions` de opgegeven naam, maar deze moeten wel zijn geregistreerd in het `manifest.yml`bestand.
+ `/test/asset-compute` bevat de testreeksen voor elke worker. Net als in de `/actions` map `/test/asset-compute` kunnen er meerdere submappen zijn die elk overeenkomen met de worker die door de map wordt getest.
   + `/test/asset-compute/worker`, die een testsuite voor een specifieke worker vertegenwoordigt, bevat submappen die een specifiek testcase vertegenwoordigen, samen met de invoer, parameters en verwachte uitvoer van de test.
+ `/build` bevat de uitvoer, logboeken en artefacten van de testcase van Asset Compute.
+ `/manifest.yml` bepaalt welke arbeiders van de Compute van Activa het project verstrekt. Elke worker-implementatie moet in dit bestand worden opgesomd om deze beschikbaar te maken voor AEM als Cloud Service.
+ `/.aio` bevat configuraties die door het hulpmiddel CLI van de AIR worden gebruikt. Dit bestand kan worden geconfigureerd via de `aio config` opdracht.
+ `/.env` definieert omgevingsvariabelen in een `key=value` syntaxis en bevat geheimen die niet mogen worden gedeeld. Om deze geheimen te beschermen, zou dit dossier NIET in Git moeten worden gecontroleerd en via het standaard `.gitignore` dossier van het project genegeerd.
   + Variabelen die in dit bestand worden gedefinieerd, kunnen worden overschreven door variabelen [op de opdrachtregel te](../deploy/runtime.md) exporteren.

Voor meer details over de overzicht van de projectstructuur, herzie de [Anatomie van een Adobe Project Firefly toepassing](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application).

Het grootste deel van de ontwikkeling vindt plaats in de `/actions` `/test/asset-compute` omslag die arbeidersimplementaties ontwikkelt, en schriftelijk tests voor de arbeiders van de Compute van de douaneMiddelen.
