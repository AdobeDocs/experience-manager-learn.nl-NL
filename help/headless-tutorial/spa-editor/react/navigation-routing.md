---
title: Navigatie en routering toevoegen | Aan de slag met de AEM SPA Editor en reageren
description: Leer hoe meerdere weergaven in de SPA kunnen worden ondersteund door aan AEM Pagina's toe te wijzen met de SPA Editor SDK. De dynamische navigatie wordt uitgevoerd gebruikend React Router en React de Componenten van de Kern.
sub-product: sites
feature: SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 24d70ebaa6a63cfd4a73f43188f25b375dc702ec
workflow-type: tm+mt
source-wordcount: '1625'
ht-degree: 0%

---


# Navigatie en routering toevoegen {#navigation-routing}

Leer hoe meerdere weergaven in de SPA kunnen worden ondersteund door aan AEM Pagina&#39;s toe te wijzen met de SPA Editor SDK. De dynamische navigatie wordt uitgevoerd gebruikend React Router en React de Componenten van de Kern.

## Doelstelling

1. Begrijp het SPA model verpletterend opties beschikbaar wanneer het gebruiken van de SPARedacteur.
1. Leer om [Reageer Router](https://reacttraining.com/react-router/) te gebruiken om tussen verschillende meningen van de SPA te navigeren.
1. Gebruik AEM React Core Components om een dynamische navigatie te implementeren die door de AEM paginahiërarchie wordt aangedreven.

## Wat u gaat maken

In dit hoofdstuk wordt navigatie toegevoegd aan een SPA in AEM. Het navigatiemenu wordt aangestuurd door de AEM paginahiërarchie en maakt gebruik van het JSON-model dat wordt geleverd door de [Navigation Core Component](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html).

![Navigatie toegevoegd](assets/navigation-routing/navigation-added.png)

## Vereisten

Controleer de vereiste gereedschappen en instructies voor het instellen van een [lokale ontwikkelomgeving](overview.md#local-dev-environment). Dit hoofdstuk is een voortzetting van het [hoofdstuk van de Componenten van de Kaart](map-components.md), nochtans om langs alles te volgen u wenst is een SPA-toegelaten AEM project dat aan een lokale AEM instantie wordt opgesteld.

## De navigatie toevoegen aan de sjabloon {#add-navigation-template}

1. Open browser en login aan AEM, [http://localhost:4502/](http://localhost:4502/). De begincodebasis zou reeds moeten worden opgesteld.
1. Navigeer naar **SPA Paginasjabloon**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Selecteer de buitenste **Basislay-outcontainer** en klik op het pictogram **Beleid**. Wees voorzichtig **not** om de **Layout Container** niet-vergrendeld voor ontwerpen te selecteren.

   ![Selecteer het beleidspictogram voor de container van de hoofdlayout](assets/navigation-routing/root-layout-container-policy.png)

1. Maak een nieuw beleid met de naam **SPA Structuur**:

   ![SPA structuurbeleid](assets/navigation-routing/spa-policy-update.png)

   Onder **Toegestane componenten** > **Algemeen** > selecteert u de **Indelingscontainer**-component.

   Onder **Toegestane componenten** > **WKND SPA REACT - STRUCTUUR** > selecteert u de **Koptekst** component:

   ![Navigatie-component selecteren](assets/navigation-routing/select-navigation-component.png)

   Onder **Toegestane componenten** > **WKND SPA REACT - Content** > selecteert u de **Image** en **Text** componenten. Er moeten in totaal vier componenten zijn geselecteerd.

   Klik **Done** om de wijzigingen op te slaan.

1. Vernieuw de pagina en voeg de **Navigation** component toe boven de niet-vergrendelde **Layoutcontainer**:

   ![Navigatiecomponent toevoegen aan sjabloon](assets/navigation-routing/add-navigation-component.png)

1. Selecteer de **Navigatie** component en klik zijn **Beleid** pictogram om het beleid uit te geven.
1. Maak een nieuw beleid met een **Beleidstitel** van **SPA Navigatie**.

   Onder **Eigenschappen**:

   * Stel de **Navigation Root** in op `/content/wknd-spa-react/us/en`.
   * Stel **Hoofdniveaus uitsluiten** in op **1**.
   * Schakel **Alle onderliggende pagina&#39;s verzamelen** uit.
   * Stel de **Navigatiestructuurdiepte** in op **3**.

   ![Navigatiebeleid configureren](assets/navigation-routing/navigation-policy.png)

   Hierdoor worden de navigatieniveaus 2 diep onder `/content/wknd-spa-react/us/en` verzameld.

1. Nadat u de wijzigingen hebt opgeslagen, ziet u de gevulde `Navigation` als onderdeel van de sjabloon:

   ![Bevolkt navigatiecomponent](assets/navigation-routing/populated-navigation.png)

## Onderliggende pagina&#39;s maken

Maak vervolgens aanvullende pagina&#39;s in AEM die als de verschillende weergaven in de SPA dienen. We zullen ook de hiërarchische structuur van het JSON-model dat door AEM wordt aangeboden, controleren.

1. Navigeer naar de **Sites**-console: [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). Selecteer **WKND SPA React Home Page** en klik **Create** > **Page**:

   ![Nieuwe pagina maken](assets/navigation-routing/create-new-page.png)

1. Selecteer **SPA Pagina** onder **Sjabloon**. Voer onder **Eigenschappen** **Pagina 1** in voor de **Titel** en **pagina-1** als naam.

   ![De eigenschappen voor de eerste pagina invoeren](assets/navigation-routing/initial-page-properties.png)

   Klik op **Maken** en klik in het dialoogvenster op **Openen** om de pagina te openen in de AEM SPA Editor.

1. Voeg een nieuwe **component Text** aan de belangrijkste **Container voor lay-out** toe. Bewerk de component en voer de tekst in: **Pagina 1** die RTE en het **H2** element gebruiken.

   ![Voorbeeld van inhoudspagina 1](assets/navigation-routing/page-1-sample-content.png)

   Voel u vrij om extra inhoud toe te voegen, zoals een afbeelding.

1. Ga terug naar de AEM Sites-console en herhaal de bovenstaande stappen. Er wordt een tweede pagina gemaakt met de naam **Pagina 2** als een pagina op hetzelfde niveau als **Pagina 1**.
1. Maak ten slotte een derde pagina, **Pagina 3** maar als **child** van **Pagina 2**. Na voltooiing zou de plaatshiërarchie als het volgende moeten kijken:

   ![Voorbeeld van sitehiërarchie](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. De navigatiecomponent kan nu worden gebruikt om naar verschillende gebieden van de SPA te navigeren.

   ![Navigatie en routering](assets/navigation-routing/navigation-working.gif)

1. Open de pagina buiten de AEM Editor: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html). Met de component **Navigatie** kunt u naar verschillende weergaven van de app navigeren.

1. Gebruik de ontwikkelaarshulpmiddelen van uw browser om de netwerkverzoeken te inspecteren, aangezien u navigeert. Onderstaande screenshots worden vastgelegd vanuit de Google Chrome-browser.

   ![Netwerkverzoeken waarnemen](assets/navigation-routing/inspect-network-requests.png)

   Let op: na het laden van de eerste pagina wordt de volledige pagina niet volledig vernieuwd en wordt het netwerkverkeer tot een minimum beperkt wanneer wordt teruggekeerd naar eerder bezochte pagina&#39;s.

## JSON-model {#hierarchy-page-json-model} hiërarchiepagina

Controleer vervolgens het JSON-model dat de multi-view ervaring van de SPA aanstuurt.

1. Open op een nieuw tabblad de API van het JSON-model die wordt geleverd door AEM: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Het kan handig zijn om een browserextensie te gebruiken om de JSON](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa) op te maken.[

   Deze JSON-inhoud wordt opgevraagd wanneer de SPA voor het eerst wordt geladen. De buitenste structuur ziet er als volgt uit:

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
      "/content/wknd-spa-react/us/en/home": {},
      "/content/wknd-spa-react/us/en/home/page-1": {},
      "/content/wknd-spa-react/us/en/home/page-2": {},
      "/content/wknd-spa-react/us/en/home/page-2/page-3": {}
      }
   }
   ```

   Onder `:children` ziet u een vermelding voor elk van de gemaakte pagina&#39;s. De inhoud voor alle pagina&#39;s staat in dit eerste JSON-verzoek. Met het navigatie verpletteren, zullen de verdere meningen van de SPA snel worden geladen, aangezien de inhoud reeds cliënt-kant beschikbaar is.

   Het is niet verstandig om **ALL** van de inhoud van een SPA in het eerste JSON-verzoek te laden, omdat dit de eerste paginalading zou vertragen. Vervolgens kunt u bekijken hoe de hiërarchiediepte van pagina&#39;s wordt verzameld.

1. Navigeer naar de sjabloon **SPA Root** op: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   Klik op het menu **Pagina-eigenschappen** > **Paginabeleid**:

   ![Het paginabeleid voor SPA basispagina openen](assets/navigation-routing/open-page-policy.png)

1. De sjabloon **SPA Root** heeft een extra **tabblad Hiërarchische structuur** om de verzamelde JSON-inhoud te beheren. De **Structuurdiepte** bepaalt hoe diep in de sitehiërarchie onderliggende pagina&#39;s onder de **root** worden verzameld. Met het veld **Structuurpatronen** kunt u ook extra pagina&#39;s filteren op basis van een reguliere expressie.

   Werk **Structuurdiepte** aan **2** bij:

   ![Structuurdiepte bijwerken](assets/navigation-routing/update-structure-depth.png)

   Klik **Done** om de wijzigingen in het beleid op te slaan.

1. Open het JSON-model opnieuw [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
      "/content/wknd-spa-react/us/en/home": {},
      "/content/wknd-spa-react/us/en/home/page-1": {},
      "/content/wknd-spa-react/us/en/home/page-2": {}
      }
   }
   ```

   De weg **Pagina 3** is verwijderd: `/content/wknd-spa-react/us/en/home/page-2/page-3` uit het oorspronkelijke JSON-model. Dit komt omdat **Pagina 3** op niveau 3 in de hiërarchie is en wij het beleid hebben bijgewerkt om slechts inhoud bij een maximumdiepte van niveau 2 te omvatten.

1. Open de SPA homepage opnieuw: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) en open de ontwikkelaarsgereedschappen van uw browser.

   Vernieuw de pagina en u zou het XHR- verzoek aan `/content/wknd-spa-react/us/en.model.json` moeten zien, dat de SPAWortel is. U ziet dat er slechts drie onderliggende pagina&#39;s zijn opgenomen op basis van de configuratie van de hiërarchiediepte voor de SPA basissjabloon die eerder in de zelfstudie is gemaakt. **Pagina 3** is hier niet in opgenomen.

   ![Aanvankelijk JSON-verzoek - SPA basis](assets/navigation-routing/initial-json-request.png)

