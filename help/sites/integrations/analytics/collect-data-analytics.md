---
title: Paginagegevens verzamelen met Adobe Analytics
description: Gebruik de gebeurtenisgestuurde Adobe Client Data-laag om gegevens over gebruikersactiviteit te verzamelen op een website die is gemaakt met Adobe Experience Manager. Leer hoe u tagregels gebruikt om naar deze gebeurtenissen te luisteren en gegevens naar een Adobe Analytics-rapportenpakket te verzenden.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
source-git-commit: 6a5e62a2a897adc421585e79c5f36f6aa759feaa
workflow-type: tm+mt
source-wordcount: '2447'
ht-degree: 1%

---

# Paginagegevens verzamelen met Adobe Analytics

>[!NOTE]
>
>Adobe Experience Platform Launch is omgedoopt tot een reeks technologieën voor gegevensverzameling in Adobe Experience Platform. Diverse terminologische wijzigingen zijn als gevolg hiervan in de productdocumentatie doorgevoerd. Raadpleeg het volgende: [document](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) voor een geconsolideerde referentie van de terminologische wijzigingen.


Leer hoe u de ingebouwde functies van de [Adobe Clientgegevenslaag met AEM kerncomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html) om gegevens over een pagina in Adobe Experience Manager Sites te verzamelen. [Labels in Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) en de [Adobe Analytics-extensie](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) worden gebruikt om regels te maken voor het verzenden van paginagegevens naar Adobe Analytics.

## Wat u gaat bouwen {#what-build}

![Paginagegevens bijhouden](assets/collect-data-analytics/analytics-page-data-tracking.png)

In dit leerprogramma, gaat u een markeringsregel teweegbrengen die op een gebeurtenis van de Laag van de Gegevens van de Cliënt van Adobe wordt gebaseerd. Ook, voeg voorwaarden toe voor wanneer de regel zou moeten worden in werking gesteld, en verzend dan **Paginanaam** en **Paginasjabloon** waarden van een AEM Page naar Adobe Analytics.

### Doelstellingen {#objective}

1. Maak een gebeurtenisgestuurde regel in de eigenschap tag die wijzigingen vastlegt vanaf de gegevenslaag
1. Eigenschappen van paginalagen toewijzen aan gegevenselementen in de eigenschap Tag
1. Paginagegevens verzamelen en naar Adobe Analytics verzenden met behulp van het paginaweergavebaken

## Vereisten

Hiervoor is het volgende vereist:

