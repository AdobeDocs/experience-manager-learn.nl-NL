---
title: AEM headless API's en React - AEM headless first-zelfstudie
description: Leer hoe u het ophalen van gegevens van inhoudsfragmenten van AEM GraphQL API's bedekt en deze weergeeft in de React-app.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: 2b726473-5a32-4046-bce8-6da3c57a1b60
duration: 280
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 0%

---

# Koploze API&#39;s AEM en reageren

Welkom bij dit zelfstudie-hoofdstuk waarin we de configuratie van een React-app voor verbinding met Adobe Experience Manager (AEM) Headless API&#39;s met behulp van de AEM Headless SDK onderzoeken. We gaan het ophalen van gegevens van inhoudsfragmenten van AEM GraphQL API&#39;s en het weergeven ervan in de React-app behandelen.

AEM Headless-API&#39;s bieden toegang tot AEM inhoud van elke client-app. We begeleiden u bij het configureren van uw React-app om verbinding te maken met AEM headless API&#39;s met behulp van de AEM Headless SDK. Met deze instelling wordt een herbruikbaar communicatiekanaal tot stand gebracht tussen uw React-app en AEM.

Vervolgens gebruiken we de AEM Headless SDK om gegevens van inhoudsfragmenten van AEM GraphQL API&#39;s op te halen. Inhoudsfragmenten in AEM bieden gestructureerd inhoudsbeheer. Met de SDK AEM Headless kunt u gemakkelijk zoeken naar gegevens van inhoudsfragmenten en deze ophalen met GraphQL.

Zodra we de gegevens van het inhoudsfragment hebben, integreren we deze in uw React-app. U leert de gegevens op een aantrekkelijke manier opmaken en weergeven. We bieden tips en trucs voor het verwerken en renderen van gegevens over inhoudsfragmenten in React-componenten, zodat u verzekerd bent van een naadloze integratie met de gebruikersinterface van uw app.

Tijdens de gehele zelfstudie geven we uitleg, codevoorbeelden en praktische tips. Tegen het einde kunt u uw React-app configureren om verbinding te maken met AEM Headless API&#39;s, gegevens van inhoudsfragmenten op te halen met de AEM Headless SDK en deze naadloos weer te geven in uw React-app. Laten we beginnen!


## De React-app klonen

