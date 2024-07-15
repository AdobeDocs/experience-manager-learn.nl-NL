---
title: SPA componenten toewijzen aan AEM componenten | Aan de slag met de AEM SPA Editor en Angular
description: Leer hoe u Angulars aan Adobe Experience Manager-componenten (AEM) toewijst met de AEM SPA Editor JS SDK. Met componenttoewijzing kunnen gebruikers dynamische updates uitvoeren naar SPA componenten in de AEM SPA Editor, net als bij traditionele AEM ontwerpen.
feature: SPA Editor
version: Cloud Service
jira: KT-5311
thumbnail: 5311-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 19a8917c-a1e7-4293-9ce1-9f4c1a565861
duration: 509
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2211'
ht-degree: 0%

---

# SPA componenten toewijzen aan AEM componenten {#map-components}

Leer hoe u Angulars aan Adobe Experience Manager-componenten (AEM) toewijst met de AEM SPA Editor JS SDK. Met componenttoewijzing kunnen gebruikers dynamische updates uitvoeren naar SPA componenten in de AEM SPA Editor, net als bij traditionele AEM ontwerpen.

In dit hoofdstuk wordt dieper ingegaan op de AEM JSON-model-API en wordt uitgelegd hoe de JSON-inhoud die door een AEM wordt aangeboden, automatisch als props in een Angular-component kan worden geïnjecteerd.

## Doelstelling

