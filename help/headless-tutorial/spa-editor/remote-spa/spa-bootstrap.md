---
title: De externe SPA voor SPA Editor Bootstrappen
description: Leer hoe te om een verre SPA voor de verenigbaarheid van AEM SPA van de Redacteur op te starten.
topic: Headless, SPA, Development
feature: SPA Editor, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7633
thumbnail: kt-7633.jpeg
last-substantial-update: 2022-11-01T00:00:00Z
doc-type: Tutorial
exl-id: b8d43e44-014c-4142-b89c-ff4824b89c78
duration: 421
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1167'
ht-degree: 0%

---

# De externe SPA voor SPA Editor Bootstrappen

Voordat de bewerkbare gebieden aan de externe SPA kunnen worden toegevoegd, moet deze worden opgepakt met de AEM SPA JavaScript SDK van de Editor en enkele andere configuraties.

## Npm-afhankelijkheden voor AEM editor voor JS SDK installeren

Eerst, herzie AEM npm gebiedsdelen voor het project van de Reactie, en installeer hen.

+ [`@adobe/aem-spa-page-model-manager`](https://github.com/adobe/aem-spa-page-model-manager) : bevat de API voor het ophalen van inhoud van AEM.
+ [`@adobe/aem-spa-component-mapping`](https://github.com/adobe/aem-spa-component-mapping) : biedt de API die AEM inhoud toewijst aan SPA componenten.
+ [`@adobe/aem-react-editable-components` v2](https://github.com/adobe/aem-react-editable-components) : biedt een API voor het bouwen van aangepaste SPA componenten en biedt veelgebruikte implementaties, zoals de `AEMPage` Reageer component.

```shell
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
$ npm install @adobe/aem-spa-page-model-manager 
$ npm install @adobe/aem-spa-component-mapping
$ npm install @adobe/aem-react-editable-components 
```

## SPA omgevingsvariabelen bekijken

Verschillende omgevingsvariabelen moeten worden blootgesteld aan de externe SPA zodat deze weet hoe ze met AEM moeten werken.

1. Externe SPA openen bij `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` in uw IDE
1. Het bestand openen `.env.development`
1. Let in het bestand op de toetsen en werk de bestanden zo nodig bij:

   ```
   REACT_APP_HOST_URI=http://localhost:4502
   
   REACT_APP_USE_PROXY=true
   
   REACT_APP_AUTH_METHOD=basic
   
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

   ![Externe SPA omgevingsvariabelen](./assets/spa-bootstrap/env-variables.png)

   *Onthoud dat aangepaste omgevingsvariabelen in React vooraf moeten worden ingesteld `REACT_APP_`.*

   + `REACT_APP_HOST_URI`: de regeling en de gastheer van de AEM dienst de Verre SPA verbindt met.
      + Deze waarde verandert op basis van het type AEM (lokaal, Ontwikkelen, Werkgebied of Productie) en het type AEM Service (Auteur versus Publiceren).
   + `REACT_APP_USE_PROXY`: hiermee worden CORS-problemen tijdens de ontwikkeling voorkomen door de responsontwikkelserver te informeren naar proxyverzoeken AEM `/content, /graphql, .model.json` gebruiken `http-proxy-middleware` -module.
   + `REACT_APP_AUTH_METHOD`: verificatiemethode voor AEM ingediende aanvragen, opties zijn &#39;service-token&#39;, &#39;dev-token&#39;, &#39;basic&#39; of blanco laten voor gebruiksgeval zonder toestemming
      + Vereist voor gebruik met AEM auteur
      + Mogelijk vereist voor gebruik met AEM Publiceren (als de inhoud is beveiligd)
      + Ontwikkelen tegen de AEM SDK ondersteunt lokale accounts via Basic Auth. Dit is de methode die in deze zelfstudie wordt gebruikt.
      + Wanneer het integreren met AEM as a Cloud Service, gebruik [toegangstokens](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
   + `REACT_APP_BASIC_AUTH_USER`: de AEM __gebruikersnaam__ door de SPA voor verificatie bij het ophalen van AEM inhoud.
   + `REACT_APP_BASIC_AUTH_PASS`: de AEM __password__ door de SPA voor verificatie bij het ophalen van AEM inhoud.

## De ModelManager-API integreren

Activeer AEM met de AEM SPA npm-afhankelijkheden die beschikbaar zijn voor de app `ModelManager` in de `index.js` voor `ReactDOM.render(...)` wordt aangeroepen.

De [ModelManager](https://github.com/adobe/aem-spa-page-model-manager/blob/master/src/ModelManager.ts) is verantwoordelijk voor het verbinden met AEM aan het ophalen van bewerkbare inhoud.

1. Open het Verre SPA project in uw winde
1. Het bestand openen `src/index.js`
1. Importeren toevoegen `ModelManager` en initialiseer het voor `root.render(..)` oproeping,

   ```javascript
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking root.render(..).
   ModelManager.initializeAsync();
   
   const container = document.getElementById('root');
   const root = createRoot(container);
   root.render(<App />);
   ```

De `src/index.js` bestand moet er als volgt uitzien:

![src/index.js](./assets/spa-bootstrap/index-js.png)

## Een interne SPA instellen

Wanneer u een bewerkbare SPA maakt, kunt u het beste een [interne proxy in de SPA](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually), dat wordt gevormd om de aangewezen verzoeken aan AEM te leiden. Dit doet u door [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) npm-module, die al is geïnstalleerd door de basis-WKND GraphQL App.

1. Open het Verre SPA project in uw winde
1. Open het bestand op `src/proxy/setupProxy.spa-editor.auth.basic.js`
1. Werk het bestand bij met de volgende code:

   ```javascript
   const { createProxyMiddleware } = require('http-proxy-middleware');
   const {REACT_APP_HOST_URI, REACT_APP_BASIC_AUTH_USER, REACT_APP_BASIC_AUTH_PASS } = process.env;
   
   /*
       Set up a proxy with AEM for local development
       In a production environment this proxy should be set up at the webserver level or absolute URLs should be used.
   */
   module.exports = function(app) {
   
       /**
       * Filter to check if the request should be re-routed to AEM. The paths to be re-routed at:
       * - Starts with /content (AEM content)
       * - Starts with /graphql (AEM graphQL endpoint)
       * - Ends with .model.json (AEM Content Services)
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns true if the SPA request should be re-routed to AEM
       */
       const toAEM = function(path, req) {
           return path.startsWith('/content') || 
               path.startsWith('/graphql') ||
               path.endsWith('.model.json')
       }
   
       /**
       * Re-writes URLs being proxied to AEM such that they can resolve to real AEM resources
       * - The "root" case of `/.model.json` are rewritten to the SPA's home page in AEM
       * - .model.json requests for /adventure:xxx routes are rewritten to their corresponding adventure page under /content/wknd-app/us/en/home/adventure/ 
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns returns a re-written path, or nothing to use the @param path
       */
       const pathRewriteToAEM = function (path, req) { 
           if (path === '/.model.json') {
               return '/content/wknd-app/us/en/home.model.json';
           } else if (path.startsWith('/adventure/') && path.endsWith('.model.json')) {
               return '/content/wknd-app/us/en/home/adventure/' + path.split('/').pop();
           }    
       }
   
       /**
       * Register the proxy middleware using the toAEM filter and pathRewriteToAEM rewriter 
       */
       app.use(
           createProxyMiddleware(
               toAEM, // Only route the configured requests to AEM
               {
                   target: REACT_APP_HOST_URI,
                   changeOrigin: true,
                   // Pass in credentials when developing against an Author environment
                   auth: `${REACT_APP_BASIC_AUTH_USER}:${REACT_APP_BASIC_AUTH_PASS}`,
                   pathRewrite: pathRewriteToAEM // Rewrite SPA paths being sent to AEM
               }
           )
       );
   
       /**
       * Enable CORS on requests from the SPA to AEM
       * 
       * If this rule is not in place, CORS errors will occur when running the SPA on http://localhost:3000
       */
       app.use((req, res, next) => {
           res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);
           next();
       });
   };
   ```

   De `setupProxy.spa-editor.auth.basic.js` bestand moet er als volgt uitzien:

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   Deze proxyconfiguratie heeft twee belangrijke functies:

   1. Verantwoordelijke specifieke verzoeken aan de SPA (`http://localhost:3000`) tot AEM `http://localhost:4502`
      + Het is alleen volmachten waarvan de paden overeenkomen met patronen die aangeven dat ze moeten worden AEM, zoals gedefinieerd in `toAEM(path, req)`.
      + Het herschrijft SPA paden naar hun tegenhanger AEM pagina&#39;s, zoals gedefinieerd in `pathRewriteToAEM(path, req)`
   1. Het voegt kopballen CORS aan alle verzoeken toe om toegang tot AEM inhoud toe te staan, zoals bepaald door `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`
      + Als dit niet wordt toegevoegd, treden CORS-fouten op bij het laden van AEM inhoud in de SPA.

1. Het bestand openen `src/setupProxy.js`
1. Herzie de lijn die aan `setupProxy.spa-editor.auth.basic` proxyconfiguratiebestand:

   ```
   ...
   case BASIC:
   // Use user/pass for local development with Local Author Env
   return require('./proxy/setupProxy.spa-editor.auth.basic');
   ...
   ```

Opmerking: wijzigingen in de `src/setupProxy.js` of bestanden waarnaar wordt verwezen, moeten opnieuw worden opgestart.

## Statische SPA

Statische SPA bronnen zoals het WKND-logo en het laden van afbeeldingen moeten hun URL&#39;s van de bron laten bijwerken om te zorgen dat deze worden geladen vanaf de host voor de externe SPA. Als de SPA relatief links is en in SPA Editor wordt geladen voor ontwerpen, gebruiken deze URL&#39;s standaard AEM host in plaats van de SPA. Dit levert 404 verzoeken op, zoals in de onderstaande afbeelding wordt geïllustreerd.

![Verbroken statische bronnen](./assets/spa-bootstrap/broken-static-resource.png)

Om deze kwestie op te lossen, maak een statische die bron door de Verre SPA wordt ontvangen absolute wegen gebruiken die de Verre SPA oorsprong omvatten.

1. Open het SPA project in uw winde
1. Het bestand met SPA omgevingsvariabelen openen `src/.env.development` en voeg een variabele voor SPA openbare URI toe:

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _Wanneer het opstellen aan AEM as a Cloud Service, moet u het zelfde voor het overeenkomstige `.env` bestanden._

1. Het bestand openen `src/App.js`
1. Importeer de SPA public URI uit de SPA omgevingsvariabelen

   ```javascript
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. Voorvoegsel van het WKND-logo `<img src=.../>` with `REACT_APP_PUBLIC_URI` de resolutie tegen de SPA af te dwingen.

   ```html
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. Doe het zelfde als het laden van beeld in `src/components/Loading.js`

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   class Loading extends Component {
   
       render() {
           return (<div className="loading">
               <img src={REACT_APP_PUBLIC_URI + '/' + loadingIcon} alt="Loading..." />
           </div>);
       }
   }
   ```

1. En voor de __twee instanties__ van de knop Terug in `src/components/AdventureDetails.js`

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

De `App.js`, `Loading.js`, en `AdventureDetails.js` bestanden moeten er als volgt uitzien:

![Statische bronnen](./assets/spa-bootstrap/static-resources.png)

## AEM responsief raster

Om de lay-outmodus van SPA Editor voor bewerkbare gebieden in de SPA te ondersteunen, moeten AEM CSS voor responsieve rasters in de SPA worden geïntegreerd. Maak u geen zorgen - dit rastersysteem is alleen van toepassing op de bewerkbare containers en u kunt uw keuzerondje in het raster gebruiken om de lay-out van de rest van uw SPA te bepalen.

Voeg de AEM Responsieve Raster SCSS-bestanden toe aan de SPA.

1. Open het SPA project in uw winde
1. De volgende twee bestanden downloaden en kopiëren naar `src/styles`
   + [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      + De AEM Responsieve generator van het Net SCSS
   + [_grid-init.scss](./assets/spa-bootstrap/_grid-init.scss)
      + Oproepen `_grid.scss` de SPA specifieke onderbrekingspunten (bureaublad en mobiel) en kolommen (12) gebruiken.
1. Openen `src/App.scss` en importeren `./styles/grid-init.scss`

   ```scss
   ...
   @import './styles/grid-init';
   ...
   ```

De `_grid.scss` en `_grid-init.scss` bestanden moeten er als volgt uitzien:

![AEM responsieve raster-SCSS](./assets/spa-bootstrap/aem-responsive-grid.png)

Nu bevat de SPA de CSS die nodig is om AEM lay-outmodus te ondersteunen voor componenten die aan een AEM-container zijn toegevoegd.

## Hulpprogrammaklassen

Kopieer in de volgende hulpprogrammaklassen naar uw Reactie-app-project.

+ [RoutedLink.js](./assets/spa-bootstrap/RoutedLink.js) tot `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/RoutedLink.js`
+ [EditorPlaceholder.js](./assets/spa-bootstrap/EditorPlaceholder.js) tot `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/EditorPlaceholder.js`
+ [withConditionalPlaceholder.js](./assets/spa-bootstrap/withConditionalPlaceholder.js) tot `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withConditionalPlaceholder.js`
+ [withStandardBaseCssClass.js](./assets/spa-bootstrap/withStandardBaseCssClass.js) tot `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withStandardBaseCssClass.js`

![Hulpprogrammaklassen voor externe SPA](./assets/spa-bootstrap/utility-classes.png)

## De SPA starten

Nu de SPA is opgepakt voor integratie met AEM, laten we de SPA uitvoeren en zien hoe het eruit ziet!

1. Navigeer op de opdrachtregel naar de hoofdmap van het SPA project
1. Start de SPA met de normale opdrachten (als u dat nog niet hebt gedaan).

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
   $ npm install 
   $ npm run start
   ```

1. Blader door de SPA op [http://localhost:3000](http://localhost:3000). Alles moet er goed uitzien!

![SPA op http://localhost:3000](./assets/spa-bootstrap/localhost-3000.png)

## De SPA openen in AEM SPA Editor

Als de SPA ingeschakeld is [http://localhost:3000](http://localhost:3000), laten we het openen met AEM SPA Editor. Niets is editable in de SPA nog, bevestigt dit slechts de SPA in AEM.

1. Aanmelden bij AEM auteur
1. Navigeren naar __Sites > WKND App > us > en__
1. Selecteer de __WKND App Home Page__ en tikken __Bewerken__ en wordt de SPA weergegeven.

   ![WKND-startpagina bewerken](./assets/spa-bootstrap/edit-home.png)

1. Overschakelen op __Voorvertoning__ met de modusschakelaar rechtsboven
1. Klik om de SPA

   ![SPA op http://localhost:3000](./assets/spa-bootstrap/spa-editor.png)

## Gefeliciteerd!

U hebt de Verre SPA opgepakt om compatibel SPA Redacteur AEM te zijn! Nu weet u hoe:

+ Voeg de gebiedsdelen van JS SDK npm van de Redacteur van de AEM aan het SPA project toe
+ De SPA omgevingsvariabelen configureren
+ De ModelManager-API integreren met de SPA
+ Opstelling een interne volmacht voor de SPA zodat leidt het de aangewezen inhoudverzoeken om te AEM
+ Problemen met statische SPA oplossen in de context van SPA Editor
+ CSS voor AEM responsieve raster toevoegen ter ondersteuning van lay-out in AEM bewerkbare containers

## Volgende stappen

Nu we een basislijn voor compatibiliteit met AEM SPA Editor hebben bereikt, kunnen we bewerkbare gebieden gaan introduceren. We bekijken eerst hoe u een [vaste bewerkbare component](./spa-fixed-component.md) in de SPA.
