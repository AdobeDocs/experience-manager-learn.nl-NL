---
title: AEM Headless en Target integreren
description: Leer hoe u AEM Headless en Adobe Target integreert om ervaringen zonder kop aan te passen met de Experience Platform Web SDK.
version: Experience Manager as a Cloud Service
feature: Content Fragments, Integrations
topic: Personalization, Headless
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-09T00:00:00Z
jira: KT-12433
thumbnail: KT-12433.jpeg
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: be886c64-9b8e-498d-983c-75f32c34be4b
duration: 1549
source-git-commit: adc2f352544b4718522073642c6bf971b3600616
workflow-type: tm+mt
source-wordcount: '1618'
ht-degree: 0%

---

# AEM Headless en Target integreren

Leer hoe u AEM Headless kunt integreren met Adobe Target door AEM Content Fragments te exporteren naar Adobe Target en deze te gebruiken voor het aanpassen van ervaringen zonder kop met Adobe Experience Platform Web SDK alloy.js. [ Reageer WKND App ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html?lang=nl-NL) wordt gebruikt om te onderzoeken hoe een gepersonaliseerde activiteit van het Doel gebruikend de Aanbiedingen van de Fragmenten van de Inhoud aan de ervaring kan worden toegevoegd, om een avontuur te bevorderen WKND.

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

In de zelfstudie worden de stappen beschreven die nodig zijn voor het instellen van AEM en Adobe Target:

