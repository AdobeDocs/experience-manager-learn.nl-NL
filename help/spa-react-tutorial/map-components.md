---
title: SPA componenten toewijzen aan AEM componenten | Aan de slag met de AEM SPA Editor en reageren
description: Leer hoe u React-componenten toewijst aan Adobe Experience Manager (AEM)-componenten met de AEM SPA Editor JS SDK. Met componenttoewijzing kunnen gebruikers dynamische updates uitvoeren naar SPA componenten in de AEM SPA Editor, net als bij traditionele AEM ontwerpen.
sub-product: sites
feature: SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Begin
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2264'
ht-degree: 0%

---


# SPA componenten toewijzen aan AEM componenten {#map-components}

Leer hoe u React-componenten toewijst aan Adobe Experience Manager (AEM)-componenten met de AEM SPA Editor JS SDK. Met componenttoewijzing kunnen gebruikers dynamische updates uitvoeren naar SPA componenten in de AEM SPA Editor, net als bij traditionele AEM ontwerpen.

In dit hoofdstuk wordt dieper ingegaan op de AEM JSON-model-API en wordt uitgelegd hoe de JSON-inhoud die door een AEM wordt aangeboden, automatisch kan worden geïnjecteerd in een React-component als props.

## Doelstelling

1. Leer hoe u AEM componenten kunt toewijzen aan SPA Componenten.
2. Begrijp het verschil tussen **Container** componenten en **Content** componenten.
3. Maak een nieuwe component React die aan een bestaande AEM wordt toegewezen.

## Wat u gaat maken