1. Met de open ontwikkelaars hulpmiddelen, gebruik `Navigation` component om direct aan **Pagina 3** te navigeren:

   Merk op dat een nieuw XHR-verzoek wordt ingediend aan: `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![Pagina drie XHR-verzoek](assets/navigation-routing/page-3-xhr-request.png)

   De AEM ModelManager begrijpt dat de **Pagina 3** JSON-inhoud niet beschikbaar is en activeert automatisch het extra XHR-verzoek.

1. Experimenteer met diepe koppelingen door rechtstreeks naar: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). Let ook op dat de knop Terug van de browser blijft werken.

## Inspect React Routing {#react-routing}

De navigatie en het verpletteren worden uitgevoerd met [Reageer Router](https://reactrouter.com/). Reageer Router is een inzameling van navigatiecomponenten voor React toepassingen. [AEM Reageer de ](https://github.com/adobe/aem-react-core-wcm-components-base) Componenten van de Kern van de Reageren eigenschappen van de Router van het Reageren om de component uit te voeren  **** Navigationdie in de vorige stappen wordt gebruikt.

Daarna, inspecteer hoe de Router van het Reageren met de SPA wordt geïntegreerd en experimenteer gebruikend de component [Link](https://reactrouter.com/web/api/Link) van de Router van het Reageren.

1. Open in winde het dossier `index.js` om `ui.frontend/src/index.js`.

   ```js
   /* index.js */
   import { Router } from 'react-router-dom';
   ...
   ...
    ModelManager.initialize().then(pageModel => {
       const history = createBrowserHistory();
       render(
       <Router history={history}>
           <App
           history={history}
           cqChildren={pageModel[Constants.CHILDREN_PROP]}
           cqItems={pageModel[Constants.ITEMS_PROP]}
           cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
           cqPath={pageModel[Constants.PATH_PROP]}
           locationPathname={window.location.pathname}
           />
       </Router>,
       document.getElementById('spa-root')
       );
   });
   ```

   Merk op dat `App` in `Router` component van [Reageer Router](https://reacttraining.com/react-router/) verpakt is. De `ModelManager`, verstrekt door de AEM SPA Redacteur JS SDK, voegt de dynamische routes aan AEM Pagina&#39;s toe die op JSON model API worden gebaseerd.

1. Open het bestand `Page.js` op `ui.frontend/src/components/Page/Page.js`

   ```js
   class AppPage extends Page {
     get containerProps() {
       let attrs = super.containerProps;
       attrs.className =
         (attrs.className || '') + ' page ' + (this.props.cssClassNames || '');
       return attrs;
     }
   }
   
   export default MapTo('wknd-spa-react/components/page')(
     withComponentMappingContext(withRoute(AppPage))
   );
   ```

   De component `Page` SPA gebruikt de functie `MapTo` om **Pagina&#39;s** in AEM toe te wijzen aan een overeenkomstige SPA component. Het `withRoute` nut helpt om de SPA aan de aangewezen AEM pagina van het Kind dynamisch te leiden die op `cqPath` bezit wordt gebaseerd.

1. Open de `Header.js` component bij `ui.frontend/src/components/Header/Header.js`.
1. Werk `Header` bij om de `<h1>` markering in een [Verbinding](https://reactrouter.com/web/api/Link) aan de homepage te verpakken:

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + import {Link} from 'react-router-dom';
     require('./Header.css');
   
   export default class Header extends Component {
   
       render() {
           return (
               <header className="Header">
               <div className="Header-container">
   +              <Link to="/content/wknd-spa-react/us/en/home.html">
                       <h1>WKND</h1>
   +              </Link>
               </div>
               </header>
           );
       }
   ```

   In plaats van het gebruiken van een standaard `<a>` ankermarkering gebruiken wij `<Link>` die door React Router wordt verstrekt. Zolang `to=` aan een geldige route richt, zal de SPA op die route schakelen en **not** voert een volledige pagina uit vernieuwt. Hier is eenvoudig hard-code de verbinding aan de homepage om het gebruik van `Link` te illustreren.

