---
title: Navigatie en routering toevoegen | Aan de slag met de AEM SPA Editor en Reageren
description: Leer hoe de veelvoudige meningen in het KUUROORD door afbeelding aan AEM Pagina's met de Redacteur SDK van het KUUROORD kunnen worden gesteund. De dynamische navigatie wordt uitgevoerd gebruikend React Router en toegevoegd aan een bestaande component van de Kopbal.
sub-product: sites
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
translation-type: tm+mt
source-git-commit: 892cb074814eabd347ba7aef883721df0ee4d431
workflow-type: tm+mt
source-wordcount: '2112'
ht-degree: 0%

---


# Navigatie en routering toevoegen {#navigation-routing}

Leer hoe de veelvoudige meningen in het KUUROORD door afbeelding aan AEM Pagina&#39;s met de Redacteur SDK van het KUUROORD kunnen worden gesteund. De dynamische navigatie wordt uitgevoerd gebruikend React Router en toegevoegd aan een bestaande component van de Kopbal.

## Doelstelling

1. Begrijp het model dat van het KUUROORD opties verplettert beschikbaar wanneer het gebruiken van de Redacteur van het KUUROORD.
1. Leer om [React Router](https://reacttraining.com/react-router/) te gebruiken om tussen verschillende meningen van SPA te navigeren.
1. Voer een dynamische navigatie uit die door de AEM paginahiërarchie wordt aangedreven.

## Wat u gaat maken

In dit hoofdstuk wordt een navigatiemenu toegevoegd aan een bestaande `Header` component. Het navigatiemenu wordt aangestuurd door de AEM paginahiërarchie en maakt gebruik van het JSON-model dat wordt geleverd door de [Navigation Core Component](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html).

![Navigatie geïmplementeerd](assets/navigation-routing/final-navigation-implemented.gif)

## Vereisten

Controleer de vereiste gereedschappen en instructies voor het instellen van een [lokale ontwikkelomgeving](overview.md#local-dev-environment).

### De code ophalen

1. Download het beginpunt voor deze zelfstudie via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/navigation-routing-start
   ```

1. Implementeer de basis van de code op een lokale AEM met Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Als u [AEM 6.x](overview.md#compatibility) gebruikt, voegt u het `classic` profiel toe:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Installeer het voltooide pakket voor de traditionele [WKND verwijzingsplaats](https://github.com/adobe/aem-guides-wknd/releases/latest). De beelden die door de [WKND verwijzingsplaats](https://github.com/adobe/aem-guides-wknd/releases/latest) worden verstrekt zullen op het KND SPA worden hergebruikt. U kunt het pakket installeren met [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Pakketbeheer installeren wknd.all](./assets/map-components/package-manager-wknd-all.png)

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) altijd bekijken of de code plaatselijk controleren door aan de tak te schakelen `React/navigation-routing-solution`.

## Inspect Header-updates {#inspect-header}

In vorige hoofdstukken werd de `Header` component toegevoegd als een zuivere component React die via `App.js`werd opgenomen. In dit hoofdstuk is de `Header` component verwijderd en wordt deze toegevoegd via de [Sjablooneditor](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html). Hierdoor kunnen gebruikers het navigatiemenu van de `Header` binnenste AEM configureren.

>[!NOTE]
>
> Er zijn al verschillende CSS- en JavaScript-updates aangebracht in de codebasis om dit hoofdstuk te starten. Om zich op kernconcepten te concentreren, niet worden **alle** codeveranderingen besproken. U kunt de volledige wijzigingen [hier](https://github.com/adobe/aem-guides-wknd-spa/compare/React/map-components-solution...React/navigation-routing-start)bekijken.

1. In winde van uw keus open het de starterproject van het KUUROORD voor dit hoofdstuk.
1. Onder de `ui.frontend` module inspecteert u het bestand `Header.js` op: `ui.frontend/src/components/Header/Header.js`.

   Er zijn verschillende updates uitgevoerd, waaronder de toevoeging van een `HeaderEditConfig` en een `MapTo` om de component aan een AEM component toe te wijzen `wknd-spa-react/components/header`.

   ```js
   /* Header.js */
   ...
   export const HeaderEditConfig = {
       ...
   }
   ...
   MapTo('wknd-spa-react/components/header')(withRouter(Header), HeaderEditConfig);
   ```

1. Controleer in de `ui.apps` module de componentdefinitie van de AEM `Header` component: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-react/components/navigation"
       componentGroup="WKND SPA React - Structure"/>
   ```

   De AEM `Header` component zal alle functionaliteit van de Component [van de](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) Kern van de Navigatie via het `sling:resourceSuperType` bezit erven.

## De koptekst toevoegen aan de sjabloon {#add-header-template}

1. Open een browser en meld u aan bij AEM, [http://localhost:4502/](http://localhost:4502/). De begincodebasis zou reeds moeten worden opgesteld.
1. Navigeer aan het Malplaatje **van de Pagina van het** KUUROORD: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Selecteer de buitenste container voor **basislayout** en klik op het bijbehorende pictogram **Beleid** . Zorg ervoor **dat u de niet-vergrendelde** container **voor layout niet** selecteert voor ontwerpen.

   ![Selecteer het beleidspictogram voor de container van de hoofdlayout](assets/navigation-routing/root-layout-container-policy.png)

1. Creeer een nieuw beleid genoemd **Structuur** SPA:

   ![SPA-structuurbeleid](assets/navigation-routing/spa-policy-update.png)

   Selecteer onder **Toegestane componenten** > **Algemeen** > de **component Layout Container** .

   Onder **Toegestane Componenten** > **WKND SPA REACT - STRUCTUUR** > selecteer de component van de **Kopbal** :

   ![Koptekstcomponent selecteren](assets/navigation-routing/select-header-component.png)

   Onder **Toegestane Componenten** > **WKND SPA REACT - Inhoud** > selecteer de componenten **Afbeelding** en **Tekst** . Er moeten in totaal vier componenten zijn geselecteerd.

   Click **Done** to save the changes.

1. Vernieuw de pagina en voeg de component **Koptekst** toe boven de niet-vergrendelde **container** voor layout:

   ![component Header toevoegen aan sjabloon](./assets/navigation-routing/add-header-component.gif)

1. Selecteer de component **Koptekst** en klik op het pictogram **Beleid** om het beleid te bewerken.
1. Creeer een nieuw beleid met een Titel **van het** Beleid van de Kopbal **van** KND SPA.

   Onder de **eigenschappen**:

   * Stel de **navigatieroot** in op `/content/wknd-spa-react/us/en`.
   * Stel de **basisniveaus** uitsluiten in op **1**.
   * Schakel **Alle onderliggende pagina&#39;s** verzamelen uit.
   * Stel de **navigatiestructuurdiepte** in op **3**.

   ![Koptekstbeleid configureren](assets/navigation-routing/header-policy.png)

   Hierdoor worden de navigatieniveaus 2 diep onder de curve verzameld `/content/wknd-spa-react/us/en`.

1. Nadat u de wijzigingen hebt opgeslagen, ziet u de ingevulde sjabloon `Header` als onderdeel van de sjabloon:

   ![Bevolkte koptekstcomponent](assets/navigation-routing/populated-header.png)

## Onderliggende pagina&#39;s maken

Daarna, creeer extra pagina&#39;s in AEM die als verschillende meningen in het KUUROORD zullen dienen. We zullen ook de hiërarchische structuur van het JSON-model dat door AEM wordt aangeboden, controleren.

1. Navigeer naar de **Sites** -console: [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). Selecteer de **WKND SPA React Home Page** en klik **Create** > **Pagina**:

   ![Nieuwe pagina maken](assets/navigation-routing/create-new-page.png)

1. Selecteer onder **Sjabloon** de optie **SPA-pagina**. Voer onder **Eigenschappen** **pagina 1** in als naam voor de **titel** en **pagina-1** .

   ![De eigenschappen voor de eerste pagina invoeren](assets/navigation-routing/initial-page-properties.png)

   Klik op **Maken** en klik in het dialoogvenster op **Openen** om de pagina te openen in de AEM SPA Editor.

1. Voeg een nieuwe **component Text** toe aan de **container** van de hoofdlay-out. Bewerk de component en voer de tekst in: **Pagina 1** die RTE en het element **H1** gebruikt (u zult volledig-schermwijze moeten ingaan om de paragraafelementen te veranderen)

   ![Voorbeeld van inhoudspagina 1](assets/navigation-routing/page-1-sample-content.png)

   Voel u vrij om extra inhoud toe te voegen, zoals een afbeelding.

1. Ga terug naar de AEM Sites-console en herhaal de bovenstaande stappen, waardoor een tweede pagina met de naam **Pagina 2** wordt gemaakt op hetzelfde niveau als **Pagina 1**.
1. Maak ten slotte een derde pagina, **pagina 3** , maar als **onderliggend** item van **pagina 2**. Na voltooiing zou de plaatshiërarchie als het volgende moeten kijken:

   ![Voorbeeld van sitehiërarchie](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. Open op een nieuw tabblad de API van het JSON-model die wordt geleverd door AEM: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Deze inhoud JSON wordt gevraagd wanneer het KUUROORD eerst wordt geladen. De buitenste structuur ziet er als volgt uit:

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

   Onder `:children` moet u een item zien voor elk van de gemaakte pagina&#39;s. De inhoud voor alle pagina&#39;s staat in dit eerste JSON-verzoek. Zodra, navigatie die wordt uitgevoerd, zullen de verdere meningen van SPA snel worden geladen, aangezien de inhoud reeds beschikbare cliënt-kant is.

   Het is niet wijs om **ALLE** van de inhoud van een KUUROORD in het aanvankelijke JSON- verzoek te laden, aangezien dit de aanvankelijke paginading zou vertragen. Vervolgens kunt u bekijken hoe de hiërarchische diepte van pagina&#39;s wordt verzameld.

1. Navigeer aan het malplaatje van de **Wortel** van het KUUROORD bij: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   Klik op het menu **** Pagina-eigenschappen > **Paginabeleid**:

   ![Open het paginabeleid voor SPA Root](assets/navigation-routing/open-page-policy.png)

1. Het malplaatje van de **Wortel** van het KUUROORD heeft een extra **Hiërarchische lusje van de Structuur** om de inhoud te controleren JSON die wordt verzameld. De **structuurdiepte** bepaalt hoe diep in de sitehiërarchie onderliggende pagina&#39;s onder het **basiselement** moeten worden verzameld. U kunt ook het veld **Structuurpatronen** gebruiken om extra pagina&#39;s te filteren op basis van een reguliere expressie.

   Werk de **structuurdiepte** bij naar **2**:

   ![Structuurdiepte bijwerken](assets/navigation-routing/update-structure-depth.png)

   Klik op **Gereed** om de wijzigingen in het beleid op te slaan.

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

   Het pad **Pagina 3** is verwijderd: `/content/wknd-spa-react/us/en/home/page-2/page-3` uit het oorspronkelijke JSON-model.

   Later, zullen wij zien hoe de AEM Redacteur SDK van het KUUROORD extra inhoud dynamisch kan laden.

## Navigatie implementeren

Implementeer vervolgens het navigatiemenu als onderdeel van het `Header`venster. We kunnen de code rechtstreeks toevoegen, `Header.js` maar het is beter om grote componenten te voorkomen. In plaats daarvan, zullen wij een component van het `Navigation` KUUROORD uitvoeren die potentieel later opnieuw zou kunnen worden gebruikt.

1. Bekijk de JSON die door de AEM `Header` component beschikbaar is gesteld op [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json):

   ```json
   ...
   "header": {
       "items": [
       {
       "level": 0,
       "active": true,
       "path": "/content/wknd-spa-react/us/en/home",
       "description": null,
       "url": "/content/wknd-spa-react/us/en/home.html",
       "lastModified": 1589062597083,
       "title": "WKND SPA React Home Page",
       "children": [
               {
               "children": [],
               "level": 1,
               "active": false,
               "path": "/content/wknd-spa-react/us/en/home/page-1",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-1.html",
               "lastModified": 1589429385100,
               "title": "Page 1"
               },
               {
               "level": 1,
               "active": true,
               "path": "/content/wknd-spa-react/us/en/home/page-2",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-2.html",
               "lastModified": 1589429603507,
               "title": "Page 2",
               "children": [
                   {
                   "children": [],
                   "level": 2,
                   "active": false,
                   "path": "/content/wknd-spa-react/us/en/home/page-2/page-3",
                   "description": null,
                   "url": "/content/wknd-spa-react/us/en/home/page-2/page-3.html",
                   "lastModified": 1589430413831,
                   "title": "Page 3"
                   }
               ],
               }
           ]
           }
       ],
   ":type": "wknd-spa-react/components/header"
   ```

   De hiërarchische aard van de AEM pagina&#39;s wordt gemodelleerd in JSON die kan worden gebruikt om een navigatiemenu te bevolken. Herinnering dat de `Header` component alle functionaliteit van de Component [van de Kern van de](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) Navigatie erft en de inhoud die door JSON wordt blootgesteld zal automatisch aan React reacties worden in kaart gebracht.

1. Open een nieuw eindvenster en navigeer aan de `ui.frontend` omslag van het project van het KUUROORD. Start de **webpack-dev-server** met de opdracht `npm start`.

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

1. Open een nieuw browsertabblad en navigeer naar [http://localhost:3000/](http://localhost:3000/).

   De **webpack-dev-server** moet worden geconfigureerd om het JSON-model te proxylen vanuit een lokale instantie van AEM (`ui.frontend/.env.development`). Op deze manier kunnen we rechtstreeks code schrijven op basis van de inhoud die in AEM in de vorige exercitie is gemaakt. Zorg ervoor dat u in AEM in dezelfde bladersessie bent geverifieerd.

   ![menuschakeloptie werken](./assets/navigation-routing/nav-toggle-static.gif)

   Op `Header` dit moment is de menuschakelfunctionaliteit al geïmplementeerd. Implementeer vervolgens het navigatiemenu.

1. Ga terug naar IDE van uw keus, en open `Header.js` bij `ui.frontend/src/components/Header/Header.js`.
1. Werk de `homeLink()` methode bij om het hard-gecodeerde Koord te verwijderen en de dynamische die steunen te gebruiken binnen door de AEM worden overgegaan component:

   ```js
   /* Header.js */
   ...
   get homeLink() {
        //expect a single root defined as part of the navigation
       if(!this.props.items || this.props.items.length !== 1) {
           return null;
       }
   
       return this.props.items[0].url;
   }
   ...
   ```

   De bovenstaande code vult een URL op basis van het basisnavigatiepunt dat door de component is geconfigureerd. `homeLink()` wordt gebruikt om het logo in de `logo()` methode te vullen en wordt gebruikt om te bepalen of de achterknop moet worden weergegeven in `backButton()`.

   Wijzigingen opslaan in `Header.js`.

1. Voeg een lijn bij de bovenkant van toe `Header.js` om de `Navigation` component onder de andere invoer in te voeren:

   ```js
   /* Header.js */
   ...
   import Navigation from '../Navigation/Navigation';
   ```

1. Werk vervolgens de `get navigation()` methode bij om de `Navigation` component te instantiëren:

   ```js
   /* Header.js */
   ...
   get navigation() {
       //pass all the props to Navigation component
       return <Navigation {...this.props} />;
   }
   ...
   ```

   Zoals eerder vermeld, zullen `Header` wij in plaats van de navigatie binnen de Unie te implementeren de meeste logica in de `Navigation` component implementeren.  De profielen van de `Header` include de JSON-structuur die nodig is om het menu te maken, we geven alle profielen door.
1. Open het bestand `Navigation.js` om `ui.frontend/src/components/Navigation/Navigation.js`.
1. Implementeer de `renderGroupNav(children)` methode:

   ```js
   /* Navigation.js */
   ...
   renderGroupNav(children) {
   
       if(children === null || children.length < 1 ) {
           return null;
       }
       return (<ul className={this.baseCss + '__group'}>
                   {children.map(
                       (item,index) => { return this.renderNavItem(item,index)}
                   )}
               </ul>
       );
   }
   ...
   ```

   Met deze methode wordt een array van navigatie-items gemaakt `children`en wordt een niet-geordende lijst gemaakt. Vervolgens wordt de array doorlopen en wordt het item doorgegeven aan de array `renderNavItem`, die daarna wordt geïmplementeerd.

1. Implementeer de `renderNavItem`:

   ```js
   /* Navigation.js */
   ...
   renderNavItem(item, index) {
       const cssClass = this.baseCss + '__item ' + 
                        this.baseCss + '__item--level-' + item.level + ' ' +
                        (item.active ? ' ' + this.baseCss + '__item--active' : '');
       return (
           <li key={this.baseCss + '__item-' + index} className={cssClass}>
                   { this.renderLink(item) }
                   { this.renderGroupNav(item.children) }
           </li>
       );
   }
   ...
   ```

   Deze methode geeft een lijstitem weer, met CSS-klassen op basis van eigenschappen `level` en `active`. De methode roept vervolgens `renderLink` aan om de ankertag te maken. Aangezien de `Navigation` inhoud hiërarchisch is, wordt een recursieve strategie gebruikt om `renderGroupNav` voor de kinderen van het huidige punt te roepen.

1. Implementeer de `renderLink` methode:

   Voeg een de invoermethode voor de component van de [Verbinding](https://reacttraining.com/react-router/web/api/Link) , een deel van React router, bij de bovenkant van het dossier met de andere invoer toe:

   ```js
   import {Link} from "react-router-dom";
   ```

   Voltooi vervolgens de implementatie van de `renderLink` methode:

   ```js
   renderLink(item){
       return (
           <Link to={item.url} title={item.title} aria-current={item.active && 'page'}
              className={this.baseCss + '__item-link'}>{item.title}</Link>
       );
   }
   ```

   U ziet dat in plaats van een normale ankertag `<a>`de component [Koppeling](https://reacttraining.com/react-router/web/api/Link) wordt gebruikt. Dit zorgt ervoor dat een volledige pagina verfrist niet wordt teweeggebracht, en in plaats daarvan hefboomwerkingen de React router die door de AEM Redacteur JS SDK van het KUUROORD wordt verstrekt.

1. Wijzigingen opslaan in `Navigation.js` en terugkeren naar de **webpack-dev-server**: [http://localhost:3000](http://localhost:3000)

   ![Voltooide koptekstnavigatie](assets/navigation-routing/completed-header.png)

   Open de navigatie door de menuknevel te klikken en u zou de bevolkte navigatiekoppelingen moeten zien. U zou aan verschillende meningen van SPA moeten kunnen navigeren.

## Inspect the SPA Routing

Nu de navigatie is uitgevoerd, inspecteer het verpletteren in AEM.

1. In winde open het dossier `index.js` bij `ui.frontend/src/index.js`.

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

   Bericht dat het in de `App` component van de Router `Router` van het [Reageren](https://reacttraining.com/react-router/)wordt verpakt. De `ModelManager`, verstrekt door de AEM redacteur JS SDK van het KUUROORD, voegt de dynamische routes aan AEM Pagina&#39;s toe die op JSON model API worden gebaseerd.

1. Open een terminal, navigeer aan de wortel van het project, en stel het project in om het gebruiken van uw Geweven vaardigheden te AEM:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Navigeer aan de homepage van het KUUROORD in AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) en open de ontwikkelaarsgereedschappen van uw browser. Onderstaande screenshots worden vastgelegd vanuit de Google Chrome-browser.

   Vernieuw de pagina en u zou een XHR- verzoek moeten zien aan `/content/wknd-spa-react/us/en.model.json`, dat de Wortel van het KUUROORD is. Bericht dat slechts drie kindpagina&#39;s op de configuratie van de hiërarchiediepte aan het malplaatje van de Wortel van het KUUROORD worden gebaseerd vroeger in het leerprogramma worden gemaakt. Dit is exclusief **pagina 3**.

   ![Aanvankelijk JSON-verzoek - SPA-hoofdmap](assets/navigation-routing/initial-json-request.png)

1. Open de gereedschappen voor ontwikkelaars en navigeer met de `Header` navigatie naar **pagina 3**:

   ![Navigeren op pagina 3](assets/navigation-routing/page-three-navigation.png)

   Merk op dat een nieuw XHR-verzoek wordt ingediend aan: `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![Pagina drie XHR-verzoek](assets/navigation-routing/page-3-xhr-request.png)

   De AEM Model Manager begrijpt dat de **pagina 3** JSON-inhoud niet beschikbaar is en activeert automatisch het extra XHR-verzoek.

1. Ga door het navigeren van het KUUROORD gebruikend de diverse navigatiekoppelingen van de `Header` component. Merk op dat er geen extra XHR-verzoeken worden gedaan en dat er geen volledige pagina-vernieuwingen plaatsvinden. Dit maakt het KUUROORD snel voor de eindgebruiker en vermindert onnodige verzoeken terug naar AEM.

   ![Navigatie geïmplementeerd](assets/navigation-routing/final-navigation-implemented.gif)

1. Experimenteer met diepe koppelingen door rechtstreeks naar: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). Let erop dat de knop Terug van de browser blijft werken.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, leerde u hoe de veelvoudige meningen in het KUUROORD door afbeelding aan AEM Pagina&#39;s met de Redacteur SDK van het KUUROORD kunnen worden gesteund. De dynamische navigatie is uitgevoerd gebruikend React Router en toegevoegd aan de `Header` component.

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) altijd bekijken of de code plaatselijk controleren door aan de tak te schakelen `React/navigation-routing-solution`.
