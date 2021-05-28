---
title: SPA componenten toewijzen aan AEM componenten | Aan de slag met de AEM SPA Editor en reageren
description: Leer hoe u React-componenten toewijst aan Adobe Experience Manager (AEM)-componenten met de AEM SPA Editor JS SDK. Met componenttoewijzing kunnen gebruikers dynamische updates uitvoeren naar SPA componenten in de AEM SPA Editor, net als bij traditionele AEM ontwerpen. U zult ook leren hoe te om uit de doos AEM React de Componenten van de Kern te gebruiken.
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
level: Beginner
source-git-commit: 24d70ebaa6a63cfd4a73f43188f25b375dc702ec
workflow-type: tm+mt
source-wordcount: '2273'
ht-degree: 0%

---


# SPA componenten toewijzen aan AEM componenten {#map-components}

Leer hoe u React-componenten toewijst aan Adobe Experience Manager (AEM)-componenten met de AEM SPA Editor JS SDK. Met componenttoewijzing kunnen gebruikers dynamische updates uitvoeren naar SPA componenten in de AEM SPA Editor, net als bij traditionele AEM ontwerpen.

In dit hoofdstuk wordt dieper ingegaan op de AEM JSON-model-API en wordt uitgelegd hoe de JSON-inhoud die door een AEM wordt aangeboden, automatisch kan worden geïnjecteerd in een React-component als props.

## Doelstelling

