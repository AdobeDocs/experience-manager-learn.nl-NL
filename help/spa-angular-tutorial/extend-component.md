---
title: Een component uitbreiden | Aan de slag met de AEM SPA-editor en hoekig
description: Leer hoe te om een bestaande Component van de Kern uit te breiden die met de Redacteur van het AEMKUUROORD moet worden gebruikt. Het begrip hoe te om eigenschappen en inhoud aan een bestaande component toe te voegen is een krachtige techniek om de mogelijkheden van een implementatie van AEMRedacteur van het KUUROORD uit te breiden. Leer om het delegatiepatroon te gebruiken voor het uitbreiden van Sling Models en eigenschappen van de Verzameling van Middel.
sub-product: sites
feature: SPA Editor
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5871
thumbnail: 5871-spa-angular.jpg
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '1984'
ht-degree: 1%

---


# Een kerncomponent uitbreiden {#extend-component}

Leer hoe te om een bestaande Component van de Kern uit te breiden die met de Redacteur van het AEMKUUROORD moet worden gebruikt. Het begrip hoe te om een bestaande component uit te breiden is een krachtige techniek om de mogelijkheden van een implementatie van AEMRedacteur aan te passen en uit te breiden SPA.

## Doelstelling

1. Breid een bestaande Component van de Kern met extra eigenschappen en inhoud uit.
2. Begrijp de basis van Component Overerving met het gebruik van `sling:resourceSuperType`.
3. Leer hoe u het [delegatiepatroon](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) voor verkoopmodellen kunt gebruiken om bestaande logica en functionaliteit opnieuw te gebruiken.

## Wat u gaat maken

In dit hoofdstuk wordt een nieuwe `Card` component gemaakt. De `Card` component zal de Component [van de Kern van het](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html) Beeld uitbreiden toevoegend extra inhoudsgebieden zoals een Titel en een Vraag aan de knoop van de Actie om de rol van een meetmiddel voor andere inhoud binnen het KUUROORD uit te voeren.

