---
title: Navigatie en routering toevoegen | Aan de slag met de AEM SPA Editor en Angular
description: Leer hoe meerdere weergaven in de SPA worden ondersteund met AEM Pagina's en de SPA Editor SDK. Dynamische navigatie wordt uitgevoerd gebruikend de routes van de Angular en toegevoegd aan een bestaande component van de Kopbal.
sub-product: sites
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5312
thumbnail: 5312-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: bf9ab30f57faa23721d7d27b837d8e0f0e8cf4f1
workflow-type: tm+mt
source-wordcount: '2646'
ht-degree: 0%

---


# Navigatie en routering toevoegen {#navigation-routing}

Leer hoe meerdere weergaven in de SPA worden ondersteund met AEM Pagina&#39;s en de SPA Editor SDK. Dynamische navigatie wordt uitgevoerd gebruikend de routes van de Angular en toegevoegd aan een bestaande component van de Kopbal.

## Doelstelling

1. Begrijp het SPA model verpletterend opties beschikbaar wanneer het gebruiken van de SPARedacteur.
2. Leer om [Angular te gebruiken die ](https://angular.io/guide/router) verplettert om tussen verschillende meningen van de SPA te navigeren.
3. Voer een dynamische navigatie uit die door de AEM paginahiërarchie wordt aangedreven.

## Wat u gaat maken

In dit hoofdstuk wordt een navigatiemenu toegevoegd aan een bestaande `Header`-component. Het navigatiemenu wordt gedreven door de AEM paginahiërarchie en gebruikt het model JSON dat door [de Component van de Kern van de Navigatie](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) wordt verstrekt.

![Navigatie geïmplementeerd](assets/navigation-routing/final-navigation-implemented.gif)

## Vereisten

Controleer de vereiste gereedschappen en instructies voor het instellen van een [lokale ontwikkelomgeving](overview.md#local-dev-environment).

### De code ophalen

1. Download het beginpunt voor deze zelfstudie via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/navigation-routing-start
   ```

2. Implementeer de basis van de code op een lokale AEM met Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Als u [AEM 6.x](overview.md#compatibility) gebruikt, voegt u het profiel `classic` toe:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installeer het voltooide pakket voor de traditionele [WKND-referentiesite](https://github.com/adobe/aem-guides-wknd/releases/latest). De afbeeldingen die worden geleverd door [WKND-referentiesite](https://github.com/adobe/aem-guides-wknd/releases/latest) worden opnieuw gebruikt op de WKND-SPA. Het pakket kan worden geïnstalleerd met [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Pakketbeheer installeren wknd.all](./assets/map-components/package-manager-wknd-all.png)

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) altijd bekijken of de code plaatselijk controleren door aan de tak `Angular/navigation-routing-solution` te schakelen.

## Inspect HeaderComponent-updates {#inspect-header}

In vorige hoofdstukken werd de `HeaderComponent`-component toegevoegd als een zuivere Angular die via `app.component.html` werd opgenomen. In dit hoofdstuk wordt de `HeaderComponent`-component verwijderd uit de app en toegevoegd via [Sjablooneditor](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html). Hierdoor kunnen gebruikers het navigatiemenu van de `HeaderComponent` vanuit AEM configureren.

>[!NOTE]
>
> Er zijn al verschillende CSS- en JavaScript-updates aangebracht in de codebasis om dit hoofdstuk te starten. Om zich op kernconcepten te concentreren, niet **wordt al** van de codeveranderingen besproken. U kunt de volledige veranderingen [hier](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start) bekijken.

1. In winde van uw keus opent het SPA starterproject voor dit hoofdstuk.
2. Onder de `ui.frontend` module inspecteert u het bestand `header.component.ts` op: `ui.frontend/src/app/components/header/header.component.ts`.

   Er zijn verschillende updates uitgevoerd, waaronder de toevoeging van een `HeaderEditConfig` en een `MapTo` om het mogelijk te maken dat de component wordt toegewezen aan een AEM `wknd-spa-angular/components/header`.

   ```js
   /* header.component.ts */
   ...
   const HeaderEditConfig = {
       ...
   };
   
   @Component({
   selector: 'app-header',
   templateUrl: './header.component.html',
   styleUrls: ['./header.component.scss']
   })
   export class HeaderComponent implements OnInit {
   @Input() items: object[];
       ...
   }
   ...
   MapTo('wknd-spa-angular/components/header')(withRouter(Header), HeaderEditConfig);
   ```

   Noteer de `@Input()`-annotatie voor `items`. `items` bevat een array van navigatieobjecten die vanuit AEM worden doorgegeven.

3. Controleer in de module `ui.apps` de componentdefinitie van de AEM `Header` component: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   De AEM `Header` component erft alle functionaliteit van [Navigation Core Component](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) via het `sling:resourceSuperType` bezit.

## Voeg de HeaderComponent aan het SPA toe malplaatje {#add-header-template}

1. Open browser en login aan AEM, [http://localhost:4502/](http://localhost:4502/). De begincodebasis zou reeds moeten worden opgesteld.
2. Navigeer naar **[!UICONTROL SPA Page Template]**: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Selecteer de buitenste **[!UICONTROL Root Layout Container]** en klik op het **[!UICONTROL Policy]**-pictogram. Wees voorzichtig **en niet** om **[!UICONTROL Layout Container]** niet-vergrendeld te selecteren voor ontwerpen.

   ![Selecteer het beleidspictogram voor de container van de hoofdlayout](assets/navigation-routing/root-layout-container-policy.png)

4. Kopieer het huidige beleid en maak een nieuw beleid met de naam **[!UICONTROL SPA Structure]**:

   ![SPA structuurbeleid](assets/map-components/spa-policy-update.png)

   Selecteer onder **[!UICONTROL Allowed Components]** > **[!UICONTROL General]** de **[!UICONTROL Layout Container]**-component.

   Selecteer onder **[!UICONTROL Allowed Components]** > **[!UICONTROL WKND SPA ANGULAR - STRUCTURE]** de **[!UICONTROL Header]**-component:

   ![Koptekstcomponent selecteren](assets/map-components/select-header-component.png)

   Selecteer onder **[!UICONTROL Allowed Components]** > **[!UICONTROL WKND SPA ANGULAR - Content]** de componenten **[!UICONTROL Image]** en **[!UICONTROL Text]**. Er moeten in totaal vier componenten zijn geselecteerd.

   Klik **[!UICONTROL Done]** om de veranderingen te bewaren.

5. **De pagina** vernieuwen. Voeg de **[!UICONTROL Header]** component boven niet-vergrendelde **[!UICONTROL Layout Container]** toe:

   ![component Header toevoegen aan sjabloon](./assets/navigation-routing/add-header-component.gif)

6. Selecteer de **[!UICONTROL Header]** component en klik zijn **Beleid** pictogram om het beleid uit te geven.

   ![Klikken op koptekstbeleid](assets/navigation-routing/header-policy-icon.png)

7. Maak een nieuw beleid met een **[!UICONTROL Policy Title]** van **&quot;WKND SPA Koptekst&quot;**.

   Onder **[!UICONTROL Properties]**:

   * Stel **[!UICONTROL Navigation Root]** in op `/content/wknd-spa-angular/us/en`.
   * Stel **[!UICONTROL Exclude Root Levels]** in op **1**.
   * Schakel **[!UICONTROL Collect al child pages]** uit.
   * Stel **[!UICONTROL Navigation Structure Depth]** in op **3**.

   ![Koptekstbeleid configureren](assets/navigation-routing/header-policy.png)

   Hierdoor worden de navigatieniveaus 2 diep onder `/content/wknd-spa-angular/us/en` verzameld.

8. Nadat u de wijzigingen hebt opgeslagen, ziet u de gevulde `Header` als onderdeel van de sjabloon:

   ![Bevolkte koptekstcomponent](assets/navigation-routing/populated-header.png)

## Onderliggende pagina&#39;s maken

Maak vervolgens aanvullende pagina&#39;s in AEM die als de verschillende weergaven in de SPA dienen. We zullen ook de hiërarchische structuur van het JSON-model dat door AEM wordt aangeboden, controleren.

1. Navigeer naar de **Sites**-console: [http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home). Selecteer **WKND SPA Angular startpagina** en klik **[!UICONTROL Create]** > **[!UICONTROL Page]**:

   ![Nieuwe pagina maken](assets/navigation-routing/create-new-page.png)

2. Selecteer **[!UICONTROL SPA Page]** onder **[!UICONTROL Template]**. Voer onder **[!UICONTROL Properties]** **&quot;Page 1&quot;** voor **[!UICONTROL Title]** en **&quot;page-1&quot;** in als naam.

   ![De eigenschappen voor de eerste pagina invoeren](assets/navigation-routing/initial-page-properties.png)

   Klik op **[!UICONTROL Create]** en klik in het dialoogvenster op **[!UICONTROL Open]** om de pagina te openen in de AEM SPA Editor.

3. Voeg een nieuwe **[!UICONTROL Text]** component aan belangrijkste **[!UICONTROL Layout Container]** toe. Bewerk de component en voer de tekst in: **&quot;Pagina 1&quot;** die RTE en het **H1** element gebruiken (u zult volledig-schermwijze moeten ingaan om de paragraafelementen te veranderen)

   ![Voorbeeld van inhoudspagina 1](assets/navigation-routing/page-1-sample-content.png)

   Voel u vrij om extra inhoud toe te voegen, zoals een afbeelding.

4. Ga terug naar de AEM Sites-console en herhaal de bovenstaande stappen, waardoor een tweede pagina met de naam **&quot;Page 2&quot;** wordt gemaakt als een verwant van **Pagina 1**. Voeg inhoud aan **Pagina 2** toe zodat het gemakkelijk wordt geïdentificeerd.
5. Maak ten slotte een derde pagina, **&quot;Pagina 3&quot;** maar als **child** van **Pagina 2**. Na voltooiing zou de plaatshiërarchie als het volgende moeten kijken:

   ![Voorbeeld van sitehiërarchie](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. Open op een nieuw tabblad de API van het JSON-model die wordt geleverd door AEM: [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Deze JSON-inhoud wordt opgevraagd wanneer de SPA voor het eerst wordt geladen. De buitenste structuur ziet er als volgt uit:

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-angular/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-angular/us/en",
   ":children": {
       "/content/wknd-spa-angular/us/en/home": {},
       "/content/wknd-spa-angular/us/en/home/page-1": {},
       "/content/wknd-spa-angular/us/en/home/page-2": {},
       "/content/wknd-spa-angular/us/en/home/page-2/page-3": {}
       }
   }
   ```

   Onder `:children` ziet u een vermelding voor elk van de gemaakte pagina&#39;s. De inhoud voor alle pagina&#39;s staat in dit eerste JSON-verzoek. Zodra, wordt het navigatie verpletteren uitgevoerd, zullen de verdere meningen van de SPA snel worden geladen, aangezien de inhoud reeds beschikbare cliënt-kant is.

   Het is niet verstandig om **ALL** van de inhoud van een SPA in het eerste JSON-verzoek te laden, omdat dit de eerste paginalading zou vertragen. Vervolgens kunt u bekijken hoe de hiërarchische diepte van pagina&#39;s wordt verzameld.

7. Navigeer naar de sjabloon **SPA Root** op: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html).

   Klik op **[!UICONTROL Page properties menu]** > **[!UICONTROL Page Policy]**:

   ![Het paginabeleid voor SPA basispagina openen](assets/navigation-routing/open-page-policy.png)

8. De sjabloon **SPA Root** heeft een extra **[!UICONTROL Hierarchical Structure]** tab om de verzamelde JSON-inhoud te beheren. Met **[!UICONTROL Structure Depth]** bepaalt u hoe diep in de sitehiërarchie onderliggende pagina&#39;s onder de **root** worden verzameld. Met het veld **[!UICONTROL Structure Patterns]** kunt u ook extra pagina&#39;s filteren op basis van een reguliere expressie.

   Werk **[!UICONTROL Structure Depth]** aan **&quot;2&quot;** bij:

   ![Structuurdiepte bijwerken](assets/navigation-routing/update-structure-depth.png)

   Klik **[!UICONTROL Done]** om de veranderingen in het beleid te bewaren.

9. Open het JSON-model opnieuw [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-angular/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-angular/us/en",
   ":children": {
       "/content/wknd-spa-angular/us/en/home": {},
       "/content/wknd-spa-angular/us/en/home/page-1": {},
       "/content/wknd-spa-angular/us/en/home/page-2": {}
       }
   }
   ```

   De weg **Pagina 3** is verwijderd: `/content/wknd-spa-angular/us/en/home/page-2/page-3` uit het oorspronkelijke JSON-model.

   Later zullen we zien hoe de AEM SPA Editor SDK dynamisch aanvullende inhoud kan laden.

## Navigatie implementeren

Implementeer vervolgens het navigatiemenu met een nieuwe `NavigationComponent`. We kunnen de code rechtstreeks toevoegen in `header.component.html`, maar het is beter grote componenten te voorkomen. In plaats daarvan implementeert u een `NavigationComponent` die later mogelijk opnieuw kan worden gebruikt.

1. Bekijk de JSON die door de AEM `Header`-component op [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) wordt weergegeven:

   ```json
   ...
   "header": {
       "items": [
       {
       "level": 0,
       "active": true,
       "path": "/content/wknd-spa-angular/us/en/home",
       "description": null,
       "url": "/content/wknd-spa-angular/us/en/home.html",
       "lastModified": 1589062597083,
       "title": "WKND SPA Angular Home Page",
       "children": [
               {
               "children": [],
               "level": 1,
               "active": false,
               "path": "/content/wknd-spa-angular/us/en/home/page-1",
               "description": null,
               "url": "/content/wknd-spa-angular/us/en/home/page-1.html",
               "lastModified": 1589429385100,
               "title": "Page 1"
               },
               {
               "level": 1,
               "active": true,
               "path": "/content/wknd-spa-angular/us/en/home/page-2",
               "description": null,
               "url": "/content/wknd-spa-angular/us/en/home/page-2.html",
               "lastModified": 1589429603507,
               "title": "Page 2",
               "children": [
                   {
                   "children": [],
                   "level": 2,
                   "active": false,
                   "path": "/content/wknd-spa-angular/us/en/home/page-2/page-3",
                   "description": null,
                   "url": "/content/wknd-spa-angular/us/en/home/page-2/page-3.html",
                   "lastModified": 1589430413831,
                   "title": "Page 3"
                   }
               ],
               }
           ]
           }
       ],
   ":type": "wknd-spa-angular/components/header"
   ```

   De hiërarchische aard van de AEM pagina&#39;s wordt gemodelleerd in JSON die kan worden gebruikt om een navigatiemenu te bevolken. De `Header`-component overerft alle functionaliteit van de [Navigation Core Component](https://www.aemcomponents.dev/content/core-components-examples/library/templating/navigation.html) en de inhoud die via JSON beschikbaar wordt gemaakt, wordt automatisch toegewezen aan de Angular `@Input`-annotatie.

2. Open een nieuw eindvenster en navigeer aan de `ui.frontend` omslag van het SPA project. Creeer nieuw `NavigationComponent` gebruikend het hulpmiddel CLI van de Angular:

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. Maak vervolgens een klasse met de naam `NavigationLink` met de Angular CLI in de nieuwe map `components/navigation`:

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. Keer terug naar winde van uw keus en open het dossier bij `navigation-link.ts` om `/src/app/components/navigation/navigation-link.ts`.

   ![Het bestand navigation-link.ts openen](assets/navigation-routing/ide-navigation-link-file.png)

5. Vul `navigation-link.ts` met het volgende:

   ```js
   export class NavigationLink {
   
       title: string;
       path: string;
       url: string;
       level: number;
       children: NavigationLink[];
       active: boolean;
   
       constructor(data) {
           this.path = data.path;
           this.title = data.title;
           this.url = data.url;
           this.level = data.level;
           this.active = data.active;
           this.children = data.children.map( item => {
               return new NavigationLink(item);
           });
       }
   }
   ```

   Dit is een eenvoudige klasse die een afzonderlijke navigatiekoppeling vertegenwoordigt. In de klasseconstructor verwachten we dat `data` het JSON-object is dat vanuit AEM wordt doorgegeven. Deze klasse wordt gebruikt binnen zowel `NavigationComponent` als `HeaderComponent` om de navigatiestructuur gemakkelijk te vullen.

   Er wordt geen gegevenstransformatie uitgevoerd. Deze klasse wordt vooral gemaakt om het JSON-model sterk te typen. `this.children` wordt getypt als `NavigationLink[]` en dat de aannemer recursief nieuwe `NavigationLink` voorwerpen voor elk van de punten in `children` serie leidt. Rappel dat JSON model voor `Header` hiërarchisch is.

6. Open het bestand `navigation-link.spec.ts`. Dit is het testbestand voor de klasse `NavigationLink`. Werk het bij met het volgende:

   ```js
   import { NavigationLink } from './navigation-link';
   
   describe('NavigationLink', () => {
       it('should create an instance', () => {
           const data = {
               children: [],
               level: 1,
               active: false,
               path: '/content/wknd-spa-angular/us/en/home/page-1',
               description: null,
               url: '/content/wknd-spa-angular/us/en/home/page-1.html',
               lastModified: 1589429385100,
               title: 'Page 1'
           };
           expect(new NavigationLink(data)).toBeTruthy();
       });
   });
   ```

   Merk op dat `const data` het zelfde model volgt JSON dat vroeger voor één enkele verbinding wordt geïnspecteerd. Dit is verre van een robuuste eenheidstest, nochtans zou het moeten volstaan om de aannemer van `NavigationLink` te testen.

7. Open het bestand `navigation.component.ts`. Werk het bij met het volgende:

   ```js
   import { Component, OnInit, Input } from '@angular/core';
   import { NavigationLink } from './navigation-link';
   
   @Component({
   selector: 'app-navigation',
   templateUrl: './navigation.component.html',
   styleUrls: ['./navigation.component.scss']
   })
   export class NavigationComponent implements OnInit {
   
       @Input() items: object[];
   
       constructor() { }
   
       get navigationLinks(): NavigationLink[] {
   
           if (this.items && this.items.length > 0) {
               return this.items.map(item => {
                   return new NavigationLink(item);
               });
           }
   
           return null;
       }
   
       ngOnInit() {}
   
   }
   ```

   `NavigationComponent` verwacht een  `object[]` genoemde  `items` die het model JSON van AEM is. Deze klasse stelt één enkele methode `get navigationLinks()` bloot die een serie van `NavigationLink` voorwerpen terugkeert.

8. Open het bestand `navigation.component.html` en werk het als volgt bij:

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   Dit produceert een aanvankelijke `<ul>` en roept `get navigationLinks()` methode van `navigation.component.ts`. Een `<ng-container>` wordt gebruikt om een vraag aan een malplaatje te maken genoemd `recursiveListTmpl` en geeft het `navigationLinks` als variabele genoemd `links` door.

   Voeg de volgende `recursiveListTmpl` toe:

   ```html
   <ng-template #recursiveListTmpl let-links="links">
       <li *ngFor="let link of links" class="{{'navigation__item navigation__item--' + link.level}}">
           <a [routerLink]="link.url" class="navigation__item-link" [title]="link.title" [attr.aria-current]="link.active">
               {{link.title}}
           </a>
           <ul *ngIf="link.children && link.children.length > 0">
               <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: link.children }"></ng-container>
           </ul>
       </li>
   </ng-template>
   ```

   Hier wordt de rest van de rendering voor de navigatiekoppeling geïmplementeerd. De variabele `link` is van het type `NavigationLink` en alle methoden/eigenschappen die door die klasse zijn gemaakt, zijn beschikbaar. [`[routerLink]`](https://angular.io/api/router/RouterLink) wordt gebruikt in plaats van het normale  `href` kenmerk. Hierdoor kunnen we koppelingen maken naar specifieke routes in de app, zonder dat de volledige pagina wordt vernieuwd.

   Het recursieve gedeelte van de navigatie wordt ook geïmplementeerd door een andere `<ul>` te maken als de huidige `link` een niet-lege `children` array heeft.

9. `navigation.component.spec.ts` bijwerken om ondersteuning voor `RouterTestingModule` toe te voegen:

   ```diff
    ...
   + import { RouterTestingModule } from '@angular/router/testing';
    ...
    beforeEach(async(() => {
       TestBed.configureTestingModule({
   +   imports: [ RouterTestingModule ],
       declarations: [ NavigationComponent ]
       })
       .compileComponents();
    }));
    ...
   ```

   Het toevoegen van `RouterTestingModule` wordt vereist omdat de component `[routerLink]` gebruikt.

10. Werk `navigation.component.scss` bij om enkele basisstijlen toe te voegen aan `NavigationComponent`:

   ```scss
   @import "~src/styles/variables";
   
   $link-color: $black;
   $link-hover-color: $white;
   $link-background: $black;
   
   :host-context {
       display: block;
       width: 100%;
   }
   
   .navigation__item {
       list-style: none;
   }
   
   .navigation__item-link {
       color: $link-color;
       font-size: $font-size-large;
       text-transform: uppercase;
       padding: $gutter-padding;
       display: flex;
       border-bottom: 1px solid $gray;
   
       &:hover {
           background: $link-background;
           color: $link-hover-color;
       }
   
   }
   ```

## De koptekstcomponent bijwerken

Nu `NavigationComponent` is geïmplementeerd, moet `HeaderComponent` worden bijgewerkt om ernaar te verwijzen.

1. Open een terminal en navigeer naar de map `ui.frontend` in het SPA project. Start de **webpack dev-server**:

   ```shell
   $ npm start
   ```

2. Open een browsertabblad en navigeer naar [http://localhost:4200/](http://localhost:4200/).

   De **webpack dev-server** moet zo worden geconfigureerd dat het JSON-model wordt gepropageerd vanuit een lokale instantie van AEM (`ui.frontend/proxy.conf.json`). Op deze manier kunnen we rechtstreeks code toevoegen aan de inhoud die in AEM van eerder in de zelfstudie is gemaakt.

   ![menuschakeloptie werken](./assets/navigation-routing/nav-toggle-static.gif)

   De `HeaderComponent` heeft momenteel de reeds uitgevoerde functionaliteit van de menuknevel. Voeg vervolgens de navigatiecomponent toe.

3. Keer terug naar winde van uw keus, en open het dossier `header.component.ts` om `ui.frontend/src/app/components/header/header.component.ts`.
4. Werk de `setHomePage()` methode bij om het hard-gecodeerde Koord te verwijderen en de dynamische die steunen te gebruiken binnen door de AEM worden overgegaan component:

   ```js
   /* header.component.ts */
   import { NavigationLink } from '../navigation/navigation-link';
   ...
    setHomePage() {
       if (this.hasNavigation) {
           const rootNavigationLink: NavigationLink = new NavigationLink(this.items[0]);
           this.isHome = rootNavigationLink.path === this.route.snapshot.data.path;
           this.homePageUrl = rootNavigationLink.url;
       }
   }
   ...
   ```

   Er wordt een nieuwe instantie van `NavigationLink` gemaakt op basis van `items[0]`, de basis van het JSON-navigatiemodel dat vanuit AEM wordt doorgegeven. `this.route.snapshot.data.path` keert de weg van de huidige route van de Angular terug. Deze waarde wordt gebruikt om te bepalen als de huidige route **Homepage** is. `this.homePageUrl` wordt gebruikt om de ankerkoppeling in het  **logo** te vullen.

5. Open `header.component.html` en vervang de statische tijdelijke aanduiding voor de navigatie door een verwijzing naar het nieuwe `NavigationComponent`:

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` geeft het kenmerk de navigatie door  `@Input() items` van de  `HeaderComponent` naar de  `NavigationComponent` locatie waar de navigatie wordt opgebouwd.

6. Open `header.component.spec.ts` en voeg een verklaring voor `NavigationComponent` toe:

   ```diff
       /* header.component.spect.ts */
   +   import { NavigationComponent } from '../navigation/navigation.component';
   
       describe('HeaderComponent', () => {
       let component: HeaderComponent;
       let fixture: ComponentFixture<HeaderComponent>;
   
       beforeEach(async(() => {
           TestBed.configureTestingModule({
           imports: [ RouterTestingModule ],
   +       declarations: [ HeaderComponent, NavigationComponent ]
           })
           .compileComponents();
       }));
   ```

   Aangezien de `NavigationComponent` nu als deel van `HeaderComponent` wordt gebruikt moet het als deel van de testbed worden verklaard.

7. Sla wijzigingen in geopende bestanden op en ga terug naar de **webpack-ontwikkelserver**: [http://localhost:4200/](http://localhost:4200/)

   ![Voltooide koptekstnavigatie](assets/navigation-routing/completed-header.png)

   Open de navigatie door de menuknevel te klikken en u zou de bevolkte navigatiekoppelingen moeten zien. U zou aan verschillende meningen van de SPA moeten kunnen navigeren.

## Begrijp het SPA verpletteren

Nu de navigatie is uitgevoerd, inspecteer het verpletteren in AEM.

1. Open in winde het dossier `app-routing.module.ts` om `ui.frontend/src/app`.

   ```js
   /* app-routing.module.ts */
   import { AemPageDataResolver, AemPageRouteReuseStrategy } from '@adobe/cq-angular-editable-components';
   import { NgModule } from '@angular/core';
   import { RouteReuseStrategy, RouterModule, Routes, UrlMatchResult, UrlSegment } from '@angular/router';
   import { PageComponent } from './components/page/page.component';
   
   export function AemPageMatcher(url: UrlSegment[]): UrlMatchResult {
       if (url.length) {
           return {
               consumed: url,
               posParams: {
                   path: url[url.length - 1]
               }
           };
       }
   }
   
   const routes: Routes = [
       {
           matcher: AemPageMatcher,
           component: PageComponent,
           resolve: {
               path: AemPageDataResolver
           }
       }
   ];
   @NgModule({
       imports: [RouterModule.forRoot(routes)],
       exports: [RouterModule],
       providers: [
           AemPageDataResolver,
           {
           provide: RouteReuseStrategy,
           useClass: AemPageRouteReuseStrategy
           }
       ]
   })
   export class AppRoutingModule {}
   ```

   De array `routes: Routes = [];` definieert de routes of navigatiepaden naar componenttoewijzingen van de Angular.

   `AemPageMatcher` is een router  [UrlMatcher](https://angular.io/api/router/UrlMatcher) van de douane Angular, die om het even wat aanpast dat &quot;als&quot;een pagina in AEM kijkt die deel van deze toepassing van Angular uitmaakt.

   `PageComponent` is de Component van de Angular die een Pagina in AEM vertegenwoordigt, en de aangepaste routes zullen aanhalen. De `PageComponent` wordt verder geïnspecteerd.

   `AemPageDataResolver`, die door de AEM SPA Redacteur JS SDK wordt verstrekt, is een Router van de  [Angular van de douane die ](https://angular.io/api/router/Resolve) wordt omgezet om route URL om te zetten, die de weg in AEM met inbegrip van de uitbreiding .html, aan de middelweg in AEM is, die de paginadad minus de uitbreiding is.

   Bijvoorbeeld, `AemPageDataResolver` transformeert URL van een route van `content/wknd-spa-angular/us/en/home.html` in een weg van `/content/wknd-spa-angular/us/en/home`. Hiermee wordt de inhoud van de pagina opgelost op basis van het pad in de JSON-model-API.

   `AemPageRouteReuseStrategy`, verstrekt door de AEM SPA Redacteur JS SDK, is een douane  [](https://angular.io/api/router/RouteReuseStrategy) RouteReuseStrategydie hergebruik van de  `PageComponent` over routes verhindert. Anders wordt de inhoud van pagina &quot;A&quot; mogelijk weergegeven wanneer u naar pagina &quot;B&quot; navigeert.

2. Open het bestand `page.component.ts` om `ui.frontend/src/app/components/page/`.

   ```js
   ...
   export class PageComponent {
       items;
       itemsOrder;
       path;
   
       constructor(
           private route: ActivatedRoute,
           private modelManagerService: ModelManagerService
       ) {
           this.modelManagerService
           .getData({ path: this.route.snapshot.data.path })
           .then(data => {
               this.path = data[Constants.PATH_PROP];
               this.items = data[Constants.ITEMS_PROP];
               this.itemsOrder = data[Constants.ITEMS_ORDER_PROP];
           });
       }
   }
   ```

   `PageComponent` wordt vereist om JSON te verwerken die van AEM wordt teruggewonnen en als component van de Angular wordt gebruikt om de routes terug te geven.

   `ActivatedRoute`, die door de module van de Router van de Angular wordt verstrekt, bevat de staat erop wijst die welke inhoud JSON van AEM Pagina in deze de componenteninstantie van de Pagina van de Angular zou moeten worden geladen.

   `ModelManagerService`, krijgt de JSON-gegevens op basis van de route en wijst de gegevens toe aan klassevariabelen  `path`,  `items`,  `itemsOrder`. Deze worden vervolgens doorgegeven aan de [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. Open het bestand `page.component.html` op `ui.frontend/src/app/components/page/`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` bevat de  [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md). De variabelen `path`, `items` en `itemsOrder` worden doorgegeven aan `AEMPageComponent`. De `AemPageComponent`, verstrekt via de SPA Redacteur JavaScript SDK zal dan over deze gegevens herhalen en dynamisch Angular componenten concretiseren die op de gegevens JSON worden gebaseerd zoals die in [de zelfstudie van Componenten van de Kaart](./map-components.md) worden gezien.

   `PageComponent` is eigenlijk enkel een volmacht voor `AEMPageComponent` en het is `AEMPageComponent` die de meerderheid van het zware heffen doet om het JSON model aan de componenten van de Angular correct in kaart te brengen.

## Inspect de SPA routering in AEM

1. Open een terminal en stop **webpack dev server** als deze wordt gestart. Navigeer aan de wortel van het project, en stel het project in om het gebruiken van uw Geweven vaardigheden te AEM:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > Het project van de Angular heeft sommige zeer strikte toegelaten verbindingsregels. Als de Maven-build mislukt, controleert u de fout en zoekt u naar **Tinfouten in de vermelde bestanden.**. Los om het even welke die kwesties door linter worden gevonden en stel het Maven bevel opnieuw in werking.

2. Navigeer naar de SPA homepage in AEM: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html) en open de ontwikkelaarsgereedschappen van uw browser. Onderstaande screenshots worden vastgelegd vanuit de Google Chrome-browser.

   Vernieuw de pagina en u zou een XHR- verzoek aan `/content/wknd-spa-angular/us/en.model.json` moeten zien, wat de SPAWortel is. U ziet dat er slechts drie onderliggende pagina&#39;s zijn opgenomen op basis van de configuratie van de hiërarchiediepte voor de SPA basissjabloon die eerder in de zelfstudie is gemaakt. **Pagina 3** is hier niet in opgenomen.

   ![Aanvankelijk JSON-verzoek - SPA basis](assets/navigation-routing/initial-json-request.png)

3. Met de open ontwikkelaars hulpmiddelen, navigeer aan **Pagina 3**:

   ![Navigeren op pagina 3](assets/navigation-routing/page-three-navigation.png)

   Merk op dat een nieuw XHR-verzoek wordt ingediend aan: `/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![Pagina drie XHR-verzoek](assets/navigation-routing/page-3-xhr-request.png)

   De AEM ModelManager begrijpt dat de **Pagina 3** JSON-inhoud niet beschikbaar is en activeert automatisch het extra XHR-verzoek.

4. Ga verder met de navigatiekoppelingen in de SPA. Merk op dat er geen extra XHR-verzoeken worden gedaan en dat er geen volledige pagina-vernieuwingen plaatsvinden. Dit maakt de SPA snel voor de eindgebruiker en vermindert onnodige verzoeken terug naar AEM.

   ![Navigatie geïmplementeerd](assets/navigation-routing/final-navigation-implemented.gif)

5. Experimenteer met diepe koppelingen door rechtstreeks naar: [http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html). Let erop dat de knop Terug van de browser blijft werken.

## Gefeliciteerd! {#congratulations}

U hebt geleerd hoe meerdere weergaven in de SPA kunnen worden ondersteund door de SPA Editor SDK toe te wijzen aan AEM pagina&#39;s. De dynamische navigatie is uitgevoerd gebruikend het verpletteren van de Angular en toegevoegd aan `Header` component.

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) altijd bekijken of de code plaatselijk controleren door aan de tak `Angular/navigation-routing-solution` te schakelen.

### Volgende stappen {#next-steps}

[Een aangepaste component](custom-component.md)  maken - Leer hoe u een aangepaste component maakt die u wilt gebruiken met de AEM SPA Editor. Leer hoe u dialoogvensters met auteurs en Sling Models ontwikkelt om het JSON-model uit te breiden en een aangepaste component te vullen.
