---
title: Aangeklikte component bijhouden met Adobe Analytics
description: Gebruik de gebeurtenisgestuurde Adobe Client Data-laag om kliks van specifieke componenten op een Adobe Experience Manager-site bij te houden. Leer hoe u regels in Experience Platform Launch gebruikt om naar deze gebeurtenissen te luisteren en gegevens naar een Adobe Analytics te verzenden met een baken voor trackkoppelingen.
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6296
thumbnail: KT-6296.jpg
translation-type: tm+mt
source-git-commit: 97fe98c8c62f5472f7771bbc803b2a47dc97044d
workflow-type: tm+mt
source-wordcount: '1773'
ht-degree: 1%

---


# Aangeklikte component bijhouden met Adobe Analytics

Gebruik de gebeurtenis-gedreven Laag van de Gegevens van de Cliënt van [Adobe met AEM Componenten](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html) van de Kern om kliks van specifieke componenten op een plaats van Adobe Experience Manager te volgen. Leer hoe te om regels in Experience Platform Launch te gebruiken om op klikgebeurtenissen te luisteren, filter door component en verzend de gegevens naar een Adobe Analytics met een baken van de spoorverbinding.

## Wat u gaat maken

Het WKND marketing team wil begrijpen welke Vraag aan de knopen van de Actie (CTA) het beste op de homepage uitvoert. In deze zelfstudie zullen we een nieuwe regel in Experience Platform Launch toevoegen die luistert naar `cmp:click` gebeurtenissen van **Taser** - en **Button** -componenten en die de component-id en een nieuwe gebeurtenis naast het spoorkoppelingsbaken naar Adobe Analytics verzendt.