![Definitief ontwerpen van kaartcomponent](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> In een implementatie in de praktijk is het wellicht beter om de [Taser-component](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/teaser.html) te gebruiken en vervolgens de [Image Core-component](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html) uit te breiden om een `Card` component te maken die afhankelijk is van de projectvereisten. Het wordt altijd aangeraden [Core Components](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html) zo mogelijk rechtstreeks te gebruiken.

## Vereisten

Controleer de vereiste gereedschappen en instructies voor het instellen van een [lokale ontwikkelomgeving](overview.md#local-dev-environment).

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

   Als u [AEM 6.x](overview.md#compatibility) gebruikt, voegt u het `classic` profiel toe:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installeer het voltooide pakket voor de traditionele [WKND verwijzingsplaats](https://github.com/adobe/aem-guides-wknd/releases/latest). De beelden die door de [WKND verwijzingsplaats](https://github.com/adobe/aem-guides-wknd/releases/latest) worden verstrekt zullen op het KND SPA worden hergebruikt. U kunt het pakket installeren met [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Pakketbeheer installeren wknd.all](./assets/map-components/package-manager-wknd-all.png)

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) altijd bekijken of de code plaatselijk controleren door aan de tak te schakelen `Angular/extend-component-solution`.

## Inspect - initiële kaartimplementatie

De begincode van het hoofdstuk bevat een eerste kaartcomponent. Inspect het beginpunt voor de implementatie van de Kaart.

1. Open de `ui.apps` module in IDE van uw keuze.
2. Navigeer naar `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card` en bekijk het `.content.xml` bestand.

   ![Start AEM van kaartcomponent](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   Het bezit `sling:resourceSuperType` wijst erop `wknd-spa-angular/components/image` erop dat de `Card` component alle functionaliteit van de component van het Beeld zal erven WKND SPA.

3. Inspect het bestand `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   Let op: de `sling:resourceSuperType` punten `core/wcm/components/image/v2/image`. Dit wijst erop dat de component van het Beeld WKND SPA alle functionaliteit van het Beeld van de Component van de Kern erft.

   Ook genoemd geworden het [Proxy patroon](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) het Verspreiden middelerving is een krachtig ontwerppatroon voor het toestaan van kindcomponenten om functionaliteit te erven en gedrag uit te breiden/met voeten te treden wanneer gewenst. De overerving van de verkoop steunt veelvoudige niveaus van overerving, zodat erft de nieuwe `Card` component uiteindelijk functionaliteit van het Beeld van de Component van de Kern.

   Veel ontwikkelingsteams streven ernaar om D.R.Y. te zijn. (Herhaal dit niet). Dit is mogelijk met AEM.

4. Open het bestand onder de `card` map `_cq_dialog/.content.xml`.

   Dit bestand is de definitie in het dialoogvenster Component voor de `Card` component. Als het gebruiken van het Verschuiven overerving, zijn mogelijk om eigenschappen van de [Verspreide Fusie](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/sling-resource-merger.html) van het Middel te gebruiken om gedeelten van de dialoog met voeten te treden of uit te breiden. In dit voorbeeld is een nieuw tabblad toegevoegd aan het dialoogvenster om aanvullende gegevens van een auteur vast te leggen om de kaartcomponent te vullen.

   Met eigenschappen als `sling:orderBefore` eigenschappen kan een ontwikkelaar kiezen waar nieuwe tabbladen of formuliervelden moeten worden ingevoegd. In dat geval wordt het `Text` tabblad ingevoegd vóór het `asset` tabblad. Om ten volle gebruik te maken van de Sling Resource Merger is het belangrijk om de oorspronkelijke dialoogknoopstructuur voor de component van het [Beeld te kennen dialoog](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml).

5. Open het bestand onder de `card` map `_cq_editConfig.xml`. Dit bestand dicteert het gedrag van slepen en neerzetten in de AEM-ontwerpinterface. Wanneer het uitbreiden van de component van het Beeld is het belangrijk dat het middeltype de component zelf aanpast. Het `<parameters>` knooppunt controleren:

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   De meeste componenten vereisen geen `cq:editConfig`, de Afbeelding en onderliggende afstammelingen van de component Image zijn uitzonderingen.

6. In de schakelaar van winde aan de `ui.frontend` module, navigerend aan `ui.frontend/src/app/components/card`:

   ![Begin hoekcomponent](assets/extend-component/angular-card-component-start.png)

7. Inspect het bestand `card.component.ts`.

   De component is al uitgestald om aan de AEM `Card` Component toe te wijzen gebruikend de standaardfunctie `MapTo` .

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   Controleer de drie `@Input` parameters in de klasse voor `src`, `alt`en `title`. Dit zijn verwachte JSON-waarden van de AEM component die aan de hoekcomponent worden toegewezen.

8. Open the file `card.component.html`:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   In dit voorbeeld hebben we ervoor gekozen de bestaande component Angular Image opnieuw te gebruiken `app-image` door de `@Input` parameters van gewoon door te geven `card.component.ts`. Later in de zelfstudie worden aanvullende eigenschappen toegevoegd en weergegeven.

## Sjabloonbeleid bijwerken

Met deze aanvankelijke `Card` implementatie herziet de functionaliteit in de Redacteur van AEMSPA. Om de aanvankelijke `Card` component te zien is een update aan het beleid van het Malplaatje nodig.

1. Implementeer de startcode naar een lokale instantie van AEM, als u dat nog niet hebt gedaan:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navigeer aan het Malplaatje van de Pagina van het KUUROORD in [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Werk het beleid van de Container van de Lay-out bij om de nieuwe `Card` component als toegestane component toe te voegen:

   ![Layoutcontainerbeleid bijwerken](assets/extend-component/card-component-allowed.png)

   Sla de wijzigingen in het beleid op en bekijk de `Card` component als een toegestane component:

   ![Kaartcomponent als toegestane component](assets/extend-component/card-component-allowed-layout-container.png)

## Aanvankelijke kaartcomponent van auteur

Daarna, auteur de `Card` component gebruikend de Redacteur van het AEMKUUROORD.

1. Ga naar [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. Voeg in de `Edit` modus de `Card` component toe aan de `Layout Container`:

   ![Nieuwe component invoegen](assets/extend-component/insert-custom-component.png)

3. Sleep een afbeelding van de zoekfunctie voor middelen naar de `Card` component:

   ![Afbeelding toevoegen](assets/extend-component/card-add-image.png)

4. Open het `Card` componentdialoogvenster en bekijk de toevoeging van een **teksttab** .
5. Voer de volgende waarden in op het tabblad **Tekst** :

   ![Tekstcomponent, tabblad](assets/extend-component/card-component-text.png)

   **Kaartpad** - kies een pagina onder de startpagina van de SPA.

   **CTA-tekst** - &quot;Meer informatie&quot;

   **Kaarttitel** - leeg laten

   **Titel ophalen van gekoppelde pagina** - schakel het selectievakje in om de waarde true aan te geven.

6. Werk het tabblad Metagegevens **van** element bij om waarden voor **Alternatieve tekst** en **Bijschrift** toe te voegen.

   Er worden momenteel geen aanvullende wijzigingen weergegeven na het bijwerken van het dialoogvenster. Als u de nieuwe velden toegankelijk wilt maken voor de hoekcomponent, moet het Sling-model voor de `Card` component worden bijgewerkt.

7. Open een nieuw tabblad en navigeer naar [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card). Inspect de onderliggende inhoudsknooppunten `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` om de inhoud van de `Card` component te zoeken.

   ![Eigenschappen van de component CRXDE-Lite](assets/extend-component/crxde-lite-properties.png)

   Let op: eigenschappen `cardPath`, `ctaText`, `titleFromPage` blijven bestaan in het dialoogvenster.

## Model voor kaartverkoop bijwerken

Als u uiteindelijk de waarden van het dialoogvenster met componenten toegankelijk wilt maken voor de hoekcomponent, moet het Sling-model dat de JSON voor de `Card` component vult, worden bijgewerkt. Wij hebben ook de kans om twee stukken bedrijfslogica uit te voeren:

* Als `titleFromPage` de waarde **true** is, wordt de titel van de pagina geretourneerd die `cardPath` anders wordt opgegeven. Retourneert de waarde van `cardTitle` textfield.
* Retourneer de laatste gewijzigde datum van de pagina die is opgegeven door `cardPath`.

Keer terug naar winde van uw keus en open de `core` module.

1. Open het bestand `Card.java` om `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`.

   Merk op dat de `Card` interface momenteel uitbreidt `com.adobe.cq.wcm.core.components.models.Image` en daarom alle methodes van de `Image` interface erft. De `Image` interface breidt reeds de `ComponentExporter` interface uit die het het Verdelen Model om als JSON toestaat worden uitgevoerd en door de redacteur van het KUUROORD in kaart gebracht. Daarom te hoeven wij om geen interface uitdrukkelijk uit te breiden zoals wij in het hoofdstuk `ComponentExporter` van de Component van de [](custom-component.md)Douane deden.

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

   Deze methoden worden beschikbaar gesteld via de JSON-model-API en doorgegeven aan de hoekcomponent.

3. Open `CardImpl.java`. Dit is de implementatie van `Card.java` interface. Deze implementatie is al gedeeltelijk stopgezet om de zelfstudie te versnellen.  Let op het gebruik van de `@Model` en `@Exporter` annotaties om ervoor te zorgen dat het verkoopmodel met serienummering kan worden gecodeerd als JSON via de verkoopmodel-exportfunctie.

   `CardImpl.java` gebruikt ook het patroon van de [Delegatie voor het Verdelen Modellen](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) om te vermijden herschrijvend alle logica van de kerncomponent van het Beeld.

4. Neem de volgende regels in acht:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   Met de bovenstaande annotatie wordt een afbeeldingsobject met de naam geïnstantieerd `image` op basis van de `sling:resourceSuperType` overerving van de `Card` component.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   Het is dan mogelijk om het `image` voorwerp eenvoudig te gebruiken om methodes uit te voeren die door de `Image` interface worden bepaald, zonder het moeten zelf de logica schrijven. Deze techniek wordt gebruikt voor `getSrc()`, `getAlt()` en `getTitle()`.

5. Implementeer vervolgens de `initModel()` methode voor het initiëren van een variabele van het type private `cardPage` op basis van de waarde van `cardPath`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   Het `@PostConstruct initModel()` wordt altijd aangeroepen wanneer het Sling-model wordt geïnitialiseerd. Het is daarom een goede gelegenheid om objecten te initialiseren die door andere methoden in het model kunnen worden gebruikt. Het `pageManager` is een van een aantal door [Java ondersteunde globale objecten](https://docs.adobe.com/content/help/en/experience-manager-htl/using/htl/global-objects.html#java-backed-objects) die via de annotatie ter beschikking worden gesteld aan Sling Models `@ScriptVariable` . De methode [getPage](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/PageManager.html#getPage-java.lang.String-) neemt een pad in en retourneert een AEM [Page](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/Page.html) -object of null als het pad niet naar een geldige pagina wijst.

   Hierdoor wordt de `cardPage` variabele geïnitialiseerd. Deze wordt door de andere nieuwe methoden gebruikt om gegevens over de onderliggende gekoppelde pagina te retourneren.

6. Bekijk de algemene variabelen die al zijn toegewezen aan de JCR-eigenschappen die in het dialoogvenster met auteurs zijn opgeslagen. De `@ValueMapValue` annotatie wordt gebruikt om de toewijzing automatisch uit te voeren.

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

   Deze variabelen zullen worden gebruikt om de extra methodes voor de `Card.java` interface uit te voeren.

7. Voer de extra methodes uit die in de `Card.java` interface worden bepaald:

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
   > U kunt de [voltooide CardImpl.java hier](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java)bekijken.

8. Open een eindvenster en stel enkel de updates aan de `core` module op gebruikend het Gemaakt `autoInstallBundle` profiel van de `core` folder.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   Als u [AEM 6.x](overview.md#compatibility) gebruikt, voegt u het `classic` profiel toe.

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

   U ziet dat het JSON-model wordt bijgewerkt met extra sleutel-/waardeparen nadat de methoden in het `CardImpl` Sling-model zijn bijgewerkt.

## Hoekcomponent bijwerken

Nu het JSON-model is gevuld met nieuwe eigenschappen voor `ctaLinkURL`, `ctaText``cardTitle` en `cardLastModified` kunnen we de hoekcomponent bijwerken en deze weergeven.

1. Keer aan winde terug en open de `ui.frontend` module. Start eventueel de webpack-ontwikkelserver vanuit een nieuw terminalvenster om de wijzigingen in real-time te zien:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. Openen `card.component.ts` om `ui.frontend/src/app/components/card/card.component.ts`. Voeg de aanvullende `@Input` annotaties toe om het nieuwe model vast te leggen:

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

3. Voeg methodes toe om te controleren of de Vraag aan Actie klaar is en voor het terugkeren van een datum/tijdkoord dat op de `cardLastModified` input wordt gebaseerd:

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

4. Open `card.component.html` en voeg de volgende prijsverhoging toe om de titel, de vraag aan actie en laatste gewijzigde datum te tonen:

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

   Er zijn al sorteerregels toegevoegd om de titel, de aanroep van de handeling en de datum van de laatste wijziging op te maken. `card.component.scss`

   >[!NOTE]
   >
   > U kunt hier [de voltooide code van de](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card)hoekkaart bekijken.

5. Implementeer de volledige wijzigingen in AEM vanuit de hoofdmap van het project met Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. Ga naar [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html) om de bijgewerkte component te bekijken:

   ![Bijgewerkte kaartcomponent in AEM](assets/extend-component/updated-card-in-aem.png)

7. U moet de bestaande inhoud opnieuw kunnen ontwerpen om een pagina te maken die lijkt op het volgende:

   ![Definitief ontwerpen van kaartcomponent](assets/extend-component/final-authoring-card.png)

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, hebt u geleerd hoe u een AEM component kunt uitbreiden met de code en hoe de Sling-modellen en -dialoogvensters werken met het JSON-model.

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) altijd bekijken of de code plaatselijk controleren door aan de tak te schakelen `Angular/extend-component-solution`.
