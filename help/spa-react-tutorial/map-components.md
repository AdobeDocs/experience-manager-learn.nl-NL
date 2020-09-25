---
title: De componenten van het KUUROORD van de kaart aan AEM componenten | Aan de slag met de AEM SPA Editor en Reageren
description: Leer hoe te om React componenten aan de componenten van Adobe Experience Manager (AEM) met AEM redacteur JS SDK in kaart te brengen. De afbeelding van de component laat gebruikers toe om dynamische updates aan de componenten van het KUUROORD binnen de AEM Redacteur van het KUUROORD, gelijkend op traditioneel AEM te maken.
sub-product: sites
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
translation-type: tm+mt
source-git-commit: 52748ff530e98c4ec21b84250bd73543899db4e4
workflow-type: tm+mt
source-wordcount: '2259'
ht-degree: 0%

---


# De componenten van het KUUROORD van de kaart aan AEM componenten {#map-components}

Leer hoe te om React componenten aan de componenten van Adobe Experience Manager (AEM) met AEM redacteur JS SDK in kaart te brengen. De afbeelding van de component laat gebruikers toe om dynamische updates aan de componenten van het KUUROORD binnen de AEM Redacteur van het KUUROORD, gelijkend op traditioneel AEM te maken.

In dit hoofdstuk wordt dieper ingegaan op de AEM JSON-model-API en wordt uitgelegd hoe de JSON-inhoud die door een AEM wordt aangeboden, automatisch kan worden geïnjecteerd in een React-component als props.

## Doelstelling

1. Leer hoe te om AEM componenten aan de Componenten van het KUUROORD in kaart te brengen.
2. Begrijp het verschil tussen de componenten van de **Container** en de componenten van de **Inhoud** .
3. Maak een nieuwe component React die aan een bestaande AEM wordt toegewezen.

## Wat u gaat maken

Dit hoofdstuk zal inspecteren hoe de verstrekte component van het `Text` KUUROORD aan de AEM `Text`component in kaart wordt gebracht. Een nieuwe component van het `Image` KUUROORD zal worden gecreeerd die in het KUUROORD kan worden gebruikt en in AEM wordt geschreven. Buiten de vakeigenschappen van het beleid van de Container **van de** Lay-out en van de Redacteur **van het** Malplaatje zullen ook worden gebruikt om een mening tot stand te brengen die een weinig gevarieerder in verschijning is.

![Definitieve ontwerpversie van hoofdstukvoorbeeld](./assets/map-components/final-page.png)

## Vereisten