1. De app klonen vanuit [Github](https://github.com/lamontacrook/headless-first/tree/main) door de volgende opdracht op de opdrachtregel uit te voeren.

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. Wijzigen in de `headless-first` en installeer de afhankelijkheden.

   ```
   $ cd headless-first
   $ npm ci
   ```

## De React-app configureren

1. Een bestand met de naam `.env` aan de basis van het project. In `.env` Stel de volgende waarden in:

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. U kunt een ontwikkelaarstoken ophalen in Cloud Manager. Aanmelden bij [Adobe Cloud Manager](https://experience.adobe.com/). Klikken __Experience Manager > Cloud Manager__. Kies het aangewezen Programma en klik dan de ellipsen naast het Milieu.

   ![AEM Developer Console](./assets/2/developer-console.png)

   1. Klik in het dialoogvenster __Integraties__ tab
   1. Klikken __Lokaal token, tabblad en lokaal ontwikkelingstoken ophalen__ knop
   1. Kopieer het toegangstoken dat begint na het open citaat tot vóór het dichte citaat.
   1. Plak de gekopieerde token als waarde voor `REACT_APP_TOKEN` in de `.env` bestand.
   1. Laten we de app nu maken door deze uit te voeren `npm ci` op de opdrachtregel.
   1. Start nu de React-app en voer deze uit `npm run start` op de opdrachtregel.
   1. In [./src/utils](https://github.com/lamontacrook/headless-first/tree/main/src/utils) een bestand met de naam `context.js`  bevat de code waarmee de waarden in het dialoogvenster `.env` in de context van de app.

## De React-app uitvoeren

1. Start de React-app via uitvoeren `npm run start` op de opdrachtregel.

   ```
   $ npm run start
   ```

   De React-app start en opent een browservenster waarin `http://localhost:3000`. Wijzigingen in de React-app worden automatisch opnieuw geladen in de browser.

## Verbinding maken met AEM headless API&#39;s

1. Als u de React-app wilt aansluiten op AEM as a Cloud Service, kunt u het volgende toevoegen: `App.js`. In de `React` importeren, toevoegen `useContext`.

   ```javascript
   import React, {useContext} from 'react';
   ```

   Importeren `AppContext` van de `context.js` bestand.

   ```javascript
   import { AppContext } from './utils/context';
   ```

   Definieer nu binnen de toepassingscode een contextvariabele.

   ```javascript
   const context = useContext(AppContext);
   ```

   En ten slotte plaatst u de retourcode in `<AppContext.Provider> ... </AppContext.Provider>`.

   ```javascript
   ...
   return(<div className='App'>
       <AppContext.Provider value={context}>
           ...
       </AppContext.Provider>
   </div>);
   ```

   Ter referentie: `App.js` zou nu zo moeten zijn.

   ```javascript
   import React, {useContext} from 'react';
   import './App.css';
   import { BrowserRouter, Routes, Route } from 'react-router-dom';
   import Home from './screens/home/home';
   import { AppContext } from './utils/context';
   
   const App = () => {
   const context = useContext(AppContext);
   return (
       <div className='App'>
       <AppContext.Provider value={context}>
           <BrowserRouter>
           <Routes>
               <Route exact={true} path={'/'} element={<Home />} />
           </Routes>
           </BrowserRouter>
       </AppContext.Provider>
       </div>
   );
   };
   
   export default App;
   ```

1. Het dialoogvenster Importeren `AEMHeadless` SDK. Deze SDK is een hulpbibliotheek die door de app wordt gebruikt voor interactie met AEM headless API&#39;s.

   Deze importinstructie toevoegen aan de `home.js`.

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   Voeg het volgende toe `{ useContext, useEffect, useState }` aan de` React` import, instructie.

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   Het dialoogvenster Importeren `AppContext`.

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   Binnen de `Home` component, ophalen `context` variabele uit de `AppContext`.

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. Initialiseer de AEM Headless SDK in een  `useEffect()`, aangezien de AEM Headless SDK moet veranderen wanneer de  `context` variabele wijzigingen.

   ```javascript
   useEffect(() => {
   const sdk = new AEMHeadless({
       serviceURL: context.url,
       endpoint: context.endpoint,
       auth: context.token
   });
   }, [context]);  
   ```

   >[!NOTE]
   >
   > Er is een `context.js` bestand onder `/utils` dat elementen leest uit de `.env` bestand. Ter referentie: `context.url` is de URL van de AEM as a Cloud Service omgeving. De `context.endpoint` is de volledige weg aan het eindpunt dat in de vorige les wordt gecreeerd. Tot slot `context.token` is het ontwikkelaarstoken.


1. Creeer React staat die de inhoud blootstelt die uit AEM Headless SDK komt.

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. Sluit de app aan op AEM. Gebruik de voortgezette vraag die in de vorige les wordt gecreeerd. Voeg de volgende code toe in de `useEffect` nadat de AEM Headless SDK is geïnitialiseerd. Maak de `useEffect` afhankelijk van de  `context` variabel, zoals hieronder weergegeven.


   ```javascript
   useEffect(() => {
   ...
   sdk.runPersistedQuery('<name of the endpoint>/<name of the persisted query>', { path: `/content/dam/${context.project}/<name of the teaser fragment>` })
       .then(({ data }) => {
       if (data) {
           setContent(data);
       }
       })
       .catch((error) => {
       console.log(`Error with pure-headless/teaser. ${error.message}`);
       });
   }, [context]);
   ```

1. Open de netwerkweergave van de ontwikkelaarsgereedschappen om de GraphQL-aanvraag te bekijken.

   `<url to environment>/graphql/execute.json/pure-headless/teaser%3Bpath%3D%2Fcontent%2Fdam%2Fpure-headless%2Fhero`

   ![Chrome Dev-gereedschappen](./assets/2/dev-tools.png)

   De AEM Headless SDK codeert de aanvraag voor GraphQL en voegt de opgegeven parameters toe. U kunt de aanvraag openen in de browser.

   >[!NOTE]
   >
   > Aangezien het verzoek naar het auteursmilieu gaat, moet u in het milieu in een ander lusje van zelfde browser worden geregistreerd.


## Inhoud van inhoudsfragment renderen

1. Geef de inhoudsfragmenten in de app weer. Retourneer een `<div>` met de titel van de taser.

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && (content.component.item.title)}</div>
   </div>
   );
   ```

   Het titelveld van de taser wordt weergegeven op het scherm.

1. De laatste stap bestaat uit het toevoegen van het gummetje aan de pagina. In het pakket is een component React teaser opgenomen. Eerst, laten wij de invoer omvatten. Aan de bovenkant van de `home.js` bestand, voeg de regel toe:

   `import Teaser from '../../components/teaser/teaser';`

   Werk de instructie return bij:

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && <Teaser content={content.component.item} />}</div>
   </div>
   );
   ```

   U moet nu het gummetje zien met de inhoud in het fragment.


## Volgende stappen

Gefeliciteerd! U hebt de React-app bijgewerkt en geïntegreerd met AEM Headless API&#39;s met de AEM Headless SDK!

Daarna, maken wij een complexere component van de Lijst van het Beeld die dynamisch referenced Inhoudsfragmenten van AEM teruggeeft.

[Volgend hoofdstuk: Een component van de Lijst van het Beeld bouwen](./3-complex-components.md)
