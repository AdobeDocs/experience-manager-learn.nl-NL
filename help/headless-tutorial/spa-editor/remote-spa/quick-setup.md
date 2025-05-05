---
title: Snelle installatie SPA Editor en externe SPA
description: Leer hoe u in 15 minuten aan de slag kunt met een externe SPA en AEM SPA Editor!
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
duration: 647
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 0%

---

# Snelle installatie

Snelle opstelling is een versnelde looppas-door illustrerend hoe te om de app WKND en als Verre SPA te installeren en in werking te stellen, en het te schrijven gebruikend AEM SPA Redacteur.

Met Snelle installatie gaat u rechtstreeks naar de eindstaat van deze zelfstudie.

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_A videolooppas-door van de snelle opstelling_

## Vereisten

Voor deze zelfstudie is het volgende vereist:

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=nl-NL)
+ [ Node.js v18 ](https://nodejs.org/en/)
+ [ Java™ 11 ](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [ Gemaakt 3.6+ ](https://maven.apache.org/)
+ [ Git ](https://git-scm.com/downloads)
+ Alleen macOS-voorwaarden
   + [ Xcode ](https://developer.apple.com/xcode/) of [ Xcode bevel-lijn hulpmiddelen ](https://developer.apple.com/xcode/resources/)
+ [ aem-guides-wknd.all-2.1.0.zip of groter ](https://github.com/adobe/aem-guides-wknd/releases)
+ [ aem-gidsen-wknd-grafisch broncode (tak: eigenschap/spa-redacteur) ](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Deze zelfstudie gaat uit van:

+ [ Microsoft® Visual Studio Code ](https://visualstudio.microsoft.com/) als winde
+ Een werkmap van `~/Code/wknd-app`
+ De AEM SDK uitvoeren als een auteurservice op `http://localhost:4502`
+ De AEM SDK uitvoeren met het lokale `admin` -account met het wachtwoord `admin`
+ De SPA uitvoeren op `http://localhost:3000`

## Start de AEM SDK QuickStart

Download en installeer de AEM SDK QuickStart op poort 4502, met standaard `admin/admin` referenties.

1. [ Download recentste AEM SDK ](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Pak de AEM SDK uit tot `~/aem-sdk`
1. Voer de AEM SDK QuickStart Jar uit

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK begint en begint automatisch op [ http://localhost:4502 ](http://localhost:4502). Meld u aan met de volgende referenties:

+ Gebruikersnaam: `admin`
+ Wachtwoord: `admin`

## WKND-sitepakket downloaden en installeren

Dit leerprogramma heeft een gebiedsdeel op __WKND 2.1.0+__ project (voor inhoud).

1. [ Download de recentste versie van `aem-guides-wknd.all.x.x.x.zip` ](https://github.com/adobe/aem-guides-wknd/releases)
1. Login aan AEM Manager van het Pakket van SDK in [ http://localhost:4502/crx/packmgr ](http://localhost:4502/crx/packmgr) met de `admin` geloofsbrieven.
1. __uploadt__ `aem-guides-wknd.all.x.x.x.zip` gedownload in stap 1
1. Tik __installeer__ knoop voor de ingang `aem-guides-wknd.all-x.x.x.zip`

## WKND App-SPA downloaden en installeren

Om een snelle opstelling uit te voeren, worden AEM pakketten verstrekt hier die de definitieve AEM van het leerprogramma configuratie en inhoud bevatten.

1. [Downloaden ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Downloaden ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. Login aan AEM Manager van het Pakket van SDK in [ http://localhost:4502/crx/packmgr ](http://localhost:4502/crx/packmgr) met de `admin` geloofsbrieven.
1. __uploadt__ `wknd-app.all.x.x.x.zip` gedownload in stap 1
1. Tik __installeer__ knoop voor de ingang `wknd-app.all.x.x.x.zip`
1. __uploadt__ `wknd-app.ui.content.sample.x.x.x.zip` gedownload in stap 2
1. Tik __installeer__ knoop voor de ingang `wknd-app.ui.content.sample.x.x.x.zip`

## De WKND App-bron downloaden

Download de broncode van de WKND App door van Github.com, en schakelaar de tak die de veranderingen in de SPA bevat die in dit leerprogramma worden uitgevoerd.

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

Voer de volgende stappen uit als er fouten optreden bij het uitvoeren van `npm install` :

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Verifieer dat de SPA in [ http://localhost:3000 ](http://localhost:3000) loopt.

## Inhoud van auteur in AEM SPA Editor

Alvorens inhoud te ontwerpen rangschikt uw browser vensters zodanig dat AEM Auteur (`http://localhost:4502`) op de linkerzijde is, en de verre SPA (`http://localhost:3000`) loopt op het recht. Op deze manier kunt u zien hoe wijzigingen in inhoud op basis van AEM direct in de SPA worden doorgevoerd.

1. Login aan [ AEM de dienst van de Auteur van SDK ](http://localhost:4502) als `admin`
1. Navigeer aan __Plaatsen > App WKND > gebruiken > en__
1. Bewerk __WKND App Home Page__
1. Schakelaar aan __geeft__ wijze uit

### De vaste component van de weergave Home maken

1. Tik op de tekst __WKND avonturen__ om de vaste component van de Titel (die in de SPA mening van het Huis wordt gecodeerd) te activeren
1. Tik het __moersleutelpictogram__ op de de actiebar van de component van de Titel
1. Hiermee wordt de inhoud van de component Title gewijzigd en opgeslagen
1. De SPA vernieuwen die op `http://localhost:3000` wordt uitgevoerd en controleren of de wijzigingen zijn doorgevoerd

### Auteur van de containercomponent van de weergave Home

1. Terwijl nog het uitgeven van de __WKND App Homepage__..
1. Breid sidebar van de __SPA Redacteur__ (op de linkerzijde) uit
1. Tik de __Componenten__ pictogrammen
1. Componenten toevoegen, wijzigen of verwijderen uit de containercomponent die zich onder het WKND-logo en boven de vaste component Title bevindt
1. De SPA vernieuwen die op `http://localhost:3000` wordt uitgevoerd en controleren of de wijzigingen zijn doorgevoerd

### Auteur een containercomponent op een dynamische route

1. Schakelaar aan __wijze van de Voorproef__ in SPA Redacteur
1. Tik op de __kaart van de Camp van Surf van Bali__ en navigeer aan zijn dynamische route
1. Voeg toe, verander, of verwijder componenten uit de containercomponent die plaatsen boven de __rubriek van de 1&rbrace; Reis__
1. De SPA vernieuwen die op `http://localhost:3000` wordt uitgevoerd en controleren of de wijzigingen zijn doorgevoerd

De nieuwe AEM pagina&#39;s onder de __WKND startpagina van de App > Avontuur__ _moeten_ een AEM paginanaam hebben die de naam van het de tevreden Fragment van de Inhoud van het overeenkomstige avontuur aanpast. Dit is omdat de SPA route aan AEM van de Pagina afbeelding van het laatste segment van de route, die de naam van het Fragment van de Inhoud is gebaseerd.

## Gefeliciteerd!

U hebt net een snelle smaak van hoe AEM SPA Editor uw SPA kan verbeteren met gecontroleerde, bewerkbare gebieden! Als u geïnteresseerd bent - bekijk de rest van de zelfstudie, maar zorg ervoor dat u opnieuw begint, want in deze snelle installatie bevindt uw lokale ontwikkelomgeving zich nu in de eindfase van de zelfstudie!