![Wat u spoorklikken zult bouwen](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Doelstellingen {#objective}

1. Maak een gebeurtenisgestuurde regel in Launch op basis van de `cmp:click` gebeurtenis.
1. Filter de verschillende gebeurtenissen op componentenmiddeltype.
1. Stel de component-id waarop wordt geklikt in en verzend de gebeurtenis Adobe Analytics met het trackkoppelingsbaken.

## Vereisten

Deze zelfstudie is een vervolg op Pagina-gegevens [verzamelen met Adobe Analytics](./collect-data-analytics.md) en gaat ervan uit dat u het volgende hebt:

* Een **opstarteigenschap** met de [Adobe Analytics-extensie](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) ingeschakeld
* **Adobe Analytics** test/dev rapport suite ID and tracking server. Raadpleeg de volgende documentatie voor het [maken van een nieuwe rapportsuite](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [De browser van Foutopsporing](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) van het Experience Platform uitbreiding die met uw bezit van de Lancering wordt gevormd die op [https://wknd.site/us/en.html](https://wknd.site/us/en.html) of een AEM plaats met de toegelaten Laag van Gegevens van Adobe wordt geladen.

## Inspect the Button and Teaser Schema

Alvorens regels in Lancering te maken is het nuttig om het [schema voor de Knoop en de Taser](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item) te herzien en hen in de implementatie van de gegevenslaag te inspecteren.

1. Ga naar [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Open de ontwikkelaarsgereedschappen van de browser en navigeer naar de **Console**. Voer de volgende opdracht uit:

   ```js
   adobeDataLayer.getState();
   ```

   Dit keert de huidige staat van de Laag van Gegevens van de Cliënt van Adobe terug.

   ![Toestand gegevenslaag via browserconsole](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. Vouw de reactie uit en zoek de items die vooraf aan `button-` en `teaser-xyz-cta` invoer zijn toegewezen. U zou een gegevensschema als het volgende moeten zien:

   Knopschema:

   ```json
   button-2e6d32893a:
       @type: "wknd/components/button"
       dc:title: "View All"
       parentId: "page-2eee4f8914"
       repo:modifyDate: "2020-07-11T22:17:55Z"
       xdm:linkURL: "/content/wknd/us/en/magazine.html"
   ```

   Teaserschema:

   ```json
   teaser-da32481ec8-cta-adf3c09db9:
       @type: "nt:unstructured"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   Deze zijn gebaseerd op het Schema [van het Punt van de](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item)Component/van de Container. De regel die we in Launch maken, gebruikt dit schema.

## Een CTA-regel waarop wordt geklikt maken

De gegevenslaag van de Gegevens van de Cliënt van Adobe is een **gebeurtenis** gedreven gegevenslaag. Wanneer op een Core-component wordt geklikt, wordt een `cmp:click` gebeurtenis verzonden via de gegevenslaag. Maak vervolgens een regel om naar de `cmp:click` gebeurtenis te luisteren.

1. Navigeer aan Experience Platform Launch en in het bezit van het Web dat met de Plaats van de AEM wordt geïntegreerd.
1. Navigeer naar de sectie **Regels** in de gebruikersinterface van de Lancering en klik vervolgens op **Regel** toevoegen.
1. Noem de regel **CTA geklikt**.
1. Klik op **Gebeurtenissen** > **Toevoegen** om de wizard **Gebeurtenisconfiguratie** te openen.
1. Selecteer onder **Type** gebeurtenis de optie **Aangepaste code**.

   ![Noem de regel CTA klikte en voeg de gebeurtenis van de douanecode toe](assets/track-clicked-component/custom-code-event.png)

1. Klik op Editor **** openen in het hoofddeelvenster en voer het volgende codefragment in:

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Launch Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
         // i.e `event.component['someKey']`
         trigger(event);
      }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   //push the event listener for cmp:click into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
      dl.addEventListener("cmp:click", componentClickedHandler);
   });
   ```

   Het bovenstaande codefragment voegt een gebeurtenislistener toe door een functie [in de gegevenslaag te](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) duwen. Wanneer de `cmp:click` gebeurtenis wordt geactiveerd, wordt de `componentClickedHandler` functie aangeroepen. In deze functie worden enkele controles van de hygiëne toegevoegd en wordt een nieuw `event` object samengesteld met de meest recente [status van de gegevenslaag](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) voor de component die de gebeurtenis heeft geactiveerd.

   Daarna `trigger(event)` wordt het genoemd. `trigger()` is een gereserveerde naam in Launch en activeert &quot;de Launch-regel&quot;. We geven het `event` object door als een parameter die vervolgens wordt weergegeven door een andere gereserveerde naam in Launch met de naam `event`. Data Elements in Launch kan nu verwijzen naar verschillende eigenschappen, zoals: `event.component['someKey']`.

1. Sla de wijzigingen op.
1. Klik onder **Handelingen** op **Toevoegen** om de wizard **Configuratie** handeling te openen.
1. Kies bij Type **handeling** de optie **Aangepaste code**.

   ![Type aangepaste code-actie](assets/track-clicked-component/action-custom-code.png)

1. Klik op Editor **** openen in het hoofddeelvenster en voer het volgende codefragment in:

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   Het `event` object wordt doorgegeven via de `trigger()` methode die in de aangepaste gebeurtenis wordt aangeroepen. `component` is de huidige staat van de component die uit de gegevenslaag wordt afgeleid `getState` die de klik teweegbracht.

1. Sla de wijzigingen op en voer een [build](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html) uit in Launch om de code te promoten in de [omgeving](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html) die op uw AEM-site wordt gebruikt.

   >[!NOTE]
   >
   > Het kan zeer nuttig zijn om Foutopsporing [van](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) Adobe Experience Platform te gebruiken om de ingebedde code aan een milieu van de **Ontwikkeling** te schakelen.

1. Navigeer naar de [WKND-site](https://wknd.site/us/en.html) en open de ontwikkelaarsgereedschappen om de console weer te geven. Selecteer **Logbestand** behouden.

1. Klik op een van de knoppen **Taser** of **Button** CTA om naar een andere pagina te navigeren.

   ![CTA-knop om te klikken](assets/track-clicked-component/cta-button-to-click.png)

1. Merk in de ontwikkelaarsconsole op dat de **CTA geklikte** regel is in brand gestoken:

   ![CTA-knop geklikt](assets/track-clicked-component/cta-button-clicked-log.png)

## Gegevenselementen maken

Maak vervolgens gegevenselementen om de component-id en de titel vast te leggen waarop is geklikt. Tijdens de vorige oefening was de uitvoer van `event.path` iets gelijkaardigs `component.button-b6562c963d` en de waarde van `event.component['dc:title']` was iets als &quot;Trips weergeven&quot;.

### Component-id

1. Navigeer aan Experience Platform Launch en in het bezit van het Web dat met de Plaats van de AEM wordt geïntegreerd.
1. Navigeer naar de sectie **Gegevenselementen** en klik op **Nieuw gegevenselement** toevoegen.
1. Voer bij **Naam** de **component-id** in.
1. Selecteer **Aangepaste code** bij Type **** gegevenselement.

   ![Formulier Component ID-gegevenselement](assets/track-clicked-component/component-id-data-element.png)

1. Klik op Editor **** openen en voer het volgende in de aangepaste code-editor in:

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   Sla de wijzigingen op.

   >[!NOTE]
   >
   > Rappel dat het `event` voorwerp beschikbaar wordt gemaakt en scoped gebaseerd op de gebeurtenis die de **Regel** in Lancering teweegbracht. De waarde van een gegevenselement wordt niet geplaatst tot het Element van Gegevens binnen een Regel *van verwijzingen* wordt voorzien. Daarom is het veilig om dit Element van Gegevens binnen een Regel zoals de **CTA geklikte** regel te gebruiken die in de vorige oefening wordt gecreeerd *maar* zou niet veilig aan gebruik in andere contexten zijn.

### Componenttitel

1. Navigeer naar de sectie **Gegevenselementen** en klik op **Nieuw gegevenselement** toevoegen.
1. Voer bij **Naam** de **componenttitel** in.
1. Selecteer **Aangepaste code** bij Type **** gegevenselement.
1. Klik op Editor **** openen en voer het volgende in de aangepaste code-editor in:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Sla de wijzigingen op.

## Voeg een voorwaarde aan de CTA geklikte regel toe

Werk daarna de **CTA geklikte** regel bij om ervoor te zorgen dat de regel slechts in brand steekt wanneer de `cmp:click` gebeurtenis voor een **Taser** of een **Knoop** wordt in brand gestoken. Aangezien Taser&#39;s CTA als een afzonderlijk object in de gegevenslaag wordt beschouwd, is het belangrijk om het bovenliggende element te controleren om te controleren of het van een Taser afkomstig is.

1. Navigeer in de gebruikersinterface van Launch naar de regel **Pagina die eerder is geladen** .
1. Klik onder **Voorwaarden** op **Toevoegen** om de wizard **Voorwaardelijke configuratie** te openen.
1. Selecteer **Aangepaste code** bij Type **** voorwaarde.

   ![Aangepaste code voor CTA waarop is geklikt](assets/track-clicked-component/custom-code-condition.png)

1. Klik op Editor **** openen en voer het volgende in de aangepaste code-editor in:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       //Check for Button Type
       if(event.component['@type'] === 'wknd/components/button') {
           return true;
       } else if (event.component['@type'] == 'nt:unstructured') {
           // Check for CTA inside a Teaser
           var parentComponentId = event.component['parentId'];
           var parentComponent = window.adobeDataLayer.getState('component.' + parentComponentId);
   
           if(parentComponent['@type'] === 'wknd/components/teaser') {
               return true;
           }
       }
   }
   
   return false;
   ```

   De bovenstaande code controleert eerst om te zien of was het middeltype van een **Knoop** en controleert dan om te zien of was het middeltype van CTA binnen een **Taser**.

1. Sla de wijzigingen op.

## Analysevariabelen instellen en Track Link Beacon activeren

Momenteel voert de **CTA geklikte** regel eenvoudig een consoleverklaring uit. Gebruik vervolgens de gegevenselementen en de extensie Analytics om de variabelen Analytics in te stellen als een **handeling**. We stellen ook een aanvullende actie in om de **Track Link** te activeren en de verzamelde gegevens naar Adobe Analytics te verzenden.

1. In de **Pagina Geladen** regel **verwijdert** de actie **Kern - de Douane Code** (de consoleverklaringen):

   ![Aangepaste code verwijderen](assets/track-clicked-component/remove-console-statements.png)

1. Klik onder Handelingen op **Toevoegen** om een nieuwe handeling toe te voegen.
1. Stel het **extensietype** in op **Adobe Analytics** en stel het **actietype** in op Variabelen **** instellen.

1. Stel de volgende waarden in voor **Vars**, **Props** en **Gebeurtenissen**:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8` - `CTA Clicked`

   ![Prop en gebeurtenissen voor eVar instellen](assets/track-clicked-component/set-evar-prop-event.png)

1. Voeg vervolgens een aanvullende handeling toe aan de rechterkant van de **Adobe Analytics - Variabelen** instellen door op het **plusteken** te tikken:

   ![Een extra opstarthandeling toevoegen](assets/track-clicked-component/add-additional-launch-action.png)

1. Stel het **extensietype** in op **Adobe Analytics** en stel het **actietype** in op **Verzendbaken**.
1. Onder **Volgen** stelt u het keuzerondje in op **`s.tl()`**.
1. Kies bij **Koppelingstype** de optie **Aangepaste koppeling** en stel bij Naam **** koppeling de waarde in op de titel **van de** component van het gegevenselement:

   ![Configuratie voor het baken van de Verbinding verzenden](assets/track-clicked-component/analytics-send-beacon-link-track.png)

1. Sla de wijzigingen op. De **CTA geklikt** regel zou nu de volgende configuratie moeten hebben:

   ![Definitieve opstartconfiguratie](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Luister naar de `cmp:click` gebeurtenis.
   * **2.** Controleer of de gebeurtenis is geactiveerd door een **Button** of **Teaser**.
   * **3.** Stel de variabelen Analytics in voor om de **Component ID** bij te houden als een **eVar**, **prop** en een **gebeurtenis**.
   * **4.** Verzend het Beacon van de Verbinding van het Spoor Analytics (en behandelt het **niet** als paginamening).

1. Sla alle wijzigingen op en maak uw opstartbibliotheek, waarbij u een upgrade uitvoert naar de juiste omgeving.

## Valideer de vraag van het Beacon en van de Analyse van de Verbinding van het Spoor

Nu de **CTA geklikte** regel het baken van Analytics verzendt, zou u de variabelen moeten kunnen zien die van Analytics gebruikend Foutopsporing van het Experience Platform volgen.

1. Open de [WKND-site](https://wknd.site/us/en.html) in uw browser.
1. Klik op het pictogram Foutopsporing op het pictogram Foutopsporing van ![het ervaringsplatform](assets/track-clicked-component/experience-cloud-debugger.png) om Foutopsporing op Experience Platform te openen.
1. Zorg ervoor Debugger het bezit van de Lancering aan *uw* milieu van de Ontwikkeling in kaart brengt, zoals vroeger beschreven en het Registreren van de **Console** wordt gecontroleerd.
1. Open het menu Analytics en controleer of de rapportsuite is ingesteld op *uw* rapportsuite.

   ![Foutopsporing op het tabblad Analyse](assets/track-clicked-component/analytics-tab-debugger.png)

1. Klik in de browser op een van de **Taser** - of **Button** CTA-knoppen om naar een andere pagina te navigeren.

   ![CTA-knop om te klikken](assets/track-clicked-component/cta-button-to-click.png)

1. Ga terug naar Foutopsporing van het Experience Platform en schuif neer en breid **Netwerkverzoeken** > *Uw Reeks* van het Rapport uit. U zou de **eVar**, **steun**, en **gebeurtenisreeks** moeten kunnen vinden.

   ![Analytische gebeurtenissen, evar en prop bijgehouden bij klikken](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Ga terug naar de browser en open de ontwikkelaarsconsole. Navigeer naar de voettekst van de site en klik op een van de navigatiekoppelingen:

   ![Klik op Navigatiekoppeling in de voettekst](assets/track-clicked-component/click-navigation-link-footer.png)

1. Neem in de browser console waar het bericht *&quot;Douane Code&quot;voor regel &quot;CTA Clicked&quot;niet werd ontmoet*.

   Dit komt doordat de navigatiecomponent wel een `cmp:click` gebeurtenis activeert, *maar* er wordt geen actie ondernomen vanwege de controle van de gebeurtenis met het middeltype.

   >[!NOTE]
   >
   > Als u geen consolelogboeken ziet, zorg ervoor dat het Registreren van de **Console** onder **Lancering** in Foutopsporing van het Experience Platform wordt gecontroleerd.

## Gefeliciteerd!

U gebruikte enkel de gebeurtenis-gedreven Laag en het Experience Platform Launch van Gegevens van de Cliënt van de Adobe om kliks van specifieke componenten op een plaats van Adobe Experience Manager te volgen.