1. Leer hoe u AEM componenten kunt toewijzen aan SPA.
2. Begrijp het verschil tussen **componenten 0} van de Container {en** Inhoud **componenten.**
3. Maak een nieuwe Angular die aan een bestaande AEM wordt toegewezen.

## Wat u gaat maken

Dit hoofdstuk zal inspecteren hoe de verstrekte `Text` SPA component aan de AEM `Text` component in kaart wordt gebracht. Er wordt een nieuwe SPA `Image` gemaakt die in de SPA kan worden gebruikt en in AEM kan worden ontworpen. Uit de dooseigenschappen van de **Container van de Lay-out** en **Redacteur van het Malplaatje** beleid zal ook worden gebruikt om een mening tot stand te brengen die een weinig gevarieerder in verschijning is.

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

2. Implementeer de basis van de code op een lokale AEM met Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Als het gebruiken van [ AEM 6.x ](overview.md#compatibility) het `classic` profiel toevoegt:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

U kunt de gebeëindigde code op [ GitHub ](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) altijd bekijken of de code uit controleren plaatselijk door aan de tak `Angular/map-components-solution` te schakelen.

## Toewijzingsmethode

Het basisconcept is om een SPA Component aan een AEM Component in kaart te brengen. AEM componenten, voer server-kant in, voer inhoud als deel van JSON model API uit. De JSON-inhoud wordt door de SPA verbruikt en wordt in de browser op de client uitgevoerd. Er wordt een 1:1-toewijzing gemaakt tussen SPA componenten en een AEM component.

![ overzicht op hoog niveau van afbeelding een AEM Component aan een Component van de Angular ](./assets/map-components/high-level-approach.png)

*overzicht op hoog niveau van afbeelding een AEM Component aan een Component van de Angular*

## De tekstcomponent Inspect

Het [ AEM Archetype van het Project ](https://github.com/adobe/aem-project-archetype) verstrekt een `Text` component die aan de AEM [ component van de Tekst ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) in kaart wordt gebracht. Dit is een voorbeeld van de component van de a **inhoud**, in die zin dat het *inhoud* van AEM teruggeeft.

Laten we eens kijken hoe de component werkt.

### Inspect het JSON-model

1. Voordat u in de SPA code springt, is het belangrijk dat u het JSON-model begrijpt dat AEM biedt. Navigeer aan de [ Bibliotheek van de Component van de Kern ](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) en bekijk de pagina voor de component van de Tekst. De Core Component Library bevat voorbeelden van alle AEM Core Components.
2. Selecteer het **JSON** lusje voor één van de voorbeelden:

   ![ JSON van de Tekst model ](./assets/map-components/text-json.png)

   Er moeten drie eigenschappen worden weergegeven: `text` , `richText` en `:type` .

   `:type` is een gereserveerde eigenschap die de `sling:resourceType` (of het pad) van de AEM Component opsomt. De waarde van `:type` is wat wordt gebruikt om de AEM component aan de SPA toe te wijzen.

   `text` en `richText` zijn extra eigenschappen die aan de SPA component worden blootgesteld.

### De component Text Inspect

1. Open een nieuwe terminal en navigeer naar de map `ui.frontend` in het project. Looppas `npm install` en dan `npm start` om de **webpack dev server** te beginnen:

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   De `ui.frontend` module is momenteel opstelling om het [ modelJSON ](./integrate-spa.md#mock-json) te gebruiken.

2. U zou een nieuw browser venster moeten zien open aan [ http://localhost:4200/content/wknd-spa-angular/us/en/home.html ](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![ WebPack dev server met mock inhoud ](assets/map-components/initial-start.png)

3. In winde van uw keus open omhoog het AEM Project voor de SPA WKND. Breid de `ui.frontend` module uit en open het dossier **text.component.ts** onder `ui.frontend/src/app/components/text/text.component.ts`:

   ![ de Code van Source van de Component van de Angular Text.js ](assets/map-components/vscode-ide-text-js.png)

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

   `@HostBinding('innerHtml') get content()` is een methode die de geschreven tekstinhoud weergeeft van de waarde van `this.text` . Als de inhoud tekst met opmaak is (bepaald door de markering `this.richText` ), wordt de ingebouwde beveiliging van de Angular omzeild. [ DomSanitizer van angular ](https://angular.io/api/platform-browser/DomSanitizer) wordt gebruikt om de ruwe HTML &quot;te schrobben&quot;en de kwetsbaarheid van Scripting van de dwars-Plaats te verhinderen. De methode is gebonden aan de eigenschap `innerHtml` met de decorator [@HostBinding ](https://angular.io/api/core/HostBinding) .

5. Controleer vervolgens de `TextEditConfig` op ~line 24:

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   De bovenstaande code bepaalt wanneer de tijdelijke aanduiding in de AEM auteursomgeving moet worden weergegeven. Als de `isEmpty` methode **waar** terugkeert dan placeholder wordt teruggegeven.

6. Bekijk ten slotte de `MapTo` oproep op ~line 53:

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **MapTo** wordt verstrekt door de AEM SPA Redacteur JS SDK (`@adobe/cq-angular-editable-components`). Het pad `wknd-spa-angular/components/text` vertegenwoordigt de `sling:resourceType` van de AEM. Dit pad komt overeen met het pad `:type` dat wordt weergegeven door het JSON-model dat u eerder hebt waargenomen. **MapTo** ontleedt de JSON modelreactie en gaat de correcte waarden tot de `@Input()` variabelen van de SPA over component.

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

8. Inspect **text.component.html** bij `ui.frontend/src/app/components/text/text.component.html`.

   Dit bestand is leeg omdat de volledige inhoud van de component wordt ingesteld door de eigenschap `innerHTML` .

9. Inspect **app.module.ts** bij `ui.frontend/src/app/app.module.ts`.

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

   **TextComponent** is niet uitdrukkelijk inbegrepen, maar eerder dynamisch via **AEMResponsiveGridComponent** die door AEM Redacteur JS SDK SPA wordt verstrekt. Daarom moet in **app.module.ts** [ entryComponents ](https://angular.io/guide/entry-components) serie worden vermeld.

## De afbeeldingscomponent maken

Daarna, creeer een `Image` component van de Angular die aan de AEM [ component van het Beeld ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) in kaart wordt gebracht. De `Image` component is een ander voorbeeld van a **inhoud** component.

### Inspect the JSON

Voordat u in de SPA code gaat springen, moet u het JSON-model controleren dat AEM biedt.

1. Navigeer aan de [ voorbeelden van het Beeld in de bibliotheek van de Component van de Kern ](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![ Component JSON van de Kern van het Beeld ](./assets/map-components/image-json.png)

   Eigenschappen van `src` , `alt` en `title` worden gebruikt om de SPA `Image` -component te vullen.

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
2. Maak een nieuwe component Image door de opdracht Angular CLI `ng generate component` uit te voeren vanuit de map `ui.frontend` :

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

   `ImageEditConfig` is de configuratie waarmee wordt bepaald of de plaatsaanduiding van de auteur AEM moet worden weergegeven, op basis van het feit of de eigenschap `src` is gevuld.

   `@Input()` van `src` , `alt` en `title` zijn de eigenschappen die zijn toegewezen via de JSON-API.

   `hasImage()` is een methode die bepaalt of de afbeelding moet worden gerenderd.

   `MapTo` wijst de SPA component toe aan de AEM component die zich op `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image` bevindt.

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
   > De `:host-context` regel is **kritiek** voor AEM SPA redacteursplaceholder correct te functioneren. Alle SPA componenten die bedoeld zijn om in de AEM paginaredacteur te worden ontworpen zullen deze regel bij een minimum vereisen.

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

   *Beeld dat aan de SPA* wordt toegevoegd

   >[!NOTE]
   >
   > **de uitdaging van de Bonus**: Voer een nieuwe methode uit om de waarde van `title` als titel onder het beeld te tonen.

## Beleid bijwerken in AEM

De `ImageComponent` component is slechts zichtbaar in **webpack dev server**. Implementeer vervolgens de bijgewerkte SPA om het sjabloonbeleid te AEM en bij te werken.

1. Stop de **webpack dev server** en van de **wortel** van het project, stel de veranderingen in AEM gebruikend uw Maven vaardigheden op:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Van het AEM scherm van het Begin navigeert aan **[!UICONTROL Tools]** > **[!UICONTROL Templates]** > **[Angular van de SPA WKND ](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**.

   Selecteer en geef **SPA Pagina** uit:

   ![ geef SPA het Malplaatje van de Pagina uit ](assets/map-components/edit-spa-page-template.png)

3. Selecteer de **Container van de Lay-out** en klik het **beleid** pictogram om het beleid uit te geven:

   ![ Beleid van de Container van de Lay-out ](./assets/map-components/layout-container-policy.png)

4. Onder **Toegestane Componenten** > **WKND SPA Angular - Inhoud** > controleer de **component van het Beeld**:

   ![ Geselecteerde Component van het Beeld ](assets/map-components/check-image-component.png)

   Onder **StandaardComponenten** > **voeg afbeelding** toe en kies het **Beeld - de Angular van de SPA van WKND - Inhoud** component:

   ![ plaats standaardcomponenten ](assets/map-components/default-components.png)

   Ga a **mime type** van `image/*` in.

   Klik **Gedaan** om de beleidsupdates te bewaren.

5. In de **Container van de Lay-out** klik het **beleid** pictogram voor de **** component van de Tekst:

   ![ pictogram van het componentenbeleid van de Tekst ](./assets/map-components/edit-text-policy.png)

   Creeer een nieuw beleid genoemd **WKND SPA Tekst**. Onder **Insteekmodules** > **Formatterend** > controleer alle dozen om extra het formatteren opties toe te laten:

   ![ laat RTE het Formatteren ](assets/map-components/enable-formatting-rte.png) toe

   Onder **Insteekmodules** > **Stijlen van de Paragraaf** > controleer de doos **alineastijlen** toelaten:

   ![ laat paragraafstijlen ](./assets/map-components/text-policy-enable-paragraphstyles.png) toe

   Klik **Gedaan** om de beleidsupdate te bewaren.

6. Navigeer aan de **Homepage** [ http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html ](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).

   U zou ook de `Text` component moeten kunnen uitgeven en extra paragraafstijlen op **volledig-scherm** wijze toevoegen.

   ![ Volledig scherm Rich Text Editing ](assets/map-components/full-screen-rte.png)

7. U zou ook een beeld van de **Vinder van Activa** moeten kunnen slepen en neerzetten:

   ![ belemmering en het beeld van de Daling ](./assets/map-components/drag-drop-image.gif)

8. Voeg uw eigen beelden via [ AEM Assets ](http://localhost:4502/assets.html/content/dam) toe of installeer de gebeëindigde codebasis voor de standaard [ WKND verwijzingsplaats ](https://github.com/adobe/aem-guides-wknd/releases/latest). De [ WKND verwijzingsplaats ](https://github.com/adobe/aem-guides-wknd/releases/latest) omvat vele beelden die op de SPA kunnen worden opnieuw gebruikt WKND. Het pakket kan worden geïnstalleerd gebruikend [ AEM de Manager van het Pakket ](http://localhost:4502/crx/packmgr/index.jsp).

   ![ Manager van het Pakket installeert wknd.all ](./assets/map-components/package-manager-wknd-all.png)

## Inspect the Layout Container

De steun voor de **Container van de Lay-out** wordt automatisch verstrekt door de AEM SPA Redacteur SDK. De **Container van de Lay-out**, zoals die door de naam wordt vermeld, is a **container** component. De componenten van de container zijn componenten die structuren goedkeuren JSON die *andere* componenten vertegenwoordigen en dynamisch hen concretiseren.

Controleer de container voor lay-out verder.

1. In winde open **responsive-grid.component.ts** bij `ui.frontend/src/app/components/responsive-grid`:

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   `AEMResponsiveGridComponent` wordt geïmplementeerd als onderdeel van de AEM SPA Editor SDK en wordt via `import-components` opgenomen in het project.

2. In browser navigeert aan [ http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![ JSON model API - het Responsieve Net ](./assets/map-components/responsive-grid-modeljson.png)

   De **component van de Container van de Lay-out** heeft a `sling:resourceType` van `wcm/foundation/components/responsivegrid` en door de SPARedacteur gebruikend het `:type` bezit, enkel als `Text` en `Image` componenten erkend.

   De zelfde mogelijkheden om een component opnieuw te rangschikken gebruikend [ Wijze van de Lay-out ](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) zijn beschikbaar met de SPARedacteur.

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

U hebt geleerd hoe u SPA componenten kunt toewijzen aan AEM Componenten en hoe u een nieuwe `Image` -component hebt geïmplementeerd. U hebt ook een kans om de ontvankelijke mogelijkheden van de **Container van de Lay-out** te onderzoeken.

U kunt de gebeëindigde code op [ GitHub ](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) altijd bekijken of de code uit controleren plaatselijk door aan de tak `Angular/map-components-solution` te schakelen.

### Volgende stappen {#next-steps}

[ Navigatie en het Verpletteren ](navigation-routing.md) - leer hoe de veelvoudige meningen in de SPA door afbeelding aan AEM Pagina&#39;s met SPA Redacteur SDK kunnen worden gesteund. De dynamische navigatie wordt uitgevoerd gebruikend de Router van de Angular en toegevoegd aan een bestaande component van de Kopbal.

## Bonus - configuraties aan broncontrole blijven {#bonus}

In veel gevallen, vooral aan het begin van een AEM project is het waardevol om configuraties, zoals malplaatjes en verwant inhoudsbeleid, aan broncontrole voort te zetten. Dit zorgt ervoor dat alle ontwikkelaars tegen de zelfde reeks inhoud en configuraties werken en extra consistentie tussen milieu&#39;s kunnen verzekeren. Wanneer een project een bepaald ontwikkelingsniveau heeft bereikt, kan het beheren van sjablonen worden overgedragen aan een speciale groep van energiegebruikers.

De volgende paar stappen zullen plaatsvinden gebruikend winde van de Code van Visual Studio en [ VSCode AEM Synchronisatie ](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) maar zouden het gebruiken van om het even welk hulpmiddel en om het even welke winde kunnen doen die u aan **trek** of **invoert** inhoud van een lokale instantie van AEM hebt gevormd.

1. In winde van de Code van Visual Studio, zorg ervoor dat u **VSCode AEM Synchronisatie** geïnstalleerd via de uitbreiding van de Marketplace hebt:

   ![ VSCode AEM Synchronisatie ](./assets/map-components/vscode-aem-sync.png)

2. Breid **ui.content** module in de ontdekkingsreiziger van het Project uit en navigeer aan `/conf/wknd-spa-angular/settings/wcm/templates`.

3. **Right+Click** de `templates` omslag en selecteer **Invoer van AEM Server**:

   ![ VSCode het invoermalplaatje ](assets/map-components/import-aem-servervscode.png)

4. Herhaal de stappen om inhoud in te voeren maar selecteer de **beleid** omslag die bij `/conf/wknd-spa-angular/settings/wcm/policies` wordt gevestigd.

5. Inspect het `filter.xml` -bestand op `ui.content/src/main/content/META-INF/vault/filter.xml` .

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
