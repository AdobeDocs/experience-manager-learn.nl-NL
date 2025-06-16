---
title: SPA-componenten toewijzen aan AEM-componenten | Aan de slag met de AEM SPA Editor en Angular
description: Leer hoe u Angular-componenten aan Adobe Experience Manager (AEM)-componenten kunt toewijzen met de AEM SPA Editor JS SDK. De afbeelding van de component laat gebruikers toe om dynamische updates aan de componenten van het KUUROORD binnen de Redacteur van AEM te maken SPA, gelijkend op traditionele het auteursrecht van AEM.
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-5311
thumbnail: 5311-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 19a8917c-a1e7-4293-9ce1-9f4c1a565861
duration: 509
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '2211'
ht-degree: 0%

---

# SPA-componenten toewijzen aan AEM-componenten {#map-components}

{{spa-editor-deprecation}}

Leer hoe u Angular-componenten aan Adobe Experience Manager (AEM)-componenten kunt toewijzen met de AEM SPA Editor JS SDK. De afbeelding van de component laat gebruikers toe om dynamische updates aan de componenten van het KUUROORD binnen de Redacteur van AEM te maken SPA, gelijkend op traditionele het auteursrecht van AEM.

In dit hoofdstuk wordt dieper ingegaan op de API van het AEM JSON-model en wordt uitgelegd hoe de JSON-inhoud die door een AEM-component wordt aangeboden, automatisch als props in een Angular-component kan worden geïnjecteerd.

## Doelstelling