1. Werk de test bij `App.test.js` om `ui.frontend/src/App.test.js`.

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   Aangezien wij eigenschappen van React Router binnen een statische component gebruiken die in `App.js` van verwijzingen wordt voorzien moeten wij de eenheidstest bijwerken om van het rekenschap te geven.

1. Open een terminal, navigeer aan de wortel van het project, en stel het project in om het gebruiken van uw Geweven vaardigheden te AEM:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Navigeer naar een van de pagina&#39;s in de SPA in AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   In plaats van de `Navigation` component te gebruiken om te navigeren, gebruik de verbinding in `Header`.

   ![Koptekstkoppeling](assets/navigation-routing/header-link.png)

   Merk op dat een volledige pagina wordt gevernieuwd **not** teweeggebracht en dat het SPA verpletteren werkt.

1. Experimenteer desgewenst met het `Header.js`-bestand met een standaardankertag `<a>`:

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   Dit kan helpen het verschil tussen SPA het verpletteren en regelmatige Web-pagina verbindingen illustreren.

## Gefeliciteerd! {#congratulations}

U hebt geleerd hoe meerdere weergaven in de SPA kunnen worden ondersteund door de SPA Editor SDK toe te wijzen aan AEM pagina&#39;s. De dynamische navigatie is uitgevoerd gebruikend React Router en toegevoegd aan de `Header` component.

