---
title: Snelle de redacteur van opstellingsSPA en Verre SPA
description: Leer hoe te om met een verre Redacteur van het KUUROORD van SPA en van AEM in 15 minuten in werking te stellen!
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer
level: Beginner
jira: KT-7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
duration: 647
hide: true
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '730'
ht-degree: 0%

---

# Snelle installatie

{{spa-editor-deprecation}}

De snelle opstelling is een versnelde looppas-door illustrerend hoe te om app WKND en als Verre KUUROORD te installeren en in werking te stellen, en auteur het gebruikend de Redacteur van AEM SPA.

Met Snelle installatie gaat u rechtstreeks naar de eindstaat van deze zelfstudie.

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_A videolooppas-door van de snelle opstelling_

## Vereisten

Voor deze zelfstudie is het volgende vereist:

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=nl-NL)
+ [&#x200B; Node.js v18 &#x200B;](https://nodejs.org/en/)
+ [&#x200B; Java™ 11 &#x200B;](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [&#x200B; Gemaakt 3.6+ &#x200B;](https://maven.apache.org/)
+ [&#x200B; Git &#x200B;](https://git-scm.com/downloads)
+ Alleen macOS-voorwaarden
   + [&#x200B; Xcode &#x200B;](https://developer.apple.com/xcode/) of [&#x200B; Xcode bevel-lijn hulpmiddelen &#x200B;](https://developer.apple.com/xcode/resources/)
+ [&#x200B; aem-guides-wknd.all-2.1.0.zip of groter &#x200B;](https://github.com/adobe/aem-guides-wknd/releases)
+ [&#x200B; aem-gidsen-wknd-grafisch broncode (tak: eigenschap/spa-redacteur) &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Deze zelfstudie gaat uit van:

+ [&#x200B; Microsoft® Visual Studio Code &#x200B;](https://visualstudio.microsoft.com/) als winde
+ Een werkmap van `~/Code/wknd-app`
+ AEM SDK uitvoeren als een auteurservice op `http://localhost:4502`
+ AEM SDK uitvoeren met lokale `admin` account met wachtwoord `admin`
+ SPA uitvoeren op `http://localhost:3000`

## AEM SDK Quickstart starten

Download en installeer de AEM SDK Quickstart op poort 4502, met standaard `admin/admin` referenties.

1. [&#x200B; Download recentste AEM SDK &#x200B;](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=1)
1. AEM SDK uitpakken naar `~/aem-sdk`
1. AEM SDK Quickstart Jar uitvoeren

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK begint en lanceert automatisch op [&#x200B; http://localhost :4502 &#x200B;](http://localhost:4502). Meld u aan met de volgende referenties:

+ Gebruikersnaam: `admin`
+ Wachtwoord: `admin`

## WKND-sitepakket downloaden en installeren

Dit leerprogramma heeft een gebiedsdeel op __WKND 2.1.0+__ project (voor inhoud).

1. [&#x200B; Download de recentste versie van `aem-guides-wknd.all.x.x.x.zip` &#x200B;](https://github.com/adobe/aem-guides-wknd/releases)
1. Login aan de Manager van het Pakket van AEM SDK op [&#x200B; http://localhost :4502/crx/packmgr &#x200B;](http://localhost:4502/crx/packmgr) met de `admin` geloofsbrieven.
1. __uploadt__ `aem-guides-wknd.all.x.x.x.zip` gedownload in stap 1
1. Tik __installeer__ knoop voor de ingang `aem-guides-wknd.all-x.x.x.zip`

## WKND App SPA-pakketten downloaden en installeren

Om een snelle opstelling uit te voeren, worden de pakketten van AEM hier verstrekt die de definitieve configuratie en de inhoud van AEM van het leerprogramma bevatten.

1. [Downloaden &#x200B;](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Downloaden &#x200B;](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. Login aan de Manager van het Pakket van AEM SDK op [&#x200B; http://localhost :4502/crx/packmgr &#x200B;](http://localhost:4502/crx/packmgr) met de `admin` geloofsbrieven.
1. __uploadt__ `wknd-app.all.x.x.x.zip` gedownload in stap 1
1. Tik __installeer__ knoop voor de ingang `wknd-app.all.x.x.x.zip`
1. __uploadt__ `wknd-app.ui.content.sample.x.x.x.zip` gedownload in stap 2
1. Tik __installeer__ knoop voor de ingang `wknd-app.ui.content.sample.x.x.x.zip`

## De WKND App-bron downloaden

Download de van de bron WKND App code door van Github.com, en schakelaar de tak die de veranderingen in het KUUROORD bevat die in dit leerprogramma worden uitgevoerd.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## De toepassing SPA starten

Van de wortel van het project, installeer de projectenNpm van het KUUROORD gebiedsdelen en stel de toepassing in werking.

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

Verifieer dat het KUUROORD in [&#x200B; http://localhost :3000 &#x200B;](http://localhost:3000) loopt.

## Inhoud auteur in AEM SPA Editor

Alvorens de auteursinhoud uw browser vensters zo schikt dat de Auteur van AEM (`http://localhost:4502`) op de linkerzijde is, en verre KUUROORD (`http://localhost:3000`) loopt op het recht. Deze regeling staat u toe om te zien hoe de veranderingen in op AEM-Gebaseerde inhoud onmiddellijk in het KUUROORD worden weerspiegeld.

1. Login aan [&#x200B; de dienst van de Auteur van AEM SDK &#x200B;](http://localhost:4502) als `admin`
1. Navigeer aan __Plaatsen > App WKND > gebruiken > en__
1. Bewerk __WKND App Home Page__
1. Schakelaar aan __geeft__ wijze uit

### De vaste component van de weergave Home maken

1. Tik op de tekst __WKND avonturen__ om de vaste component van de Titel (die in de mening van het Huis van het KUUROORD wordt gehard) te activeren
1. Tik het __moersleutelpictogram__ op de de actiebar van de component van de Titel
1. Hiermee wordt de inhoud van de component Title gewijzigd en opgeslagen
1. Vernieuw SPA die op `http://localhost:3000` loopt en zie dat de gereflecteerde veranderingen

### Auteur van de containercomponent van de weergave Home

1. Terwijl nog het uitgeven van de __WKND App Homepage__..
1. Breid sidebar van de Redacteur van het KUUROORD __(op de linkerzijde) uit__
1. Tik de __Componenten__ pictogrammen
1. Componenten toevoegen, wijzigen of verwijderen uit de containercomponent die zich onder het WKND-logo en boven de vaste component Title bevindt
1. Vernieuw SPA die op `http://localhost:3000` loopt en zie dat de gereflecteerde veranderingen

### Auteur een containercomponent op een dynamische route

1. Schakelaar aan __wijze van de Voorproef__ in de Redacteur van het KUUROORD
1. Tik op de __kaart van de Camp van Surf van Bali__ en navigeer aan zijn dynamische route
1. Voeg toe, verander, of verwijder componenten uit de containercomponent die plaatsen boven de __rubriek van de 1&rbrace; Reis__
1. Vernieuw SPA die op `http://localhost:3000` loopt en zie dat de gereflecteerde veranderingen

De nieuwe pagina&#39;s van AEM onder de __WKND startpagina van de App > Avontuur__ _moet_ een de paginanaam hebben van AEM die de naam van het de inhoudsfragment van het overeenkomstige avontuur aanpast. Dit is omdat de route van het KUUROORD aan de afbeelding van de Pagina van AEM van het laatste segment van de route, die de naam van het Fragment van de Inhoud is.

## Gefeliciteerd!

U hebt net een snelle smaak van hoe de Redacteur van AEM SPA uw SPA met gecontroleerde, editable gebieden kan verbeteren! Als u geïnteresseerd bent - bekijk de rest van de zelfstudie, maar zorg ervoor dat u opnieuw begint, want in deze snelle installatie bevindt uw lokale ontwikkelomgeving zich nu in de eindfase van de zelfstudie!
