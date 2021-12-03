---
title: Snelle installatie voor AEM as a Cloud Service AEM zonder koppen
description: Met de snelle installatie zonder AEM Headless kunt u in de praktijk werken met AEM Headless via inhoud van het WKND-voorbeeldproject voor de site en een React-app die de inhoud gebruikt via AEM Headless GraphQL API's.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 9442
thumbnail: 339073.jpg
source-git-commit: f13f32de11bda924a2d255678e58e7c982d0d004
workflow-type: tm+mt
source-wordcount: '1073'
ht-degree: 0%

---


# Snelle installatie voor AEM as a Cloud Service AEM zonder koppen

Met de snelle installatie zonder AEM Headless kunt u de inhoud in de praktijk brengen met AEM Headless via inhoud van het WKND-sitevoorbeeldproject en een voorbeeldtoepassing React (een SPA) die de inhoud gebruikt in AEM Headless GraphQL API&#39;s.

## Vereisten

U hebt het volgende nodig om deze snelle installatie te kunnen uitvoeren:

+ AEM as a Cloud Service zandbakomgeving (bij voorkeur ontwikkeling)
+ Toegang tot AEM as a Cloud Service en Cloud Manager
   + `AEM Administrator` toegang tot AEM as a Cloud Service
   + `Cloud Manager - Deployment Manager` toegang tot Cloud Manager
