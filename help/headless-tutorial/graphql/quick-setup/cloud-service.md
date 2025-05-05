---
title: Snelle installatie van AEM Headless voor AEM as a Cloud Service
description: Met de snelle installatie van AEM Headless kunt u in de praktijk werken met AEM Headless via inhoud van het WKND-sitevoorbeeldproject en een React App die de inhoud gebruikt via AEM Headless GraphQL API's.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
duration: 781
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 0%

---

# Snelle installatie van AEM Headless voor AEM as a Cloud Service

Met de snelle installatie van AEM Headless kunt u in de praktijk werken met AEM Headless via inhoud van het WKND-sitevoorbeeldproject en een voorbeeldtoepassing React (een SPA) die de inhoud gebruikt via AEM Headless GraphQL API&#39;s.

## Vereisten

U hebt het volgende nodig om deze snelle installatie te kunnen uitvoeren:

+ AEM as a Cloud Service Sandbox-omgeving (bij voorkeur ontwikkeling)
+ Toegang tot AEM as a Cloud Service en Cloud Manager
   + __de Beheerder van AEM__ toegang tot AEM as a Cloud Service
   + __Cloud Manager - de Toegang van de Manager van de Plaatsing__ tot Cloud Manager
+ De volgende gereedschappen moeten lokaal zijn geïnstalleerd:
   + [ Node.js v18 ](https://nodejs.org/en/)
   + [ Git ](https://git-scm.com/)
   + IDE (bijvoorbeeld, [ Microsoft® Visual Studio Code ](https://code.visualstudio.com/))

## 1. Een Cloud Manager Git-opslagplaats maken

Maak eerst een Cloud Manager Git-opslagplaats die wordt gebruikt om de WKND-site te implementeren. De WKND-site is een voorbeeld van een AEM-websiteproject dat inhoud (Content Fragments) en een GraphQL AEM-eindpunt bevat dat wordt gebruikt door de React App van de snelle setup.

_Screencast van stappen_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. Navigeer aan [ https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. Selecteer het Cloud Manager __Programma__ dat het milieu van AEM as a Cloud Service aan gebruik voor deze snelle opstelling bevat
1. Een Git-opslagplaats maken voor het WKND-siteproject
   1. Selecteer __Bewaarplaatsen__ in de hoogste navigatie
   1. Selecteer __toevoegt Bewaarplaats__ in de hoogste actiebar
   1. Geef de nieuwe Git-opslagplaats een naam: `aem-headless-quick-setup-wknd`
      + Namen van Git-opslagplaats moeten uniek zijn per Adobe-organisatie.
   1. Selecteer __sparen__, en wacht op de bewaarplaats van het Git om te initialiseren

## 2. Steek een voorbeeld van een WKND-siteproject naar een Cloud Manager Git Repository

Met de gemaakte opbergplaats van Cloud Manager Git, kloon de broncode van het project van de Plaats WKND van GitHub en duw het aan de bewaarplaats van Cloud Manager Git. Cloud Manager heeft nu toegang tot het WKND-siteproject en implementeert dit in de AEM as a Cloud Service-omgeving.

_Screencast van stappen_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. Van de bevellijn, kloon de broncode van het project van de steekproefWKND van de Plaats van GitHub

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. De Cloud Manager Git-opslagplaats toevoegen als een externe
   1. Selecteer __Bewaarplaatsen__ in de hoogste navigatie
   1. Selecteer __Info van de Reactie van de Toegang__ van de hoogste actiebar
   1. Uitvoeren bevel dat in __wordt gevonden voegt ver aan uw bewaarplaats van het Git__ van bevel-lijn toe

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. Zet de broncode van het voorbeeldproject van uw lokale Git-opslagplaats naar de Cloud Manager Git-opslagplaats

   ```shell
   $ git push adobe main:main
   ```

   Wanneer ertoe aangezet voor geloofsbrieven, verstrek het __Gebruikersnaam__ en __Wachtwoord__ van de Informatie van de Bewaarplaats van Cloud Manager ____ modaal.

## 3. De WKND-site implementeren in AEM as a Cloud Service

Als het WKND-siteproject naar de Cloud Manager Git-opslagplaats wordt geduwd, kan het niet worden geïmplementeerd naar AEM as a Cloud Service met behulp van Cloud Manager-pijpleidingen.

Houd er rekening mee dat het WKND-siteproject voorbeeldinhoud bevat die door de React-app wordt gebruikt via GraphQL-API&#39;s zonder AEM Headless.

_Screencast van stappen_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. Verbind a __niet-productie plaatsingspijpleiding__ aan de nieuwe bewaarplaats van het Git
   1. Selecteer __Pijpleidingen__ in de hoogste navigatie
   1. Selecteer __toevoegen Pijpleiding__ van de hoogste actiebar
   1. Op het __lusje van de Configuratie__
      1. Selecteer __optie van de Pijl van de Plaatsing 0&rbrace;__
      1. Plaats de __Naam van de niet-Productie Pijpleiding__ aan `Dev Deployment pipeline`
      1. Selecteer __Trigger van de Plaatsing > op de Veranderingen van de it__
      1. Selecteer __Belangrijk Metrisch Gedrag van Mislukt > ga onmiddellijk__
      1. Selecteer __verdergaan__
   1. Op het __Source Code__ lusje
      1. Selecteer __de Volledige optie van de Code van de Stapel 0&rbrace;__
      1. Selecteer het __de ontwikkelomgeving van AEM as a Cloud Service__ van de __In aanmerking komende Milieu&#39;s van de Plaatsing__ uitgezochte doos
      1. Selecteer `aem-headless-quick-setup-wknd` in de __2&rbrace; uitgezochte doos van de Bewaarplaats &lbrace;__
      1. Selecteer `main` van de __Tak van het Git__ uitgezochte doos
      1. Selecteer __sparen__
1. Stel de __Dev Pipeline van de Plaatsing in werking__
   1. Selecteer __Overzicht__ in de hoogste navigatie
   1. Bepaal de plaats van de pas gecreëerde __Dev pijpleiding van de Plaatsing__ in de __Pijpleidingen__ sectie
   1. Selecteer __..__ rechts van de pijpleidingsingang
   1. Selecteer __Looppas__, en bevestig in modaal
   1. Selecteer __..__ rechts van de nu lopende pijpleiding
   1. Selecteer __details van de Mening__
1. Van de details van de pijpleidingsuitvoering, controleer vooruitgang tot het met succes voltooide. De uitvoering van de pijpleiding moet tussen 30 en 40 minuten duren.

## 4. Download en voer de WKND React-app uit

Met AEM as a Cloud Service bootstrapped met de inhoud van het WKND-siteproject, downloadt en start u de voorbeeldtoepassing WKND React die de inhoud van de WKND-site verbruikt via AEM Headless GraphQL API&#39;s.

_Screencast van stappen_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. Van de bevellijn, kloon de React broncode van App van GitHub.

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Open de map `~/Code/aem-guides-wknd-graphql/react-app` in uw IDE.
1. Open het bestand `.env.development` in de IDE.
1. Punt aan AEM as a Cloud Service __publiceert__ gastheer URI van de dienst van het `REACT_APP_HOST_URI` bezit.

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   U kunt als volgt de host-URI van uw AEM as a Cloud Service-publicatieservice vinden:

   1. In Cloud Manager, uitgezochte __Milieu&#39;s__ in de hoogste navigatie
   1. Selecteer __het milieu van de Ontwikkeling__
   1. Bepaal de plaats van de __Publish Dienst (AEM &amp; Dispatcher)__ verbinding __Segmenten van het Milieu__ lijst
   1. Kopieer het adres van de koppeling en gebruik deze als URI van de AEM as a Cloud Service Publish-service

1. In winde, sparen de veranderingen in `.env.development`
1. Voer de React App vanaf de opdrachtregel uit

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. React App, die plaatselijk loopt, begint op [ http://localhost:3000 ](http://localhost:3000) en toont een lijst van avonturen, die uit AEM as a Cloud Service gebruikend GraphQL APIs van AEM Headless worden voortgebracht.

## 5. Inhoud bewerken in AEM

Met het voorbeeld van de WKND React-app die verbinding maakt met en inhoud consumeert van de AEM Headless GraphQL API&#39;s, kunt u inhoud schrijven in de AEM Author-service en zien hoe de Ervaring van React App in overleg wordt bijgewerkt.

_Screencast van stappen_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. Aanmelden bij AEM as a Cloud Service Author-service
1. Navigeer aan __Assets > Dossiers > Gedeelde WKND > Engels > avonturen__
1. Open de __Omslag van 0&rbrace; Cycli Zuidelijke Utah &lbrace;__
1. Selecteer het __Cyclische Zuidelijke Fragment van de Inhoud__, en selecteer __uitgeven__ van de hoogste actiebar
1. Werk enkele velden van het inhoudsfragment bij, bijvoorbeeld:
   + Titel: `Cycling Utah's National Parks`
   + Lengte van strook: `6 Days`
   + Probleem: `Intermediate`
   + Prijs: `3500`
   + Primaire afbeelding: `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. Selecteer __sparen__ in de hoogste actiebar
1. Selecteer __Snel publiceren__ van de hoogste actiebar __..__
1. Vernieuw React App die op [ http://localhost:3000 ](http://localhost:3000) loopt.
1. Selecteer in React App het nu bijgewerkte cyclusavontuur, en verifieer de inhoudsveranderingen die aan het Inhoudsfragment worden aangebracht.

1. Dezelfde aanpak gebruiken in AEM Author-service:
   1. Publiceer een bestaand fragment van de Inhoud van het Avontuur, en verifieer het wordt verwijderd uit React App ervaring
   1. Maak en publiceer een nieuw Adventure Content Fragment en controleer of dit wordt weergegeven in de React App-ervaring

   >[!TIP]
   >
   > Als u niet bekend bent met het maken en publiceren van nieuwe of het ongedaan maken van bestaande inhoudsfragmenten, bekijkt u de bovenstaande screencast.

## Gefeliciteerd!

Gefeliciteerd! U hebt AEM Headless met succes gebruikt om een React App aan te drijven!

Om in detail te begrijpen hoe React App inhoud van AEM as a Cloud Service verbruikt, controleer uit [ het Hoofdloze leerprogramma van AEM ](../multi-step/overview.md). In de zelfstudie wordt onderzocht hoe inhoudsfragmenten in AEM zijn gemaakt en hoe deze React-app hun inhoud als JSON-inhoud gebruikt.

### Volgende stappen

+ [Zelfstudie voor AEM Headless starten](../multi-step/overview.md)