In dit hoofdstuk wordt geïnspecteerd hoe de opgegeven `Text` SPA component wordt toegewezen aan de AEM `Text`component. Er wordt een nieuwe `Image` SPA component gemaakt die in de SPA kan worden gebruikt en in AEM kan worden geschreven. Buiten de vakeigenschappen van **de Container van de Lay-out** en **de beleid van de Redacteur van het Malplaatje** zullen ook worden gebruikt om een mening tot stand te brengen die een weinig gevarieerder in verschijning is.

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

   Als u [AEM 6.x](overview.md#compatibility) gebruikt, voegt u het profiel `classic` toe:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) altijd bekijken of de code plaatselijk controleren door aan de tak `React/map-components-solution` te schakelen.

## Toewijzingsmethode

Het basisconcept is om een SPA Component aan een AEM Component in kaart te brengen. AEM componenten, voer server-kant in, voer inhoud als deel van JSON model API uit. De JSON-inhoud wordt door de SPA verbruikt en wordt in de browser op de client uitgevoerd. Er wordt een 1:1-toewijzing gemaakt tussen SPA componenten en een AEM component.

![Overzicht op hoog niveau van het toewijzen van een AEM component aan een React Component](./assets/map-components/high-level-approach.png)

*Overzicht op hoog niveau van het toewijzen van een AEM component aan een React Component*

## De tekstcomponent Inspect

[AEM Projectarchetype](https://github.com/adobe/aem-project-archetype) verstrekt een `Text` component die aan AEM [component van de Tekst ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/text.html) in kaart wordt gebracht. Dit is een voorbeeld van een **content** component, in die zin dat *content* wordt gerenderd vanuit AEM.

Laten we eens kijken hoe de component werkt.

### Inspect het JSON-model

1. Voordat u in de SPA code springt, is het belangrijk dat u het JSON-model begrijpt dat AEM biedt. Navigeer naar [Core Component Library](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) en bekijk de pagina voor de component Text. De Core Component Library bevat voorbeelden van alle AEM Core Components.
2. Selecteer het tabblad **JSON** voor een van de voorbeelden:

   ![JSON-model tekst](./assets/map-components/text-json.png)

   Er moeten drie eigenschappen worden weergegeven: `text`, `richText` en `:type`.

   `:type` is een gereserveerde eigenschap die de  `sling:resourceType` (of het pad) van de AEM Component opsomt. De waarde van `:type` is wat wordt gebruikt om de AEM component aan de SPA in kaart te brengen.

   `text` en  `richText` zijn extra eigenschappen die aan de SPA component zullen worden blootgesteld.

### De component Text Inspect

1. Open een nieuwe terminal en navigeer naar de map `ui.frontend` in het project. Voer `npm install` en `npm start` uit om **webpack-dev-server** te starten:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

   De `ui.frontend` module is momenteel opstelling om [modelJSON model](./integrate-spa.md#mock-json) te gebruiken.

2. U zou een nieuw browser venster moeten zien open aan [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![Webpack-ontwikkelserver met mock-inhoud](./assets/map-components/initial-start.png)

3. In winde van uw keus open omhoog het AEM Project voor de SPA WKND. Breid `ui.frontend` module uit en open het dossier `Text.js` onder `ui.frontend/src/components/Text/Text.js`:

   ![Text.js React Component Source Code](./assets/map-components/vscode-ide-text-js.png)

4. Het eerste gebied dat we zullen inspecteren, is de `class Text` op ~line 40:

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

   `Text` is een standaard React component. De component gebruikt `this.props.richText` om te bepalen of de te teruggeven inhoud rijke teksten of gewone teksten zal zijn. De werkelijk gebruikte &quot;inhoud&quot; komt van `this.props.text`. Om een potentiële aanval van XSS te vermijden, wordt de rijke tekst ontsnapt via `DOMPurify` alvorens [gevaarlijkSetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) te gebruiken om de inhoud terug te geven. Herhaal de eigenschappen `richText` en `text` van het JSON-model eerder in de oefening.

5. Bekijk nu `TextEditConfig` op ~line 29:

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   De bovenstaande code bepaalt wanneer de tijdelijke aanduiding in de AEM auteursomgeving moet worden weergegeven. Als de `isEmpty` methode **true** terugkeert, zal placeholder worden teruggegeven.

6. Neem ten slotte een blik bij de `MapTo` vraag bij ~lijn 62:

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` wordt geleverd door de AEM SPA Editor JS SDK (`@adobe/aem-react-editable-components`). Het pad `wknd-spa-react/components/text` vertegenwoordigt `sling:resourceType` van de AEM component. Dit pad komt overeen met het `:type` dat wordt weergegeven door het JSON-model dat u eerder hebt waargenomen. `MapTo` zorgt ervoor dat de JSON-modelrespons wordt geparseerd en dat de juiste waarden  `props` aan de SPA worden doorgegeven.

   U kunt de AEM `Text` componentendefinitie bij `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text` vinden.

7. Experimenteer door het `mock.model.json` bestand op `ui.frontend/public/mock-content/mock.model.json` te wijzigen. Bij ~line 62 werkt u de eerste `Text`-waarde bij om een **`H1`**- en **`u`**-tag te gebruiken:

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       }
   ```

   Navigeer naar [http://localhost:3000](http://localhost:3000) om de effecten te zien:

   ![Bijgewerkt tekstmodel](./assets/map-components/updated-text-model.png)

   Probeer schakelend het `richText` bezit tussen **true** / **false** om de renderlogica in actie te zien.

8. Inspect `Text.scss` om `ui.frontend/src/components/Text/Text.scss`.

   Dit bestand is toegevoegd door de basis van de startcode voor dit hoofdstuk en maakt gebruik van de functie [Sass](https://sass-lang.com/) die in het vorige hoofdstuk is toegevoegd. Noteer de variabelen waarnaar wordt verwezen vanuit `ui.frontend/src/styles/_variables.scss`.

## De afbeeldingscomponent maken

Maak vervolgens een `Image` React-component die is toegewezen aan de AEM [Image component](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html). De `Image` component is een ander voorbeeld van een **content** component.

### Inspect the JSON

Voordat u in de SPA code gaat springen, moet u het JSON-model controleren dat AEM biedt.

1. Navigeer naar de [Voorbeelden van afbeeldingen in de Core Component Library](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html).

   ![Image Core Component JSON](./assets/map-components/image-json.png)

   Eigenschappen van `src`, `alt` en `title` worden gebruikt om de SPA `Image` component te vullen.

   >[!NOTE]
   >
   > Er zijn andere eigenschappen van het Beeld blootgesteld (`lazyEnabled`, `widths`) die een ontwikkelaar toestaan om een adaptieve en lazy ladende component tot stand te brengen. De component die in deze zelfstudie is ingebouwd, is eenvoudig en **niet** gebruikt deze geavanceerde eigenschappen.

2. Ga terug naar uw IDE en open `mock.model.json` om `ui.frontend/public/mock-content/mock.model.json`. Aangezien dit een netto-nieuwe component voor ons project is, moeten we de Image JSON &quot;modelleren&quot;.

   Voeg bij ~line 70 een JSON-item toe voor het `image`-model (vergeet niet de volgkomma `,` na de tweede `text_23828680`) en werk de `:itemsOrder`-array bij.

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

   Het project bevat een voorbeeldafbeelding op `/mock-content/adobestock-140634652.jpeg` die wordt gebruikt met de **webpack-dev-server**.

   U kunt de volledige [mock.model.json hier](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/public/mock-content/mock.model.json) bekijken.

### De component Image implementeren

1. Maak vervolgens een nieuwe map met de naam `Image` onder `ui.frontend/src/components`.
2. Onder de map `Image` maakt u een nieuw bestand met de naam `Image.js`.

   ![Image.js, bestand](./assets/map-components/image-js-file.png)

3. Voeg de volgende `import` verklaringen aan `Image.js` toe:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

4. Voeg vervolgens `ImageEditConfig` toe om te bepalen wanneer de tijdelijke aanduiding in AEM moet worden weergegeven:

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   De plaatsaanduiding wordt weergegeven als de eigenschap `src` niet is ingesteld.

5. Implementeer vervolgens de klasse `Image`:

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

   De bovenstaande code geeft een `<img>` weer op basis van de props `src`, `alt` en `title` die door het JSON-model zijn doorgegeven.

6. Voeg de `MapTo` code toe om de React component aan de AEM component in kaart te brengen:

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   De tekenreeks `wknd-spa-react/components/image` komt overeen met de locatie van de AEM component in `ui.apps` op: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

7. Maak een nieuw bestand met de naam `Image.scss` in dezelfde map en voeg het volgende toe:

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

8. Voeg in `Image.js` een verwijzing naar het bestand toe boven onder de instructies `import`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.scss');
   ```

   U kunt de voltooide [Image.js hier](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/src/components/Image/Image.js) bekijken.

9. Open het bestand `ui.frontend/src/components/import-components.js` en voeg een verwijzing naar de nieuwe `Image` component toe:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Image/Image'; //add reference to Image component
   ```

10. Start **webpack-dev-server** als dit nog niet is gestart. Navigeer naar [http://localhost:3000](http://localhost:3000) en u moet de afbeeldingsweergave zien:

   ![Afbeelding toegevoegd aan mock](./assets/map-components/image-added-mock.png)

   >[!NOTE]
   >
   > **Bonusuitdaging**: Een nieuwe methode implementeren in  `Image.js` om de waarde van  `this.props.title` als bijschrift onder de afbeelding weer te geven.

## Beleid bijwerken in AEM

De `Image` component is slechts zichtbaar in **webpack-dev-server**. Implementeer vervolgens de bijgewerkte SPA om het sjabloonbeleid te AEM en bij te werken.

1. Stop **webpack-dev-server** en van de wortel van het project, stel de veranderingen in AEM gebruikend uw Maven vaardigheden op:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navigeer in het scherm AEM Start naar **Gereedschappen** > **Sjablonen** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

   Selecteer en bewerk de **SPA Pagina**:

   ![SPA paginasjabloon bewerken](./assets/map-components/edit-spa-page-template.png)

3. Selecteer de **Indelingscontainer** en klik op het pictogram **policy** om het beleid te bewerken:

   ![Layoutcontainerbeleid](./assets/map-components/layout-container-policy.png)

4. Onder **Toegestane componenten** > **WKND SPA Reageren - Inhoud** > controleer de **Image** component:

   ![Afbeeldingscomponent geselecteerd](./assets/map-components/check-image-component.png)

   Onder **Standaardcomponenten** > **Toewijzing toevoegen** en kies de **Afbeelding - WKND SPA Reageren - Inhoud** component:

   ![Standaardcomponenten instellen](./assets/map-components/default-components.png)

   Voer een **mime type** van `image/*` in.

   Klik **Done** om de beleidsupdates te bewaren.

5. Klik in **Layout Container** op het pictogram **policy** voor de **Text**-component:

   ![Beleidspictogram voor tekstcomponent](./assets/map-components/edit-text-policy.png)

   Maak een nieuw beleid met de naam **WKND SPA Text**. Onder **Insteekmodules** > **Opmaak** > schakelt u alle vakken in om extra opmaakopties in te schakelen:

   ![RTE-opmaak inschakelen](assets/map-components/enable-formatting-rte.png)

   Onder **Insteekmodules** > **Alineastijlen** > schakel het selectievakje in op **Alineastijlen inschakelen**:

   ![Alineastijlen inschakelen](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Klik **Done** om de beleidsupdate op te slaan.

6. Navigeer naar **Homepage** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

   U zou ook de `Text` component moeten kunnen uitgeven en extra paragraafstijlen toevoegen op **volledig-scherm** wijze.

   ![Tekst bewerken op volledig scherm](assets/map-components/full-screen-rte.png)

7. U moet ook een afbeelding kunnen slepen en neerzetten vanaf de **Finder van middelen**:

   ![Afbeelding slepen en neerzetten](./assets/map-components/drag-drop-image.gif)

8. Voeg uw eigen afbeeldingen toe via [AEM Assets](http://localhost:4502/assets.html/content/dam) of installeer de voltooide codebasis voor de standaard [WKND-referentiesite](https://github.com/adobe/aem-guides-wknd/releases/latest). De [WKND-referentiesite](https://github.com/adobe/aem-guides-wknd/releases/latest) bevat veel afbeeldingen die opnieuw kunnen worden gebruikt op de WKND-SPA. Het pakket kan worden geïnstalleerd met [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Pakketbeheer installeren wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect the Layout Container

Ondersteuning voor de **Layout Container** wordt automatisch geleverd door de AEM SPA Editor SDK. De **container layout**, zoals aangegeven door de naam, is een **container** component. Containercomponenten zijn componenten die JSON-structuren accepteren die *andere*-componenten vertegenwoordigen en deze dynamisch instantiëren.

Laten we de container voor lay-out verder inspecteren.

1. Navigeer in een browser naar [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![JSON-model-API - Responsief raster](./assets/map-components/responsive-grid-modeljson.png)

   De **Layout Container**-component heeft een `sling:resourceType` van `wcm/foundation/components/responsivegrid` en wordt door de SPA Editor herkend met de eigenschap `:type`, net als de componenten `Text` en `Image`.

   Dezelfde mogelijkheden om de grootte van een component te wijzigen met [Lay-outmodus](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) zijn beschikbaar in de SPA Editor.

2. Ga terug naar [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Voeg extra **Afbeelding**-componenten toe en probeer deze opnieuw te vergroten of te verkleinen met de optie **Lay-out**:

   ![Afbeeldingen vergroten/verkleinen met de modus Lay-out](./assets/map-components/responsive-grid-layout-change.gif)

3. Open het JSON-model [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) opnieuw en bekijk `columnClassNames` als onderdeel van de JSON:

   ![Klassennamen wolken](./assets/map-components/responsive-grid-classnames.png)

   De klassenaam `aem-GridColumn--default--4` geeft aan dat de component 4 kolommen breed moet zijn op basis van een 12-kolomraster. Meer informatie over het [responsieve raster vindt u hier](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. Terugkeer aan winde en in `ui.apps` module is er een cliënt-zijbibliotheek die bij `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid` wordt bepaald. Open het bestand `less/grid.less`.

   Dit bestand bepaalt de onderbrekingspunten (`default`, `tablet` en `phone`) die worden gebruikt door de **Layout Container**. Dit dossier is bedoeld om per projectspecificaties worden aangepast. Momenteel worden de onderbrekingspunten ingesteld op `1200px` en `650px`.

5. U zou de ontvankelijke mogelijkheden en het bijgewerkte rijke tekstbeleid van de `Text` component moeten kunnen gebruiken om een mening als het volgende te ontwerpen:

   ![Definitieve ontwerpversie van hoofdstukvoorbeeld](./assets/map-components/final-page.png)

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, hebt u geleerd hoe u SPA componenten kunt toewijzen aan AEM Componenten en een nieuwe `Image` component hebt geïmplementeerd. U hebt ook de kans om de responsieve mogelijkheden van de **container Layout** te onderzoeken.

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) altijd bekijken of de code plaatselijk controleren door aan de tak `React/map-components-solution` te schakelen.

### Volgende stappen {#next-steps}

[Navigatie en het Verpletteren](navigation-routing.md)  - Leer hoe de veelvoudige meningen in de SPA door afbeelding aan AEM Pagina&#39;s met SPA Redacteur SDK kunnen worden gesteund. De dynamische navigatie wordt uitgevoerd gebruikend React Router en toegevoegd aan een bestaande component van de Kopbal.

## Bonus - configuraties aan broncontrole blijven {#bonus}

In veel gevallen, vooral aan het begin van een AEM project is het waardevol om configuraties, zoals malplaatjes en verwant inhoudsbeleid, aan broncontrole voort te zetten. Dit zorgt ervoor dat alle ontwikkelaars tegen de zelfde reeks inhoud en configuraties werken en extra consistentie tussen milieu&#39;s kunnen verzekeren. Wanneer een project een bepaald ontwikkelingsniveau heeft bereikt, kan het beheren van sjablonen worden overgedragen aan een speciale groep van energiegebruikers.

De volgende paar stappen zullen plaatsvinden gebruikend winde van de Code van Visual Studio en [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) maar zouden kunnen doen gebruikend om het even welk hulpmiddel en om het even welke winde die u aan **pull** of **import** inhoud van een lokale instantie van AEM hebt gevormd.

1. In winde van de Code van Visual Studio, zorg ervoor dat u **VSCode AEM Sync** via de uitbreiding van de Marketplace geïnstalleerd hebt:

   ![VSCode AEM Sync](./assets/map-components/vscode-aem-sync.png)

2. Breid **ui.content** module in de explorator van het Project uit en navigeer aan `/conf/wknd-spa-react/settings/wcm/templates`.

3. **Klik met** de rechtermuisknop op de  `templates` map en selecteer  **Importeren van AEM server**:

   ![VSCode-importsjabloon](./assets/map-components/import-aem-servervscode.png)

4. Herhaal de stappen voor het importeren van inhoud, maar selecteer de map **policies** op `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5. Inspect het `filter.xml`-bestand op `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   Het `filter.xml`-bestand identificeert de paden van knooppunten die samen met het pakket worden geïnstalleerd. Let op `mode="merge"` op elk van de filters die aangeeft dat bestaande inhoud niet wordt gewijzigd, alleen nieuwe inhoud wordt toegevoegd. Aangezien de inhoudsauteurs deze wegen kunnen bijwerken, is het belangrijk dat een codeplaatsing inhoud **niet** beschrijft. Raadpleeg de [FileVault-documentatie](https://jackrabbit.apache.org/filevault/filter.html) voor meer informatie over het werken met filterelementen.

   Vergelijk `ui.content/src/main/content/META-INF/vault/filter.xml` en `ui.apps/src/main/content/META-INF/vault/filter.xml` om de verschillende knopen te begrijpen die door elke module worden beheerd.