+ De volgende gereedschappen moeten lokaal zijn geïnstalleerd:
   + [Node.js v10+](https://nodejs.org/en/)
   + [npm 6+](https://www.npmjs.com/)
   + [Git](https://git-scm.com/)
   + Een IDE (bijvoorbeeld [Microsoft® Visual Studio-code](https://code.visualstudio.com/)

## 1. Creëer een gegevensopslagruimte voor cloudbeheer

Maak eerst een gegevensopslagruimte voor cloudbeheerpakketten die wordt gebruikt om de WKND-site te implementeren. De plaats WKND is een steekproef AEM websiteproject dat inhoud (Inhoudsfragmenten) en een AEM GraphQL die door Snelle React App van de opstelling wordt gebruikt bevat.

_Screencast van stappen_
>[!VIDEO](https://video.tv.adobe.com/v/339073/?quality=12&learn=on)

1. Navigeren naar [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. Cloud Manager selecteren __Programma__ die de AEM as a Cloud Service omgeving bevat die voor deze snelle installatie moet worden gebruikt
1. Een Git-opslagplaats maken voor het WKND-siteproject
   1. Selecteren __Opslagplaatsen__ in de bovenste navigatie
   1. Selecteren __Opslagplaats toevoegen__ in de bovenste actiebalk
   1. Geef de nieuwe Git-opslagplaats een naam: `aem-headless-quick-setup`
   1. Selecteren __Opslaan__ en wacht tot de Git-opslagplaats is geïnitialiseerd

## 2. Voorbeeld van WKND-siteproject naar cloudbeheermap voor informatiekit verplaatsen

Met de gegevensopslagplaats van de Git van de Manager van de Wolk creeerde, kloon de broncode van het project van de Plaats WKND van GitHub en duw het aan de bewaarplaats van de Git van de Manager van de Wolk. Cloud Manager heeft nu toegang tot het WKND-siteproject en implementeert dit in de AEM as a Cloud Service omgeving.

_Screencast van stappen_
>[!VIDEO](https://video.tv.adobe.com/v/339074/?quality=12&learn=on)

1. Van de bevellijn, kloon de broncode van het project van de steekproefWKND van de Plaats van GitHub

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. De gegevensopslagruimte van de cloudbeheerkit toevoegen als een externe
   1. Selecteren __Opslagplaatsen__ in de bovenste navigatie
   1. Selecteren __Repo-info openen__ van de bovenste actiebalk
   1. Uitvoeren, opdracht gevonden in __Een externe server toevoegen aan uw Git-opslagplaats__ vanaf de opdrachtregel

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup/
      ```

1. De broncode van het voorbeeldproject overbrengen naar de cloudbeheeropslagplaats

   1. Push the code from your local Git repository to the Cloud Manager Git repository

      ```shell
      $ git push adobe master:main
      ```

      Geef bij de aanwijzing voor referenties de __Gebruikersnaam__ en __Wachtwoord__ van Cloud Manager __Info opslagplaats__ modal.

## 3. De WKND-site implementeren om as a Cloud Service te AEM

Als het WKND-siteproject naar de gegevensopslagruimte van Cloud Manager Git wordt geduwd, kan het niet worden geïmplementeerd om as a Cloud Service te AEM met behulp van Cloud Manager-pijpleidingen.

Houd in mening, verstrekt het project van de Plaats WKND steekproefinhoud die React app over AEM Headless GraphQL APIs gebruikt.

_Screencast van stappen_
>[!VIDEO](https://video.tv.adobe.com/v/339075/?quality=12&learn=on)

1. Een __Installatie-pijpleiding voor niet-productie__ naar de nieuwe Git-opslagplaats
   1. Selecteren __Pijpleidingen__ in de bovenste navigatie
   1. Selecteren __Pipet toevoegen__ van de bovenste actiebalk
   1. Op de __Configuratie__ tab
      1. Selecteren __Implementatiepijp__ option
      1. Stel de __Naam niet-productiepijpleiding__ tot `Dev Deployment pipeline`
      1. Selecteren __Implementatieactivering > Wijzigingen in it__
      1. Selecteren __Belangrijk gedrag metrische fouten > Direct doorgaan__
      1. Selecteren __Doorgaan__
   1. Op de __Broncode__ tab
      1. Selecteren __Volledige stapelcode__ option
      1. Selecteer __AEM as a Cloud Service ontwikkelomgeving__ van de __In aanmerking komende implementatieomgevingen__ Selectievakje
      1. Selecteren `aem-headless-quick-setup` in de __Bewaarplaats__ Selectievakje
      1. Selecteren `main` van de __Git Branch__ Selectievakje
      1. Selecteren __Opslaan__
1. Voer de __Implementatiepijp voor Dev__
   1. Selecteren __Overzicht__ in de bovenste navigatie
   1. De nieuwe versie zoeken __Dev Deployment-pijplijn__ in de __Pijpleidingen__ sectie
   1. Selecteer __...__ rechts van de pijpleiding
   1. Selecteren __Uitvoeren__ en in het modaal
   1. Selecteer __...__ rechts van de nu lopende pijpleiding
   1. Selecteren __Details weergeven__
1. Van de details van de pijpleidingsuitvoering, controleer vooruitgang tot het met succes voltooide. De uitvoering van de pijpleiding moet tussen 45 en 60 minuten duren.

## 4. De WKND React-app downloaden en uitvoeren

Met AEM as a Cloud Service bootstrapped met de inhoud van het project van de Plaats WKND, download, en begin de steekproefWKND React App die de inhoud van de Plaats van WKND over AEM Headless GraphQL APIs verbruikt.

_Screencast van stappen_
>[!VIDEO](https://video.tv.adobe.com/v/339076/?quality=12&learn=on)

1. Van de bevellijn, kloon de React broncode van App van GitHub.

   ```shell
   $ cd ~/Code
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. De map openen `~/Code/aem-guides-wknd-graphql` in uw IDE.
1. Open het bestand in de IDE `react-app/.env.development`.
1. Wijs naar de as a Cloud Service AEM __Publiceren__ de gastheer URI van de dienst van  `REACT_APP_HOST_URI` eigenschap.

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com/
   ...
   ```

   U kunt als volgt de host-URI van uw AEM as a Cloud Service publicatieservice vinden:

   1. Selecteer in Cloud Manager de optie __Omgevingen__ in de bovenste navigatie
   1. Selecteren __Ontwikkeling__ milieu
   1. Zoek de __Service voor publiceren (AEM en verzender)__ link __Omgevingssegmenten__ table
   1. Kopieer het adres van de koppeling en gebruik deze als URI van de AEM as a Cloud Service publicatieservice

1. In winde, sparen de veranderingen aan `.env.development`
1. Voer de React App vanaf de opdrachtregel uit

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. De React App, die plaatselijk loopt, begint op [http://localhost:3000](http://localhost:3000) en geeft een lijst weer van avonturen die afkomstig zijn van AEM as a Cloud Service met behulp van AEM Headless GraphQL API&#39;s.

## 5. Inhoud bewerken in AEM

Met het voorbeeld WKND React App die met en het verbruiken van inhoud van AEM Headless GraphQL APIs verbindt, auteursinhoud in de dienst van de Auteur AEM en zie hoe de Ervaring van React App in concert bijwerkt.

_Screencast van stappen_
>[!VIDEO](https://video.tv.adobe.com/v/339077/?quality=12&learn=on)

1. Aanmelden bij AEM as a Cloud Service auteur-service
1. Navigeren naar __Middelen > Bestanden > WKND > Engels > Adventures__
1. Open de __Cycli Southern Utah__ Map
1. Selecteer __Cycli Southern Utah__ Inhoudsfragment en selecteer __Bewerken__ van de bovenste actiebalk
1. Werk enkele velden van het inhoudsfragment bij, bijvoorbeeld:
   + Titel: `Cycling Utah's National Parks`
   + Lengte van reisstrook: `6 Days`
   + Probleem: `Intermediate`
   + Prijs: `$3500`
   + Primaire afbeelding: `/content/dam/wknd/en/activities/cycling/mountain-biking.jpg`
1. Selecteren __Opslaan__ in de bovenste actiebalk
1. Selecteren __Snel publiceren__ van de bovenste actiebalk __...__
1. De React-app vernieuwen die wordt uitgevoerd op [http://localhost:3000](http://localhost:3000).
1. Selecteer in de React-app de nu bijgewerkte versie en controleer de wijzigingen in de inhoud van het inhoudsfragment.

1. Dezelfde aanpak gebruiken in AEM Author-service:
   1. Publiceer een bestaand fragment van de Inhoud van het Avontuur, en verifieer het wordt verwijderd uit React App ervaring
   1. Maak en publiceer een nieuw Adventure Content Fragment en controleer of dit wordt weergegeven in de React App-ervaring

   >[!TIP]
   >
   > Als u niet bekend bent met het maken en publiceren van nieuwe of het ongedaan maken van bestaande inhoudsfragmenten, bekijkt u de bovenstaande screencast.

## Gefeliciteerd!

Gefeliciteerd! U hebt AEM Headless met succes gebruikt om een React App aan te drijven!

Om in detail te begrijpen hoe React App inhoud van AEM as a Cloud Service verbruikt, controleer uit [de zelfstudie voor AEM zonder hoofd](../multi-step/overview.md). In de zelfstudie wordt onderzocht hoe inhoudsfragmenten in AEM worden gemaakt en hoe deze React-app hun inhoud als JSON-inhoud gebruikt.

+ [Zelfstudie voor AEM zonder koppen starten](../multi-step/overview.md)
