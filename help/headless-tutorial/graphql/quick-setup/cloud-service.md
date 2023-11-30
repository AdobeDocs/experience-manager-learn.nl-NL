---
title: Snelle installatie van AEM zonder hoofd voor AEM as a Cloud Service
description: Met de snelle installatie AEM Headless kunt u de inhoud in handen krijgen met AEM Headless via inhoud van het WKND-voorbeeldproject voor websites en een React-app die de inhoud gebruikt via AEM GraphQL API's zonder koppen.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1081'
ht-degree: 0%

---

# Snelle installatie van AEM zonder hoofd voor AEM as a Cloud Service

Met de snelle installatie zonder AEM Headless kunt u de inhoud in de praktijk brengen met AEM Headless via inhoud van het WKND-voorbeeldproject Site en een voorbeeldtoepassing React (een SPA) die de inhoud gebruikt boven AEM Headless GraphQL API&#39;s.

## Vereisten

U hebt het volgende nodig om deze snelle installatie te kunnen uitvoeren:

+ AEM as a Cloud Service zandbakomgeving (bij voorkeur ontwikkeling)
+ Toegang tot AEM as a Cloud Service en Cloud Manager
   + __AEM__ toegang tot AEM as a Cloud Service
   + __Cloud Manager - Implementatiebeheer__ toegang tot Cloud Manager
