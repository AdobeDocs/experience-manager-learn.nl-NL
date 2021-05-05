---
title: Snelle installatie SPA Editor en externe SPA
description: Leer hoe u in 15 minuten aan de slag kunt met een externe SPA en AEM SPA Editor!
topic: Zwaardeloze, SPA, ontwikkeling
feature: SPA Editor, kerncomponenten, API's, ontwikkelen
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: kt-7629.jpeg
translation-type: tm+mt
source-git-commit: d3a237b196ac872beda6119c854a0cae29510437
workflow-type: tm+mt
source-wordcount: '794'
ht-degree: 1%

---


# Snelle installatie

Snelle opstelling is een versnelde looppas-door illustrerend hoe te om de app WKND en als Verre SPA te installeren en in werking te stellen, en het te schrijven gebruikend AEM SPA Redacteur.

Met Snelle installatie gaat u rechtstreeks naar de eindstaat van deze zelfstudie.

## Vereisten

Voor deze zelfstudie is het volgende vereist:

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Alleen MacOS-vereisten
   + [opdrachtregelprogramma&#39;s voor ](https://developer.apple.com/xcode/) Xcodeor  [Xcode](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all.0.3.0.zip of hoger](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql broncode](https://github.com/adobe/aem-guides-wknd-graphql)


Deze zelfstudie gaat uit van:

+ [Microsoft® Visual Studio ](https://visualstudio.microsoft.com/) Codeas winde
+ Een werkmap van `~/Code/wknd-app`
+ De AEM SDK uitvoeren als een auteurservice op `http://localhost:4502`
+ De AEM SDK uitvoeren met de lokale `admin`-account met wachtwoord `admin`
+ De SPA uitvoeren op `http://localhost:3000`

## Start de AEM SDK QuickStart

Download en installeer de AEM SDK QuickStart op poort 4502 met standaard `admin/admin` referenties.

1. [De nieuwste AEM SDK downloaden](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Pak de AEM SDK uit tot `~/aem-sdk`
1. De AEM SDK QuickStart Jar uitvoeren

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK wordt gestart en automatisch gestart op [http://localhost:4502](http://localhost:4502). Meld u aan met de volgende referenties:

+ Gebruikersnaam: `admin`
+ Wachtwoord: `admin`

## WKND-sitepakket downloaden en installeren

Deze zelfstudie is afhankelijk van het __WKND 0.3.0+&#39;s__-project (voor inhoud).

1. [Download de nieuwste versie van  `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Meld u aan bij AEM pakketbeheer van SDK op [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) met de `admin`-referenties.
1. __De in stap 1__ gedownloade bestanden  `aem-guides-wknd.all.x.x.x.zip` uploaden
1. Tik op de knop __Installeren__ voor de vermelding `aem-guides-wknd.all-x.x.x.zip`

## WKND App-SPA downloaden en installeren

Om een snelle opstelling uit te voeren, worden AEM pakketten verstrekt die de definitieve AEM van het leerprogramma configuratie en inhoud bevatten.

1. [Downloaden  `wknd-app.all.x.x.x.zip`](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Downloaden  `wknd-app.ui.content.sample.x.x.x.zip`](./assets/quick-setup/wknd-app.ui.content.sample-1.0.0.zip)
1. Meld u aan bij AEM pakketbeheer van SDK op [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) met de `admin`-referenties.
1. __De in stap 1__ gedownloade bestanden  `wknd-app.all.x.x.x.zip` uploaden
1. Tik op de knop __Installeren__ voor de vermelding `wknd-app.all.x.x.x.zip`
1. __De in stap 2__ gedownloade bestanden  `wknd-app.ui.content.sample.x.x.x.zip` uploaden
1. Tik op de knop __Installeren__ voor de vermelding `wknd-app.ui.content.sample.x.x.x.zip`

## De WKND App-bron downloaden

Download de broncode van de WKND-app van Github.com en schakel de vertakking met de wijzigingen in de SPA die in deze zelfstudie worden uitgevoerd over.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
$ git checkout -b feature/spa-editor
$ git pull origin feature/spa-editor
```

## De SPA starten

Van de wortel van het project, installeer de SPA projecten npm gebiedsdelen en stel de toepassing in werking.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

Als er fouten zijn bij het uitvoeren van `npm install`, probeer de volgende stappen:

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Controleer of de SPA wordt uitgevoerd op [http://localhost:3000](http://localhost:3000).

## Inhoud van auteur in AEM SPA Editor

Voordat u inhoud gaat ontwerpen, rangschikt u de browservensters zodanig dat de AEM-auteur (`http://localhost:4502`) aan de linkerkant staat en de externe SPA (`http://localhost:3000`) aan de rechterkant. Op deze manier kunt u zien hoe wijzigingen in inhoud op basis van AEM direct in de SPA worden doorgevoerd.

1. Aanmelden bij [AEM SDK-auteurservice](http://localhost:4502) als `admin`
1. Navigeer naar __Sites > WKND App > us > en__
1. __WKND App Home Page__ bewerken
1. Overschakelen naar modus __Bewerken__

### De vaste component van de weergave Home maken

1. Tik op de tekst __WKND Adventures__ om de vaste component Title te activeren (gecodeerd in de SPA Home-weergave)
1. Tik op het pictogram __moersleutel__ op de actiebalk van de component Title
1. Hiermee wordt de inhoud van de component Title gewijzigd en opgeslagen
1. Vernieuw de SPA die op `http://localhost:3000` lopen en zie dat de veranderingen weerspiegeld waren

### Auteur van de containercomponent van de weergave Home

1. Tijdens het bewerken van __WKND App Home Page__..
1. De zijbalk van de __SPA Editor__ uitvouwen (links)
1. Tik op de pictogrammen __Components__
1. Componenten toevoegen, wijzigen of verwijderen uit de containercomponent die zich onder het WKND-logo en boven de vaste component Title bevindt
1. Vernieuw de SPA die op `http://localhost:3000` lopen en zie dat de veranderingen weerspiegeld waren

### Auteur een containercomponent op een dynamische route

1. Overschakelen naar de modus __Voorvertoning__ in SPA Editor
1. Tik op de __Bali Surf Camp__ kaart en navigeer naar de dynamische route
1. Componenten toevoegen, wijzigen of verwijderen uit de containercomponent die zich boven de kop __Inleiding__ bevindt
1. Vernieuw de SPA die op `http://localhost:3000` lopen en zie dat de veranderingen weerspiegeld waren

Nieuwe AEM onder __WKND App Home page > Adventure__ _must_ hebben een AEM paginanaam die de naam van het de inhoudfragment van het overeenkomstige avontuur aanpast. Dit is omdat de SPA route aan AEM van de Pagina afbeelding van het laatste segment van de route, die de naam van het Fragment van de Inhoud is gebaseerd.

## Gefeliciteerd!

U hebt net een snelle smaak van hoe AEM SPA Editor uw SPA kan verbeteren met gecontroleerde, bewerkbare gebieden! Als u geïnteresseerd bent - bekijk de rest van de zelfstudie, maar zorg ervoor dat u opnieuw begint, want in deze snelle installatie bevindt uw lokale ontwikkelomgeving zich nu in de eindfase van de zelfstudie!