1. [ creeer de Configuratie van Adobe IMS voor Adobe Target ](#adobe-ims-configuration) in de Auteur van AEM
2. [ creeer Adobe Target Cloud Service ](#adobe-target-cloud-service) in de Auteur van AEM
3. [ pas Adobe Target Cloud Service op de omslagen van AEM Assets ](#configure-asset-folders) in de Auteur van AEM toe
4. [ Toestemming Adobe Target Cloud Service ](#permission) in Adobe Admin Console
5. [ de Fragmenten van de Inhoud van de Uitvoer ](#export-content-fragments) van de Auteur van AEM aan Doel
6. [ creeer een Activiteit gebruikend de Aanbiedingen van het Fragment van de Inhoud ](#activity) in Adobe Target
7. [ creeer een DataStream van Experience Platform ](#datastream-id) in Experience Platform
8. [ integreer verpersoonlijking in React-Gebaseerde app van AEM Headless ](#code) gebruikend het Web SDK van Adobe.

## Adobe IMS-configuratie{#adobe-ims-configuration}

Een Adobe IMS-configuratie die verificatie tussen AEM en Adobe Target vergemakkelijkt.

Herzie [ de documentatie ](https://experienceleague.adobe.com/nl/docs/experience-manager-learn/cloud-service/integrations/target#adobe-target-cloud-service) voor geleidelijke instructies op hoe te om een configuratie van Adobe te creëren IMS.

## Adobe Target Cloud Service{#adobe-target-cloud-service}

In AEM wordt een Adobe Target Cloud Service gemaakt om het exporteren van inhoudsfragmenten naar Adobe Target te vergemakkelijken.

Herzie [ de documentatie ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html?lang=nl-NL) voor geleidelijke instructies op hoe te om een Adobe Target Cloud Service tot stand te brengen.

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## Elementmappen configureren{#configure-asset-folders}

Adobe Target Cloud Service, geconfigureerd in een contextbewuste configuratie, moet worden toegepast op de AEM Assets-maphiërarchie die de Content Fragments bevat die naar Adobe Target moeten worden geëxporteerd.

+++Vergroten voor geleidelijke instructies

1. Login aan __de dienst van de Auteur van AEM__ als beheerder DAM
1. Navigeer aan __Assets > Dossiers__, bepaal de plaats van de activa omslag die `/conf` heeft toegepast op
1. Selecteer de activaomslag, en selecteer __Eigenschappen__ van de hoogste actiebar
1. Selecteer het __lusje van de Diensten van de Wolk 0&rbrace; &lbrace;__
1. Zorg ervoor dat de Configuratie van de Wolk aan de context-bewuste config (`/conf`) wordt geplaatst die de configuratie van de Diensten van de Wolk van Adobe Target bevat.
1. Selecteer __Adobe Target__ van __Cloud Service Configurations__ dropdown.
1. Selecteer __sparen &amp; Sluiten__ in het hoogste recht

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## Toestemming voor de integratie met AEM Target{#permission}

De integratie van Adobe Target, die zich als developer.adobe.com project manifesteert, moet de __redacteur__ productrol in Adobe Admin Console worden verleend, om de Fragmenten van de Inhoud naar Adobe Target uit te voeren.

+++Vergroten voor geleidelijke instructies

1. Meld u aan bij Experience Cloud als gebruiker die het Adobe Target-product in Adobe Admin Console kan beheren
1. Open [ Adobe Admin Console ](https://adminconsole.adobe.com)
1. Selecteer __Producten__ en open dan __Adobe Target__
1. Op het __lusje van Profielen van het Product__, uitgezochte __*DefaultWorkspace*__
1. Selecteer het __API geloofsbrieven__ lusje
1. Bepaal de plaats van uw developer.adobe.com app in deze lijst en plaats zijn __Rol van het Product__ aan __Redacteur__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## Inhoudsfragmenten exporteren naar doel{#export-content-fragments}

De Fragmenten van de inhoud die onder de [ gevormde de de de omslaghiërarchie van AEM Assets ](#apply-adobe-target-cloud-service-to-aem-assets-folders) bestaan kunnen aan Adobe Target als Aanbiedingen van het Fragment van de Inhoud worden uitgevoerd. Deze Content Fragment-aanbiedingen, een speciale vorm van JSON-aanbiedingen in Target, kunnen worden gebruikt in Target-activiteiten om persoonlijke ervaringen te bieden in toepassingen zonder kop.

+++Vergroten voor geleidelijke instructies

1. Login aan __de Auteur van AEM__ als gebruiker DAM
1. Navigeer aan __Assets > Dossiers__, en bepaal de plaats van de Fragmenten van de Inhoud om als JSON aan Doel onder de &quot;toegelaten Adobe Target&quot;omslag uit te voeren
1. Selecteer de inhoudsfragmenten die u wilt exporteren naar Adobe Target
1. Selecteer __Uitvoer aan Aanbiedingen van Adobe Target__ van de hoogste actiebar
   + Met deze actie exporteert u de volledig gehydrateerde JSON-weergave van het inhoudsfragment naar Adobe Target als een &quot;Content Fragment-aanbieding&quot;
   + De volledig gehydrateerde JSON-representatie kan in AEM worden bekeken
      + Selecteer het inhoudsfragment
      + Het zijpaneel uitbreiden
      + Selecteer __pictogram van de Voorproef 0&rbrace; &lbrace;in het linkerzijpaneel__
      + De JSON-representatie die naar Adobe Target wordt geëxporteerd, wordt weergegeven in de hoofdweergave
1. Login aan [ Adobe Experience Cloud ](https://experience.adobe.com) met een gebruiker in de rol van de Redacteur voor Adobe Target
1. Van [ Experience Cloud ](https://experience.adobe.com), uitgezochte __Doel__ van de productschakelaar in hoogste recht om Adobe Target te openen.
1. Zorg ervoor dat Standaard Workspace in de __schakelaar van Workspace__ in het hoogste recht wordt geselecteerd.
1. Selecteer het __lusje van Aanbiedingen__ in de hoogste navigatie
1. Selecteer __Type__ dropdown, en het selecteren van __de Fragmenten van de Inhoud__
1. Controleren of het uit AEM geëxporteerde inhoudsfragment in de lijst wordt weergegeven
   + Beweeg over de aanbieding, en selecteer de __knoop van de Mening__
   + Herzie het __Info van de Aanbieding__ en zie de __diepe verbinding van AEM__ die het Fragment van de Inhoud direct in de dienst van de Auteur van AEM opent

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## Doelactiviteit met Content Fragment-aanbiedingen{#activity}

In Adobe Target kan een activiteit worden gemaakt die JSON-inhoud voor Content Fragment als inhoud gebruikt, waardoor persoonlijke ervaringen in een app zonder kop mogelijk zijn met inhoud die in AEM is gemaakt en beheerd.

In dit voorbeeld, gebruiken wij een eenvoudige activiteit A/B, nochtans kan om het even welke activiteit van het Doel worden gebruikt.

+++Vergroten voor geleidelijke instructies

1. Selecteer het __lusje van Activiteiten__ in de hoogste navigatie
1. Selecteer __+ activiteit creëren__, en selecteer dan het type van activiteit om tot stand te brengen.
   + Dit voorbeeld leidt tot een eenvoudige __A/B Test__ maar de Aanbiedingen van het Fragment van de Inhoud kunnen om het even welk activiteitstype aandrijven
1. In __creeer de tovenaar van de Activiteit__
   + Selecteer __Web__
   + In __kies Composer van de Ervaring__, uitgezochte __Vorm__
   + In __kies Workspace__, uitgezochte __Standaard Workspace__
   + In __kies Bezit__, selecteer het Bezit de Activiteit binnen beschikbaar is, of selecteer __Geen Beperkingen van het Bezit__ om het toe te staan om in alle Eigenschappen worden gebruikt.
   + Selecteer __daarna__ om de Activiteit tot stand te brengen
1. Verander de naam van de Activiteit door __te selecteren anders noemen__ in de linkerbovenkant
   + Geef de activiteit een betekenisvolle naam
1. In de aanvankelijke Ervaring, plaats __Plaats 1__ voor de Activiteit om te richten
   + In dit voorbeeld, richt een douaneplaats genoemd `wknd-adventure-promo`
1. Onder __Inhoud__ selecteert de Standaardinhoud, en selecteert __het Fragment van de Inhoud van de Verandering__
1. Selecteer het uitgevoerde Fragment van de Inhoud om voor deze ervaring te dienen, en selecteer __Gereed__
1. Bekijk de JSON Content Fragment Offer in het tekstgebied Inhoud. Dit is dezelfde JSON die beschikbaar is in de AEM Author-service via de Voorvertoning van Content Fragment.
1. Voeg in de linkertrack een ervaring toe en selecteer een andere Content Fragment-aanbieding die u wilt gebruiken
1. Selecteer __daarna__, en vorm de het richten regels zoals vereist voor de activiteit
   + In dit voorbeeld laat u de A/B-test handmatig 50/50 splitsen.
1. Selecteer __daarna__, en voltooi de activiteitenmontages
1. Selecteer __sparen &amp; Sluiten__ en geef het een betekenisvolle naam
1. Van de Activiteit in Adobe Target, activeert de uitgezochte ____ van Inactief/activeert/archivedropdown in het hoogste recht.

De Adobe Target-activiteit die gericht is op de `wknd-adventure-promo` -locatie kan nu worden geïntegreerd en weergegeven in een AEM Headless-app.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## Experience Platform DataStream-id{#datastream-id}

Een [ identiteitskaart van Adobe Experience Platform DataStream ](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html?lang=nl-NL) wordt vereist voor AEM Headless apps om met Adobe Target in wisselwerking te staan gebruikend [ SDK van het Web van Adobe ](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html?lang=nl-NL).

+++Vergroten voor geleidelijke instructies

1. Ga aan [ Adobe Experience Cloud ](https://experience.adobe.com/)
1. Open __Experience Platform__
1. Selecteer __de Inzameling van Gegevens > de stromen van Gegevens__ en selecteer __Nieuwe DataStream__
1. In de Nieuwe tovenaar DataStream, ga binnen:
   + Naam: `AEM Target integration`
   + Beschrijving: `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + Gebeurtenisschema: `Leave blank`
1. Selecteer __sparen__
1. Selecteer __toevoegen de Dienst__
1. In __de Dienst__ uitgezochte __Adobe Target__
   + Toegelaten: __ja__
   + Token van het bezit: __verlaten leeg__
   + Identiteitskaart van het Milieu van het doel: __Verlof__
      + Het milieu van het Doel kan in Adobe Target bij __Beleid > Gastheren__ worden geplaatst.
   + De Namespace van identiteitskaart van de Derde van het doel: __verlaten leeg__
1. Selecteer __sparen__
1. Op de rechterkant, kopieer identiteitskaart van 0&rbrace; Datastream __voor gebruik in [ Adobe Web SDK ](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html?lang=nl-NL) configuratievraag.__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## Aanpassing toevoegen aan een AEM Headless-app{#code}

Dit leerprogramma verkent het personaliseren van een eenvoudige Reactie app gebruikend de Aanbiedingen van het Fragment van de Inhoud in Adobe Target via [ SDK van het Web van Adobe Experience Platform ](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=nl-NL). Deze aanpak kan worden gebruikt om elke JavaScript-webbeleving aan te passen.

De mobiele ervaringen van Android™ en van iOS kunnen na gelijkaardige patronen worden gepersonaliseerd gebruikend [ Adobe Mobiele SDK ](https://developer.adobe.com/client-sdks/documentation/).

### Vereisten

+ Node.js 14
+ Git
+ [ Gedeelde 2.1.4+ van WKND ](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) geïnstalleerd op AEM als de Auteur van de Wolk en publiceer de diensten

### Instellen

1. Download de broncode voor steekproef React app van [ Github.com ](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Codebasis openen bij `~/Code/aem-guides-wknd-graphql/personalization-tutorial` in uw favoriete IDE
1. Werk de host van de AEM-service bij waarmee de app verbinding moet maken `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. Voer de app uit en controleer of deze verbinding maakt met de geconfigureerde AEM-service. Voer vanaf de opdrachtregel de volgende handelingen uit:

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install
   $ npm run start
   ```

1. Installeer het [ Web SDK van Adobe ](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html?lang=nl-NL#option-3%3A-using-the-npm-package) als pakket NPM.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   Web SDK kan in code worden gebruikt om de Aanbieding JSON van het Fragment van de Inhoud door activiteitenplaats te halen.

   Wanneer het vormen van het Web SDK, zijn er twee vereiste identiteitskaart:

   + `edgeConfigId` die [ identiteitskaart DataStream ](#datastream-id) is
   + `orgId` AEM as a Cloud Service/Doel Adobe Org ID die in __Experience Cloud > Profiel > de info van de Rekening > Huidige identiteitskaart van het Org__ kan worden gevonden

   Wanneer u de Web SDK aanroept, moet de locatie van de Adobe Target-activiteit (in ons voorbeeld `wknd-adventure-promo` ) worden ingesteld als de waarde in de `decisionScopes` -array.

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### Implementatie

1. Maak een React-component `AdobeTargetActivity.js` om Adobe Target-activiteiten te oppervlakken.

   __src/components/AdobeTargetActivity.js__

   ```javascript
   import React, { useEffect } from 'react';
   import { createInstance } from '@adobe/alloy';
   
   const alloy = createInstance({ name: 'alloy' });
   
   alloy('configure', { 
     'edgeConfigId': 'e3db252d-44d0-4a0b-8901-aac22dbc88dc', // AEP Datastream ID
     'orgId':'7ABB3E6A5A7491460A495D61@AdobeOrg',
     'debugEnabled': true,
   });
   
   export default function AdobeTargetActivity({ activityLocation, OfferComponent }) { 
     const [offer, setOffer] = React.useState();
   
     useEffect(() => {
       async function sendAlloyEvent() {
         // Get the activity offer from Adobe Target
         const result = await alloy('sendEvent', {
           // decisionScopes is set to an array containing the Adobe Target activity location
           'decisionScopes': [activityLocation],
         });
   
         if (result.propositions?.length > 0) {
           // Find the first proposition for the active activity location
           var proposition = result.propositions?.filter((proposition) => { return proposition.scope === activityLocation; })[0];
   
           // Get the Content Fragment Offer JSON from the Adobe Target response
           const contentFragmentOffer = proposition?.items[0]?.data?.content || { status: 'error', message: 'Personalized content unavailable'};
   
           if (contentFragmentOffer?.data) {
             // Content Fragment Offers represent a single Content Fragment, hydrated by
             // the byPath GraphQL query, we must traverse the JSON object to retrieve the 
             // Content Fragment JSON representation
             const byPath = Object.keys(contentFragmentOffer.data)[0];
             const item = contentFragmentOffer.data[byPath]?.item;
   
             if (item) {
               // Set the offer to the React state so it can be rendered
               setOffer(item);
   
               // Record the Content Fragment Offer as displayed for Adobe Target Activity reporting
               // If this request is omitted, the Target Activity's Reports will be blank
               alloy("sendEvent", {
                   xdm: {
                       eventType: "decisioning.propositionDisplay",
                       _experience: {
                           decisioning: {
                               propositions: [proposition]
                           }
                       }
                   }
               });          
             }
           }
         }
       };
   
       sendAlloyEvent();
   
     }, [activityLocation, OfferComponent]);
   
     if (!offer) {
       // Adobe Target offer initializing; we render a blank component (which has a fixed height) to prevent a layout shift
       return (<OfferComponent></OfferComponent>);
     } else if (offer.status === 'error') {
       // If Personalized content could not be retrieved either show nothing, or optionally default content.
       console.error(offer.message);
       return (<></>);
     }
   
     console.log('Activity Location', activityLocation);
     console.log('Content Fragment Offer', offer);
   
     // Render the React component with the offer's JSON
     return (<OfferComponent content={offer} />);
   };
   ```

   De component AdobeTargetActivity React wordt als volgt aangeroepen:

   ```jsx
   <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   ```

1. Maak een React-component `AdventurePromo.js` om het avontuur te renderen dat JSON Adobe Target-servers biedt.

   Deze React component neemt volledig gehydrateerde JSON die een fragment van de avontuurinhoud vertegenwoordigt, en op een promotionele manier toont. De React componenten die JSON tonen die van de Aanbiedingen van het Fragment van de Inhoud van Adobe Target wordt onderhouden kunnen zo gevarieerd en complex zijn zoals vereist gebaseerd op de Inhoudsfragmenten die naar Adobe Target worden uitgevoerd.

   __src/components/AdventurePromo.js__

   ```javascript
   import React from 'react';
   
   import './AdventurePromo.scss';
   
   /**
   * @param {*} content is the fully hydrated JSON data for a WKND Adventure Content Fragment
   * @returns the Adventure Promo component
   */
   export default function AdventurePromo({ content }) {
       if (!content) {
           // If content is still loading, then display an empty promote to prevent layout shift when Target loads the data
           return (<div className="adventure-promo"></div>)
       }
   
       const title = content.title;
       const description = content?.description?.plaintext;
       const image = content.primaryImage?._publishUrl;
   
       return (
           <div className="adventure-promo">
               <div className="adventure-promo-text-wrapper">
                   <h3 className="adventure-promo-eyebrow">Promoted adventure</h3>
                   <h2 className="adventure-promo-title">{title}</h2>
                   <p className="adventure-promo-description">{description}</p>
               </div>
               <div className="adventure-promo-image-wrapper">
                   <img className="adventure-promo-image" src={image} alt={title} />
               </div>
           </div>
       )
   }
   ```

   __src/components/AdventurePromo.scss__

   ```css
   .adventure-promo {
       display: flex;
       margin: 3rem 0;
       height: 400px;
   }
   
   .adventure-promo-text-wrapper {
       background-color: #ffea00;
       color: black;
       flex-grow: 1;
       padding: 3rem 2rem;
       width: 55%;
   }
   
   .adventure-promo-eyebrow {
       font-family: Source Sans Pro,Helvetica Neue,Helvetica,Arial,sans-serif;
       font-weight: 700;
       font-size: 1rem;
       margin: 0;
       text-transform: uppercase;
   }
   
   .adventure-promo-description {
       line-height: 1.75rem;
   }
   
   .adventure-promo-image-wrapper {
       height: 400px;
       width: 45%;
   }
   
   .adventure-promo-image {
       height: 100%;
       object-fit: cover;
       object-position: center center;
       width: 100%;
   }
   ```

   Deze component React wordt als volgt aangeroepen:

   ```jsx
   <AdventurePromo adventure={adventureJSON}/>
   ```

1. Voeg de component AdobeTargetActivity toe om de toepassing `Home.js` boven de lijst met avonturen te Reageren.

   __src/components/Home.js__

   ```javascript
   import AdventurePromo from './AdventurePromo';
   import AdobeTargetActivity from './AdobeTargetActivity';
   ... 
   export default function Home() {
       ...
       return(
           <div className="Home">
   
             <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   
             <h2>Current Adventures</h2>
             ...
       )
   }
   ```

1. Als de React-app niet wordt uitgevoerd, start u deze opnieuw met `npm run start` .

   Open de React-app in twee verschillende browsers, zodat de A/B-test de verschillende ervaringen voor elke browser kan weergeven. Als in beide browsers dezelfde avontuuraanbieding wordt weergegeven, probeert u een van de browsers te sluiten of opnieuw te openen totdat de andere ervaring wordt weergegeven.

   In de onderstaande afbeelding ziet u de twee verschillende Content Fragment-aanbiedingen die worden weergegeven voor de `wknd-adventure-promo` Activity, op basis van Adobe Target-logica.

   ![ de aanbiedingen van de Ervaring ](./assets/target/offers-in-app.png)

## Gefeliciteerd!

Nu we AEM as a Cloud Service hebben geconfigureerd voor het exporteren van inhoudsfragmenten naar Adobe Target, hebben we de Content Fragments-aanbiedingen in een Adobe Target-activiteit gebruikt, en hebben we die activiteit opgezocht in een AEM Headless-app, die de ervaring personaliseert.
