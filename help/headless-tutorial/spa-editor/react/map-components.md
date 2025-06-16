---
title: SPA-componenten toewijzen aan AEM-componenten | Aan de slag met de AEM SPA Editor en Reageren
description: Leer hoe u React-componenten aan Adobe Experience Manager (AEM)-componenten toewijst met de AEM SPA Editor JS SDK. De afbeelding van de component laat gebruikers toe om dynamische updates aan de componenten van het KUUROORD binnen de Redacteur van AEM te maken SPA, gelijkend op traditionele het auteursrecht van AEM. U leert ook hoe u de doos AEM React Core Components kunt gebruiken.
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 497ce6d7-cd39-4fb3-b5e0-6c60845f7648
duration: 477
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '2123'
ht-degree: 0%

---

# SPA-componenten toewijzen aan AEM-componenten {#map-components}

{{spa-editor-deprecation}}

Leer hoe u React-componenten aan Adobe Experience Manager (AEM)-componenten toewijst met de AEM SPA Editor JS SDK. De afbeelding van de component laat gebruikers toe om dynamische updates aan de componenten van het KUUROORD binnen de Redacteur van AEM te maken SPA, gelijkend op traditionele het auteursrecht van AEM.

In dit hoofdstuk wordt dieper ingegaan op de API van het AEM JSON-model en wordt uitgelegd hoe de JSON-inhoud die door een AEM-component beschikbaar wordt gesteld, automatisch als props in een React-component kan worden geïnjecteerd.

## Doelstelling

