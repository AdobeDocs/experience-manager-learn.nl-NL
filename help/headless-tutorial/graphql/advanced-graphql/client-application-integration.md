---
title: Integratie van clienttoepassingen - Geavanceerde concepten van AEM Headless - GraphQL
description: Voer voortgezette vragen uit en integreer het in app WKND.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
duration: 241
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 0%

---

# Integratie van clienttoepassingen

In het vorige hoofdstuk, creeerde en bijwerkte voortgeduurde vragen gebruikend Ontdekkingsreiziger GraphiQL.

Dit hoofdstuk begeleidt u door de stappen om de voortgeduurde vragen met de WKND cliënttoepassing (alias WKND App) te integreren gebruikend de verzoeken van HTTP GET binnen bestaande **componenten van het Reageren**. Het biedt ook een optionele uitdaging om uw AEM Headless-lessen toe te passen, codeerexpertise om de WKND-clienttoepassing te verbeteren.

## Vereisten {#prerequisites}

Dit document is onderdeel van een zelfstudie met meerdere onderdelen. Controleer of de vorige hoofdstukken zijn voltooid voordat u verdergaat met dit hoofdstuk. De WKND cliënttoepassing verbindt met AEM publiceert dienst, zodat is het belangrijk dat u **het volgende aan AEM publiceert de dienst** publiceert.

* Projectconfiguraties
* GraphQL-eindpunten
* Modellen van inhoudsfragmenten
* Geautoriseerde inhoudsfragmenten
* GraphQL blijft vragen