Controleer de vereiste gereedschappen en instructies voor het instellen van een [lokale ontwikkelomgeving](overview.md#local-dev-environment).

### De code ophalen

1. Download het beginpunt voor deze zelfstudie via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/map-components-start
   ```

2. Implementeer de basis van de code op een lokale AEM met Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Als u [AEM 6.x](overview.md#compatibility) gebruikt, voegt u het `classic` profiel toe:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) altijd bekijken of de code plaatselijk controleren door aan de tak te schakelen `React/map-components-solution`.

## Toewijzingsmethode

Het basisconcept moet een Component van het KUUROORD aan een AEMComponent in kaart brengen. AEM componenten, voer server-kant in, voer inhoud als deel van JSON model API uit. De inhoud JSON wordt verbruikt door het KUUROORD, lopend cliënt-kant in browser. Een afbeelding 1:1 tussen de componenten van het KUUROORD en een AEM wordt gecreeerd.

![Overzicht op hoog niveau van het toewijzen van een AEM component aan een React Component](./assets/map-components/high-level-approach.png)

*Overzicht op hoog niveau van het toewijzen van een AEM component aan een React Component*

## De tekstcomponent Inspect

Het [AEM Projectarchetype](https://github.com/adobe/aem-project-archetype) verstrekt een `Text` component die aan de AEM component [van de](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/text.html)Tekst in kaart wordt gebracht. Dit is een voorbeeld van een **inhoudcomponent** , in die zin dat *inhoud* wordt gerenderd uit AEM.

Laten we eens kijken hoe de component werkt.

### Inspect het JSON-model

1. Alvorens in de code van het KUUROORD te springen, is het belangrijk om het model te begrijpen JSON dat AEM verstrekt. Navigeer naar de [Core Component Library](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) en bekijk de pagina voor de Text-component. De Core Component Library bevat voorbeelden van alle AEM Core Components.
2. Selecteer het tabblad **JSON** voor een van de voorbeelden:

   ![JSON-model tekst](./assets/map-components/text-json.png)

   Er moeten drie eigenschappen worden weergegeven: `text`, `richText`en `:type`.

   `:type` is een gereserveerde eigenschap die de `sling:resourceType` (of het pad) van de AEM Component opsomt. De waarde van `:type` is wat wordt gebruikt om de AEM component aan de component van het KUUROORD in kaart te brengen.

   `text` en `richText` zijn extra eigenschappen die aan de component van het KUUROORD zullen worden blootgesteld.

### De component Text Inspect

1. Open een nieuwe terminal en navigeer naar de `ui.frontend` map in het project. Voer `npm install` en start vervolgens `npm start` de **webpack-dev-server**:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

   De `ui.frontend` module is momenteel ingesteld op het gebruik van het [model](./integrate-spa.md#mock-json)JSON.

2. Er wordt een nieuw browservenster geopend naar [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![Webpack-ontwikkelserver met mock-inhoud](./assets/map-components/initial-start.png)

3. In winde van uw keus open omhoog het AEM Project voor het KND SPA. Breid de `ui.frontend` module uit en open het dossier `Text.js` onder `ui.frontend/src/components/Text/Text.js`:

   ![Text.js React Component Source Code](./assets/map-components/vscode-ide-text-js.png)

4. Het eerste gebied dat we zullen inspecteren is de `class Text` bij ~line 40:

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

   `Text` is een standaard React component. De component gebruikt `this.props.richText` om te bepalen of de te renderen inhoud rijke teksten of gewone teksten zal zijn. De werkelijk gebruikte &quot;inhoud&quot; komt van `this.props.text`. Om een potentiële aanval van XSS te vermijden, wordt de rijke tekst ontsnapt via `DOMPurify` alvorens [gevaarlijkSetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) te gebruiken om de inhoud terug te geven. Herhaal de `richText` en `text` eigenschappen van het JSON-model eerder in de oefening.

5. Kijk nu eens naar ~line 29 `TextEditConfig` :

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   De bovenstaande code bepaalt wanneer de tijdelijke aanduiding in de AEM auteursomgeving moet worden weergegeven. Als de `isEmpty` methode **true** retourneert, wordt de tijdelijke aanduiding weergegeven.

6. Neem ten slotte een blik op de `MapTo` vraag bij ~lijn 62:

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` wordt verstrekt door de AEM redacteur van het KUUROORD JS SDK (`@adobe/aem-react-editable-components`). Het pad `wknd-spa-react/components/text` vertegenwoordigt de `sling:resourceType` component AEM. Dit pad komt overeen met het pad dat door het JSON-model wordt `:type` getoond dat eerder is waargenomen. `MapTo` verzorgt het ontleden van de JSON modelreactie en het overgaan van de correcte waarden `props` aan de component van het KUUROORD.

   U kunt de definitie van de AEM `Text` component vinden bij `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

7. Experimenteer door het `mock.model.json` bestand te wijzigen op `ui.frontend/public/mock-content/mock.model.json`. Bij ~line 62 werkt u de eerste `Text` waarde bij en gebruikt u een **`H1`** en **`u`** tag:

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       }
   ```

   Ga naar [http://localhost:3000](http://localhost:3000) om de effecten te bekijken:

   ![Bijgewerkt tekstmodel](./assets/map-components/updated-text-model.png)

   Probeer de `richText` eigenschap tussen **true** / **false** te schakelen om de renderlogica in actie te zien.

8. Inspect `Text.scss` om `ui.frontend/src/components/Text/Text.scss`.

   Dit bestand is toegevoegd door de basis van de startcode voor dit hoofdstuk en maakt gebruik van de functie [Sass](https://sass-lang.com/) die in het vorige hoofdstuk is toegevoegd. Noteer de variabelen waarnaar wordt verwezen vanuit `ui.frontend/src/styles/_variables.scss`.

## De afbeeldingscomponent maken

Maak vervolgens een component `Image` React die is toegewezen aan de component [AEM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html)afbeelding. De `Image` component is een ander voorbeeld van een **inhoudcomponent** .

### Inspect the JSON

Alvorens in de code van het KUUROORD te springen, inspecteer het model JSON dat door AEM wordt verstrekt.

1. Navigeer naar de voorbeelden van [afbeeldingen in de Core Component-bibliotheek](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html).

   ![Image Core Component JSON](./assets/map-components/image-json.png)

   De eigenschappen van `src`, `alt`, en `title` zullen worden gebruikt om de `Image` component van het KUUROORD te bevolken.

   >[!NOTE]
   >
   > Er zijn andere eigenschappen van het Beeld blootgesteld (`lazyEnabled`, `widths`) die een ontwikkelaar toestaan om een adaptieve en lazy ladende component tot stand te brengen. De component die in deze zelfstudie is ingebouwd, is eenvoudig en gebruikt **geen** van deze geavanceerde eigenschappen.

2. Ga terug naar uw IDE en open omhoog `mock.model.json` bij `ui.frontend/public/mock-content/mock.model.json`. Aangezien dit een netto-nieuwe component voor ons project is, moeten we de Image JSON &quot;modelleren&quot;.

   Voeg bij ~line 70 een JSON-item voor het `image` model toe (vergeet niet de volgkomma `,` na de tweede `text_23828680`) en werk de `:itemsOrder` array bij.

   ```json
   ...
   ":items": {
               ...
               "text_23828680": {
                   "text": "<p>Mock Model JSON!</p>",
                   "richText": true,
                   ":type": "wknd-spa-react/components/text"
               },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mock-content/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-react/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_23828680",
               "image"
           ],
   ```

   Het project omvat een steekproefbeeld bij `/mock-content/adobestock-140634652.jpeg` die met **webpack-dev-server** zal worden gebruikt.

   U kunt de volledige [mock.model.json hier](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/public/mock-content/mock.model.json)bekijken.

### De component Image implementeren

1. Maak vervolgens een nieuwe map met de naam `Image` onder `ui.frontend/src/components`.
2. Onder de `Image` map maakt u een nieuw bestand met de naam `Image.js`.

   ![Image.js, bestand](./assets/map-components/image-js-file.png)

3. Voeg de volgende `import` instructies toe aan `Image.js`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

4. Voeg vervolgens de code toe `ImageEditConfig` om te bepalen wanneer de tijdelijke aanduiding in AEM moet worden weergegeven:

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   De tijdelijke aanduiding geeft aan of de `src` eigenschap niet is ingesteld.

5. Implementeer vervolgens de `Image` klasse:

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

   De bovenstaande code geeft een waarde weer die is `<img>` gebaseerd op de eigenschappen `src`en `alt``title` die door het JSON-model zijn doorgegeven.

6. Voeg de `MapTo` code toe om de component React aan de AEM toe in kaart te brengen:

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   Let op: de tekenreeks `wknd-spa-react/components/image` komt overeen met de locatie van de AEM component in `ui.apps` de volgende locatie: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

7. Maak een nieuw bestand met de naam `Image.scss` in dezelfde map en voeg het volgende toe:

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

8. Voeg `Image.js` bovenaan onder de `import` instructies een verwijzing naar het bestand toe:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.scss');
   ```

   U kunt de voltooide [Image.js hier](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/src/components/Image/Image.js)bekijken.

9. Open het bestand `ui.frontend/src/components/import-components.js` en voeg een verwijzing naar de nieuwe `Image` component toe:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Image/Image'; //add reference to Image component
   ```

10. Start de **webpack-dev-server** als dit nog niet is gestart. Navigeer naar [http://localhost:3000](http://localhost:3000) en u ziet de afbeelding renderen:

   ![Afbeelding toegevoegd aan mock](./assets/map-components/image-added-mock.png)

   >[!NOTE]
   >
   > **Bonusuitdaging**: Hiermee implementeert u een nieuwe methode in `Image.js` om de waarde van `this.props.title` als een bijschrift onder de afbeelding weer te geven.

## Beleid bijwerken in AEM

De `Image` component is alleen zichtbaar in de **webpack-dev-server**. Daarna, stel het bijgewerkte KUUROORD op om het malplaatjebeleid te AEM en bij te werken.

1. Stop **webpack-dev-server** en van de wortel van het project, stel de veranderingen in AEM gebruikend uw Maven vaardigheden op:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navigeer in het scherm AEM Start naar **Gereedschappen** > **Sjablonen** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

   Selecteer en bewerk de **SPA-pagina**:

   ![SPA-paginasjabloon bewerken](./assets/map-components/edit-spa-page-template.png)

3. Selecteer de container **van de** Lay-out en klik het is **beleidspictogram** om het beleid uit te geven:

   ![Layoutcontainerbeleid](./assets/map-components/layout-container-policy.png)

4. Onder **Toegestane Componenten** > **WKND SPA React - Inhoud** > controleer de component van het **Beeld** :

   ![Afbeeldingscomponent geselecteerd](./assets/map-components/check-image-component.png)

   Onder **Standaardcomponenten** > **voeg afbeelding** toe en kies het **Beeld - WKND SPA React - de component van de Inhoud** :

   ![Standaardcomponenten instellen](./assets/map-components/default-components.png)

   Voer een **mime-type** in van `image/*`.

   Klik op **Gereed** om de beleidsupdates op te slaan.

5. Klik in de container **** layout op het **beleidspictogram** voor de **component Text** :

   ![Beleidspictogram voor tekstcomponent](./assets/map-components/edit-text-policy.png)

   Maak een nieuw beleid met de naam **WKND SPA Text**. Schakel onder **Insteekmodules** > **Opmaak** > alle vakken in om extra opmaakopties in te schakelen:

   ![RTE-opmaak inschakelen](assets/map-components/enable-formatting-rte.png)

   Schakel onder **Insteekmodules** > **Alineastijlen** > het selectievakje in om alineastijlen **** in te schakelen:

   ![Alineastijlen inschakelen](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Klik op **Gereed** om de beleidsupdate op te slaan.

6. Navigeer naar de **startpagina** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

   U moet ook de `Text` component kunnen bewerken en extra alineastijlen kunnen toevoegen in de modus **Volledig scherm** .

   ![Tekst bewerken op volledig scherm](assets/map-components/full-screen-rte.png)

7. U moet ook een afbeelding kunnen slepen en neerzetten vanaf de **Finder** Asset:

   ![Afbeelding slepen en neerzetten](./assets/map-components/drag-drop-image.gif)

8. Voeg uw eigen afbeeldingen toe via [AEM Assets](http://localhost:4502/assets.html/content/dam) of installeer de voltooide codebasis voor de standaard [WKND-referentiesite](https://github.com/adobe/aem-guides-wknd/releases/latest). De [WKND verwijzingsplaats](https://github.com/adobe/aem-guides-wknd/releases/latest) omvat vele beelden die op het KND KUUROORD kunnen worden opnieuw gebruikt. U kunt het pakket installeren met [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Pakketbeheer installeren wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect the Layout Container

De steun voor de Container **van de** Lay-out wordt automatisch verleend door de AEM Redacteur SDK van het KUUROORD. De **container** van de Lay-out, zoals aangegeven door de naam, is een **containercomponent** . Containercomponenten zijn componenten die JSON-structuren accepteren die *andere* componenten vertegenwoordigen en ze dynamisch instantiëren.

Laten we de container voor lay-out verder inspecteren.

1. Ga in een browser naar [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![JSON-model-API - Responsief raster](./assets/map-components/responsive-grid-modeljson.png)

   De **component van de Container** van de Lay-out heeft een `sling:resourceType` van `wcm/foundation/components/responsivegrid` en door de Redacteur van het KUUROORD erkend gebruikend het `:type` bezit, enkel zoals `Text` en de `Image` componenten.

   De zelfde mogelijkheden om een component opnieuw te rangschikken gebruikend de Wijze [van de](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) Lay-out zijn beschikbaar met de Redacteur van het KUUROORD.

2. Ga terug naar [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Voeg aanvullende onderdelen voor de **afbeelding** toe en probeer de grootte ervan te wijzigen met de optie **Lay-out** :

   ![Afbeeldingen vergroten/verkleinen met de modus Lay-out](./assets/map-components/responsive-grid-layout-change.gif)

3. Open het JSON-model opnieuw [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) en bekijk het `columnClassNames` als onderdeel van de JSON:

   ![Klassennamen wolken](./assets/map-components/responsive-grid-classnames.png)

   De klassenaam `aem-GridColumn--default--4` geeft aan dat de component 4 kolommen breed moet zijn op basis van een raster van 12 kolommen. Meer informatie over het [responsieve raster vindt u hier](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. Keer aan winde terug en in de `ui.apps` module is er een cliënt-zijbibliotheek die bij wordt bepaald `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`. Open the file `less/grid.less`.

   Dit bestand bepaalt de onderbrekingspunten (`default`, `tablet`en `phone`) die worden gebruikt door de container **van de** layout. Dit dossier is bedoeld om per projectspecificaties worden aangepast. Momenteel zijn de onderbrekingspunten ingesteld op `1200px` en `650px`.

5. U zou de ontvankelijke mogelijkheden en het bijgewerkte rijke tekstbeleid van de `Text` component aan auteur een mening als het volgende moeten kunnen gebruiken:

   ![Definitieve ontwerpversie van hoofdstukvoorbeeld](./assets/map-components/final-page.png)

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, leerde u hoe te om de componenten van het KUUROORD aan AEM Componenten in kaart te brengen en u uitvoerde een nieuwe `Image` component. U hebt ook de kans om de responsieve mogelijkheden van de **container** van de Lay-out te onderzoeken.

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) altijd bekijken of de code plaatselijk controleren door aan de tak te schakelen `React/map-components-solution`.

### Volgende stappen {#next-steps}

[Navigatie en het Verpletteren](navigation-routing.md) - leer hoe de veelvoudige meningen in het KUUROORD door afbeelding aan AEM Pagina&#39;s met de Redacteur SDK van het KUUROORD kunnen worden gesteund. De dynamische navigatie wordt uitgevoerd gebruikend React Router en toegevoegd aan een bestaande component van de Kopbal.

## Bonus - configuraties aan broncontrole blijven {#bonus}

In veel gevallen, vooral aan het begin van een AEM project is het waardevol om configuraties, zoals malplaatjes en verwant inhoudsbeleid, aan broncontrole voort te zetten. Dit zorgt ervoor dat alle ontwikkelaars tegen de zelfde reeks inhoud en configuraties werken en extra consistentie tussen milieu&#39;s kunnen verzekeren. Wanneer een project een bepaald ontwikkelingsniveau heeft bereikt, kan het beheren van sjablonen worden overgedragen aan een speciale groep van energiegebruikers.

De volgende paar stappen zullen plaatsvinden gebruikend winde van de Code van Visual Studio en [VSCode AEM Synchronisatie](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) maar zouden kunnen doen gebruikend om het even welk hulpmiddel en om het even welke winde die u hebt gevormd om inhoud van een lokaal geval van AEM te **trekken** of te **importeren** .

1. In winde van de Code van Visual Studio, zorg ervoor dat u **VSCode AEM Synchronisatie** via de uitbreiding van de Marketplace geïnstalleerd hebt:

   ![VSCode AEM Sync](./assets/map-components/vscode-aem-sync.png)

2. Vouw de module **ui.content** in de projectverkenner uit en navigeer naar `/conf/wknd-spa-react/settings/wcm/templates`.

3. **Klik met de rechtermuisknop** op de `templates` map en selecteer **Importeren van AEM server**:

   ![VSCode-importsjabloon](./assets/map-components/import-aem-servervscode.png)

4. Herhaal de stappen om inhoud te importeren, maar selecteer de map **policies** in `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5. Inspect het `filter.xml` bestand op `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   Het `filter.xml` bestand identificeert de paden van knooppunten die samen met het pakket worden geïnstalleerd. Let op het `mode="merge"` bij elk van de filters dat aangeeft dat bestaande inhoud niet wordt gewijzigd, alleen nieuwe inhoud wordt toegevoegd. Aangezien de inhoudsauteurs deze wegen kunnen bijwerken, is het belangrijk dat een codeplaatsing **geen** inhoud overschrijft. Raadpleeg de documentatie [bij](https://jackrabbit.apache.org/filevault/filter.html) FileVault voor meer informatie over het werken met filterelementen.

   Vergelijk `ui.content/src/main/content/META-INF/vault/filter.xml` en `ui.apps/src/main/content/META-INF/vault/filter.xml` om de verschillende knopen te begrijpen die door elke module worden beheerd.