* **Tag, eigenschap** in Experience Platform
* **Adobe Analytics** test/dev rapportsuite-id en trackingserver. Zie de volgende documentatie voor [een rapportenpakket maken](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Foutopsporing Experience Platform](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) browserextensie. Screenshots in deze zelfstudie werden vastgelegd vanuit de Chrome-browser.
* (Optioneel) AEM Site met de [Gegevenslaag Adobe-client ingeschakeld](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). In deze zelfstudie wordt het publiek onder ogen gezien [WKND](https://wknd.site/us/en.html) maar u kunt uw eigen site graag gebruiken.

>[!NOTE]
>
> Hebt u hulp nodig bij het integreren van eigenschap tag en AEM site? [Zie deze videoreeks](../experience-platform/data-collection/tags/overview.md).

## Omgeving van Switch-tag voor WKND-site

De [WKND](http://wknd.site/us/en.html) is een openbare georiënteerde locatie die is gebaseerd op [een open-source-project](https://github.com/adobe/aem-guides-wknd) ontworpen als referentie en [zelfstudie](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) voor een AEM uitvoering.

In plaats van het opzetten van een AEM milieu en het installeren van de WKND codebasis, kunt u debugger van het Experience Platform gebruiken aan **switch** het leven [WKND-site](http://wknd.site/us/en.html) tot *uw* tag, eigenschap. U kunt echter uw eigen AEM gebruiken als deze al beschikt over de [Gegevenslaag Adobe-client ingeschakeld](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

1. Aanmelden bij Experience Platform en [een eigenschap Tag maken](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html) (als je dat nog niet hebt gedaan).
1. Zorg ervoor dat een initiële tag JavaScript [bibliotheek is gemaakt](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) en worden gepromoot naar de tag [milieu](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html).
1. Kopieer de ingesloten JavaScript-code uit de tagomgeving waarin uw bibliotheek is gepubliceerd.

   ![Code ingesloten eigenschap van tag kopiëren](assets/collect-data-analytics/launch-environment-copy.png)

1. Open in uw browser een nieuw tabblad en navigeer naar [WKND-site](http://wknd.site/us/en.html)
1. De browserextensie van Foutopsporing Experience Platform openen

   ![Foutopsporing Experience Platform](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Navigeren naar **Experience Platform-tags** > **Configuratie** en onder **Ingespelde insluitcodes** de bestaande insluitcode vervangen door *uw* ingesloten code die u hebt gekopieerd uit stap 3.

   ![Insluitcode vervangen](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Inschakelen **Logboekregistratie voor console** en **Vergrendelen** het foutopsporingsprogramma op het tabblad WKND.

   ![Logboekregistratie voor console](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Gegevens Adobe client-gegevenslaag op WKND-site verifiëren

De [WKND-referentieproject](https://github.com/adobe/aem-guides-wknd) is gebouwd met AEM Core Components en bevat de [Gegevenslaag Adobe-client ingeschakeld](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) standaard. Daarna, verifieer dat de Laag van Gegevens van de Cliënt van Adobe wordt toegelaten.

1. Navigeren naar [WKND-site](http://wknd.site/us/en.html).
1. Open de ontwikkelaarsgereedschappen van de browser en navigeer naar de **Console**. Voer de volgende opdracht uit:

   ```js
   adobeDataLayer.getState();
   ```

   Boven code keert de huidige staat van de Laag van Gegevens van de Cliënt van Adobe terug.

   ![Status Adobe-gegevenslaag](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Breid de reactie uit en inspecteer `page` vermelding. U zou een gegevensschema als het volgende moeten zien:

   ```json
   page-2eee4f8914:
       @type: "wknd/components/page"
       dc:description: WKND is a collective of outdoors, music, crafts, adventure sports, and travel enthusiasts that want to share our experiences, connections, and expertise with the world.
       dc:title: "WKND Adventures and Travel"
       repo:modifyDate: "2020-08-31T21:02:21Z"
       repo:path: "/content/wknd/us/en.html"
       xdm:language: "en-US"
       xdm:tags: ["Attract"]
       xdm:template: "/conf/wknd/settings/wcm/templates/landing-page-template"
   ```

   Als u paginagegevens naar Adobe Analytics wilt verzenden, gebruiken we de standaardeigenschappen zoals `dc:title`, `xdm:language`, en `xdm:template` van de gegevenslaag.

   Voor meer informatie raadpleegt u de [Paginaschema](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) uit de gegevensschema&#39;s voor kerncomponenten.

   >[!NOTE]
   >
   > Als u de `adobeDataLayer` JavaScript-object? Zorg ervoor dat de [Adobe Client Data Layer is ingeschakeld](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) op uw site.

## Een regel maken voor het laden van pagina&#39;s

De gegevenslaag van de Cliënt van Adobe is een **event** gestuurde gegevenslaag. Wanneer de gegevenslaag AEM pagina is geladen, wordt een `cmp:show` gebeurtenis. Maak een regel die wordt geactiveerd wanneer de `cmp:show` gebeurtenis wordt geactiveerd vanaf de paginalaag.

1. Navigeer naar het Experience Platform en naar de eigenschap tag die is geïntegreerd met de AEM Site.
1. Ga naar de **Regels** in de gebruikersinterface van de eigenschap Tag en klik vervolgens op **Nieuwe regel maken**.

   ![Regel maken](assets/collect-data-analytics/analytics-create-rule.png)

1. Naam van de regel **Pagina geladen**.
1. In de **Gebeurtenissen** subsectie, klikken **Toevoegen** om de **Gebeurtenisconfiguratie** wizard.
1. Voor **Type gebeurtenis** veld, selecteren **Aangepaste code**.

   ![Geef de regel een naam en voeg de gebeurtenis van de aangepaste code toe](assets/collect-data-analytics/custom-code-event.png)

1. Klikken **Editor openen** in het hoofddeelvenster en voer het volgende codefragment in:

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger the Tag Rule and pass event
         console.log("cmp:show event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Tag Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Tag data elements
         // i.e `event.component['someKey']`
         trigger(event);
      }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:show` and callback to the `pageShownEventHandler` function
      dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

   Het bovenstaande codefragment voegt een gebeurtenislistener toe door [een functie duwen](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) in de gegevenslaag. Wanneer `cmp:show` gebeurtenis wordt geactiveerd `pageShownEventHandler` functie wordt aangeroepen. In deze functie worden een paar gezondheidscontroles toegevoegd en een nieuwe `event` is samengesteld met de meest recente [status van de gegevenslaag](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) voor de component die de gebeurtenis heeft geactiveerd.

   Tot slot `trigger(event)` functie wordt aangeroepen. De `trigger()` function is a reserved name in the tag property and it **triggers** de regel. De `event` object wordt doorgegeven als een parameter die vervolgens weer wordt weergegeven door een andere gereserveerde naam in de eigenschap tag. Gegevenselementen in de eigenschap tag kunnen nu verwijzen naar verschillende eigenschappen met een codefragment, zoals `event.component['someKey']`.

1. Sla de wijzigingen op.
1. Volgende onder **Handelingen** klikken **Toevoegen** om de **Configuratie van handelingen** wizard.
1. Voor **Type handeling** veld, kies **Aangepaste code**.

   ![Type aangepaste code-actie](assets/collect-data-analytics/action-custom-code.png)

1. Klikken **Editor openen** in het hoofddeelvenster en voer het volgende codefragment in:

   ```js
   console.log("Page Loaded ");
   console.log("Page name: " + event.component['dc:title']);
   console.log("Page type: " + event.component['@type']);
   console.log("Page template: " + event.component['xdm:template']);
   ```

   De `event` object wordt doorgegeven vanuit het `trigger()` wordt aangeroepen in de aangepaste gebeurtenis. Hier `component` is de huidige pagina die van de gegevenslaag wordt afgeleid `getState` in de aangepaste gebeurtenis.

1. Sla de wijzigingen op en voer een [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) in de eigenschap tag om de code naar de [milieu](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) gebruikt op uw AEM Site.

   >[!NOTE]
   >
   > Het kan handig zijn om de [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) om de insluitcode over te schakelen op een **Ontwikkeling** milieu.

1. Navigeer naar uw AEM en open de ontwikkelaarsgereedschappen om de console weer te geven. Vernieuw de pagina en u zou moeten zien dat de consoleberichten zijn geregistreerd:

![Berichten in geladen console van pagina](assets/collect-data-analytics/page-show-event-console.png)

## Gegevenselementen maken

Maak vervolgens verschillende gegevenselementen om verschillende waarden vast te leggen uit de gegevenslaag van de Adobe-client. Zoals in de vorige oefening wordt gezien is het mogelijk om tot de eigenschappen van de gegevenslaag rechtstreeks door douanecode toegang te hebben. Het voordeel van het gebruik van gegevenselementen is dat deze opnieuw kunnen worden gebruikt in verschillende labelregels.

Gegevenselementen worden toegewezen aan de `@type`, `dc:title`, en `xdm:template` eigenschappen.

### Type componentbron

1. Navigeer naar het Experience Platform en naar de eigenschap tag die is geïntegreerd met de AEM Site.
1. Ga naar de **Gegevenselementen** en klik op **Nieuw gegevenselement maken**.
1. Voor de **Naam** veld, voert u de **Type componentbron**.
1. Voor de **Type gegevenselement** veld, selecteren **Aangepaste code**.

   ![Type componentbron](assets/collect-data-analytics/component-resource-type-form.png)

1. Klikken **Editor openen** en voert u het volgende in de aangepaste code-editor in:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

1. Sla de wijzigingen op.

   >[!NOTE]
   >
   > Herinnert eraan dat de `event` object beschikbaar wordt gemaakt en het bereik wordt ingesteld op basis van de gebeurtenis die het **Regel** in tag-eigenschap. De waarde van een gegevenselement wordt pas ingesteld wanneer het gegevenselement *gerefereerd* binnen een regel. Daarom is het veilig om dit Element van Gegevens binnen een Regel als te gebruiken **Pagina geladen** regel die in de vorige stap is gemaakt *maar* zou niet veilig zijn om in andere contexten te gebruiken.

### Paginanaam

1. Klikken **Gegevenselement toevoegen** knop
1. Voor de **Naam** veld, Enter **Paginanaam**.
1. Voor de **Type gegevenselement** veld, selecteren **Aangepaste code**.
1. Klikken **Editor openen** en voert u het volgende in de aangepaste code-editor in:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. Sla de wijzigingen op.

### Paginasjabloon

1. Klik op de knop **Gegevenselement toevoegen** knop
1. Voor de **Naam** veld, Enter **Paginasjabloon**.
1. Voor de **Type gegevenselement** veld, selecteren **Aangepaste code**.
1. Klikken **Editor openen** en voert u het volgende in de aangepaste code-editor in:

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

1. Sla de wijzigingen op.

1. U moet nu drie gegevenselementen hebben als onderdeel van uw regel:

   ![Gegevenselementen in regel](assets/collect-data-analytics/data-elements-page-rule.png)

## De extensie Analytics toevoegen

Voeg vervolgens de extensie Analytics toe aan de eigenschap Tag om gegevens naar een rapportsuite te verzenden.

1. Navigeer naar het Experience Platform en naar de eigenschap tag die is geïntegreerd met de AEM Site.
1. Ga naar **Extensies** > **Catalogus**
1. Zoek de **Adobe Analytics** extensie en klik op **Installeren**

   ![Adobe Analytics-extensie](assets/collect-data-analytics/analytics-catalog-install.png)

1. Onder **Bibliotheekbeheer** > **Rapportageopties**, voert u de rapportsuite-id&#39;s in die u voor elke tagomgeving wilt gebruiken.

   ![Voer de rapportsuite-id&#39;s in](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > Het is oké om één rapportenreeks voor alle milieu&#39;s in dit leerprogramma te gebruiken, maar in echt zou u afzonderlijke rapportseries willen gebruiken, zoals aangetoond in het hieronder beeld

   >[!TIP]
   >
   >We raden u aan de *De optie Bibliotheek voor mij beheren* als de instelling voor Bibliotheekbeheer het veel eenvoudiger maakt om de `AppMeasurement.js` bibliotheek is bijgewerkt.

1. Schakel het selectievakje in **Activity Map gebruiken**.

   ![Activity Map voor gebruik inschakelen](assets/track-clicked-component/analytic-track-click.png)

1. Onder **Algemeen** > **Trackingserver** voert u bijvoorbeeld uw trackingserver in. `tmd.sc.omtrdc.net`. Voer uw SSL-traceringsserver in als uw site ondersteuning biedt `https://`

   ![De trackingservers invoeren](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Klikken **Opslaan** om de wijzigingen op te slaan.

## Een voorwaarde toevoegen aan de regel Pagina geladen

Werk vervolgens de **Pagina geladen** de regel om **Type componentbron** gegevenselement om ervoor te zorgen dat de regel alleen wordt geactiveerd wanneer de `cmp:show` de gebeurtenis is voor de **Pagina**. Andere componenten kunnen de `cmp:show` bijvoorbeeld door de component Carousel wordt deze geactiveerd wanneer de dia&#39;s veranderen. Daarom is het belangrijk om een voorwaarde voor deze regel toe te voegen.

1. Navigeer in de interface Tageigenschap naar de **Pagina geladen** regel die eerder is gemaakt.
1. Onder **Voorwaarden** klikken **Toevoegen** om de **Condition Configuration** wizard.
1. Voor **Type voorwaarde** veld, selecteren **Waardevergelijking** optie.
1. De eerste waarde in het formulierveld instellen op `%Component Resource Type%`. U kunt het pictogram Gegevenselement gebruiken ![pictogram data-element](assets/collect-data-analytics/cylinder-icon.png) om de **Type componentbron** gegevenselement. Laat de vergelijkingsfunctie ingesteld staan op `Equals`.
1. De tweede waarde instellen op `wknd/components/page`.

   ![Voorwaardenconfiguratie voor regel met geladen pagina](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > Het is mogelijk om deze voorwaarde binnen de functie van de douanecode toe te voegen die op `cmp:show` eerder in de zelfstudie gemaakte gebeurtenis. Nochtans, geeft het toevoegen van het binnen UI meer zicht aan extra gebruikers die veranderingen in de regel zouden kunnen moeten aanbrengen. Bovendien kunnen we ons gegevenselement gebruiken!

1. Sla de wijzigingen op.

## Analysevariabelen instellen en Paginaweergavebaken activeren

Momenteel worden de **Pagina geladen** regel output eenvoudig een consoleverklaring. Gebruik vervolgens de gegevenselementen en de extensie Analytics om de variabelen Analytics in te stellen als een **action** in de **Pagina geladen** regel. We stellen ook een extra actie in om de **Paginaweergavekenmerk** en de verzamelde gegevens naar Adobe Analytics verzenden.

1. In de regel Pagina geladen: **remove** de **Core - Aangepaste code** handeling (de consoleverklaringen):

   ![Aangepaste code verwijderen](assets/collect-data-analytics/remove-console-statements.png)

1. Klik onder de subsectie Handelingen op **Toevoegen** om een nieuwe handeling toe te voegen.

1. Stel de **Extensie** tekst naar **Adobe Analytics** en stelt de **Type handeling** tot  **Variabelen instellen**

   ![Extensie handeling instellen op Variabelen voor analyse instellen](assets/collect-data-analytics/analytics-set-variables-action.png)

1. Selecteer in het hoofdvenster een beschikbare **eVar** en instellen als de waarde van het gegevenselement **Paginasjabloon**. Het pictogram Gegevenselementen gebruiken ![Pictogram Gegevenselementen](assets/collect-data-analytics/cylinder-icon.png) om de **Paginasjabloon** element.

   ![Instellen als eVar paginasjabloon](assets/collect-data-analytics/set-evar-page-template.png)

1. Omlaag schuiven, onder **Aanvullende instellingen** set **Paginanaam** aan het gegevenselement **Paginanaam**:

   ![Omgevingsvariabele paginanaam ingesteld](assets/collect-data-analytics/page-name-env-variable-set.png)

1. Sla de wijzigingen op.

1. Voeg vervolgens een extra handeling toe aan de rechterkant van de knop **Adobe Analytics - Variabelen instellen** door op de **plus** pictogram:

   ![Een extra handeling voor een tagregel toevoegen](assets/collect-data-analytics/add-additional-launch-action.png)

1. Stel de **Extensie** tekst naar **Adobe Analytics** en stelt de **Type handeling** tot  **Band verzenden**. Aangezien deze actie als een paginaweergave wordt beschouwd, laat u de standaardtracering ingesteld op **`s.t()`**.

   ![Handeling Beacon Adobe Analytics verzenden](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Sla de wijzigingen op. De **Pagina geladen** de regel zou nu de volgende configuratie moeten hebben:

   ![Definitieve configuratie van tagregels](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Luister naar de `cmp:show` gebeurtenis.
   * **2.** Controleer of de gebeurtenis door een pagina is geactiveerd.
   * **3.** Analysevariabelen instellen voor **Paginanaam** en **Paginasjabloon**
   * **4.** Verzend het Beacon van de Mening van de Pagina van de Analyse

1. Sla alle wijzigingen op en maak uw tagbibliotheek, waarbij u een upgrade uitvoert naar de juiste omgeving.

## Valideer de oproep Beacon en Analytics voor paginaweergave

Nu **Pagina geladen** regel verzendt het baken van Analytics, zou u de Analytics volgende variabelen moeten kunnen zien gebruikend Foutopsporing van het Experience Platform.

1. Open de [WKND-site](https://wknd.site/us/en.html) in uw browser.
1. Klik op het pictogram Foutopsporing ![Het pictogram Foutopsporing op platform beleven](assets/collect-data-analytics/experience-cloud-debugger.png) om Foutopsporing op Experience Platform te openen.
1. Controleer of Foutopsporing de eigenschap tag toewijst aan *uw* Ontwikkelomgeving, zoals eerder beschreven en **Logboekregistratie voor console** is ingeschakeld.
1. Open het menu Analytics en controleer of de rapportsuite is ingesteld op *uw* rapportsuite. De paginanaam moet ook worden ingevuld:

   ![Foutopsporing op het tabblad Analyse](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Omlaag schuiven en uitvouwen **Netwerkverzoeken**. U zou moeten kunnen vinden **vervagen** voor de **Paginasjabloon**:

   ![Gebeurtenis en paginanaam ingesteld](assets/collect-data-analytics/evar-page-name-set.png)

1. Ga terug naar de browser en open de ontwikkelaarsconsole. Klik door **Carousel** boven aan de pagina.

   ![Klikken door carrouselpagina](assets/collect-data-analytics/click-carousel-page.png)

1. Neem in de browser console de consoleverklaring waar:

   ![Voldoet niet aan voorwaarde](assets/collect-data-analytics/condition-not-met.png)

   De reden hiervoor is dat de Carousel wel een `cmp:show` event *maar* vanwege onze controle van de **Type componentbron**, wordt geen gebeurtenis geactiveerd.

   >[!NOTE]
   >
   > Als u geen consolelogboeken ziet, zorg ervoor dat **Logboekregistratie voor console** is gecontroleerd onder **Experience Platform-tags** in de Foutopsporing van het Experience Platform.

1. Naar een artikelpagina navigeren zoals [Western Australia](https://wknd.site/us/en/magazine/western-australia.html). Bekijk de paginanaam en het sjabloontype worden gewijzigd.

## Gefeliciteerd!

U hebt zojuist de gebeurtenisgestuurde Adobe Client Data Layer en -tags in Experience Platform gebruikt om gegevens over gegevenspagina&#39;s van een AEM Site te verzamelen en deze naar Adobe Analytics te verzenden.

### Volgende stappen

Bekijk de volgende zelfstudie om te leren hoe u de gebeurtenisgestuurde Adobe Client Data-laag kunt gebruiken om [klikken van specifieke componenten op een Adobe Experience Manager-site bijhouden](track-clicked-component.md).
