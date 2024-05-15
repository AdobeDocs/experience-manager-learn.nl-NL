---
title: Een component uitbreiden | Aan de slag met de AEM SPA Editor en Angular
description: Leer hoe te om een bestaande Component van de Kern uit te breiden die met de Redacteur van de SPA van de AEM moet worden gebruikt. Het begrip hoe te om eigenschappen en inhoud aan een bestaande component toe te voegen is een krachtige techniek om de mogelijkheden van een implementatie van AEM SPARedacteur uit te breiden. Leer om het delegatiepatroon te gebruiken voor het uitbreiden van Sling Models en eigenschappen van het Verkopen van Middel.
feature: SPA Editor, Core Components
version: Cloud Service
jira: KT-5871
thumbnail: 5871-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 0265d3df-3de8-4a25-9611-ddf73d725f6e
duration: 435
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1713'
ht-degree: 0%

---

# Een kerncomponent uitbreiden {#extend-component}

Leer hoe te om een bestaande Component van de Kern uit te breiden die met de Redacteur van de SPA van de AEM moet worden gebruikt. Begrijpen hoe een bestaande component wordt uitgebreid is een krachtige techniek om de mogelijkheden van een AEM SPA de implementatie van de Redacteur aan te passen en uit te breiden.

## Doelstelling

1. Breid een bestaande Component van de Kern met extra eigenschappen en inhoud uit.
2. Begrijp de basis van Componentovererving met het gebruik van `sling:resourceSuperType`.
3. Leer hoe u de [Delegatiepatroon](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) voor Sling Models om bestaande logica en functionaliteit opnieuw te gebruiken.

## Wat u gaat maken

In dit hoofdstuk wordt een `Card` wordt gemaakt. De `Card` component breidt de [Component Image Core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) het toevoegen van extra inhoudsgebieden zoals een Titel en een Vraag aan de knoop van de Actie om de rol van een meetapparaat voor andere inhoud binnen de SPA uit te voeren.