+ De volgende gereedschappen moeten lokaal zijn geïnstalleerd:
   + [Node.js v18](https://nodejs.org/en/)
   + [Git](https://git-scm.com/)
   + Een IDE (bijvoorbeeld [Microsoft® Visual Studio-code](https://code.visualstudio.com/))

## 1. Maak een gegevensopslagruimte voor cloudbeheer

Maak eerst een gegevensopslagruimte voor cloudbeheerpakketten die wordt gebruikt om de WKND-site te implementeren. De WKND-site is een voorbeeld AEM websiteproject dat inhoud (Content Fragments) en een GraphQL-AEM bevat die wordt gebruikt door de React App van de snelle setup.

_Screencast van stappen_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. Navigeren naar [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. Cloud Manager selecteren __Programma__ die de AEM as a Cloud Service omgeving bevat die voor deze snelle installatie moet worden gebruikt
1. Een Git-opslagplaats maken voor het WKND-siteproject
   1. Selecteren __Opslagplaatsen__ in de bovenste navigatie
   1. Selecteren __Opslagplaats toevoegen__ in de bovenste actiebalk
   1. Geef de nieuwe Git-opslagplaats een naam: `aem-headless-quick-setup-wknd`
      + Namen van Git-opslagplaats moeten uniek zijn per Adobe-organisatie.
   1. Selecteren __Opslaan__ en wacht tot de Git-opslagplaats is geïnitialiseerd

## 2. Steek een voorbeeld van een WKND-siteproject naar de opslagplaats van de cloudmanager

Met de gegevensopslagplaats van de Git van de Manager van de Wolk creeerde, kloon de broncode van het project van de Plaats WKND van GitHub en duw het aan de bewaarplaats van de Git van de Manager van de Wolk. Cloud Manager heeft nu toegang tot het WKND-siteproject en implementeert dit in de AEM as a Cloud Service omgeving.

_Screencast van stappen_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

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
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. Verplaats de broncode van het voorbeeldproject van uw lokale Git-opslagruimte naar de cloudbeheeropslagplaats

   ```shell
   $ git push adobe main:main
   ```

   Geef bij de aanwijzing voor referenties de opdracht __Gebruikersnaam__ en __Wachtwoord__ van Cloud Manager __Info opslagplaats__ modal.

## 3. Implementeer de WKND-site om as a Cloud Service te AEM

Als het WKND-siteproject naar de gegevensopslagruimte van Cloud Manager Git wordt geduwd, kan het niet worden geïmplementeerd om as a Cloud Service te AEM met behulp van Cloud Manager-pijpleidingen.

Het WKND-siteproject biedt voorbeeldinhoud die door de React-app wordt gebruikt via AEM GraphQL-API&#39;s zonder koppen.

_Screencast van stappen_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

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
      1. Selecteer de __AEM as a Cloud Service ontwikkelomgeving__ van de __In aanmerking komende implementatieomgevingen__ Selectievakje
      1. Selecteren `aem-headless-quick-setup-wknd` in de __Bewaarplaats__ Selectievakje
      1. Selecteren `main` van de __Git Branch__ Selectievakje
      1. Selecteren __Opslaan__
1. Voer de __Implementatiepijp voor Dev__
   1. Selecteren __Overzicht__ in de bovenste navigatie
   1. De nieuwe versie zoeken __Dev Deployment-pijplijn__ in de __Pijpleidingen__ sectie
   1. Selecteer de __...__ rechts van de pijpleiding
   1. Selecteren __Uitvoeren__ en in het modaal
   1. Selecteer de __...__ rechts van de nu lopende pijpleiding
   1. Selecteren __Details weergeven__
1. Van de details van de pijpleidingsuitvoering, controleer vooruitgang tot het met succes voltooide. De uitvoering van de pijpleiding moet tussen 30 en 40 minuten duren.

## 4. Download en voer de WKND React-app uit

Met AEM as a Cloud Service bootstrapped met de inhoud van het project van de Plaats WKND, download, en begin de steekproefWKND React App die de inhoud van de Plaats van WKND over AEM Headless GraphQL APIs verbruikt.

_Screencast van stappen_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. Van de bevellijn, kloon de React broncode van App van GitHub.

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. De map openen `~/Code/aem-guides-wknd-graphql/react-app` in uw IDE.
1. Open het bestand in de IDE `.env.development`.
1. Wijs naar de as a Cloud Service AEM __Publiceren__ de gastheer URI van de dienst van  `REACT_APP_HOST_URI` eigenschap.

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
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

1. De React App, die plaatselijk loopt, begint op [http://localhost:3000](http://localhost:3000) en geeft een lijst weer van avonturen, die afkomstig zijn van AEM as a Cloud Service met behulp van AEM GraphQL API&#39;s zonder koppen.

## 5. Inhoud bewerken in AEM

Met het voorbeeld van de WKND React-app die verbinding maakt met en inhoud van de AEM Headless GraphQL-API&#39;s, ontwikkelt u inhoud in AEM Auteursservice en ziet u hoe de ervaring van React App in overleg wordt bijgewerkt.

_Screencast van stappen_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. Aanmelden bij AEM as a Cloud Service auteur-service
1. Navigeren naar __Assets > Files > WKND Shared > English > Adventures__
1. Open de __Cycli Southern Utah__ Map
1. Selecteer de __Cycli Southern Utah__ Inhoudsfragment en selecteer __Bewerken__ van de bovenste actiebalk
1. Werk enkele velden van het inhoudsfragment bij, bijvoorbeeld:
   + Titel: `Cycling Utah's National Parks`
   + Lengte van reisstrook: `6 Days`
   + Probleem: `Intermediate`
   + Prijs: `3500`
   + Primaire afbeelding: `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. Selecteren __Opslaan__ in de bovenste actiebalk
1. Selecteren __Snel publiceren__ van de bovenste actiebalk __...__
1. De React-app vernieuwen die wordt uitgevoerd op [http://localhost:3000](http://localhost:3000).
1. Selecteer in React App het nu bijgewerkte cyclusavontuur, en verifieer de inhoudsveranderingen die aan het Inhoudsfragment worden aangebracht.

1. Dezelfde aanpak gebruiken in AEM Auteursservice:
   1. Publiceer een bestaand fragment van de Inhoud van het Avontuur, en verifieer het wordt verwijderd uit React App ervaring
   1. Maak en publiceer een nieuw Adventure Content Fragment en controleer of dit wordt weergegeven in de React App-ervaring

   >[!TIP]
   >
   > Als u niet bekend bent met het maken en publiceren van nieuwe of het ongedaan maken van bestaande inhoudsfragmenten, bekijkt u de bovenstaande screencast.

## Gefeliciteerd!

Gefeliciteerd! U hebt AEM Headless met succes gebruikt om een React App aan te drijven!

Om in detail te begrijpen hoe React App inhoud van AEM as a Cloud Service verbruikt, controleer uit [de zelfstudie voor AEM zonder hoofd](../multi-step/overview.md). In de zelfstudie wordt onderzocht hoe inhoudsfragmenten in AEM worden gemaakt en hoe deze React-app hun inhoud als JSON-inhoud verbruikt.

### Volgende stappen

+ [Zelfstudie voor AEM zonder koppen starten](../multi-step/overview.md)