1. Leer hoe te om de componenten van AEM aan Componenten van het KUUROORD in kaart te brengen.
2. Begrijp het verschil tussen **componenten 0} van de Container {en** Inhoud **componenten.**
3. Maak een nieuwe Angular-component die wordt toegewezen aan een bestaande AEM-component.

## Wat u gaat maken

Dit hoofdstuk zal inspecteren hoe de verstrekte `Text` component van het KUUROORD aan de AEM `Text` component in kaart wordt gebracht. Er wordt een nieuwe `Image` SPA-component gemaakt die in de SPA kan worden gebruikt en in AEM kan worden geschreven. Uit de dooseigenschappen van de **Container van de Lay-out** en **Redacteur van het Malplaatje** beleid zal ook worden gebruikt om een mening tot stand te brengen die een weinig gevarieerder in verschijning is.

![ de steekproef definitieve van het Hoofdstuk creatie ](./assets/map-components/final-page.png)

## Vereisten

Herzie het vereiste tooling en de instructies voor vestiging a [ lokale ontwikkelomgeving ](overview.md#local-dev-environment).

### De code ophalen

1. Download het beginpunt voor deze zelfstudie via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. Implementeer de codebasis naar een lokale AEM-instantie met Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Als het gebruiken van [ AEM 6.x ](overview.md#compatibility) voeg het `classic` profiel toe:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

U kunt de gebeëindigde code op [ GitHub ](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) altijd bekijken of de code uit controleren plaatselijk door aan de tak `Angular/map-components-solution` te schakelen.

## Toewijzingsmethode

Het basisconcept is een Component van het KUUROORD aan een Component van AEM in kaart te brengen. AEM-componenten, serveronderdelen uitvoeren, inhoud exporteren als onderdeel van de JSON-model-API. De inhoud JSON wordt verbruikt door het KUUROORD, lopend cliënt-kant in browser. Een afbeelding 1:1 tussen de componenten van het KUUROORD en een component van AEM wordt gecreeerd.

![ overzicht op hoog niveau van het in kaart brengen van een Component van AEM aan een Component van Angular ](./assets/map-components/high-level-approach.png)

*overzicht op hoog niveau van het in kaart brengen van een Component van AEM aan een Component van Angular*

## De tekstcomponent controleren

Het [ Archieftype van het Project van AEM ](https://github.com/adobe/aem-project-archetype) verstrekt een `Text` component die aan de component van de Tekst van AEM [ ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) in kaart wordt gebracht. Dit is een voorbeeld van de component van de a **inhoud**, in die zin dat het *inhoud* van AEM teruggeeft.

Laten we eens kijken hoe de component werkt.

### Het JSON-model controleren

1. Alvorens in de code van het KUUROORD te springen, is het belangrijk om het model te begrijpen JSON dat AEM verstrekt. Navigeer aan de [ Bibliotheek van de Component van de Kern ](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) en bekijk de pagina voor de component van de Tekst. De Core Component Library bevat voorbeelden van alle AEM Core Components.
2. Selecteer het **JSON** lusje voor één van de voorbeelden:

   ![ JSON van de Tekst model ](./assets/map-components/text-json.png)

   Er moeten drie eigenschappen worden weergegeven: `text` , `richText` en `:type` .

   `:type` is een gereserveerde eigenschap die de `sling:resourceType` (of het pad) van de AEM-component opsomt. De waarde van `:type` is wat wordt gebruikt om de component van AEM aan de component van SPA in kaart te brengen.

   `text` en `richText` zijn extra eigenschappen die aan de component SPA worden blootgesteld.

### De tekstcomponent controleren

1. Open een nieuwe terminal en navigeer naar de map `ui.frontend` in het project. Looppas `npm install` en dan `npm start` om de **webpack dev server** te beginnen:

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   De `ui.frontend` module is momenteel opstelling om het [ modelJSON ](./integrate-spa.md#mock-json) te gebruiken.

2. U zou een nieuw browser venster moeten zien open aan [ http://localhost:4200/content/wknd-spa-angular/us/en/home.html ](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![ WebPack dev server met mock inhoud ](assets/map-components/initial-start.png)

3. In winde van uw keus open omhoog het Project van AEM voor het KND SPA. Breid de `ui.frontend` module uit en open het dossier **text.component.ts** onder `ui.frontend/src/app/components/text/text.component.ts`:

   ![ Text.js Angular Component Source Code ](assets/map-components/vscode-ide-text-js.png)

4. Het eerste gebied dat moet worden geïnspecteerd, is de `class TextComponent` bij ~line 35:

   ```js
   export class TextComponent {
       @Input() richText: boolean;
       @Input() text: string;
       @Input() itemName: string;
   
       @HostBinding('innerHtml') get content() {
           return this.richText
           ? this.sanitizer.bypassSecurityTrustHtml(this.text)
           : this.text;
       }
       @HostBinding('attr.data-rte-editelement') editAttribute = true;
   
       constructor(private sanitizer: DomSanitizer) {}
   }
   ```

   [@Input () ](https://angular.io/api/core/Input) decorator wordt gebruikt om gebieden te verklaren die de waarden via het in kaart gebrachte voorwerp worden geplaatst JSON, eerder herzien.

   `@HostBinding('innerHtml') get content()` is een methode die de geschreven tekstinhoud weergeeft van de waarde van `this.text` . Als de inhoud tekst met opmaak is (bepaald door de markering `this.richText` ), wordt de ingebouwde beveiliging van Angular omzeild. Angular [ DomSanitizer ](https://angular.io/api/platform-browser/DomSanitizer) wordt gebruikt om &quot;ruwe HTML&quot;te schrobben en de kwetsbaarheid van Scripting van de dwars-Plaats te verhinderen. De methode is gebonden aan de eigenschap `innerHtml` met de decorator [@HostBinding ](https://angular.io/api/core/HostBinding) .

5. Controleer vervolgens de `TextEditConfig` op ~line 24:

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   De bovenstaande code bepaalt wanneer de tijdelijke aanduiding in de AEM-auteursomgeving moet worden weergegeven. Als de `isEmpty` methode **waar** terugkeert dan placeholder wordt teruggegeven.

6. Bekijk ten slotte de `MapTo` oproep op ~line 53:

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **MapTo** wordt verstrekt door de Redacteur JS SDK van AEM SPA (`@adobe/cq-angular-editable-components`). Het pad `wknd-spa-angular/components/text` vertegenwoordigt de `sling:resourceType` van de AEM-component. Dit pad komt overeen met het pad `:type` dat wordt weergegeven door het JSON-model dat u eerder hebt waargenomen. **MapTo** ontleedt de JSON modelreactie en gaat de correcte waarden tot de `@Input()` variabelen van de component van het KUUROORD over.

   U kunt de definitie van de AEM `Text` -component vinden op `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text` .

7. Experimenteer door het {**dossier 0} en.model.json bij `ui.frontend/src/mocks/json/en.model.json` te wijzigen.**

   Bij ~line 62 werkt u de eerste `Text` -waarde bij om de tags **`H1`** en **`u`** te gebruiken:

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   Terugkeer aan browser om de gevolgen te zien die door de **webpack dev server** worden gediend:

   ![ Bijgewerkt model van de Tekst ](assets/map-components/updated-text-model.png)

   Probeer die het `richText` bezit tussen **** van een knevel voorzien **vals** om terug te zien logica in actie.

8. Inspecteer **text.component.html** bij `ui.frontend/src/app/components/text/text.component.html`.

   Dit bestand is leeg omdat de volledige inhoud van de component wordt ingesteld door de eigenschap `innerHTML` .

9. Inspecteer **app.module.ts** bij `ui.frontend/src/app/app.module.ts`.

   ```js
   @NgModule({
   imports: [
       BrowserModule,
       SpaAngularEditableComponentsModule,
       AppRoutingModule
   ],
   providers: [ModelManagerService, { provide: APP_BASE_HREF, useValue: '/' }],
   declarations: [AppComponent, TextComponent, PageComponent, HeaderComponent],
   entryComponents: [TextComponent, PageComponent],
   bootstrap: [AppComponent]
   })
   export class AppModule {}
   ```

   **TextComponent** is niet uitdrukkelijk inbegrepen, maar eerder dynamisch via **AEMResponsiveGridComponent** die door de Redacteur JS SDK van AEM SPA wordt verstrekt. Daarom moet in **app.module.ts** [ entryComponents ](https://angular.io/guide/entry-components) serie worden vermeld.

## De afbeeldingscomponent maken

Daarna, creeer een `Image` component van Angular die aan de component van het Beeld van AEM [ ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) in kaart wordt gebracht. De `Image` component is een ander voorbeeld van a **inhoud** component.

### De JSON inspecteren

Alvorens in de code van het KUUROORD te springen, inspecteer het model JSON dat door AEM wordt verstrekt.

1. Navigeer aan de [ voorbeelden van het Beeld in de bibliotheek van de Component van de Kern ](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![ Component JSON van de Kern van het Beeld ](./assets/map-components/image-json.png)

   Eigenschappen van `src` , `alt` en `title` worden gebruikt om de component SPA `Image` te vullen.

   >[!NOTE]
   >
   > Er zijn andere eigenschappen van het Beeld blootgesteld (`lazyEnabled`, `widths`) die een ontwikkelaar toestaan om een adaptieve en lui-ladende component tot stand te brengen. De component die in dit leerprogramma wordt gebouwd is eenvoudig en **gebruikt** deze geavanceerde eigenschappen niet.

2. Ga terug naar uw IDE en open de `en.model.json` om `ui.frontend/src/mocks/json/en.model.json`. Aangezien dit een netto-nieuwe component voor ons project is, moeten we de Image JSON &quot;modelleren&quot;.

   Voeg bij ~line 70 een JSON-item toe voor het `image` -model (vergeet niet de volgkomma `,` na de tweede `text_386303036` ) en werk de array `:itemsOrder` bij.

   ```json
   ...
   ":items": {
               ...
               "text_386303036": {
                   "text": "<p>A new text component.</p>\r\n",
                   "richText": true,
                   ":type": "wknd-spa-angular/components/text"
                   },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mocks/images/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-angular/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_386303036",
               "image"
           ],
   ```

   Het project omvat een steekproefbeeld bij `/mock-content/adobestock-140634652.jpeg` dat met **webpack dev server** wordt gebruikt.

   U kunt volledige [ en.model.json hier bekijken ](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json).

3. Voeg een stockfoto toe die door de component moet worden weergegeven.

   Creeer een nieuwe omslag genoemd **beelden** onder `ui.frontend/src/mocks`. De download [ adobestock-140634652.jpeg ](assets/map-components/adobestock-140634652.jpeg) en plaatst het in de pas gecreëerde **beelden** omslag. Voel u vrij om desgewenst uw eigen afbeelding te gebruiken.

### De component Image implementeren

1. Stop de **webpack dev server** als begonnen.
2. Maak een nieuwe afbeeldingscomponent door de Angular CLI-opdracht `ng generate component` uit te voeren vanuit de map `ui.frontend` :

   ```shell
   $ ng generate component components/image
   ```

3. In winde, open **image.component.ts** bij `ui.frontend/src/app/components/image/image.component.ts` en werk als volgt bij:

   ```js
   import {Component, Input, OnInit} from '@angular/core';
   import {MapTo} from '@adobe/cq-angular-editable-components';
   
   const ImageEditConfig = {
   emptyLabel: 'Image',
   isEmpty: cqModel =>
       !cqModel || !cqModel.src || cqModel.src.trim().length < 1
   };
   
   @Component({
   selector: 'app-image',
   templateUrl: './image.component.html',
   styleUrls: ['./image.component.scss']
   })
   export class ImageComponent implements OnInit {
   
   @Input() src: string;
   @Input() alt: string;
   @Input() title: string;
   
   constructor() { }
   
   get hasImage() {
       return this.src && this.src.trim().length > 0;
   }
   
   ngOnInit() { }
   }
   
   MapTo('wknd-spa-angular/components/image')(ImageComponent, ImageEditConfig);
   ```

   `ImageEditConfig` is de configuratie waarmee wordt bepaald of de plaatsaanduiding van de auteur in AEM moet worden weergegeven, op basis van het feit of de eigenschap `src` is gevuld.

   `@Input()` van `src` , `alt` en `title` zijn de eigenschappen die zijn toegewezen via de JSON-API.

   `hasImage()` is een methode die bepaalt of de afbeelding moet worden gerenderd.

   `MapTo` wijst de component SPA toe aan de AEM-component in `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image` .

4. Open **image.component.html** en werk het als volgt bij:

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   Dit zal het `<img>` element teruggeven als `hasImage` **waar** terugkeert.

5. Open **image.component.scss** en werk het als volgt bij:

   ```scss
   :host-context {
       display: block;
   }
   
   .image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

   >[!NOTE]
   >
   > De `:host-context` regel is **kritiek** voor de redacteursplaceholder van AEM SPA correct te functioneren. Alle componenten van het KUUROORD die bestemd zijn om in de AEM paginaredacteur te worden ontworpen zullen deze regel bij een minimum vereisen.

6. Open `app.module.ts` en voeg de `ImageComponent` toe aan de array `entryComponents` :

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   Net als bij `TextComponent` wordt `ImageComponent` dynamisch geladen en moet deze worden opgenomen in de array `entryComponents` .

7. Begin **webpack dev server** om `ImageComponent` terug te zien.

   ```shell
   $ npm run start:mock
   ```

   ![ Beeld dat aan het mock ](assets/map-components/image-added-mock.png) wordt toegevoegd

   *Beeld dat aan het KUUUROORD* wordt toegevoegd

   >[!NOTE]
   >
   > **de uitdaging van de Bonus**: Voer een nieuwe methode uit om de waarde van `title` als titel onder het beeld te tonen.

## Beleid in AEM bijwerken

De `ImageComponent` component is slechts zichtbaar in **webpack dev server**. Daarna, stel het bijgewerkte KUUROORD aan AEM op en werk het malplaatjebeleid bij.

1. Stop de **webpack dev server** en van de **wortel** van het project, stel de veranderingen in AEM op gebruikend uw Maven vaardigheden:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Van het scherm van het Begin van AEM navigeert aan **[!UICONTROL Tools]** > **[!UICONTROL Templates]** > **[WKND SPA Angular ](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**.

   Selecteer en geef de **Pagina van het KUUROORD** uit:

   ![ geef het Malplaatje van de Pagina van het KUUROORD ](assets/map-components/edit-spa-page-template.png) uit

3. Selecteer de **Container van de Lay-out** en klik het **beleid** pictogram om het beleid uit te geven:

   ![ Beleid van de Container van de Lay-out ](./assets/map-components/layout-container-policy.png)

4. Onder **Toegestane Componenten** > **WKND SPA Angular - Inhoud** > controleer de **component van het Beeld**:

   ![ Geselecteerde Component van het Beeld ](assets/map-components/check-image-component.png)

   Onder **StandaardComponenten** > **voeg afbeelding** toe en kies het **Beeld - WKND SPA Angular - Inhoud** component:

   ![ plaats standaardcomponenten ](assets/map-components/default-components.png)

   Ga a **mime type** van `image/*` in.

   Klik **Gedaan** om de beleidsupdates te bewaren.

5. In de **Container van de Lay-out** klik het **beleid** pictogram voor de **** component van de Tekst:

   ![ pictogram van het componentenbeleid van de Tekst ](./assets/map-components/edit-text-policy.png)

   Creeer een nieuw beleid genoemd **WKND Tekst van het KUUROORD**. Onder **Insteekmodules** > **Formatterend** > controleer alle dozen om extra het formatteren opties toe te laten:

   ![ laat RTE het Formatteren ](assets/map-components/enable-formatting-rte.png) toe

   Onder **Insteekmodules** > **Stijlen van de Paragraaf** > controleer de doos **alineastijlen** toelaten:

   ![ laat paragraafstijlen ](./assets/map-components/text-policy-enable-paragraphstyles.png) toe

   Klik **Gedaan** om de beleidsupdate te bewaren.

6. Navigeer aan de **Homepage** [ http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html ](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).

   U zou ook de `Text` component moeten kunnen uitgeven en extra paragraafstijlen op **volledig-scherm** wijze toevoegen.

   ![ Volledig scherm Rich Text Editing ](assets/map-components/full-screen-rte.png)

7. U zou ook een beeld van de **Vinder van Activa** moeten kunnen slepen en neerzetten:

   ![ belemmering en het beeld van de Daling ](./assets/map-components/drag-drop-image.gif)

8. Voeg uw eigen beelden via [ AEM Assets ](http://localhost:4502/assets.html/content/dam) toe of installeer de gebeëindigde codebasis voor de standaard [ WKND verwijzingsplaats ](https://github.com/adobe/aem-guides-wknd/releases/latest). De [ WKND verwijzingsplaats ](https://github.com/adobe/aem-guides-wknd/releases/latest) omvat vele beelden die op het KND KUUROORD kunnen worden opnieuw gebruikt. Het pakket kan worden geïnstalleerd gebruikend [ de Manager van het Pakket van AEM ](http://localhost:4502/crx/packmgr/index.jsp).

   ![ Manager van het Pakket installeert wknd.all ](./assets/map-components/package-manager-wknd-all.png)

## De container voor lay-out controleren

De steun voor de **Container van de Lay-out** wordt automatisch verstrekt door de Redacteur SDK van AEM SPA. De **Container van de Lay-out**, zoals die door de naam wordt vermeld, is a **container** component. De componenten van de container zijn componenten die structuren goedkeuren JSON die *andere* componenten vertegenwoordigen en dynamisch hen concretiseren.

Controleer de container voor lay-out verder.

1. In winde open **responsive-grid.component.ts** bij `ui.frontend/src/app/components/responsive-grid`:

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   `AEMResponsiveGridComponent` wordt geïmplementeerd als onderdeel van de AEM SPA Editor SDK en is opgenomen in het project via `import-components` .

2. In browser navigeert aan [ http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![ JSON model API - het Responsieve Net ](./assets/map-components/responsive-grid-modeljson.png)

   De **component van de Container van de Lay-out** heeft a `sling:resourceType` van `wcm/foundation/components/responsivegrid` en door de Redacteur van het KUUROORD erkend gebruikend het `:type` bezit, enkel als `Text` en `Image` componenten.

   De zelfde mogelijkheden om een component opnieuw te rangschikken gebruikend [ Wijze van de Lay-out ](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) zijn beschikbaar met de Redacteur van het KUUROORD.

3. Keer terug naar [ http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html ](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Voeg extra **componenten van het Beeld 0} toe {en probeer re-sizing hen gebruikend de** optie van de Lay-out **:**

   ![ resize beeld gebruikend de wijze van de Lay-out ](./assets/map-components/responsive-grid-layout-change.gif)

4. Heropen het model JSON [ http://localhost:4502/content/wknd-spa-angular/us/en.model.json ](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) en bekijk `columnClassNames` als deel van JSON:

   {de namen van de Klasse van 0} Wolk ](./assets/map-components/responsive-grid-classnames.png)![

   De klassenaam `aem-GridColumn--default--4` geeft aan dat de component 4 kolommen breed moet zijn op basis van een raster van 12 kolommen. Meer details over het [ ontvankelijke net kunnen hier ](https://adobe-marketing-cloud.github.io/aem-responsivegrid/) worden gevonden.

5. Keer terug naar winde en in de `ui.apps` module is er een cliënt-zijbibliotheek die bij `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid` wordt bepaald. Open het bestand `less/grid.less` .

   Dit dossier bepaalt de breekpunten (`default`, `tablet`, en `phone`) die door de **Container van de Lay-out** worden gebruikt. Dit dossier is bedoeld om per projectspecificaties worden aangepast. Momenteel zijn de onderbrekingspunten ingesteld op `1200px` en `650px` .

6. U moet de responsieve mogelijkheden en het bijgewerkte rijke tekstbeleid van de `Text` component kunnen gebruiken om een mening als het volgende te ontwerpen:

   ![ de steekproef definitieve van het Hoofdstuk creatie ](assets/map-components/final-page.png)

## Gefeliciteerd! {#congratulations}

U hebt geleerd hoe u SPA-componenten kunt toewijzen aan AEM Components en een nieuwe `Image` -component hebt geïmplementeerd. U hebt ook een kans om de ontvankelijke mogelijkheden van de **Container van de Lay-out** te onderzoeken.

U kunt de gebeëindigde code op [ GitHub ](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) altijd bekijken of de code uit controleren plaatselijk door aan de tak `Angular/map-components-solution` te schakelen.

### Volgende stappen {#next-steps}

[ Navigatie en het Verpletteren ](navigation-routing.md) - leer hoe de veelvoudige meningen in het KUUROORD door afbeelding aan de Pagina&#39;s van AEM met de Redacteur SDK van het KUUROORD kunnen worden gesteund. De dynamische navigatie wordt uitgevoerd gebruikend de Router van Angular en toegevoegd aan een bestaande component van de Kopbal.

## Bonus - configuraties aan broncontrole blijven {#bonus}

In veel gevallen, vooral aan het begin van een project van AEM is het waardevol om configuraties, zoals malplaatjes en verwant inhoudsbeleid, aan broncontrole voort te zetten. Dit zorgt ervoor dat alle ontwikkelaars tegen de zelfde reeks inhoud en configuraties werken en extra consistentie tussen milieu&#39;s kunnen verzekeren. Wanneer een project een bepaald ontwikkelingsniveau heeft bereikt, kan het beheren van sjablonen worden overgedragen aan een speciale groep van energiegebruikers.

De volgende weinige stappen zullen plaatsvinden gebruikend winde van de Code van Visual Studio en [ Synchronisatie van VSCode AEM ](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) maar zouden het gebruiken van om het even welk hulpmiddel en om het even welke winde kunnen doen die u aan **trek** of **invoert** inhoud van een lokale instantie van AEM hebt gevormd.

1. In winde van de Code van Visual Studio, zorg ervoor dat u {de Synchronisatie van AEM van 0} VSCode **via de uitbreiding van de Marketplace geïnstalleerd hebt:**

   ![ de Synchronisatie van AEM VSCode ](./assets/map-components/vscode-aem-sync.png)

2. Breid **ui.content** module in de ontdekkingsreiziger van het Project uit en navigeer aan `/conf/wknd-spa-angular/settings/wcm/templates`.

3. **Right+Click** de `templates` omslag en selecteer **Invoer van de Server van AEM**:

   ![ VSCode het invoermalplaatje ](assets/map-components/import-aem-servervscode.png)

4. Herhaal de stappen om inhoud in te voeren maar selecteer de **beleid** omslag die bij `/conf/wknd-spa-angular/settings/wcm/policies` wordt gevestigd.

5. Controleer het `filter.xml` -bestand in `ui.content/src/main/content/META-INF/vault/filter.xml` .

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-angular" mode="merge"/>
        <filter root="/content/wknd-spa-angular" mode="merge"/>
        <filter root="/content/dam/wknd-spa-angular" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-angular" mode="merge"/>
    </workspaceFilter>
   ```

   Het bestand `filter.xml` identificeert de paden van knooppunten die met het pakket zijn geïnstalleerd. Let op `mode="merge"` op elk van de filters die aangeeft dat bestaande inhoud niet wordt gewijzigd, alleen nieuwe inhoud wordt toegevoegd. Aangezien de inhoudsauteurs deze wegen kunnen bijwerken, is het belangrijk dat een codeplaatsing **** geen inhoud overschrijft. Zie de [ documentatie FileVault ](https://jackrabbit.apache.org/filevault/filter.html) voor meer details bij het werken met filterelementen.

   Vergelijk `ui.content/src/main/content/META-INF/vault/filter.xml` en `ui.apps/src/main/content/META-INF/vault/filter.xml` om inzicht te krijgen in de verschillende knooppunten die door elke module worden beheerd.
