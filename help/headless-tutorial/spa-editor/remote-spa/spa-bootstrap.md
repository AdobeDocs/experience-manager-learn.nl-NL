---
title: Bootstrap the Remote SPA for SPA Editor
description: Leer hoe te om een verre SPA voor de verenigbaarheid van de Redacteur van AEM te laarzen SPA.
topic: Headless, SPA, Development
feature: SPA Editor, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7633
thumbnail: kt-7633.jpeg
last-substantial-update: 2022-11-01T00:00:00Z
doc-type: Tutorial
exl-id: b8d43e44-014c-4142-b89c-ff4824b89c78
duration: 327
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '1167'
ht-degree: 0%

---

# Bootstrap the Remote SPA for SPA Editor

{{spa-editor-deprecation}}

Alvorens de editable gebieden aan het Verre KUUROORD kunnen worden toegevoegd, moet het met de Redacteur van AEM SPA JavaScript SDK, en een paar andere configuraties worden bootstrapped.

## AEM SPA Editor JS SDK npm-afhankelijkheden installeren

Eerst, herzie de gebiedsdelen van AEM SPA npm voor het project van het Reageren, en installeer hen.

* [`@adobe/aem-spa-page-model-manager` ](https://github.com/adobe/aem-spa-page-model-manager) : bevat de API voor het ophalen van inhoud van AEM.
* [`@adobe/aem-spa-component-mapping` ](https://github.com/adobe/aem-spa-component-mapping) : verstrekt API die inhoud van AEM aan componenten van het KUUROORD in kaart brengt.
* [`@adobe/aem-react-editable-components` v2 ](https://github.com/adobe/aem-react-editable-components) : verstrekt API voor de bouw van de componenten van douaneSPA en verstrekt gemeenschappelijk-gebruik implementaties zoals de `AEMPage` component React.

```shell
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
$ npm install @adobe/aem-spa-page-model-manager 
$ npm install @adobe/aem-spa-component-mapping
$ npm install @adobe/aem-react-editable-components 
```

## Omgevingsvariabelen van SPA controleren

Verschillende omgevingsvariabelen moeten worden blootgesteld aan de Remote SPA zodat deze weet hoe ze met AEM moeten communiceren.

* Open Verre project van het KUUROORD bij `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` in uw winde
* Het bestand openen `.env.development`
* Let in het bestand op de toetsen en werk de bestanden zo nodig bij:

  ```
  REACT_APP_HOST_URI=http://localhost:4502
  
  REACT_APP_USE_PROXY=true
  
  REACT_APP_AUTH_METHOD=basic 
  
  REACT_APP_BASIC_AUTH_USER=admin
  REACT_APP_BASIC_AUTH_PASS=admin
  ```

  ![ Verre de Variabelen van het Milieu van het KUUROORD ](./assets/spa-bootstrap/env-variables.png)

  *herinner dat de variabelen van het douanemilieu in Reageren met `REACT_APP_` moeten worden vooraf bepaald.*

   * `REACT_APP_HOST_URI`: het schema en de gastheer van de dienst van AEM de Verre SPA verbindt met.
      * Deze waarde verandert op basis van het type AEM (lokaal, Ontwikkelen, Stage of Productie) en AEM Service (Auteur versus Publiceren).
   * `REACT_APP_USE_PROXY`: hiermee vermijdt u CORS-problemen tijdens de ontwikkeling door de responsontwikkelingsserver te informeren naar AEM-proxyverzoeken zoals `/content, /graphql, .model.json` using `http-proxy-middleware` module.
   * `REACT_APP_AUTH_METHOD`: verificatiemethode voor door AEM verzonden aanvragen, opties zijn &#39;service-token&#39;, &#39;dev-token&#39;, &#39;basic&#39; of blanco laten voor gebruik zonder toezicht
      * Vereist voor gebruik met AEM Author
      * Mogelijk vereist voor gebruik met AEM Publish (als de inhoud is beveiligd)
      * Ontwikkelen tegen de AEM SDK ondersteunt lokale accounts via Basic Auth. Dit is de methode die in deze zelfstudie wordt gebruikt.
      * Wanneer het integreren met AEM as a Cloud Service, gebruik [ toegangstokens ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=nl-NL)
   * `REACT_APP_BASIC_AUTH_USER`: de AEM __gebruikersbenaming__ door het KUUROORD voor authentiek te verklaren terwijl het terugwinnen van de inhoud van AEM.
   * `REACT_APP_BASIC_AUTH_PASS`: het AEM __wachtwoord__ door het KUUROORD voor authentiek te verklaren terwijl het terugwinnen van de inhoud van AEM.

## De ModelManager-API integreren

Met de AEM SPA npm-afhankelijkheden die beschikbaar zijn voor de app, initialiseert u AEM `ModelManager` in het project `index.js` before `ReactDOM.render(...)` wordt aangeroepen.

[ ModelManager ](https://github.com/adobe/aem-spa-page-model-manager/blob/master/src/ModelManager.ts) is verantwoordelijk voor het verbinden met AEM aan het terugwinnen van editable inhoud.

1. Open het Verre project van het KUUROORD in uw winde
1. Het bestand openen `src/index.js`
1. Importeren `ModelManager` toevoegen en initialiseren voordat `root.render(..)` wordt aangeroepen,

   ```javascript
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking root.render(..).
   ModelManager.initializeAsync();
   
   const container = document.getElementById('root');
   const root = createRoot(container);
   root.render(<App />);
   ```

Het bestand `src/index.js` moet er als volgt uitzien:

![ src/index.js](./assets/spa-bootstrap/index-js.png)

## Een interne SPA-proxy instellen

Wanneer het creëren van een editable KUUROORD, is het best aan opstelling een [ interne volmacht in het KUUROORD ](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually), die wordt gevormd om de aangewezen verzoeken aan AEM te leiden. Dit wordt gedaan door [ http-proxy-middleware ](https://www.npmjs.com/package/http-proxy-middleware) te gebruiken npm module, die reeds door de basisWKND GraphQL App geïnstalleerd is.

1. Open het Verre project van het KUUROORD in uw winde
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

   Het bestand `setupProxy.spa-editor.auth.basic.js` moet er als volgt uitzien:

   ![ src/proxy/setupProxy.spa-editor.auth.basic.js ](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   Deze proxyconfiguratie heeft twee belangrijke functies:

   1. Vermeldt specifieke verzoeken aan SPA (`http://localhost:3000`) aan AEM `http://localhost:4502`
      * Alleen proxy&#39;s aanvragen waarvan de paden overeenkomen met patronen die aangeven dat ze door AEM moeten worden aangeleverd, zoals gedefinieerd in `toAEM(path, req)` .
      * Het herschrijft de wegen van het KUUROORD aan hun tegenhanger AEM pagina&#39;s, zoals die in `pathRewriteToAEM(path, req)` worden bepaald
   1. CORS-koppen worden toegevoegd aan alle aanvragen om toegang tot AEM-inhoud toe te staan, zoals gedefinieerd door `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`
      * Als dit niet wordt toegevoegd, komen de fouten CORS voor wanneer het laden van de inhoud van AEM in het KUUROORD.

1. Het bestand openen `src/setupProxy.js`
1. Controleer de regel die naar het configuratiebestand van de proxy `setupProxy.spa-editor.auth.basic` verwijst:

   ```
   ...
   case BASIC:
   // Use user/pass for local development with Local Author Env
   return require('./proxy/setupProxy.spa-editor.auth.basic');
   ...
   ```

Nota, om het even welke veranderingen in `src/setupProxy.js` of het heeft van verwijzingen voorzien dossiers vereisen een nieuw begin van het KUUROORD.

## Statische bron van SPA

De statische middelen van het KUUROORD zoals het Logo WKND en de grafiek van de Lading moeten hun src URLs hebben worden bijgewerkt om hen te dwingen laden van de Verre gastheer van het KUUROORD. Als verlaten relatief, wanneer het KUUROORD in de Redacteur van het KUUROORD voor creatie wordt geladen, dit gebrek URLs om de gastheer van AEM eerder dan het KUUROORD te gebruiken, resulterend in 404 verzoeken zoals die in het hieronder beeld worden geïllustreerd.

![ Gebroken statische middelen ](./assets/spa-bootstrap/broken-static-resource.png)

Om deze kwestie op te lossen, maak een statisch middel dat door het Verre gebruik van het KUUROORD absolute wegen wordt ontvangen die de oorsprong van het Verre KUUROORD omvatten.

1. Open het project van het KUUROORD in uw winde
1. Open het dossier van de milieuvariabelen van uw SPA `src/.env.development` en voeg een variabele voor het openbare URI van het KUUROORD toe:

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _wanneer het opstellen aan AEM as a Cloud Service, moet u het zelfde voor de overeenkomstige `.env` dossiers._

1. Het bestand openen `src/App.js`
1. Importeer openbare URI van het KUUROORD van de het milieuvariabelen van het KUUROORD

   ```javascript
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. Plaats het WKND-logo `<img src=.../>` voor `REACT_APP_PUBLIC_URI` om de resolutie tegen de SPA te forceren.

   ```html
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. Doe hetzelfde voor het laden van de afbeelding in `src/components/Loading.js`

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

1. En voor de __twee instanties__ van de achterknoop in `src/components/AdventureDetails.js`

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

De bestanden `App.js` , `Loading.js` en `AdventureDetails.js` moeten er als volgt uitzien:

![ Statische middelen ](./assets/spa-bootstrap/static-resources.png)

## AEM responsief raster

Om de lay-outwijze van de Redacteur van het KUUROORD voor editable gebieden in het KUUROORD te steunen, moeten wij AEM het Responsieve Net CSS in het KUUROORD integreren. Maak u geen zorgen - dit rastersysteem is alleen van toepassing op de bewerkbare containers en u kunt uw keuzerondje in het raster gebruiken om de lay-out van de rest van de SPA te bepalen.

Voeg de AEM Responsive Grid SCSS-bestanden toe aan de SPA.

1. Open het project van het KUUROORD in uw winde
1. De volgende twee bestanden downloaden en kopiëren naar `src/styles`
   * [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      * De AEM Responsive Grid SCSS-generator
   * [_grid-init.scss](./assets/spa-bootstrap/_grid-init.scss)
      * Roept `_grid.scss` aan gebruikend de specifieke breekpunten van het KUUROORD (Desktop en mobiel) en kolommen (12).
1. Openen `src/App.scss` en importeren `./styles/grid-init.scss`

   ```scss
   ...
   @import './styles/grid-init';
   ...
   ```

De bestanden `_grid.scss` en `_grid-init.scss` moeten er als volgt uitzien:

![ AEM Responsive Grid SCSS ](./assets/spa-bootstrap/aem-responsive-grid.png)

Nu omvat het SPA CSS die wordt vereist om de Wijze van de Lay-out van AEM voor componenten te steunen die aan een container van AEM worden toegevoegd.

## Hulpprogrammaklassen

Kopieer in de volgende hulpprogrammaklassen naar uw Reactie-app-project.

* [ RoutedLink.js ](./assets/spa-bootstrap/RoutedLink.js) aan `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/RoutedLink.js`
* [ EditorPlaceholder.js ](./assets/spa-bootstrap/EditorPlaceholder.js) aan `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/EditorPlaceholder.js`
* [ withConditionalPlaceholder.js ](./assets/spa-bootstrap/withConditionalPlaceholder.js) aan `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withConditionalPlaceholder.js`
* [ withStandardBaseCssClass.js ](./assets/spa-bootstrap/withStandardBaseCssClass.js) aan `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withStandardBaseCssClass.js`

![ Verre het hulpprogrammaklassen van het KUUROORD ](./assets/spa-bootstrap/utility-classes.png)

## Begin SPA

Nu het KUUROORD voor integratie met AEM wordt opgepakt, laten wij het KUUROORD in werking stellen en zien hoe het kijkt!

1. Op de bevellijn, navigeer aan de wortel van het project van SPA
1. Begin SPA gebruikend de normale bevelen (als u het nog niet hebt gedaan)

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
   $ npm install 
   $ npm run start
   ```

1. Blader het KUUROORD op [ http://localhost:3000 ](http://localhost:3000). Alles moet er goed uitzien!

![ KUUROORD die op http://localhost:3000 ](./assets/spa-bootstrap/localhost-3000.png) loopt

## De SPA openen in de AEM SPA Editor

Met het KUUROORD dat op [ http://localhost:3000 ](http://localhost:3000) loopt, open het gebruikend de Redacteur van AEM SPA. Niets is editable in het KUUROORD nog, bevestigt dit slechts het KUUROORD in AEM.

1. Aanmelden bij AEM-auteur
1. Navigeer aan __Plaatsen > App WKND > gebruiken > en__
1. Selecteer de __WebND pagina van het Huis van de App__ en de Tik __uitgeven__, en het KUUROORD verschijnt.

   ![ geeft WKND App Homepage ](./assets/spa-bootstrap/edit-home.png) uit

1. Schakelaar aan __Voorproef__ gebruikend de wijzeschakelaar in het hoogste recht
1. Klik rond SPA

   ![ KUUROORD die op http://localhost:3000 ](./assets/spa-bootstrap/spa-editor.png) loopt

## Gefeliciteerd!

U hebt bootstrapped het Verre KUUROORD om de Redacteur van AEM SPA compatibel te zijn! Nu weet u hoe:

* Voeg de gebiedsdelen van AEM SPA Editor JS SDK npm aan het project van het KUUROORD toe
* Vorm de het milieuvariabelen van uw SPA
* Integreer ModelManager API met SPA
* Opstelling een interne volmacht voor SPA zodat leidt het de aangewezen inhoudsverzoeken aan AEM
* De kwesties van het adres met statische middelen die van SPA in de context van de Redacteur van het KUUROORD oplossen
* CSS voor AEM-responsief raster toevoegen ter ondersteuning van lay-out in bewerkbare AEM-containers

## Volgende stappen

Nu wij een basislijn van verenigbaarheid met de Redacteur van AEM SPA hebben bereikt, kunnen wij beginnen editable gebieden in te voeren. Wij kijken eerst hoe te om a [ vaste editable component ](./spa-fixed-component.md) in het KUUROORD te plaatsen.
