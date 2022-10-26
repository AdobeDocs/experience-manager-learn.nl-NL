---
title: Navigatie en routering toevoegen | Aan de slag met de AEM SPA Editor en Angular
description: Leer hoe meerdere weergaven in de SPA worden ondersteund met AEM Pagina's en de SPA Editor SDK. Dynamische navigatie wordt uitgevoerd gebruikend de routes van de Angular en toegevoegd aan een bestaande component van de Kopbal.
feature: SPA Editor
topics: development
doc-type: tutorial
version: Cloud Service
activity: develop
audience: developer
kt: 5312
thumbnail: 5312-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 197a0c1f-4d0a-4b99-ba89-cdff2e6ac4ec
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '2635'
ht-degree: 0%

---

# Navigatie en routering toevoegen {#navigation-routing}

Leer hoe meerdere weergaven in de SPA worden ondersteund met AEM Pagina&#39;s en de SPA Editor SDK. Dynamische navigatie wordt uitgevoerd gebruikend de routes van de Angular en toegevoegd aan een bestaande component van de Kopbal.

## Doelstelling

1. Begrijp het SPA model verpletterend opties beschikbaar wanneer het gebruiken van de SPARedacteur.
2. Leren gebruiken [Angular routering](https://angular.io/guide/router) om te navigeren tussen verschillende weergaven van de SPA.
3. Voer een dynamische navigatie uit die door de AEM paginahiërarchie wordt aangedreven.

## Wat u gaat maken

In dit hoofdstuk wordt een navigatiemenu toegevoegd aan een bestaand `Header` component. Het navigatiemenu wordt aangestuurd door de AEM paginahiërarchie en gebruikt het JSON-model dat wordt geleverd door de [Navigation Core-component](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![Navigatie geïmplementeerd](assets/navigation-routing/final-navigation-implemented.gif)

## Vereisten

Controleer de vereiste gereedschappen en instructies voor het instellen van een [plaatselijke ontwikkelomgeving](overview.md#local-dev-environment).

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

   Als u [AEM 6,x](overview.md#compatibility) toevoegen `classic` profiel:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Het voltooide pakket installeren voor de traditionele [WKND-referentiesite](https://github.com/adobe/aem-guides-wknd/releases/latest). De afbeeldingen van [WKND-referentiesite](https://github.com/adobe/aem-guides-wknd/releases/latest) worden opnieuw gebruikt op de WKND-SPA. Het pakket kan worden geïnstalleerd met [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Pakketbeheer installeren wknd.all](./assets/map-components/package-manager-wknd-all.png)

U kunt de voltooide code altijd weergeven op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) of controleer de code plaatselijk door aan de tak over te schakelen `Angular/navigation-routing-solution`.

## Inspect HeaderComponent-updates {#inspect-header}

In vorige hoofdstukken `HeaderComponent` component is toegevoegd als een zuivere Angular die via `app.component.html`. In dit hoofdstuk wordt `HeaderComponent` wordt verwijderd uit de app en toegevoegd via de [Sjablooneditor](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html). Hiermee kunnen gebruikers het navigatiemenu van het dialoogvenster `HeaderComponent` vanuit AEM.

>[!NOTE]
>
> Er zijn al verschillende CSS- en JavaScript-updates aangebracht in de codebasis om dit hoofdstuk te starten. Om zich op kernconcepten te concentreren, niet **alles** van de code worden wijzigingen besproken. U kunt de volledige wijzigingen weergeven [hier](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start).

1. In winde van uw keus opent het SPA starterproject voor dit hoofdstuk.
2. Onder de `ui.frontend` inspecteer het bestand `header.component.ts` om: `ui.frontend/src/app/components/header/header.component.ts`.

   Er zijn verscheidene updates uitgevoerd, waaronder de toevoeging van een `HeaderEditConfig` en `MapTo` om de component toe te laten om aan een AEM component worden in kaart gebracht `wknd-spa-angular/components/header`.

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

   Noteer de `@Input()` aantekening voor `items`. `items` bevat een array van navigatieobjecten die vanuit AEM worden doorgegeven.

3. In de `ui.apps` de componentendefinitie van de AEM inspecteren `Header` component: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   De AEM `Header` zal alle functionaliteit van [Navigation Core-component](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html) via de `sling:resourceSuperType` eigenschap.

## Voeg de component HeaderComponent aan het SPA toe malplaatje {#add-header-template}

1. Open een browser en meld u aan bij AEM. [http://localhost:4502/](http://localhost:4502/). De begincodebasis zou reeds moeten worden opgesteld.
2. Ga naar de **[!UICONTROL SPA Page Template]**: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. De meest buitenste selecteren **[!UICONTROL Root Layout Container]** en klik op **[!UICONTROL Policy]** pictogram. Wees voorzichtig **niet** om de **[!UICONTROL Layout Container]** niet-vergrendeld voor ontwerpen.

   ![Selecteer het beleidspictogram voor de container van de hoofdlayout](assets/navigation-routing/root-layout-container-policy.png)

4. Kopieer het huidige beleid en maak een nieuw beleid met de naam **[!UICONTROL SPA Structure]**:

   ![SPA structuurbeleid](assets/map-components/spa-policy-update.png)

   Onder **[!UICONTROL Allowed Components]** > **[!UICONTROL General]** > selecteer de **[!UICONTROL Layout Container]** component.

   Onder **[!UICONTROL Allowed Components]** > **[!UICONTROL WKND SPA ANGULAR - STRUCTURE]** > selecteer de **[!UICONTROL Header]** component:

   ![Koptekstcomponent selecteren](assets/map-components/select-header-component.png)

   Onder **[!UICONTROL Allowed Components]** > **[!UICONTROL WKND SPA ANGULAR - Content]** > selecteer de **[!UICONTROL Image]** en **[!UICONTROL Text]** componenten. Er moeten in totaal vier componenten zijn geselecteerd.

   Klikken **[!UICONTROL Done]** om de wijzigingen op te slaan.

5. **Vernieuwen** de pagina. Voeg de **[!UICONTROL Header]** component boven het niet-vergrendelde **[!UICONTROL Layout Container]**:

   ![component Header toevoegen aan sjabloon](./assets/navigation-routing/add-header-component.gif)

6. Selecteer **[!UICONTROL Header]** component en klik op de component **Beleid** pictogram om het beleid te bewerken.

   ![Klikken op koptekstbeleid](assets/navigation-routing/header-policy-icon.png)

7. Een nieuw beleid maken met een **[!UICONTROL Policy Title]** van **&quot;WKND SPA Header&quot;**.

   Onder de **[!UICONTROL Properties]**:

   * Stel de **[!UICONTROL Navigation Root]** tot `/content/wknd-spa-angular/us/en`.
   * Stel de **[!UICONTROL Exclude Root Levels]** tot **1**.
   * Uitschakelen **[!UICONTROL Collect al child pages]**.
   * Stel de **[!UICONTROL Navigation Structure Depth]** tot **3**.

   ![Koptekstbeleid configureren](assets/navigation-routing/header-policy.png)

   Hierdoor worden de navigatieniveaus 2 diep onder elkaar verzameld `/content/wknd-spa-angular/us/en`.

8. Nadat u de wijzigingen hebt opgeslagen, ziet u de gevulde `Header` als onderdeel van de template:

   ![Bevolkte koptekstcomponent](assets/navigation-routing/populated-header.png)

## Onderliggende pagina&#39;s maken

Maak vervolgens aanvullende pagina&#39;s in AEM die als de verschillende weergaven in de SPA dienen. We zullen ook de hiërarchische structuur van het JSON-model dat door AEM wordt aangeboden, controleren.

1. Ga naar de **Sites** console: [http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home). Selecteer **Startpagina WKND SPA Angular** en klik op **[!UICONTROL Create]** > **[!UICONTROL Page]**:

   ![Nieuwe pagina maken](assets/navigation-routing/create-new-page.png)

2. Onder **[!UICONTROL Template]** selecteren **[!UICONTROL SPA Page]**. Onder **[!UICONTROL Properties]** enter **&quot;Pagina 1&quot;** voor de **[!UICONTROL Title]** en **&quot;page-1&quot;** als de naam.

   ![De eigenschappen voor de eerste pagina invoeren](assets/navigation-routing/initial-page-properties.png)

   Klikken **[!UICONTROL Create]** en in het dialoogvenster klikt u op **[!UICONTROL Open]** om de pagina te openen in de AEM SPA Editor.

3. Een nieuwe toevoegen **[!UICONTROL Text]** aan de hoofdcomponent **[!UICONTROL Layout Container]**. Bewerk de component en voer de tekst in: **&quot;Pagina 1&quot;** het gebruik van de RTE en de **H1** element (u moet de modus Volledig scherm activeren om de alinea-elementen te wijzigen)

   ![Voorbeeld van inhoudspagina 1](assets/navigation-routing/page-1-sample-content.png)

   Voel u vrij om extra inhoud toe te voegen, zoals een afbeelding.

4. Ga terug naar de AEM Sites-console en herhaal de bovenstaande stappen en maak een tweede pagina met de naam **&quot;Pagina 2&quot;** als een **Pagina 1**. Inhoud toevoegen aan **Pagina 2** zodat het gemakkelijk kan worden geïdentificeerd.
5. Ten slotte maakt u een derde pagina. **&quot;Pagina 3&quot;** maar als **onderliggend** van **Pagina 2**. Na voltooiing zou de plaatshiërarchie als het volgende moeten kijken:

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

   Onder `:children` u zou een ingang voor elk van de gecreeerde pagina&#39;s moeten zien. De inhoud voor alle pagina&#39;s staat in dit eerste JSON-verzoek. Zodra, wordt het navigatie verpletteren uitgevoerd, worden de verdere meningen van de SPA geladen snel, aangezien de inhoud reeds beschikbare cliënt-kant is.

   Het is niet verstandig om te laden **ALLES** van de inhoud van een SPA in het eerste JSON-verzoek, aangezien dit het laden van de eerste pagina zou vertragen. Vervolgens kunt u bekijken hoe de hiërarchische diepte van pagina&#39;s wordt verzameld.

7. Ga naar de **SPA** sjabloon op: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html).

   Klik op de knop **[!UICONTROL Page properties menu]** > **[!UICONTROL Page Policy]**:

   ![Het paginabeleid voor SPA basispagina openen](assets/navigation-routing/open-page-policy.png)

8. De **SPA** sjabloon heeft een extra **[!UICONTROL Hierarchical Structure]** om de verzamelde JSON-inhoud te beheren. De **[!UICONTROL Structure Depth]** bepaalt hoe diep in de sitehiërarchie onderliggende pagina&#39;s onder de **basis**. U kunt ook de opdracht **[!UICONTROL Structure Patterns]** veld om extra pagina&#39;s te filteren op basis van een reguliere expressie.

   Werk de **[!UICONTROL Structure Depth]** tot **&quot;2&quot;**:

   ![Structuurdiepte bijwerken](assets/navigation-routing/update-structure-depth.png)

   Klikken **[!UICONTROL Done]** om de wijzigingen in het beleid op te slaan.

9. Het JSON-model opnieuw openen [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

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

   Let erop dat de **Pagina 3** pad is verwijderd: `/content/wknd-spa-angular/us/en/home/page-2/page-3` uit het oorspronkelijke JSON-model.

   Later zullen we zien hoe de AEM SPA Editor SDK dynamisch aanvullende inhoud kan laden.

## Navigatie implementeren

Implementeer vervolgens het navigatiemenu met een nieuwe `NavigationComponent`. We kunnen de code rechtstreeks toevoegen in `header.component.html` maar het is beter om grote onderdelen te vermijden . In plaats daarvan implementeert u een `NavigationComponent` die later mogelijk opnieuw kunnen worden gebruikt.

1. Bekijk de JSON die door de AEM wordt weergegeven `Header` component bij [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json):

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

   De hiërarchische aard van de AEM pagina&#39;s wordt gemodelleerd in JSON die kan worden gebruikt om een navigatiemenu te bevolken. Herinnert eraan dat de `Header` alle functionaliteit van de component overerft [Navigation Core-component](https://www.aemcomponents.dev/content/core-components-examples/library/core-structure/navigation.html) en de inhoud die via de JSON beschikbaar wordt gemaakt, wordt automatisch toegewezen aan de Angular `@Input` aantekening.

2. Open een nieuw terminalvenster en navigeer naar de `ui.frontend` map van het SPA project. Een nieuwe `NavigationComponent` het gebruiken van het hulpmiddel CLI van de Angular:

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. Maak vervolgens een klasse met de naam `NavigationLink` het gebruiken van Angular CLI in nieuw gecreeerd `components/navigation` map:

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. Ga terug naar IDE van uw keuze en open het bestand op `navigation-link.ts` om `/src/app/components/navigation/navigation-link.ts`.

   ![Het bestand navigation-link.ts openen](assets/navigation-routing/ide-navigation-link-file.png)

5. Vullen `navigation-link.ts` met het volgende:

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

   Dit is een eenvoudige klasse die een afzonderlijke navigatiekoppeling vertegenwoordigt. In de klasseconstructor verwachten we `data` om het JSON-object te zijn dat vanuit AEM wordt doorgegeven. Deze klasse wordt gebruikt binnen beide `NavigationComponent` en `HeaderComponent` om de navigatiestructuur gemakkelijk te vullen.

   Er wordt geen gegevenstransformatie uitgevoerd. Deze klasse wordt vooral gemaakt om het JSON-model sterk te typen. Let op: `this.children` wordt getypt als `NavigationLink[]` en dat de constructor recursief nieuwe `NavigationLink` objecten voor elk item in het dialoogvenster `children` array. Herinnert dat JSON-model voor de `Header` is hiërarchisch.

6. Het bestand openen `navigation-link.spec.ts`. Dit is het testbestand voor de `NavigationLink` klasse. Werk het bij met het volgende:

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

   Let op: `const data` volgt hetzelfde JSON-model dat eerder is geïnspecteerd voor één koppeling. Dit is verre van een robuuste eenheidstest, maar het zou moeten volstaan om de aannemer van `NavigationLink`.

7. Het bestand openen `navigation.component.ts`. Werk het bij met het volgende:

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

   `NavigationComponent` verwacht een `object[]` benoemd `items` Dat is het JSON-model van AEM. Deze klasse maakt één methode beschikbaar `get navigationLinks()` die een array van `NavigationLink` objecten.

8. Het bestand openen `navigation.component.html` en het bijwerken met het volgende:

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   Hiermee wordt een eerste `<ul>` en roept de `get navigationLinks()` methode van `navigation.component.ts`. An `<ng-container>` wordt gebruikt om een vraag aan een malplaatje te maken genoemd `recursiveListTmpl` en geeft deze door `navigationLinks` als een variabele met de naam `links`.

   Voeg de `recursiveListTmpl` volgende:

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

   Hier wordt de rest van de rendering voor de navigatiekoppeling geïmplementeerd. De variabele `link` is van type `NavigationLink` en alle methoden/eigenschappen die door die klasse worden gemaakt, zijn beschikbaar. [`[routerLink]`](https://angular.io/api/router/RouterLink) wordt gebruikt in plaats van normaal `href` kenmerk. Hierdoor kunnen we koppelingen maken naar specifieke routes in de app, zonder dat de volledige pagina wordt vernieuwd.

   Het recursieve gedeelte van de navigatie wordt ook uitgevoerd door een andere te creëren `<ul>` als de huidige `link` heeft een niet-leeg `children` array.

9. Bijwerken `navigation.component.spec.ts` om ondersteuning toe te voegen voor `RouterTestingModule`:

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

   Het toevoegen van `RouterTestingModule` is vereist omdat de component het gebruik `[routerLink]`.

10. Bijwerken `navigation.component.scss` om enkele basisstijlen toe te voegen aan de `NavigationComponent`:

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

Nu `NavigationComponent` is uitgevoerd, `HeaderComponent` moet worden bijgewerkt om ernaar te verwijzen.

1. Open een terminal en navigeer naar de `ui.frontend` in het SPA project. Start de **webpack-ontwikkelserver**:

   ```shell
   $ npm start
   ```

2. Open een browsertabblad en navigeer naar [http://localhost:4200/](http://localhost:4200/).

   De **webpack-ontwikkelserver** moet zo worden geconfigureerd dat het JSON-model kan worden geproxy door een lokale instantie van AEM (`ui.frontend/proxy.conf.json`). Op deze manier kunnen we rechtstreeks code toevoegen aan de inhoud die in AEM van eerder in de zelfstudie is gemaakt.

   ![menuschakeloptie werken](./assets/navigation-routing/nav-toggle-static.gif)

   De `HeaderComponent` de menuschakelfunctionaliteit is al geïmplementeerd. Voeg vervolgens de navigatiecomponent toe.

3. Ga terug naar IDE van uw keuze en open het bestand `header.component.ts` om `ui.frontend/src/app/components/header/header.component.ts`.
4. Werk de `setHomePage()` methode om de hard-gecodeerde Koord te verwijderen en de dynamische die binnen eigenschappen te gebruiken door de AEM component worden overgegaan:

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

   Een nieuwe instantie van `NavigationLink` wordt gemaakt op basis van `items[0]`, de basis van het JSON-navigatiemodel dat vanuit AEM is doorgegeven. `this.route.snapshot.data.path` keert de weg van de huidige route van de Angular terug. Deze waarde wordt gebruikt om te bepalen als de huidige route is **Startpagina**. `this.homePageUrl` wordt gebruikt om de ankerkoppeling in het dialoogvenster **logo**.

5. Openen `header.component.html` en vervang de statische plaatsaanduiding voor de navigatie door een verwijzing naar het zojuist gemaakte `NavigationComponent`:

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` kenmerk geeft het `@Input() items` van de `HeaderComponent` aan de `NavigationComponent` waar de navigatie zal worden opgebouwd.

6. Openen `header.component.spec.ts` en voegt een verklaring toe voor de `NavigationComponent`:

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

   Aangezien de `NavigationComponent` wordt nu gebruikt als onderdeel van het `HeaderComponent` het moet als onderdeel van de testbank worden aangegeven.

7. Wijzigingen in geopende bestanden opslaan en terugkeren naar de map **webpack-ontwikkelserver**: [http://localhost:4200/](http://localhost:4200/)

   ![Voltooide koptekstnavigatie](assets/navigation-routing/completed-header.png)

   Open de navigatie door de menuknevel te klikken en u zou de bevolkte navigatiekoppelingen moeten zien. U zou aan verschillende meningen van de SPA moeten kunnen navigeren.

## Begrijp het SPA verpletteren

Nu de navigatie is uitgevoerd, inspecteer het verpletteren in AEM.

1. In winde open het dossier `app-routing.module.ts` om `ui.frontend/src/app`.

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

   De `routes: Routes = [];` array definieert de routes of navigatiepaden naar componenttoewijzingen van Angulars.

   `AemPageMatcher` is een router van de douaneAngular [UrlMatcher](https://angular.io/api/router/UrlMatcher), die overeenkomt met alles wat er &#39;uitziet als&#39; een pagina in AEM die deel uitmaakt van deze Angular.

   `PageComponent` is de Component van de Angular die een Pagina in AEM vertegenwoordigt, en gebruikt om de aangepaste routes terug te geven. De `PageComponent` wordt later in de zelfstudie besproken.

   `AemPageDataResolver`, verstrekt door de AEM SPA Editor JS SDK, is een aangepast [Angular Router Resolver](https://angular.io/api/router/Resolve) wordt gebruikt om de route URL, die de weg in AEM met inbegrip van de uitbreiding .html is, aan de middelweg in AEM om te zetten, die de paginadad minus de uitbreiding is.

   De `AemPageDataResolver` transformeert de URL van een route van `content/wknd-spa-angular/us/en/home.html` in een pad van `/content/wknd-spa-angular/us/en/home`. Hiermee wordt de inhoud van de pagina opgelost op basis van het pad in de JSON-model-API.

   `AemPageRouteReuseStrategy`, verstrekt door de AEM SPA Editor JS SDK, is een aangepast [RouteReuseStrategy](https://angular.io/api/router/RouteReuseStrategy) die hergebruik van de `PageComponent` over routes. Anders wordt de inhoud van pagina &quot;A&quot; mogelijk weergegeven wanneer u naar pagina &quot;B&quot; navigeert.

2. Het bestand openen `page.component.ts` om `ui.frontend/src/app/components/page/`.

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

   De `PageComponent` wordt vereist om JSON te verwerken die van AEM wordt teruggewonnen en als component van de Angular wordt gebruikt om de routes terug te geven.

   `ActivatedRoute`, die door de module van de Router van de Angular wordt verstrekt, bevat de staat erop wijst die welke inhoud JSON van AEM Pagina in deze de componenteninstantie van de Pagina van de Angular zou moeten worden geladen.

   `ModelManagerService`, krijgt de JSON-gegevens op basis van de route en wijst de gegevens toe aan klassevariabelen `path`, `items`, `itemsOrder`. Deze worden vervolgens doorgegeven aan de [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. Het bestand openen `page.component.html` om `ui.frontend/src/app/components/page/`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` bevat de [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md). De variabelen `path`, `items`, en `itemsOrder` worden doorgegeven aan de `AEMPageComponent`. De `AemPageComponent`, verstrekt via de SPA redacteur zal JavaScript SDK dan over deze gegevens herhalen en dynamisch Angular componenten concretiseren die op de gegevens JSON worden gebaseerd zoals die in [Zelfstudie Kaartcomponenten](./map-components.md).

   De `PageComponent` is eigenlijk slechts een proxy voor de `AEMPageComponent` en het is `AEMPageComponent` Dat doet het grootste deel van het zware heffen om het model JSON correct aan de componenten van de Angular in kaart te brengen.

## Inspect de SPA routering in AEM

1. Open een terminal en stop de **webpack-ontwikkelserver** indien gestart. Navigeer aan de wortel van het project, en stel het project in om het gebruiken van uw Geweven vaardigheden te AEM:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > Het project van de Angular heeft sommige zeer strikte toegelaten verbindingsregels. Als de Maven-build mislukt, controleert u de fout en zoekt u naar **In de weergegeven bestanden aangetroffen puntfouten.**. Los om het even welke die kwesties door linter worden gevonden en stel het Maven bevel opnieuw in werking.

2. Navigeer naar de SPA homepage in AEM: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html) en open de ontwikkelaarsgereedschappen van uw browser. Onderstaande screenshots worden vastgelegd vanuit de Google Chrome-browser.

   De pagina vernieuwen. Er wordt een XHR-verzoek weergegeven voor `/content/wknd-spa-angular/us/en.model.json`, dat is de SPA. U ziet dat er slechts drie onderliggende pagina&#39;s zijn opgenomen op basis van de configuratie van de hiërarchiediepte voor de SPA basissjabloon die eerder in de zelfstudie is gemaakt. Dit omvat niet **Pagina 3**.

   ![Aanvankelijk JSON-verzoek - SPA basis](assets/navigation-routing/initial-json-request.png)

3. Met de open hulpmiddelen van de ontwikkelaar, navigeer aan **Pagina 3**:

   ![Navigeren op pagina 3](assets/navigation-routing/page-three-navigation.png)

   Merk op dat een nieuw XHR-verzoek wordt ingediend aan: `/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![Pagina drie XHR-verzoek](assets/navigation-routing/page-3-xhr-request.png)

   De AEM Model Manager begrijpt dat de **Pagina 3** JSON-inhoud is niet beschikbaar en activeert automatisch het aanvullende XHR-verzoek.

4. Ga verder met de navigatiekoppelingen in de SPA. Merk op dat er geen extra XHR-verzoeken worden gedaan en dat er geen volledige pagina-vernieuwingen plaatsvinden. Dit maakt de SPA snel voor de eindgebruiker en vermindert onnodige verzoeken terug naar AEM.

   ![Navigatie geïmplementeerd](assets/navigation-routing/final-navigation-implemented.gif)

5. Experimenteer met diepe koppelingen door rechtstreeks naar: [http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html). Let erop dat de knop Terug van de browser blijft werken.

## Gefeliciteerd! {#congratulations}

U hebt geleerd hoe meerdere weergaven in de SPA kunnen worden ondersteund door de SPA Editor SDK toe te wijzen aan AEM pagina&#39;s. De dynamische navigatie is uitgevoerd gebruikend het verpletteren van de Angular en toegevoegd aan `Header` component.

U kunt de voltooide code altijd weergeven op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) of controleer de code plaatselijk door aan de tak over te schakelen `Angular/navigation-routing-solution`.

### Volgende stappen {#next-steps}

[Een aangepaste component maken](custom-component.md) - Leer hoe u een aangepaste component maakt die u wilt gebruiken met de AEM SPA Editor. Leer hoe u dialoogvensters met auteurs en Sling Models ontwikkelt om het JSON-model uit te breiden en een aangepaste component te vullen.
