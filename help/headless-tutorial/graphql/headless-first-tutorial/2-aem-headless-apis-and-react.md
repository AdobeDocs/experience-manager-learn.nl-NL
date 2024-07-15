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
duration: 225
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 0%

---

# Koploze API&#39;s AEM en reageren

Welkom bij dit zelfstudie-hoofdstuk waarin we de configuratie van een React-app voor verbinding met Adobe Experience Manager (AEM) Headless API&#39;s met behulp van de AEM Headless SDK onderzoeken. Het ophalen van gegevens van inhoudsfragmenten uit AEM GraphQL API&#39;s en het weergeven ervan in de React-app wordt behandeld.

AEM Headless-API&#39;s bieden toegang tot AEM inhoud van elke client-app. We begeleiden u bij het configureren van uw React-app om verbinding te maken met AEM headless API&#39;s met behulp van de AEM Headless SDK. Met deze instelling wordt een herbruikbaar communicatiekanaal tot stand gebracht tussen uw React-app en AEM.

Vervolgens gebruiken we de AEM Headless SDK om gegevens van inhoudsfragmenten op te halen van AEM GraphQL API&#39;s. Inhoudsfragmenten in AEM bieden gestructureerd inhoudsbeheer. Met de SDK AEM Headless kunt u gemakkelijk zoeken naar gegevens van inhoudsfragmenten en deze ophalen met GraphQL.

Zodra we de gegevens van het inhoudsfragment hebben, integreren we deze in uw React-app. U leert de gegevens op een aantrekkelijke manier opmaken en weergeven. We bieden tips en trucs voor het verwerken en renderen van gegevens over inhoudsfragmenten in React-componenten, zodat u verzekerd bent van een naadloze integratie met de gebruikersinterface van uw app.

Tijdens de gehele zelfstudie geven we uitleg, codevoorbeelden en praktische tips. Tegen het einde kunt u uw React-app configureren om verbinding te maken met AEM Headless API&#39;s, gegevens van inhoudsfragmenten op te halen met de AEM Headless SDK en deze naadloos weer te geven in uw React-app. Laten we beginnen!


## De React-app klonen

1. Kloon app van [ Github ](https://github.com/lamontacrook/headless-first/tree/main) door het volgende bevel op de bevellijn uit te voeren.

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. Ga naar de map `headless-first` en installeer de afhankelijkheden.

   ```
   $ cd headless-first
   $ npm ci
   ```

## De React-app configureren

1. Maak een bestand met de naam `.env` in de hoofdmap van het project. Stel in `.env` de volgende waarden in:

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. U kunt een ontwikkelaarstoken in Cloud Manager terugwinnen. Login aan [ Adobe Cloud Manager ](https://experience.adobe.com/). Klik __Experience Manager > Cloud Manager__. Kies het aangewezen Programma en klik dan de ellipsen naast het Milieu.

   ![ AEM Developer Console ](./assets/2/developer-console.png)

   1. Klik in het __lusje van de Integraties__
   1. Klik __Lokale Symbolische lusje &amp; krijg de Lokale Token van de Ontwikkeling__ knoop
   1. Kopieer het toegangstoken dat begint na het open citaat tot vóór het dichte citaat.
   1. Plak het gekopieerde token als de waarde voor `REACT_APP_TOKEN` in het `.env` -bestand.
   1. Laten we de app nu maken door `npm ci` uit te voeren op de opdrachtregel.
   1. Start nu de React-app en door `npm run start` uit te voeren op de opdrachtregel.
   1. In [ ./src/utils ](https://github.com/lamontacrook/headless-first/tree/main/src/utils) een dossier genoemd `context.js` omvat de code om de waarden in het `.env` dossier in de context van app te plaatsen.

## De React-app uitvoeren

1. Start de React-app door `npm run start` uit te voeren op de opdrachtregel.

   ```
   $ npm run start
   ```

   De React-app start en opent een browservenster naar `http://localhost:3000` . Wijzigingen in de React-app worden automatisch opnieuw geladen in de browser.

## Verbinding maken met AEM headless API&#39;s

1. Als u de React-app wilt verbinden met AEM as a Cloud Service, voegt u een paar dingen toe aan `App.js` . Voeg `useContext` toe in het dialoogvenster `React` importeren.

   ```javascript
   import React, {useContext} from 'react';
   ```

   Importeer `AppContext` uit het `context.js` -bestand.

   ```javascript
   import { AppContext } from './utils/context';
   ```

   Definieer nu binnen de toepassingscode een contextvariabele.

   ```javascript
   const context = useContext(AppContext);
   ```

   Plaats ten slotte de retourcode in `<AppContext.Provider> ... </AppContext.Provider>` .

   ```javascript
   ...
   return(<div className='App'>
       <AppContext.Provider value={context}>
           ...
       </AppContext.Provider>
   </div>);
   ```

   Ter referentie: de `App.js` zou nu zo moeten zijn.

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

1. Importeer de `AEMHeadless` SDK. Deze SDK is een hulpbibliotheek die door de app wordt gebruikt voor interactie met AEM koploze API&#39;s.

   Voeg deze importinstructie toe aan de `home.js` .

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   Voeg de volgende `{ useContext, useEffect, useState }` aan de ` React` de invoerverklaring toe.

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   Importeer de `AppContext` .

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   In de component `Home` haalt u de variabele `context` op vanuit de variabele `AppContext` .

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. Initialiseer de AEM Headless SDK in een `useEffect()` , omdat de AEM Headless SDK moet wijzigen wanneer de variabele `context` verandert.

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
   > Er is een `context.js` -bestand onder `/utils` dat elementen uit het `.env` -bestand leest. Ter referentie is `context.url` de URL van de AEM as a Cloud Service-omgeving. `context.endpoint` is de volledige weg aan het eindpunt dat in de vorige les wordt gecreeerd. Tot slot is `context.token` de ontwikkelaarstoken.


1. Creeer React staat die de inhoud blootstelt die uit AEM Headless SDK komt.

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. Sluit de app aan op AEM. Gebruik de voortgezette vraag die in de vorige les wordt gecreeerd. Voeg de volgende code toe in de `useEffect` nadat de AEM Headless SDK is geïnitialiseerd. Maak de `useEffect` afhankelijk van de `context` -variabele, zoals hieronder wordt weergegeven.


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

   ![ Chrome Dev Hulpmiddelen ](./assets/2/dev-tools.png)

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

1. De laatste stap bestaat uit het toevoegen van het gummetje aan de pagina. In het pakket is een component React teaser opgenomen. Eerst, laten wij de invoer omvatten. Voeg boven aan het bestand `home.js` de regel toe:

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
