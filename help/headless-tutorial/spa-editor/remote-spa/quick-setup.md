---
title: Snelle installatie SPA Editor en externe SPA
description: Leer hoe u in 15 minuten aan de slag kunt met een externe SPA en AEM SPA Editor!
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 1%

---

# Snelle installatie

Snelle opstelling is een versnelde looppas-door illustrerend hoe te om de app WKND en als Verre SPA te installeren en in werking te stellen, en het te schrijven gebruikend AEM SPA Redacteur.

Met Snelle installatie gaat u rechtstreeks naar de eindstaat van deze zelfstudie.

>[!VIDEO](https://video.tv.adobe.com/v/333181/?quality=12&learn=on)

_Een video die door de snelle opstelling loopt_

## Vereisten

Voor deze zelfstudie is het volgende vereist:

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v16+](https://nodejs.org/en/)
+ [npm v8+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Alleen macOS-voorwaarden
   + [Xcode](https://developer.apple.com/xcode/) of [Xcode-opdrachtregelprogramma&#39;s](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip of hoger](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphic broncode (vertakking: feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Deze zelfstudie gaat uit van:

+ [Microsoft® Visual Studio-code](https://visualstudio.microsoft.com/) als IDE
+ Een werkmap van `~/Code/wknd-app`
+ De AEM SDK uitvoeren als een auteurservice ingeschakeld `http://localhost:4502`
+ De AEM SDK uitvoeren met de lokale `admin` account met wachtwoord `admin`
+ De SPA uitvoeren `http://localhost:3000`

## Start de AEM SDK QuickStart

Download en installeer de AEM SDK QuickStart op poort 4502, standaard `admin/admin` referenties.

1. [De nieuwste AEM SDK downloaden](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Pak de AEM SDK uit naar `~/aem-sdk`
1. De AEM SDK QuickStart Jar uitvoeren

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK wordt gestart en automatisch gestart [http://localhost:4502](http://localhost:4502). Meld u aan met de volgende referenties:

+ Gebruikersnaam: `admin`
+ Wachtwoord: `admin`

## WKND-sitepakket downloaden en installeren

Deze zelfstudie is afhankelijk van __WKND 2.1.0+&#39;s__ project (voor inhoud).

1. [Download de nieuwste versie van `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Meld u aan bij AEM pakketbeheer van SDK op [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) met de `admin` referenties.
1. __Uploaden__ de `aem-guides-wknd.all.x.x.x.zip` gedownload in stap 1
1. Tik op de knop __Installeren__ knop voor de vermelding `aem-guides-wknd.all-x.x.x.zip`

## WKND App-SPA downloaden en installeren

Om een snelle opstelling uit te voeren, worden AEM pakketten verstrekt hier die de definitieve AEM van het leerprogramma configuratie en inhoud bevatten.

1. [Downloaden ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Downloaden ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. Meld u aan bij AEM pakketbeheer van SDK op [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) met de `admin` referenties.
1. __Uploaden__ de `wknd-app.all.x.x.x.zip` gedownload in stap 1
1. Tik op de knop __Installeren__ knop voor de vermelding `wknd-app.all.x.x.x.zip`
1. __Uploaden__ de `wknd-app.ui.content.sample.x.x.x.zip` gedownload in stap 2
1. Tik op de knop __Installeren__ knop voor de vermelding `wknd-app.ui.content.sample.x.x.x.zip`

## De WKND App-bron downloaden

Download de broncode van de WKND-app van Github.com en schakel de vertakking met de wijzigingen in de SPA die in deze zelfstudie worden uitgevoerd over.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## De SPA starten

Van de wortel van het project, installeer de SPA projecten npm gebiedsdelen en stel de toepassing in werking.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

Als er fouten optreden bij het uitvoeren `npm install` Voer de volgende stappen uit:

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Controleren of de SPA wordt uitgevoerd op [http://localhost:3000](http://localhost:3000).

## Inhoud van auteur in AEM SPA Editor

Voordat u inhoud gaat ontwerpen, moet u uw browservensters zodanig rangschikken dat AEM-auteur (`http://localhost:4502`) staat links en de SPA (`http://localhost:3000`) aan de rechterkant. Op deze manier kunt u zien hoe wijzigingen in inhoud op basis van AEM direct in de SPA worden doorgevoerd.

1. Aanmelden bij [AEM SDK-auteurservice](http://localhost:4502) als `admin`
1. Navigeren naar __Sites > WKND App > us > en__
1. Bewerken __WKND App Home Page__
1. Overschakelen op __Bewerken__ mode

### De vaste component van de weergave Home maken

1. Tik op de tekst __WKND Adventures__ om de vaste component Titel te activeren (gecodeerd in de SPA weergave Home)
1. Tik op de knop __moersleutel__ pictogram op de actiebalk van de component Title
1. Hiermee wordt de inhoud van de component Title gewijzigd en opgeslagen
1. De SPA die wordt uitgevoerd vernieuwen `http://localhost:3000` en zie de wijzigingen

### Auteur van de containercomponent van de weergave Home

1. Tijdens het bewerken van de __WKND App Home Page__...
1. Breid uit __SPA zijbalk van de Editor__ (links)
1. Tik op de knop __Componenten__ pictogrammen
1. Componenten toevoegen, wijzigen of verwijderen uit de containercomponent die zich onder het WKND-logo en boven de vaste component Title bevindt
1. De SPA die wordt uitgevoerd vernieuwen `http://localhost:3000` en zie de wijzigingen

### Auteur een containercomponent op een dynamische route

1. Overschakelen op __Voorvertoning__ modus in SPA Editor
1. Tik op de knop __Bali Surf Camp__ kaart en navigeer aan zijn dynamische route
1. Componenten toevoegen, wijzigen of verwijderen uit de containercomponent die zich boven de __Urinertie__ kop
1. De SPA die wordt uitgevoerd vernieuwen `http://localhost:3000` en zie de wijzigingen

Nieuwe AEM onder de __WKND App Home page > Adventure__ _moet_ hebben een AEM paginanaam die overeenkomt met de naam van het inhoudsfragment van het desbetreffende avontuur. Dit is omdat de SPA route aan AEM van de Pagina afbeelding van het laatste segment van de route, die de naam van het Fragment van de Inhoud is gebaseerd.

## Gefeliciteerd!

U hebt net een snelle smaak van hoe AEM SPA Editor uw SPA kan verbeteren met gecontroleerde, bewerkbare gebieden! Als u geïnteresseerd bent - bekijk de rest van de zelfstudie, maar zorg ervoor dat u opnieuw begint, want in deze snelle installatie bevindt uw lokale ontwikkelomgeving zich nu in de eindfase van de zelfstudie!