De _IDE screenshots in dit hoofdstuk komen uit [ Code van Visual Studio ](https://code.visualstudio.com/)_

### Hoofdstuk 1-4 Oplossingspakket (optioneel) {#solution-package}

Er is een oplossingspakket beschikbaar dat de stappen in de AEM-gebruikersinterface voor hoofdstukken 1-4 voltooit. Dit pakket is **niet nodig** als de vorige hoofdstukken zijn voltooid.

1. Download [ geavanceerd-GraphQL-Tutorial-Oplossing-Pakket-1.2.zip ](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip).
1. In AEM, navigeer aan **Hulpmiddelen** > **Plaatsing** > **Pakketten** om tot **de Manager van het Pakket** toegang te hebben.
1. Upload en installeer het pakket (ZIP-bestand) dat u in de vorige stap hebt gedownload.
1. Repliceer het pakket naar de AEM-publicatieservice

## Doelstellingen {#objectives}

In dit leerprogramma, leert u hoe te om de verzoeken om voortgezette vragen in de steekproefWKND GraphQL React app te integreren gebruikend de [ Hoofdloze Cliënt van AEM voor JavaScript ](https://github.com/adobe/aem-headless-client-js).

## De voorbeeldclienttoepassing klonen en uitvoeren {#clone-client-app}

Om de zelfstudie te versnellen, wordt een startapp van React JS geleverd.

1. Kloon de [ adobe/aem-guides-wknd-grafisch ](https://github.com/adobe/aem-guides-wknd-graphql) bewaarplaats:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Bewerk het `aem-guides-wknd-graphql/advanced-tutorial/.env.development` -bestand en stel `REACT_APP_HOST_URI` zo in dat dit naar de AEM-publicatieservice van uw doel verwijst.

   Werk de authentificatiemethode bij als het verbinden met een auteursinstantie.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-pxx-eyy.adobeaemcloud.com
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=
   REACT_APP_BASIC_AUTH_PASS=
   ```

   ![ Reageer de Milieu van de Ontwikkeling van de Toepassing ](assets/client-application-integration/react-app-dev-env-settings.png)


   >[!NOTE]
   > 
   > De bovenstaande instructies moeten Reageren app met de **AEM verbinden publiceer dienst**, nochtans om met de **dienst van de Auteur van AEM** te verbinden een lokaal ontwikkelingsteken voor uw milieu van doelAEM as a Cloud Service verkrijgen.
   >
   > Het is ook mogelijk om app met a [ lokale instantie van de Auteur te verbinden gebruikend AEMaaCS SDK ](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) gebruikend basisauthentificatie.


1. Open een terminal en voer de opdrachten uit:

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. Een nieuw browser venster zou op [ http://localhost:3000 ](http://localhost:3000) moeten laden


1. Tik **Camping** > **Yosemite Backpackaging** om de Yosemite details van het Backpackaging-avontuur te bekijken.

   ![ Yosemite het BackpackagingScherm ](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. Open de ontwikkelaarsgereedschappen van de browser en inspecteer de aanvraag `XHR`

   ![ POST GraphQL ](assets/client-application-integration/graphql-persisted-query.png)

   U zou `GET` verzoeken aan het eindpunt van GraphQL met project config naam (`wknd-shared`), voortgeduurde vraagnaam (`adventure-by-slug`), veranderlijke naam (`slug`), waarde (`yosemite-backpacking`), en speciale karaktercoderingen moeten zien.

>[!IMPORTANT]
>
>    Als u zich afvraagt waarom het GraphQL API verzoek tegen `http://localhost:3000` en NIET tegen AEM wordt gemaakt publiceer het domein van de Dienst, overzicht [ onder het Hood ](../multi-step/graphql-and-react-app.md#under-the-hood) van Basiszelfstudie.


## De code controleren

In het [ Basisleerprogramma - bouw een Reactie app die AEM APIs ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object) stap gebruikt hadden wij herzien en verbeterd weinig zeer belangrijke dossiers om hands-on deskundigheid te krijgen. Controleer de belangrijkste bestanden voordat u de WKND-app verbetert.

* [ herzie het voorwerp AEMHeadless ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object)

* [ Implementeer om AEM GraphQL in werking te stellen persisted query&#39;s ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#implement-to-run-aem-graphql-persisted-queries)

### Component redigeren `Adventures` Reageren

De WKND React app belangrijkste mening is de lijst van alle avonturen en u kunt deze die avonturen filtreren op activiteitstype zoals _Camping, Cyclling_ worden gebaseerd. Deze weergave wordt gerenderd door de component `Adventures` . Hieronder vindt u de belangrijkste implementatiedetails:

* Het argument `src/components/Adventures.js` call `useAllAdventures(adventureActivity)` haak en here `adventureActivity` is activity type.

* De `useAllAdventures(adventureActivity)` haak wordt gedefinieerd in het `src/api/usePersistedQueries.js` -bestand. Gebaseerd op `adventureActivity` waarde, bepaalt het welke voortgezette vraag om te roepen. Als de waarde null is, wordt `wknd-shared/adventures-by-activity` aangeroepen, anders worden alle beschikbare avonturen `wknd-shared/adventures-all` opgehaald.

* De haak gebruikt de hoofdfunctie `fetchPersistedQuery(..)` die de uitvoering van de query aan `AEMHeadless` via `aemHeadlessClient.js` delegeert.

* De haak retourneert ook alleen de relevante gegevens van de AEM GraphQL-respons op `response.data?.adventureList?.items` , zodat de `Adventures` React view-componenten niet op de hoogte zijn van de JSON-bovenliggende structuren.

* Op succesvolle vraaguitvoering, `AdventureListItem(..)` geef functie van `Adventures.js` terug voegt het element van HTML toe om het _Beeld, de Lengte van de Trip, de Prijs, en de informatie van de Titel_ te tonen.

### Component redigeren `AdventureDetail` Reageren

De component `AdventureDetail` React geeft de details van het avontuur terug. Hieronder vindt u de belangrijkste implementatiedetails:

* Het argument `src/components/AdventureDetail.js` call `useAdventureBySlug(slug)` haak en here `slug` is queryparameter.

* Zoals hierboven wordt de `useAdventureBySlug(slug)` haak gedefinieerd in het `src/api/usePersistedQueries.js` -bestand. `wknd-shared/adventure-by-slug` persisted query wordt aangeroepen door te delegeren aan `AEMHeadless` via `aemHeadlessClient.js` .

* Als de query met succes is uitgevoerd, voegt de renderfunctie `AdventureDetailRender(..)` van `AdventureDetail.js` het HTML-element toe om de Adventure-details weer te geven.


## De code verbeteren

### Gebruik `adventure-details-by-slug` voortgezette query

In het vorige hoofdstuk, creeerden wij de `adventure-details-by-slug` voortgezette vraag, verstrekt het extra informatie van het avontuur zoals _plaats, instructorTeam, en beheerder_. Vervang `adventure-by-slug` door `adventure-details-by-slug` aanhoudende query om deze extra informatie te renderen.

1. Open `src/api/usePersistedQueries.js`.

1. De functie zoeken `useAdventureBySlug()` en query bijwerken als

```javascript
 ...

 // Call the AEM GraphQL persisted query named "wknd-shared/adventure-details-by-slug" with parameters
 response = await fetchPersistedQuery(
 "wknd-shared/adventure-details-by-slug",
 queryParameters
 );

 ...
```

### Aanvullende informatie weergeven

1. Open `src/components/AdventureDetail.js` als u aanvullende informatie over de avontuur wilt weergeven

1. Zoek de functie `AdventureDetailRender(..)` en werk de retourfunctie bij als

   ```javascript
   ...
   
   return (<>
       <h1 className="adventure-detail-title">{title}</h1>
       <div className="adventure-detail-info">
   
           <LocationInfo {...location} />
   
           ...
   
           <Location {...location} />
   
           <Administrator {...administrator} />
   
           <InstructorTeam {...instructorTeam} />
   
       </div>
   </>); 
   
   ...
   ```

1. Definieer ook de corresponderende renderfuncties:

   **LocationInfo**

   ```javascript
   function LocationInfo({name}) {
   
       if (!name) {
           return null;
       }
   
       return (
           <>
               <div className="adventure-detail-info-label">Location</div>
               <div className="adventure-detail-info-description">{name}</div>
           </>
       );
   
   }
   ```

   **Plaats**

   ```javascript
   function Location({ contactInfo }) {
   
       if (!contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-location'>
                   <h2>Where we meet</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Phone:{contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email:{contactInfo.email}</div>
               </div>
           </>);
   }
   ```

   **InstructorTeam**

   ```javascript
   function InstructorTeam({ _metadata }) {
   
       if (!_metadata) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-team'>
                   <h2>Instruction Team</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Team Name: {_metadata.stringMetadata[0].value}</div>
               </div>
           </>);
   }
   ```

   **Beheerder**

   ```javascript
   function Administrator({ fullName, contactInfo }) {
   
       if (!fullName || !contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-administrator'>
                   <h2>Administrator</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Name: {fullName}</div>
                   <div className="adventure-detail-addtional-info">Phone: {contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email: {contactInfo.email}</div>
               </div>
           </>);
   }
   ```

### Nieuwe stijlen definiëren

1. Open `src/components/AdventureDetail.scss` en voeg de volgende klassendefinities toe

   ```CSS
   .adventure-detail-administrator,
   .adventure-detail-team,
   .adventure-detail-location {
   margin-top: 1em;
   width: 100%;
   float: right;
   }
   
   .adventure-detail-addtional-info {
   padding: 10px 0px 5px 0px;
   text-transform: uppercase;
   }
   ```

>[!TIP]
>
>De bijgewerkte dossiers zijn beschikbaar onder **AEM Guides WKND - GraphQL** project, zie [ Geavanceerde sectie van het Leerprogramma ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial).


Nadat de bovenstaande verbeteringen zijn voltooid, ziet de WKND-app er als volgt uit en toont de ontwikkelaarsgereedschappen van de browser `adventure-details-by-slug` de voortgezette queryaanroep.

![ Verbeterde APP van WKND ](assets/client-application-integration/Enhanced-WKND-APP.gif)

## Uitdaging voor uitbreiding (optioneel)

WKND Reageer app&#39;s belangrijkste mening staat u toe om deze avonturen te filtreren die op activiteitentype zoals _worden gebaseerd Camping, het Cycliseren_. Nochtans het bedrijfsteam van WKND wil een extra _Gebaseerde het filtreren van de Plaats_ vermogen hebben. De eisen zijn:

* Op WKND App&#39;s belangrijkste mening, in de hoogste linkerzijde of juiste hoek voeg _het filtreren van de Plaats_ pictogram toe.
* Het klikken _het filtreren van de Plaats_ pictogram zou lijst van plaatsen moeten tonen.
* Als u op een gewenste locatie in de lijst klikt, worden alleen overeenkomende avonturen weergegeven.
* Als er slechts één passend avontuur is, wordt de mening van de Details van het Avontuur getoond.

## Gefeliciteerd

Gefeliciteerd! U hebt de voortgezette query&#39;s nu geïntegreerd en geïmplementeerd in de WKND-voorbeeldapp.