1. Leer hoe te om de componenten van AEM aan Componenten van het KUUROORD in kaart te brengen.
1. Controleer hoe een component React dynamische eigenschappen gebruikt die van AEM worden overgegaan.
1. Leer hoe te om uit de doos [ te gebruiken Reageer de Componenten van de Kern van AEM ](https://github.com/adobe/aem-react-core-wcm-components-examples).

## Wat u gaat maken

Dit hoofdstuk inspecteert hoe de verstrekte `Text` component van het KUUROORD aan de AEM `Text` component in kaart wordt gebracht. Reageer de Componenten van de Kern zoals de `Image` component van het KUUROORD wordt gebruikt in het KUUROORD en authored in AEM. Uit de dooseigenschappen van de **Container van de Lay-out** en **Redacteur van het Malplaatje** beleid worden ook gebruikt om een mening tot stand te brengen die een weinig gevarieerder in verschijning is.

![ de steekproef definitieve van het Hoofdstuk creatie ](./assets/map-components/final-page.png)

## Vereisten

Herzie het vereiste tooling en de instructies voor vestiging a [ lokale ontwikkelomgeving ](overview.md#local-dev-environment). Dit hoofdstuk is een voortzetting van [ integreer het hoofdstuk van het KUUROORD ](integrate-spa.md), nochtans om langs allen te volgen u een SPA-Toegelaten project van AEM nodig hebt.

## Toewijzingsmethode

Het basisconcept is een Component van het KUUROORD aan een Component van AEM in kaart te brengen. AEM-componenten, serveronderdelen uitvoeren, inhoud exporteren als onderdeel van de JSON-model-API. De inhoud JSON wordt verbruikt door het KUUROORD, lopend cliënt-kant in browser. Een afbeelding 1:1 tussen de componenten van het KUUROORD en een component van AEM wordt gecreeerd.

![ overzicht op hoog niveau van het in kaart brengen van een Component van AEM aan een Component van het Reageren ](./assets/map-components/high-level-approach.png)

*overzicht op hoog niveau van het in kaart brengen van een Component van AEM aan een Component van het Reageren*

## De tekstcomponent controleren

Het [ Archieftype van het Project van AEM ](https://github.com/adobe/aem-project-archetype) verstrekt een `Text` component die aan de component van de Tekst van AEM [ ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) in kaart wordt gebracht. Dit is een voorbeeld van de component van de a **inhoud**, in die zin dat het *inhoud* van AEM teruggeeft.

Laten we eens kijken hoe de component werkt.

### Het JSON-model controleren

1. Alvorens in de code van het KUUROORD te springen, is het belangrijk om het model te begrijpen JSON dat AEM verstrekt. Navigeer aan de [ Bibliotheek van de Component van de Kern ](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) en bekijk de pagina voor de component van de Tekst. De Core Component Library bevat voorbeelden van alle AEM Core Components.
1. Selecteer het **JSON** lusje voor één van de voorbeelden:

   ![ JSON van de Tekst model ](./assets/map-components/text-json.png)

   Er moeten drie eigenschappen worden weergegeven: `text` , `richText` en `:type` .

   `:type` is een gereserveerde eigenschap die de `sling:resourceType` (of het pad) van de AEM-component opsomt. De waarde van `:type` is wat wordt gebruikt om de component van AEM aan de component van SPA in kaart te brengen.

   `text` en `richText` zijn extra eigenschappen die aan de component SPA worden blootgesteld.

1. Bekijk de output JSON in [ http://localhost:4502/content/wknd-spa-react/us/en.model.json ](http://localhost:4502/content/wknd-spa-react/us/en.model.json). U zou een ingang moeten kunnen vinden gelijkend op:

   ```json
   "text": {
       "id": "text-a647cec03a",
       "text": "<p>Hello World! Updated content!</p>\r\n",
       "richText": true,
       ":type": "wknd-spa-react/components/text",
       "dataLayer": {}
      }
   ```

### De Text SPA-component controleren

1. In winde van uw keus open omhoog het Project van AEM voor het KUUROORD. Vouw de module `ui.frontend` uit en open het bestand `Text.js` onder `ui.frontend/src/components/Text/Text.js` .

1. Het eerste gebied dat we zullen inspecteren, is de `class Text` bij ~line 40:

   ```js
   class Text extends Component {
   
       get richTextContent() {
           return (<div
                   id={extractModelId(this.props.cqPath)}
                   data-rte-editelement
                   dangerouslySetInnerHTML={{__html: DOMPurify.sanitize(this.props.text)}} />
                   );
       }
   
       get textContent() {
           return <div>{this.props.text}</div>;
       }
   
       render() {
           return this.props.richText ? this.richTextContent : this.textContent;
       }
   }
   ```

   `Text` is een standaardcomponent React. De component gebruikt `this.props.richText` om te bepalen of de inhoud die moet worden gerenderd RTF-tekst of tekst zonder opmaak is. De werkelijk gebruikte &quot;inhoud&quot; komt van `this.props.text`.

   Om een potentiële aanval van XSS te vermijden, wordt de rijke tekst ontsnapt via `DOMPurify` alvorens [ gevaarlijkSetInnerHTML ](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) te gebruiken om de inhoud terug te geven. Herhaal de eigenschappen `richText` en `text` van het JSON-model eerder in de oefening.

1. Open `ui.frontend/src/components/import-components.js` en bekijk vervolgens de `TextEditConfig` bij ~line 86:

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   De bovenstaande code bepaalt wanneer de tijdelijke aanduiding in de AEM-auteursomgeving moet worden weergegeven. Als de `isEmpty` methode **waar** terugkeert dan placeholder wordt teruggegeven.

1. Bekijk ten slotte de `MapTo` oproep bij ~line 94:

   ```js
   export default MapTo('wknd-spa-react/components/text')(LazyTextComponent, TextEditConfig);
   ```

   `MapTo` wordt verstrekt door de Redacteur JS SDK van de SPA van AEM (`@adobe/aem-react-editable-components`). Het pad `wknd-spa-react/components/text` vertegenwoordigt de `sling:resourceType` van de AEM-component. Dit pad komt overeen met het pad `:type` dat wordt weergegeven door het JSON-model dat u eerder hebt waargenomen. `MapTo` zorgt ervoor dat de JSON-modelreactie wordt geparseerd en dat de juiste waarden als `props` worden doorgegeven aan de SPA-component.

   U kunt de definitie van de AEM `Text` -component vinden op `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text` .

## Reageren kerncomponenten gebruiken

[ AEM WCM Componenten - Reageer de implementatie van de Kern ](https://github.com/adobe/aem-react-core-wcm-components-base) en [ de Componenten van WCM van AEM - de redacteur van de SPA - Reageer de implementatie van de Kern ](https://github.com/adobe/aem-react-core-wcm-components-spa). Dit is een set herbruikbare UI-componenten die worden toegewezen aan AEM-componenten die buiten de box vallen. De meeste projecten kunnen deze componenten als uitgangspunt voor hun eigen implementatie hergebruiken.

1. Open in de projectcode het bestand `import-components.js` op `ui.frontend/src/components` .
Dit dossier voert alle componenten van het KUUROORD in die aan de componenten van AEM in kaart brengen. Gezien de dynamische aard van de implementatie van de Redacteur van het KUUROORD, moeten wij om het even welke componenten van het KUUROORD uitdrukkelijk van verwijzingen voorzien die aan auteur-able componenten van AEM verbonden zijn. Op deze manier kan een AEM-auteur ervoor kiezen om een component te gebruiken op de plaats waar hij of zij dat wil in de toepassing.
1. De volgende invoerverklaringen omvatten de componenten van het KUUROORD die in het project worden geschreven:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   ```

1. Er zijn verschillende andere `imports` van `@adobe/aem-core-components-react-spa` en `@adobe/aem-core-components-react-base` . Deze importeren de React Core-componenten en stellen deze beschikbaar in het huidige project. Deze worden vervolgens toegewezen aan projectspecifieke AEM-componenten met behulp van `MapTo` , net als in het voorbeeld van de `Text` -component.

### AEM-beleid bijwerken

Het beleid is een eigenschap van de malplaatjes van AEM geeft ontwikkelaars en macht-gebruikers korrelige controle waarover de componenten beschikbaar zijn om te worden gebruikt. De React Componenten van de Kern zijn inbegrepen in de Code van het KUUROORD maar moeten via een beleid worden toegelaten alvorens zij in de toepassing kunnen worden gebruikt.

1. Van het scherm van het Begin van AEM navigeert aan **Hulpmiddelen** > **Malplaatjes** > **[WKND SPA React ](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

1. Selecteer en open het **malplaatje van de Pagina van het KUUROORD** voor het uitgeven.

1. Selecteer de **Container van de Lay-out** en klik het **beleid** pictogram om het beleid uit te geven:

   ![ lay-outcontainerbeleid ](assets/map-components/edit-spa-page-template.png)

1. Onder **Toegestane Componenten** > **WKND SPA Reageren - Inhoud** > controleer **Beeld**, **Taser**, en **Titel**.

   ![ Bijgewerkte beschikbare Componenten ](assets/map-components/update-components-available.png)

   Onder **StandaardComponenten** > **voeg afbeelding** toe en kies het **Beeld - WKND SPA React - Inhoud** component:

   ![ plaats standaardcomponenten ](./assets/map-components/default-components.png)

   Ga a **mime type** van `image/*` in.

   Klik **Gedaan** om de beleidsupdates te bewaren.

1. In de **Container van de Lay-out** klik het **beleid** pictogram voor de **** component van de Tekst.

   Creeer een nieuw beleid genoemd **WKND Tekst van het KUUROORD**. Onder **Insteekmodules** > **Formatterend** > controleer alle dozen om extra het formatteren opties toe te laten:

   ![ laat RTE het Formatteren ](assets/map-components/enable-formatting-rte.png) toe

   Onder **Insteekmodules** > **Stijlen van de Paragraaf** > controleer de doos **alineastijlen** toelaten:

   ![ laat paragraafstijlen ](./assets/map-components/text-policy-enable-paragraphstyles.png) toe

   Klik **Gedaan** om de beleidsupdate te bewaren.

### Inhoud auteur

1. Navigeer aan de **Homepage** [ http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html ](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

1. U zou nu de extra componenten **Beeld**, **Taser**, en **Titel** op de pagina moeten kunnen gebruiken.

   ![ Extra componenten ](assets/map-components/additional-components.png)

1. U zou ook de `Text` component moeten kunnen uitgeven en extra paragraafstijlen op **volledig-scherm** wijze toevoegen.

   ![ Volledig scherm Rich Text Editing ](assets/map-components/full-screen-rte.png)

1. U zou ook een beeld van de **Vinder van Activa** moeten kunnen slepen en neerzetten:

   ![ belemmering en het beeld van de Daling ](assets/map-components/drag-drop-image.png)

1. Ervaring met de **Titel** en **Taser** componenten.

1. Voeg uw eigen beelden via [ AEM Assets ](http://localhost:4502/assets.html/content/dam) toe of installeer de gebeëindigde codebasis voor de standaard [ WKND verwijzingsplaats ](https://github.com/adobe/aem-guides-wknd/releases/latest). De [ WKND verwijzingsplaats ](https://github.com/adobe/aem-guides-wknd/releases/latest) omvat vele beelden die op het KND KUUROORD kunnen worden opnieuw gebruikt. Het pakket kan worden geïnstalleerd gebruikend [ de Manager van het Pakket van AEM ](http://localhost:4502/crx/packmgr/index.jsp).

   ![ Manager van het Pakket installeert wknd.all ](./assets/map-components/package-manager-wknd-all.png)

## De container voor lay-out controleren

De steun voor de **Container van de Lay-out** wordt automatisch verstrekt door de Redacteur SDK van AEM SPA. De **Container van de Lay-out**, zoals die door de naam wordt vermeld, is a **container** component. De componenten van de container zijn componenten die structuren goedkeuren JSON die *andere* componenten vertegenwoordigen en dynamisch hen concretiseren.

Controleer de container voor lay-out verder.

1. In browser navigeert aan [ http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![ JSON model API - het Responsieve Net ](./assets/map-components/responsive-grid-modeljson.png)

   De **component van de Container van de Lay-out** heeft a `sling:resourceType` van `wcm/foundation/components/responsivegrid` en door de Redacteur van het KUUROORD erkend gebruikend het `:type` bezit, enkel als `Text` en `Image` componenten.

   De zelfde mogelijkheden om een component opnieuw te rangschikken gebruikend [ Wijze van de Lay-out ](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) zijn beschikbaar met de Redacteur van het KUUROORD.

2. Keer terug naar [ http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html ](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Voeg extra **componenten van het Beeld 0} toe {en probeer re-sizing hen gebruikend de** optie van de Lay-out **:**

   ![ resize beeld gebruikend de wijze van de Lay-out ](./assets/map-components/responsive-grid-layout-change.gif)

3. Heropen het model JSON [ http://localhost:4502/content/wknd-spa-react/us/en.model.json ](http://localhost:4502/content/wknd-spa-react/us/en.model.json) en bekijk `columnClassNames` als deel van JSON:

   {de namen van de Klasse van 0} Wolk ](./assets/map-components/responsive-grid-classnames.png)![

   De klassenaam `aem-GridColumn--default--4` geeft aan dat de component 4 kolommen breed moet zijn op basis van een raster van 12 kolommen. Meer details over het [ ontvankelijke net kunnen hier ](https://adobe-marketing-cloud.github.io/aem-responsivegrid/) worden gevonden.

4. Keer terug naar winde en in de `ui.apps` module is er een cliënt-zijbibliotheek die bij `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid` wordt bepaald. Open het bestand `less/grid.less` .

   Dit dossier bepaalt de breekpunten (`default`, `tablet`, en `phone`) die door de **Container van de Lay-out** worden gebruikt. Dit dossier is bedoeld om per projectspecificaties worden aangepast. Momenteel zijn de onderbrekingspunten ingesteld op `1200px` en `768px` .

5. U moet de responsieve mogelijkheden en het bijgewerkte rijke tekstbeleid van de `Text` component kunnen gebruiken om een mening als het volgende te ontwerpen:

   ![ de steekproef definitieve van het Hoofdstuk creatie ](assets/map-components/final-page.png)

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, leerde u hoe te om de componenten van het KUUROORD aan de Componenten van AEM in kaart te brengen en u gebruikte de Componenten van de Kern React. U hebt ook een kans om de ontvankelijke mogelijkheden van de **Container van de Lay-out** te onderzoeken.

### Volgende stappen {#next-steps}

[ Navigatie en het Verpletteren ](navigation-routing.md) - leer hoe de veelvoudige meningen in het KUUROORD door afbeelding aan de Pagina&#39;s van AEM met de Redacteur SDK van het KUUROORD kunnen worden gesteund. De dynamische navigatie wordt uitgevoerd gebruikend React Router en React de Componenten van de Kern.

## (Bonus) zet configuraties aan broncontrole aan {#bonus-configs}

In veel gevallen, vooral aan het begin van een project van AEM is het waardevol om configuraties, zoals malplaatjes en verwant inhoudsbeleid, aan broncontrole voort te zetten. Dit zorgt ervoor dat alle ontwikkelaars tegen de zelfde reeks inhoud en configuraties werken en extra consistentie tussen milieu&#39;s kunnen verzekeren. Wanneer een project een bepaald ontwikkelingsniveau heeft bereikt, kan het beheren van sjablonen worden overgedragen aan een speciale groep van energiegebruikers.

De volgende weinige stappen zullen plaatsvinden gebruikend winde van de Code van Visual Studio en [ Synchronisatie van VSCode AEM ](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) maar zouden het gebruiken van om het even welk hulpmiddel en om het even welke winde kunnen doen die u aan **trek** of **invoert** inhoud van een lokale instantie van AEM hebt gevormd.

1. In winde van de Code van Visual Studio, zorg ervoor dat u {de Synchronisatie van AEM van 0} VSCode **via de uitbreiding van de Marketplace geïnstalleerd hebt:**

   ![ de Synchronisatie van AEM VSCode ](./assets/map-components/vscode-aem-sync.png)

2. Breid **ui.content** module in de ontdekkingsreiziger van het Project uit en navigeer aan `/conf/wknd-spa-react/settings/wcm/templates`.

3. **Right+Click** de `templates` omslag en selecteer **Invoer van de Server van AEM**:

   ![ VSCode het invoermalplaatje ](./assets/map-components/import-aem-servervscode.png)

4. Herhaal de stappen om inhoud in te voeren maar selecteer de **beleid** omslag die bij `/conf/wknd-spa-react/settings/wcm/templates/policies` wordt gevestigd.

5. Controleer het `filter.xml` -bestand in `ui.content/src/main/content/META-INF/vault/filter.xml` .

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-react" mode="merge"/>
        <filter root="/content/wknd-spa-react" mode="merge"/>
        <filter root="/content/dam/wknd-spa-react" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-react" mode="merge"/>
    </workspaceFilter>
   ```

   Het bestand `filter.xml` identificeert de paden van knooppunten die met het pakket zijn geïnstalleerd. Let op `mode="merge"` op elk van de filters die aangeeft dat bestaande inhoud niet wordt gewijzigd, alleen nieuwe inhoud wordt toegevoegd. Aangezien de inhoudsauteurs deze wegen kunnen bijwerken, is het belangrijk dat een codeplaatsing **** geen inhoud overschrijft. Zie de [ documentatie FileVault ](https://jackrabbit.apache.org/filevault/filter.html) voor meer details bij het werken met filterelementen.

   Vergelijk `ui.content/src/main/content/META-INF/vault/filter.xml` en `ui.apps/src/main/content/META-INF/vault/filter.xml` om inzicht te krijgen in de verschillende knooppunten die door elke module worden beheerd.

## (Bonus) Aangepaste afbeeldingscomponent maken {#bonus-image}

Een component van het Beeld van het KUUROORD is reeds verstrekt door de React componenten van de Kern. Nochtans, als u extra praktijk wilt, creeer uw eigen implementatie van het Reageren die kaarten aan de component van het Beeld van AEM [ ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html). De `Image` component is een ander voorbeeld van a **inhoud** component.

### De JSON inspecteren

Alvorens in de code van het KUUROORD te springen, inspecteer het model JSON dat door AEM wordt verstrekt.

1. Navigeer aan de [ voorbeelden van het Beeld in de bibliotheek van de Component van de Kern ](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![ Component JSON van de Kern van het Beeld ](./assets/map-components/image-json.png)

   Eigenschappen van `src` , `alt` en `title` worden gebruikt om de component SPA `Image` te vullen.

   >[!NOTE]
   >
   > Er zijn andere eigenschappen van het Beeld blootgesteld (`lazyEnabled`, `widths`) die een ontwikkelaar toestaan om een adaptieve en lui-ladende component tot stand te brengen. De component die in dit leerprogramma wordt gebouwd is eenvoudig en **gebruikt** deze geavanceerde eigenschappen niet.

### De component Image implementeren

1. Maak vervolgens een nieuwe map met de naam `Image` onder `ui.frontend/src/components` .
1. Onder de map `Image` maakt u een nieuw bestand met de naam `Image.js` .

   ![ Image.js- dossier ](./assets/map-components/image-js-file.png)

1. Voeg de volgende `import` instructies toe aan `Image.js` :

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

1. Voeg vervolgens de `ImageEditConfig` toe om te bepalen wanneer de tijdelijke aanduiding in AEM moet worden weergegeven:

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   De plaatsaanduiding wordt weergegeven als de eigenschap `src` niet is ingesteld.

1. Implementeer vervolgens de klasse `Image` :

   ```js
    export default class Image extends Component {
   
       get content() {
           return <img     className="Image-src"
                           src={this.props.src}
                           alt={this.props.alt}
                           title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       render() {
           if(ImageEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
                   <div className="Image">
                       {this.content}
                   </div>
           );
       }
   }
   ```

   De bovenstaande code geeft een `<img>` weer op basis van de eigenschappen `src` , `alt` en `title` die door het JSON-model zijn doorgegeven.

1. Voeg de code `MapTo` toe om de component React toe te wijzen aan de component AEM:

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   De tekenreeks `wknd-spa-react/components/image` komt overeen met de locatie van de AEM-component in `ui.apps` at: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image` .

1. Maak een nieuw bestand met de naam `Image.css` in dezelfde map en voeg het volgende toe:

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

1. Voeg in `Image.js` een verwijzing naar het bestand toe boven onder de instructies `import` :

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.css');
   ```

1. Open het bestand `ui.frontend/src/components/import-components.js` en voeg een verwijzing naar de nieuwe component `Image` toe:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   import './Image/Image'; //add reference to Image component
   ```

1. Verwijder in `import-components.js` het commentaar React Core Component Image:

   ```js
   //MapTo('wknd-spa-react/components/image')(ImageV2, {isEmpty: ImageV2IsEmptyFn});
   ```

   Op deze manier zorgt u ervoor dat de aangepaste component Image wordt gebruikt.

1. Van de wortel van het project stelt de code van het KUUROORD aan AEM op gebruikend Maven:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Inspecteer de SPA in AEM. Alle onderdelen van de afbeelding op de pagina blijven werken. Inspecteer de gerenderde uitvoer en u zou de prijsverhoging voor onze component van het douaneBeeld in plaats van React de Component van de Kern moeten zien.

   *de componentenprijsverhoging van het Beeld van de Douane*

   ```html
   <div class="Image">
       <img class="Image-src" src="/content/image-src.jpg">
   </div>
   ```

   *React de prijsverhoging van het Beeld van de Component van de Kern*

   ```html
   <div class="cmp-image cq-dd-image">
       <img src="/content/image-src.jpg" class="cmp-image__image">
   </div>
   ```

   Dit is een goede inleiding om uw eigen componenten uit te breiden en uit te voeren.