1. Leer hoe u AEM componenten kunt toewijzen aan SPA Componenten.
1. Inspect hoe een component React dynamische eigenschappen gebruikt die van AEM worden overgegaan.
1. Leer hoe te om uit de doos [Reageer AEM de Componenten van de Kern ](https://github.com/adobe/aem-react-core-wcm-components-examples) te gebruiken.

## Wat u gaat maken

In dit hoofdstuk wordt geïnspecteerd hoe de opgegeven `Text` SPA component wordt toegewezen aan de AEM `Text`component. Reageer de Componenten van de Kern zoals de `Image` SPA component zullen in de SPA worden gebruikt en in AEM worden ontworpen. Buiten de vakeigenschappen van **de Container van de Lay-out** en **de beleid van de Redacteur van het Malplaatje** zullen ook worden gebruikt om een mening tot stand te brengen die een weinig gevarieerder in verschijning is.

![Definitieve ontwerpversie van hoofdstukvoorbeeld](./assets/map-components/final-page.png)

## Vereisten

Controleer de vereiste gereedschappen en instructies voor het instellen van een [lokale ontwikkelomgeving](overview.md#local-dev-environment). Dit hoofdstuk is een voortzetting van het [Integreer het hoofdstuk SPA](integrate-spa.md), nochtans om langs alles te volgen u nodig is een SPA-toegelaten AEM project.

## Toewijzingsmethode

Het basisconcept is om een SPA Component aan een AEM Component in kaart te brengen. AEM componenten, voer server-kant in, voer inhoud als deel van JSON model API uit. De JSON-inhoud wordt door de SPA verbruikt en wordt in de browser op de client uitgevoerd. Er wordt een 1:1-toewijzing gemaakt tussen SPA componenten en een AEM component.

![Overzicht op hoog niveau van het toewijzen van een AEM component aan een React Component](./assets/map-components/high-level-approach.png)

*Overzicht op hoog niveau van het toewijzen van een AEM component aan een React Component*

## De tekstcomponent Inspect

[AEM Projectarchetype](https://github.com/adobe/aem-project-archetype) verstrekt een `Text` component die aan AEM [component van de Tekst ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/text.html) in kaart wordt gebracht. Dit is een voorbeeld van een **content** component, in die zin dat *content* wordt gerenderd vanuit AEM.

Laten we eens kijken hoe de component werkt.

### Inspect het JSON-model

1. Voordat u in de SPA code springt, is het belangrijk dat u het JSON-model begrijpt dat AEM biedt. Navigeer naar [Core Component Library](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) en bekijk de pagina voor de component Text. De Core Component Library bevat voorbeelden van alle AEM Core Components.
1. Selecteer het tabblad **JSON** voor een van de voorbeelden:

   ![JSON-model tekst](./assets/map-components/text-json.png)

   Er moeten drie eigenschappen worden weergegeven: `text`, `richText` en `:type`.

   `:type` is een gereserveerde eigenschap die de  `sling:resourceType` (of het pad) van de AEM Component opsomt. De waarde van `:type` is wat wordt gebruikt om de AEM component aan de SPA in kaart te brengen.

   `text` en  `richText` zijn extra eigenschappen die aan de SPA component zullen worden blootgesteld.

1. Bekijk de JSON-uitvoer op [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). U zou een ingang moeten kunnen vinden gelijkend op:

   ```json
   "text": {
       "id": "text-a647cec03a",
       "text": "<p>Hello World! Updated content!</p>\r\n",
       "richText": true,
       ":type": "wknd-spa-react/components/text",
       "dataLayer": {}
      }
   ```

### De SPA Tekst Inspect

1. In winde van uw keus open omhoog het AEM Project voor de SPA. Vouw de module `ui.frontend` uit en open het bestand `Text.js` onder `ui.frontend/src/components/Text/Text.js`.

1. Het eerste gebied dat we zullen inspecteren, is de `class Text` op ~line 40:

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

   `Text` is een standaard React component. De component gebruikt `this.props.richText` om te bepalen of de te teruggeven inhoud rijke teksten of gewone teksten zal zijn. De werkelijk gebruikte &quot;inhoud&quot; komt van `this.props.text`.

   Om een potentiële aanval van XSS te vermijden, wordt de rijke tekst ontsnapt via `DOMPurify` alvorens [gevaarlijkSetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) te gebruiken om de inhoud terug te geven. Herhaal de eigenschappen `richText` en `text` van het JSON-model eerder in de oefening.

1. Bekijk nu `TextEditConfig` op ~line 29:

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   De bovenstaande code bepaalt wanneer de tijdelijke aanduiding in de AEM auteursomgeving moet worden weergegeven. Als de `isEmpty` methode **true** terugkeert, zal placeholder worden teruggegeven.

1. Neem ten slotte een blik bij de `MapTo` vraag bij ~lijn 62:

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` wordt geleverd door de AEM SPA Editor JS SDK (`@adobe/aem-react-editable-components`). Het pad `wknd-spa-react/components/text` vertegenwoordigt `sling:resourceType` van de AEM component. Dit pad komt overeen met het `:type` dat wordt weergegeven door het JSON-model dat u eerder hebt waargenomen. `MapTo` zorgt ervoor dat de JSON-modelrespons wordt geparseerd en dat de juiste waarden  `props` aan de SPA worden doorgegeven.

   U kunt de AEM `Text` componentendefinitie bij `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text` vinden.

## Reageren kerncomponenten gebruiken

[AEM componenten WCM - React de ](https://github.com/adobe/aem-react-core-wcm-components-base) implementatie van de Kern en  [AEM componenten WCM - de redacteur van de SPA - React de implementatie](https://github.com/adobe/aem-react-core-wcm-components-spa) van de Kern. Dit is een reeks herbruikbare UI-componenten die uit de doos AEM componenten worden toegewezen. De meeste projecten kunnen deze componenten als uitgangspunt voor hun eigen implementatie hergebruiken.

1. Open in de projectcode het bestand `import-components.js` op `ui.frontend/src/components`.
Dit bestand importeert alle SPA componenten die zijn toegewezen aan AEM componenten. Gezien de dynamische aard van de implementatie van de SPARedacteur, moeten wij om het even welke SPA componenten uitdrukkelijk van verwijzingen voorzien die aan AEM auteur-able componenten gebonden zijn. Hierdoor kan een AEM auteur ervoor kiezen om een component te gebruiken waar hij of zij dat in de toepassing wil.
1. De volgende instructies voor importeren bevatten SPA componenten die in het project zijn geschreven:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   ```

1. Er zijn verscheidene andere `imports` van `@adobe/aem-core-components-react-spa` en `@adobe/aem-core-components-react-base`. Deze importeren de React Core-componenten en stellen deze beschikbaar in het huidige project. Deze worden dan in kaart gebracht aan projectspecifieke AEM componenten gebruikend `MapTo`, enkel zoals met het `Text` componentenvoorbeeld vroeger.

### AEM bijwerken

Het beleid is een eigenschap van AEM malplaatjes geeft ontwikkelaars en macht-gebruikers korrelige controle waarover de componenten beschikbaar zijn om te worden gebruikt. De componenten React Core zijn inbegrepen in de SPA Code maar moeten via een beleid worden toegelaten alvorens zij in de toepassing kunnen worden gebruikt.

1. Navigeer in het scherm AEM Start naar **Gereedschappen** > **Sjablonen** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

1. Selecteer en open **SPA Pagina** malplaatje voor het uitgeven.

1. Selecteer de **Indelingscontainer** en klik op het pictogram **policy** om het beleid te bewerken:

   ![indelingscontainerbeleid](assets/map-components/edit-spa-page-template.png)

1. Onder **Toegestane componenten** > **WKND SPA Reageren - Inhoud** > controleren **Afbeelding**, **Taser** en **Titel**.

   ![Bijgewerkte componenten beschikbaar](assets/map-components/update-components-available.png)

   Onder **Standaardcomponenten** > **Toewijzing toevoegen** en kies de **Afbeelding - WKND SPA Reageren - Inhoud** component:

   ![Standaardcomponenten instellen](./assets/map-components/default-components.png)

   Voer een **mime type** van `image/*` in.

   Klik **Done** om de beleidsupdates te bewaren.

1. Klik in **Layout Container** op het pictogram **policy** voor de **Text**-component.

   Maak een nieuw beleid met de naam **WKND SPA Text**. Onder **Insteekmodules** > **Opmaak** > schakelt u alle vakken in om extra opmaakopties in te schakelen:

   ![RTE-opmaak inschakelen](assets/map-components/enable-formatting-rte.png)

   Onder **Insteekmodules** > **Alineastijlen** > schakel het selectievakje in op **Alineastijlen inschakelen**:

   ![Alineastijlen inschakelen](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Klik **Done** om de beleidsupdate op te slaan.

### Inhoud auteur

1. Navigeer naar **Homepage** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

1. U moet nu de extra componenten **Image**, **Teaser**, en **Title** op de pagina kunnen gebruiken.

   ![Aanvullende componenten](assets/map-components/additional-components.png)

1. U zou ook de `Text` component moeten kunnen uitgeven en extra paragraafstijlen toevoegen op **volledig-scherm** wijze.

   ![Tekst bewerken op volledig scherm](assets/map-components/full-screen-rte.png)

1. U moet ook een afbeelding kunnen slepen en neerzetten vanaf de **Finder van middelen**:

   ![Afbeelding slepen en neerzetten](assets/map-components/drag-drop-image.png)

1. Ervaring met de **Title** en **Teaser** componenten.

1. Voeg uw eigen afbeeldingen toe via [AEM Assets](http://localhost:4502/assets.html/content/dam) of installeer de voltooide codebasis voor de standaard [WKND-referentiesite](https://github.com/adobe/aem-guides-wknd/releases/latest). De [WKND-referentiesite](https://github.com/adobe/aem-guides-wknd/releases/latest) bevat veel afbeeldingen die opnieuw kunnen worden gebruikt op de WKND-SPA. Het pakket kan worden geïnstalleerd met [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

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

   Dit bestand bepaalt de onderbrekingspunten (`default`, `tablet` en `phone`) die worden gebruikt door de **Layout Container**. Dit dossier is bedoeld om per projectspecificaties worden aangepast. Momenteel worden de onderbrekingspunten ingesteld op `1200px` en `768px`.

5. U zou de ontvankelijke mogelijkheden en het bijgewerkte rijke tekstbeleid van de `Text` component moeten kunnen gebruiken om een mening als het volgende te ontwerpen:

   ![Definitieve ontwerpversie van hoofdstukvoorbeeld](assets/map-components/final-page.png)

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, leerde u hoe te om SPA componenten aan AEM Componenten in kaart te brengen en u gebruikte de Componenten van de Kern React. U hebt ook de kans om de responsieve mogelijkheden van de **container Layout** te onderzoeken.

### Volgende stappen {#next-steps}

[Navigatie en het Verpletteren](navigation-routing.md)  - Leer hoe de veelvoudige meningen in de SPA door afbeelding aan AEM Pagina&#39;s met SPA Redacteur SDK kunnen worden gesteund. De dynamische navigatie wordt uitgevoerd gebruikend React Router en React de Componenten van de Kern.

## (Bonus) Blijf configuraties aan broncontrole {#bonus-configs}

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

## (Bonus) Aangepaste afbeeldingscomponent maken {#bonus-image}

Er is al een SPA Image-component geleverd door de React Core-componenten. Nochtans, als u extra praktijk wilt, creeer uw eigen React implementatie die aan de AEM [component van het Beeld ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html) in kaart brengt. De `Image` component is een ander voorbeeld van een **content** component.

### Inspect the JSON

Voordat u in de SPA code gaat springen, moet u het JSON-model controleren dat AEM biedt.

1. Navigeer naar de [Voorbeelden van afbeeldingen in de Core Component Library](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html).

   ![Image Core Component JSON](./assets/map-components/image-json.png)

   Eigenschappen van `src`, `alt` en `title` worden gebruikt om de SPA `Image` component te vullen.

   >[!NOTE]
   >
   > Er zijn andere eigenschappen van het Beeld blootgesteld (`lazyEnabled`, `widths`) die een ontwikkelaar toestaan om een adaptieve en lazy ladende component tot stand te brengen. De component die in deze zelfstudie is ingebouwd, is eenvoudig en **niet** gebruikt deze geavanceerde eigenschappen.

### De component Image implementeren

1. Maak vervolgens een nieuwe map met de naam `Image` onder `ui.frontend/src/components`.
1. Onder de map `Image` maakt u een nieuw bestand met de naam `Image.js`.

   ![Image.js, bestand](./assets/map-components/image-js-file.png)

1. Voeg de volgende `import` verklaringen aan `Image.js` toe:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

1. Voeg vervolgens `ImageEditConfig` toe om te bepalen wanneer de tijdelijke aanduiding in AEM moet worden weergegeven:

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   De plaatsaanduiding wordt weergegeven als de eigenschap `src` niet is ingesteld.

1. Implementeer vervolgens de klasse `Image`:

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

1. Voeg de `MapTo` code toe om de React component aan de AEM component in kaart te brengen:

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   De tekenreeks `wknd-spa-react/components/image` komt overeen met de locatie van de AEM component in `ui.apps` op: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

1. Maak een nieuw bestand met de naam `Image.css` in dezelfde map en voeg het volgende toe:

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

1. Voeg in `Image.js` een verwijzing naar het bestand toe boven onder de instructies `import`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.css');
   ```

1. Open het bestand `ui.frontend/src/components/import-components.js` en voeg een verwijzing naar de nieuwe `Image` component toe:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   import './Image/Image'; //add reference to Image component
   ```

1. Plaats in `import-components.js` de commentaarmarkering React Core Component Image:

   ```js
   //MapTo('wknd-spa-react/components/image')(ImageV2, {isEmpty: ImageV2IsEmptyFn});
   ```

   Op deze manier zorgt u ervoor dat de aangepaste component Image wordt gebruikt.

1. Van de wortel van het project stel de SPA code aan AEM gebruikend Maven op:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Inspect de SPA in AEM. Alle onderdelen van de afbeelding op de pagina blijven werken. Inspect de gerenderde uitvoer en u moet de markering voor onze aangepaste afbeeldingscomponent zien in plaats van de React Core-component.

   *Aangepaste markering voor afbeeldingscomponent*

   ```html
   <div class="Image">
       <img class="Image-src" src="/content/image-src.jpg">
   </div>
   ```

   *Opmaak van Core Component Image Reageren*

   ```html
   <div class="cmp-image cq-dd-image">
       <img src="/content/image-src.jpg" class="cmp-image__image">
   </div>
   ```

   Dit is een goede inleiding om uw eigen componenten uit te breiden en uit te voeren.