![Definitief ontwerpen van kaartcomponent](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> In een echte implementatie is het wellicht beter om de [Teaser-component](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/teaser.html) dan de uitbreiding van de [Component Image Core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) om een `Card` afhankelijk van de projectvereisten. Het wordt altijd aanbevolen [Kernonderdelen](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) rechtstreeks, indien mogelijk.

## Vereisten

Controleer de vereiste gereedschappen en instructies voor het instellen van een [plaatselijke ontwikkelomgeving](overview.md#local-dev-environment).

### De code ophalen

1. Download het beginpunt voor deze zelfstudie via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/extend-component-start
   ```

2. Implementeer de basis van de code op een lokale AEM met Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Als u [AEM 6,x](overview.md#compatibility) voeg toe `classic` profiel:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Het voltooide pakket installeren voor de traditionele [WKND-referentiesite](https://github.com/adobe/aem-guides-wknd/releases/tag/aem-guides-wknd-2.1.0). De afbeeldingen van [WKND-referentiesite](https://github.com/adobe/aem-guides-wknd/releases/latest) wordt opnieuw gebruikt op de WKND-SPA. Het pakket kan worden geïnstalleerd met [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Pakketbeheer installeren wknd.all](./assets/map-components/package-manager-wknd-all.png)

U kunt de voltooide code altijd weergeven op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) of controleer de code plaatselijk door aan de tak over te schakelen `Angular/extend-component-solution`.

## Inspect - initiële kaartimplementatie

De begincode van het hoofdstuk bevat een eerste kaartcomponent. Inspect het beginpunt voor de implementatie van de Kaart.

1. Open in de IDE van uw keuze de `ui.apps` -module.
2. Navigeren naar `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card` en bekijk de `.content.xml` bestand.

   ![Start AEM van kaartcomponent](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   De eigenschap `sling:resourceSuperType` punten naar `wknd-spa-angular/components/image` erop wijst dat de `Card` overerft de functionaliteit van de WKND SPA Image-component.

3. Het bestand Inspect `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   Let erop dat de `sling:resourceSuperType` punten naar `core/wcm/components/image/v2/image`. Dit geeft aan dat de WKND SPA Image-component de functionaliteit overerft van de Core Component Image.

   Ook bekend als de [Proxypatroon](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) Het verkopen van middelovererving is een krachtig ontwerppatroon voor het toestaan van kindcomponenten om functionaliteit over te erven en gedrag uit te breiden/met voeten te treden wanneer gewenst. De overerving van de verkoop steunt veelvoudige niveaus van overerving, zodat uiteindelijk het nieuwe `Card` -component overerft de functionaliteit van de Core Component Image.

   Veel ontwikkelingsteams streven ernaar om D.R.Y. te zijn (herhaal jezelf niet). Dit is mogelijk met AEM.

4. Onder de `card` map, het bestand openen `_cq_dialog/.content.xml`.

   Dit bestand is de definitie in het dialoogvenster Component voor het dialoogvenster `Card` component. Als u de overerving Verschuiven gebruikt, kunt u de functies van de [Samenvoeging van verkoopbronnen](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html) delen van het dialoogvenster overschrijven of uitbreiden. In dit voorbeeld is een nieuw tabblad toegevoegd aan het dialoogvenster om aanvullende gegevens van een auteur vast te leggen om de kaartcomponent te vullen.

   Eigenschappen als `sling:orderBefore` een ontwikkelaar toestaan te kiezen waar nieuwe tabbladen of formuliervelden worden ingevoegd. In dit geval worden de `Text` tab wordt ingevoegd vóór de `asset` tab. Om volledig gebruik te maken van Sling Resource Merger, is het belangrijk om de oorspronkelijke structuur van de dialoogknoop voor te kennen [Dialoogvenster Afbeeldingscomponent](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml).

5. Onder de `card` map, het bestand openen `_cq_editConfig.xml`. Dit bestand dicteert het gedrag van slepen en neerzetten in de AEM-ontwerpinterface. Wanneer het uitbreiden van de component van het Beeld, is het belangrijk dat het middeltype de component zelf aanpast. Controleer de `<parameters>` knooppunt:

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   Voor de meeste componenten is geen `cq:editConfig`, de afbeelding en onderliggende afstammelingen van de component Image zijn uitzonderingen.

6. In de schakelaar van winde aan `ui.frontend` module, navigeren naar `ui.frontend/src/app/components/card`:

   ![Start angular-component](assets/extend-component/angular-card-component-start.png)

7. Het bestand Inspect `card.component.ts`.

   De component is al uitgestald om aan de AEM toe te wijzen `Card` Onderdeel dat gebruikmaakt van de norm `MapTo` functie.

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   Bekijk de drie `@Input` parameters in de klasse voor `src`, `alt`, en `title`. Deze worden JSON-waarden van de AEM component verwacht die aan de component Angular worden toegewezen.

8. Het bestand openen `card.component.html`:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   In dit voorbeeld hebben we ervoor gekozen de bestaande component Afbeelding van Angular opnieuw te gebruiken `app-image` door de `@Input` parameters van `card.component.ts`. Verderop in de zelfstudie worden extra eigenschappen toegevoegd en weergegeven.

## Sjabloonbeleid bijwerken

Met deze eerste `Card` de implementatie controleert de functionaliteit in AEM SPA Editor. De eerste `Card` is een update van het sjabloonbeleid nodig.

1. Implementeer de startcode naar een lokale instantie van AEM, als u dat nog niet hebt gedaan:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navigeer naar de SPA paginasjabloon op [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Werk het beleid van de Container van de Lay-out bij om nieuwe toe te voegen `Card` component als toegestane component:

   ![Layoutcontainerbeleid bijwerken](assets/extend-component/card-component-allowed.png)

   Sla de wijzigingen in het beleid op en bekijk de `Card` component als toegestane component:

   ![Kaartcomponent als toegestane component](assets/extend-component/card-component-allowed-layout-container.png)

## Aanvankelijke kaartcomponent van auteur

Nu, auteur `Card` met de AEM SPA Editor.

1. Navigeren naar [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. In `Edit` in de modus, voegt u de `Card` aan de component `Layout Container`:

   ![Nieuwe component invoegen](assets/extend-component/insert-custom-component.png)

3. Sleep een afbeelding van de Finder Asset naar de map `Card` component:

   ![Afbeelding toevoegen](assets/extend-component/card-add-image.png)

4. Open de `Card` dialoogvenster en ziet u de toevoeging van een **Tekst** Tab.
5. Voer de volgende waarden in op het tabblad **Tekst** tab:

   ![Tekstcomponent, tabblad](assets/extend-component/card-component-text.png)

   **Kaartpad** - kies een pagina onder de SPA homepage.

   **CTA-tekst** - &quot;Meer informatie&quot;

   **Kaarttitel** - leeg laten

   **Titel ophalen van gekoppelde pagina** - schakel het selectievakje in om de waarde true aan te geven.

6. Werk de **Metagegevens van element** tab om waarden toe te voegen voor **Alternatieve tekst** en **Bijschrift**.

   Er worden momenteel geen aanvullende wijzigingen weergegeven na het bijwerken van het dialoogvenster. Om de nieuwe gebieden aan de Component van de Angular bloot te stellen, moeten wij het het Verkopen Model voor `Card` component.

7. Een nieuw tabblad openen en naar [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card). Inspect the content nodes under `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` om de `Card` componentinhoud.

   ![Eigenschappen van de component CRXDE-Lite](assets/extend-component/crxde-lite-properties.png)

   Waarnemen van die eigenschappen `cardPath`, `ctaText`, `titleFromPage` worden door het dialoogvenster voortgezet.

## Model voor kaartverkoop bijwerken

Als u uiteindelijk de waarden uit het dialoogvenster met componenten toegankelijk wilt maken voor de component Angular, moet u het Sling-model bijwerken dat de JSON-code voor de component `Card` component. Wij hebben ook de kans om twee stukken bedrijfslogica uit te voeren:

* Indien `titleFromPage` tot **true**, retourneert de titel van de pagina opgegeven door `cardPath` anders de waarde van `cardTitle` textfield.
* Retourneer de laatste gewijzigde datum van de pagina die is opgegeven door `cardPath`.

Ga terug naar de IDE van uw keuze en open de `core` -module.

1. Het bestand openen `Card.java` om `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`.

   Waarnemen dat de `Card` interface momenteel is uitgebreid `com.adobe.cq.wcm.core.components.models.Image` en erft derhalve de methoden van de `Image` interface. De `Image` interface breidt reeds `ComponentExporter` interface waarmee het verkoopmodel kan worden geëxporteerd als JSON en toegewezen door de SPA editor. Daarom hoeven we deze niet expliciet uit te breiden `ComponentExporter` interface zoals we dat deden in de [Hoofdstuk Aangepaste component](custom-component.md).

2. Voeg de volgende methodes aan de interface toe:

   ```java
   @ProviderType
   public interface Card extends Image {
   
       /***
       * The URL to populate the CTA button as part of the card.
       * The link should be based on the cardPath property that points to a page.
       * @return String URL
       */
       public String getCtaLinkURL();
   
       /***
       * The text to display on the CTA button of the card.
       * @return String CTA text
       */
       public String getCtaText();
   
   
   
       /***
       * The date to be displayed as part of the card.
       * This is based on the last modified date of the page specified by the cardPath
       * @return
       */
       public Calendar getCardLastModified();
   
   
       /**
       * Return the title of the page specified by cardPath if `titleFromPage` is set to true.
       * Otherwise return the value of `cardTitle`
       * @return
       */
       public String getCardTitle();
   }
   ```

   Deze methoden worden beschikbaar gemaakt via de JSON-model-API en doorgegeven aan de Angular-component.

3. Openen `CardImpl.java`. Dit is de uitvoering van `Card.java` interface. Deze implementatie is gedeeltelijk stopgezet om de zelfstudie te versnellen.  Let op het gebruik van de `@Model` en `@Exporter` annotaties die ervoor zorgen dat het verkoopmodel met serienummering kan worden gecodeerd als JSON via de verkoopmodel-exportfunctie.

   `CardImpl.java` gebruikt ook [Delegatiepatroon voor verkoopmodellen](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) om te voorkomen dat de logica wordt herschreven van de Image Core-component.

4. Neem de volgende regels in acht:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   Bovenstaande aantekening instantieert een afbeeldingsobject met de naam `image` op basis van de `sling:resourceSuperType` erfenis van de `Card` component.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   Het is dan mogelijk om de `image` object voor het implementeren van methoden die door de `Image` interface, zonder zelf de logica te moeten schrijven. Deze techniek wordt gebruikt voor `getSrc()`, `getAlt()`, en `getTitle()`.

5. Vervolgens implementeert u de `initModel()` methode om een variabele van het type private te initiëren `cardPage` op basis van de waarde van `cardPath`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   De `@PostConstruct initModel()` wordt aangeroepen wanneer het Sling-model wordt geïnitialiseerd. Het is daarom een goede gelegenheid om objecten te initialiseren die door andere methoden in het model kunnen worden gebruikt. De `pageManager` is één van verscheidene [Door Java™ ondersteunde globale objecten](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html) ter beschikking gesteld aan verkoopmodellen via de `@ScriptVariable` aantekening. De [getPage](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) de methode neemt in een weg en keert een AEM terug [Pagina](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) -object of null als het pad niet naar een geldige pagina verwijst.

   Hierdoor wordt het `cardPage` variabele, die door de andere nieuwe methoden wordt gebruikt om gegevens over de onderliggende gekoppelde pagina te retourneren.

6. Bekijk de algemene variabelen die al zijn toegewezen aan de JCR-eigenschappen die in het dialoogvenster met auteurs zijn opgeslagen. De `@ValueMapValue` de annotatie wordt gebruikt om de toewijzing automatisch uit te voeren.

   ```java
   @ValueMapValue
   private String cardPath;
   
   @ValueMapValue
   private String ctaText;
   
   @ValueMapValue
   private boolean titleFromPage;
   
   @ValueMapValue
   private String cardTitle;
   ```

   Deze variabelen worden gebruikt om de aanvullende methoden voor de `Card.java` interface.

7. Voer de extra methodes uit die in `Card.java` interface:

   ```java
   @Override
   public String getCtaLinkURL() {
       if(cardPage != null) {
           return cardPage.getPath() + ".html";
       }
       return null;
   }
   
   @Override
   public String getCtaText() {
       return ctaText;
   }
   
   @Override
   public Calendar getCardLastModified() {
      if(cardPage != null) {
          return cardPage.getLastModified();
      }
      return null;
   }
   
   @Override
   public String getCardTitle() {
       if(titleFromPage) {
           return cardPage != null ? cardPage.getTitle() : null;
       }
       return cardTitle;
   }
   ```

   >[!NOTE]
   >
   > U kunt de [Voltooid CardImpl.java hier](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java).

8. Open een terminalvenster en stel alleen de updates in voor de `core` met de Maven `autoInstallBundle` van de `core` directory.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   Als u [AEM 6,x](overview.md#compatibility) voeg toe `classic` profiel.

9. Bekijk de JSON-modelreactie op: [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) en zoek naar `wknd-spa-angular/components/card`:

   ```json
   "card": {
       "ctaText": "Read More",
       "cardTitle": "Page 1",
       "title": "Woman chillaxing with river views in Australian bushland",
       "src": "/content/wknd-spa-angular/us/en/home/_jcr_content/root/responsivegrid/card.coreimg.jpeg/1595190732886/adobestock-216674449.jpeg",
       "alt": "Female sitting on a large rock relaxing in afternoon dappled light the Australian bushland with views over the river",
       "cardLastModified": 1591360492414,
       "ctaLinkURL": "/content/wknd-spa-angular/us/en/home/page-1.html",
       ":type": "wknd-spa-angular/components/card"
   }
   ```

   U ziet dat het JSON-model wordt bijgewerkt met extra sleutel-/waardeparen nadat de methoden in het dialoogvenster `CardImpl` Verkoopmodel.

## Angular-component bijwerken

Nu het JSON-model is gevuld met nieuwe eigenschappen voor `ctaLinkURL`, `ctaText`, `cardTitle`, en `cardLastModified` wij kunnen de component van de Angular bijwerken om deze te tonen.

1. Terugkeren naar de IDE en de `ui.frontend` -module. Start eventueel de webpack-ontwikkelserver vanuit een nieuw terminalvenster om de wijzigingen in real-time te zien:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. Openen `card.component.ts` om `ui.frontend/src/app/components/card/card.component.ts`. Voeg de extra `@Input` annotaties voor het vastleggen van het nieuwe model:

   ```diff
   export class CardComponent implements OnInit {
   
        @Input() src: string;
        @Input() alt: string;
        @Input() title: string;
   +    @Input() cardTitle: string;
   +    @Input() cardLastModified: number;
   +    @Input() ctaLinkURL: string;
   +    @Input() ctaText: string;
   ```

3. Voeg methodes toe om te controleren of de Vraag aan Actie klaar is en voor het terugkeren van een datum/tijdkoord dat op `cardLastModified` invoer:

   ```js
   export class CardComponent implements OnInit {
       ...
       get hasCTA(): boolean {
           return this.ctaLinkURL && this.ctaLinkURL.trim().length > 0 && this.ctaText && this.ctaText.trim().length > 0;
       }
   
       get lastModifiedDate(): string {
           const lastModifiedDate = this.cardLastModified ? new Date(this.cardLastModified) : null;
   
           if (lastModifiedDate) {
           return lastModifiedDate.toLocaleDateString();
           }
           return null;
       }
       ...
   }
   ```

4. Openen `card.component.html` en voeg de volgende prijsverhoging toe om de titel, de vraag aan actie en laatste gewijzigde datum te tonen:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
       <div class="card__content">
           <h2 class="card__title">
               {{cardTitle}}
               <span class="card__lastmod" *ngIf="lastModifiedDate">{{lastModifiedDate}}</span>
           </h2>
           <div class="card__action-container" *ngIf="hasCTA">
               <a [routerLink]="ctaLinkURL" class="card__action-link" [title]="ctaText">
                   {{ctaText}}
               </a>
           </div>
       </div>
   </div>
   ```

   Er zijn al verificatieregels toegevoegd op `card.component.scss` als u de titel, de aanroep van de handeling en de datum van laatste wijziging wilt opmaken.

   >[!NOTE]
   >
   > U kunt de voltooide [Angular-kaartcomponentcode hier](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card).

5. Implementeer de volledige wijzigingen in AEM vanuit de hoofdmap van het project met Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. Navigeren naar [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html) om de bijgewerkte component weer te geven:

   ![Bijgewerkte kaartcomponent in AEM](assets/extend-component/updated-card-in-aem.png)

7. U moet de bestaande inhoud opnieuw kunnen ontwerpen om een pagina te maken die lijkt op het volgende:

   ![Definitief ontwerpen van kaartcomponent](assets/extend-component/final-authoring-card.png)

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, hebt u geleerd hoe u een AEM component kunt uitbreiden en hoe het Verdelen Modellen en dialoogvensters met het JSON-model werken.

U kunt de voltooide code altijd weergeven op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) of controleer de code plaatselijk door aan de tak over te schakelen `Angular/extend-component-solution`.
