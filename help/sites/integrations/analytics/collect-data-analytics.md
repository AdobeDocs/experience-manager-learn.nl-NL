---
title: Paginagegevens verzamelen met Adobe Analytics
description: Gebruik de gebeurtenisgestuurde Adobe Client Data-laag om gegevens over gebruikersactiviteit te verzamelen op een website die is gemaakt met Adobe Experience Manager. Leer hoe u regels in Experience Platform Launch gebruikt om naar deze gebeurtenissen te luisteren en gegevens naar een Adobe Analytics-rapportenpakket te verzenden.
version: cloud-service
topic: Integrations
feature: Gegevenslaag Adobe-client
role: Developer
level: Intermediate
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '2378'
ht-degree: 1%

---


# Paginagegevens verzamelen met Adobe Analytics

Leer om de ingebouwde eigenschappen van [de Laag van Gegevens van de Adobe Cliënt met AEM Core Componenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html) te gebruiken om gegevens over een pagina in Adobe Experience Manager Sites te verzamelen. [Experience Platform ](https://www.adobe.com/experience-platform/launch.html) Launchand de  [Adobe Analytics-](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html) extensie wordt gebruikt om regels te maken voor het verzenden van paginagegevens naar Adobe Analytics.

## Wat u gaat maken

![Paginagegevens bijhouden](assets/collect-data-analytics/analytics-page-data-tracking.png)

In dit leerprogramma zult u een regel van de Lancering teweegbrengen die op een gebeurtenis van de Laag van de Gegevens van de Cliënt van Adobe wordt gebaseerd, toevoegt voorwaarden voor wanneer de regel zou moeten worden in brand gestoken, en verzendt **Paginanaam** en **Paginamalplaatje** van een AEM Pagina aan Adobe Analytics.

### Doelstellingen {#objective}

1. Creeer een gebeurtenis-gedreven regel in Lancering die op veranderingen in de gegevenslaag wordt gebaseerd
1. Eigenschappen van paginalaag toewijzen aan gegevenselementen in Starten
1. Paginagegevens verzamelen en naar Adobe Analytics verzenden met het paginaweergavebaken

## Vereisten

Hiervoor is het volgende vereist:

* **Experience Platform** LaunchProperty
* **Adobe** AnalyticSnelst/dev-rapportsuite-id en trackingserver. Zie de volgende documentatie voor [het creëren van een nieuwe rapportreeks](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Experience Platform ](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) Foutopsporing browser extensie. Screenshots in deze zelfstudie werden vastgelegd vanuit de Chrome-browser.
* (Optioneel) AEM Site met de [Adobe Client Data Layer ingeschakeld](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). In deze zelfstudie wordt de openbare website [https://wknd.site/us/en.html](https://wknd.site/us/en.html) gebruikt, maar u kunt uw eigen site graag gebruiken.

>[!NOTE]
>
> Hebt u hulp nodig bij het integreren van Starten en uw AEM site? [Zie deze videoreeks](../experience-platform-launch/overview.md).

## Overschakelen van opstartomgevingen voor WKND-site

[https://wknd.](https://wknd.site) site is een openbare site die is gebouwd op basis van  [een open-source-](https://github.com/adobe/aem-guides-wknd) project dat is ontworpen als referentie en  [](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) zelfstudie voor AEM implementaties.

In plaats van het opzetten van een AEM milieu en het installeren van de WKND codebasis, kunt u het debugger van het Experience Platform aan **switch** levende [https://wknd.site/](https://wknd.site/) aan *uw* bezit van de Lancering gebruiken. Natuurlijk kunt u uw eigen AEM gebruiken als de [Adobe Client Data Layer is ingeschakeld](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)

1. Login aan Experience Platform Launch en [creeer een Bezit van de Lancering](https://experienceleague.adobe.com/docs/launch-learn/implementing-in-websites-with-launch/configure-launch/launch.html) (als u nog niet hebt).
1. Zorg ervoor dat een initiële Starten [Bibliotheek is gecreeerd](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) en aan een Starten [milieu](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments.html) bevorderd.
1. Kopieer de insluitcode voor Starten vanuit de omgeving waarnaar de bibliotheek is gepubliceerd.

   ![Insluitcode starten kopiëren](assets/collect-data-analytics/launch-environment-copy.png)

1. Open in uw browser een nieuw tabblad en navigeer naar [https://wknd.site/](https://wknd.site/)
1. De browserextensie van Foutopsporing Experience Platform openen

   ![Foutopsporing Experience Platform](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Navigeer naar **Launch** > **Configuration** en vervang onder **Ingespoten Embed Codes** de bestaande Insluitcode van de Lancering door *uw* ingebedde code die u uit stap 3 hebt gekopieerd.

   ![Insluitcode vervangen](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Schakel **Console Logging** en **Lock** op het foutopsporingsprogramma op het tabblad WKND in.

   ![Logboekregistratie voor console](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Gegevens Adobe client-gegevenslaag op WKND-site verifiëren

Het [WKND project van de Verwijzing](https://github.com/adobe/aem-guides-wknd) wordt gebouwd met AEM Componenten van de Kern en heeft [de Laag van Gegevens van de Cliënt van de Adobe toegelaten ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) door gebrek. Daarna, verifieer de Laag van Gegevens van de Cliënt van Adobe wordt toegelaten.

1. Navigeer naar [https://wknd.site](https://wknd.site).
1. Open de ontwikkelaarsgereedschappen van de browser en navigeer naar de **Console**. Voer de volgende opdracht uit:

   ```js
   adobeDataLayer.getState();
   ```

   Dit keert de huidige staat van de Laag van Gegevens van de Cliënt van Adobe terug.

   ![Status Adobe-gegevenslaag](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Breid de reactie uit en inspecteer de `page` ingang. U zou een gegevensschema als het volgende moeten zien:

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

   We gebruiken standaardeigenschappen die zijn afgeleid van het [Paginaschema](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page), `dc:title`, `xdm:language` en `xdm:template` van de gegevenslaag om paginagegevens naar Adobe Analytics te verzenden.

   >[!NOTE]
   >
   > Ziet u het javascript-object `adobeDataLayer` niet? Zorg ervoor dat de [Adobe Client Data Layer is ingeschakeld](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) op uw site.

## Een regel maken voor het laden van pagina&#39;s

De gegevenslaag van de Gegevens van de Cliënt van Adobe is een **gebeurtenis** gedreven gegevenslaag. Wanneer de AEM **Page** gegevenslaag wordt geladen, wordt een gebeurtenis `cmp:show` geactiveerd. Maak een regel die op basis van de gebeurtenis `cmp:show` wordt geactiveerd.

1. Navigeer aan Experience Platform Launch en in het bezit van het Web dat met de Plaats van de AEM wordt geïntegreerd.
1. Navigeer naar de sectie **Rules** in de gebruikersinterface van de Lancering en klik vervolgens **Nieuwe regel maken**.

   ![Regel maken](assets/collect-data-analytics/analytics-create-rule.png)

1. Geef de regel **Geladen pagina** een naam.
1. Klik **Gebeurtenissen** **Toevoegen** om de wizard **Gebeurtenisconfiguratie** te openen.
1. Selecteer **Aangepaste code** onder **Type gebeurtenis**.

   ![Geef de regel een naam en voeg de gebeurtenis van de aangepaste code toe](assets/collect-data-analytics/custom-code-event.png)

1. Klik **Editor openen** in het hoofddeelvenster en voer het volgende codefragment in:

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:show event: " + evt.eventInfo.path);
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
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:show` and callback to the `pageShownEventHandler` function
      dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

   Het bovenstaande codefragment voegt een gebeurtenislistener toe door een functie [in de gegevenslaag te duwen. ](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) Wanneer de gebeurtenis `cmp:show` wordt geactiveerd, wordt de functie `pageShownEventHandler` aangeroepen. In deze functie worden een paar gezondheidscontroles toegevoegd en een nieuw `event` wordt geconstrueerd met de recentste [staat van de gegevenslaag](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) voor de component die de gebeurtenis teweegbracht.

   Nadat `trigger(event)` wordt geroepen. `trigger()` is een gereserveerde naam in Launch en activeert &quot;de Launch-regel&quot;. We geven het object `event` door als een parameter die vervolgens weer wordt vrijgegeven door een andere gereserveerde naam in Launch met de naam `event`. Data Elements in Launch kan nu verwijzen naar verschillende eigenschappen, zoals: `event.component['someKey']`.

1. Sla de wijzigingen op.
1. Vervolgens klikt u onder **Handelingen** op **Toevoegen** om de wizard **Configuratie handeling** te openen.
1. Kies **Aangepaste code** onder **Type handeling**.

   ![Type aangepaste code-actie](assets/collect-data-analytics/action-custom-code.png)

1. Klik **Editor openen** in het hoofddeelvenster en voer het volgende codefragment in:

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   Het object `event` wordt doorgegeven via de methode `trigger()` die in de aangepaste gebeurtenis wordt aangeroepen. `component` Dit is de huidige pagina die wordt afgeleid van de gegevenslaag  `getState` in de aangepaste gebeurtenis. Herhaal van vroeger het [paginaschema](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) dat door de gegevenslaag wordt blootgesteld om de diverse sleutels te zien die uit de doos worden blootgesteld.

1. Sla de wijzigingen op en voer een [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) in Launch uit om de code te promoten naar de [omgeving](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments.html) die op uw AEM-site wordt gebruikt.

   >[!NOTE]
   >
   > Het kan zeer nuttig zijn om [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) te gebruiken om de ingebedde code aan een **ontwikkelings** milieu te schakelen.

1. Navigeer naar uw AEM en open de ontwikkelaarsgereedschappen om de console weer te geven. Vernieuw de pagina en u zou moeten zien dat de consoleberichten zijn geregistreerd:

   ![Berichten in geladen console van pagina](assets/collect-data-analytics/page-show-event-console.png)

## Gegevenselementen maken

Maak vervolgens verschillende gegevenselementen om verschillende waarden vast te leggen uit de gegevenslaag van de Adobe-client. Zoals gezien in de vorige oefening hebben wij het mogelijk gezien om tot de eigenschappen van de gegevenslaag rechtstreeks door douanecode toegang te hebben. Het voordeel van het gebruik van gegevenselementen is dat deze opnieuw kunnen worden gebruikt in alle opstartregels.

Herinneren van vroeger [Paginaschema](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) blootgesteld door de gegevenslaag:

Gegevenselementen worden toegewezen aan de eigenschappen `@type`, `dc:title` en `xdm:template`.

### Type componentbron

1. Navigeer aan Experience Platform Launch en in het bezit van het Web dat met de Plaats van de AEM wordt geïntegreerd.
1. Navigeer naar de sectie **Gegevenselementen** en klik **Nieuw gegevenselement maken**.
1. Voer voor **Naam** **Component Resource Type** in.
1. Voor **Gegevenselement Type** selecteert **Aangepaste code**.

   ![Type componentbron](assets/collect-data-analytics/component-resource-type-form.png)

1. Klik op **Editor openen** en voer het volgende in de aangepaste code-editor in:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

   Sla de wijzigingen op.

   >[!NOTE]
   >
   > Herinnering dat het `event` voorwerp beschikbaar wordt gemaakt en scoped gebaseerd op de gebeurtenis die **Rule** in Lancering teweegbracht. De waarde van een Element van Gegevens wordt niet geplaatst tot het Element van Gegevens *referenced* binnen een Regel is. Daarom is het veilig om dit Element van Gegevens binnen van een Regel zoals **Pagina Geladen** regel te gebruiken die in de vorige stap *maar* wordt gecreeerd zou niet veilig om in andere contexten te gebruiken zijn.

### Paginanaam

1. Klik **Gegevenselement toevoegen**.
1. Typ **Naam** voor **Paginanaam**.
1. Voor **Gegevenselement Type** selecteert **Aangepaste code**.
1. Klik op **Editor openen** en voer het volgende in de aangepaste code-editor in:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Sla de wijzigingen op.

### Paginasjabloon

1. Klik **Gegevenselement toevoegen**.
1. Voer voor **Naam** **Paginasjabloon** in.
1. Voor **Gegevenselement Type** selecteert **Aangepaste code**.
1. Klik op **Editor openen** en voer het volgende in de aangepaste code-editor in:

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

   Sla de wijzigingen op.

1. U moet nu drie gegevenselementen hebben als onderdeel van uw regel:

   ![Gegevenselementen in regel](assets/collect-data-analytics/data-elements-page-rule.png)

## De extensie Analytics toevoegen

Voeg vervolgens de extensie Analytics toe aan de eigenschap Launch. We moeten deze gegevens ergens naartoe sturen!

1. Navigeer aan Experience Platform Launch en in het bezit van het Web dat met de Plaats van de AEM wordt geïntegreerd.
1. Ga naar **Extensies** > **Catalogus**
1. Zoek de extensie **Adobe Analytics** en klik op **Installeren**

   ![Adobe Analytics-extensie](assets/collect-data-analytics/analytics-catalog-install.png)

1. Voer onder **Bibliotheekbeheer** > **Suites rapporteren** de rapportsuite-id&#39;s in die u wilt gebruiken bij elke opstartomgeving.

   ![Voer de rapportsuite-id&#39;s in](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > Het is oké om één rapportenreeks voor alle milieu&#39;s in dit leerprogramma te gebruiken, maar in echt zou u afzonderlijke rapportseries willen gebruiken, zoals aangetoond in het hieronder beeld

   >[!TIP]
   >
   >We raden u aan de optie *Bibliotheek voor mij beheren* te gebruiken als instelling voor Bibliotheekbeheer, omdat de bibliotheek `AppMeasurement.js` hierdoor veel gemakkelijker up-to-date kan worden gehouden.

1. Schakel het selectievakje in om **Activity Map gebruiken** in te schakelen.

   ![Activity Map voor gebruik inschakelen](assets/track-clicked-component/analytic-track-click.png)

1. Voer onder **Algemeen** > **Trackingserver** uw trackingserver in, bijvoorbeeld `tmd.sc.omtrdc.net`. Voer uw SSL-traceringsserver in als uw site `https://` ondersteunt

   ![De trackingservers invoeren](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Klik **Opslaan** om de wijzigingen op te slaan.

## Een voorwaarde toevoegen aan de regel Pagina geladen

Vervolgens werkt u de **Pagina geladen**-regel bij om het **Component Resource Type**-gegevenselement te gebruiken om ervoor te zorgen dat de regel alleen wordt geactiveerd wanneer de `cmp:show`-gebeurtenis voor **Page** is. Andere componenten kunnen de gebeurtenis `cmp:show` in brand steken, bijvoorbeeld zal de component Carousel het in brand steken wanneer de dia&#39;s veranderen. Daarom is het belangrijk om een voorwaarde voor deze regel toe te voegen.

1. Navigeer in de gebruikersinterface van Launch naar de regel **Pagina geladen** die u eerder hebt gemaakt.
1. Klik onder **Voorwaarden** op **Toevoegen** om de wizard **Condition Configuration** te openen.
1. Selecteer **Value Comparison** voor **Condition Type**.
1. Stel de eerste waarde in het formulierveld in op `%Component Resource Type%`. U kunt het pictogram Gegevenselement ![data-element](assets/collect-data-analytics/cylinder-icon.png) gebruiken om het **Component Resource Type** gegevenselement te selecteren. Laat het comparator ingesteld op `Equals`.
1. Stel de tweede waarde in op `wknd/components/page`.

   ![Voorwaardenconfiguratie voor regel met geladen pagina](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > Het is mogelijk om deze voorwaarde binnen de functie van de douanecode toe te voegen die op de `cmp:show` gebeurtenis luistert die vroeger in het leerprogramma wordt gecreeerd. Nochtans, geeft het toevoegen van het binnen UI meer zicht aan extra gebruikers die veranderingen in de regel zouden kunnen moeten aanbrengen. Bovendien kunnen we ons gegevenselement gebruiken!

1. Sla de wijzigingen op.

## Analysevariabelen instellen en Paginaweergavebaken activeren

Momenteel voert de **Geladen Pagina** regel eenvoudig een consoleverklaring uit. Vervolgens gebruikt u de gegevenselementen en de extensie Analytics om de variabelen Analytics in te stellen als een **action** in de regel **Pagina geladen**. We stellen ook een extra actie in om het **Paginaweergavebaken** te activeren en de verzamelde gegevens naar Adobe Analytics te verzenden.

1. In **Pagina Geladen** regel **remove** de **Core - Aangepaste Code** actie (de consoleverklaringen):

   ![Aangepaste code verwijderen](assets/collect-data-analytics/remove-console-statements.png)

1. Klik onder Handelingen op **Toevoegen** om een nieuwe handeling toe te voegen.
1. Stel het type **Extension** in op **Adobe Analytics** en stel het **Action Type** in op **Variabelen instellen**

   ![Extensie handeling instellen op Variabelen voor analyse instellen](assets/collect-data-analytics/analytics-set-variables-action.png)

1. Selecteer in het hoofddeelvenster een beschikbare **eVar** en stel deze in als de waarde van het gegevenselement **Paginasjabloon**. Gebruik het pictogram Gegevenselementen ![Gegevenselementen pictogram](assets/collect-data-analytics/cylinder-icon.png) om het **Paginamalplaatje** element te selecteren.

   ![Instellen als eVar paginasjabloon](assets/collect-data-analytics/set-evar-page-template.png)

1. Schuif omlaag, onder **Aanvullende instellingen** om **Paginanaam** in te stellen op het gegevenselement **Paginanaam**:

   ![Omgevingsvariabele paginanaam ingesteld](assets/collect-data-analytics/page-name-env-variable-set.png)

   Sla de wijzigingen op.

1. Voeg vervolgens een aanvullende handeling toe aan de rechterkant van de **Adobe Analytics - Set Variables** door op het pictogram **plus** te tikken:

   ![Een extra opstarthandeling toevoegen](assets/collect-data-analytics/add-additional-launch-action.png)

1. Stel het type **Extension** in op **Adobe Analytics** en stel het **Action Type** in op **Beacon** verzenden. Omdat dit als een paginaweergave wordt beschouwd, laat u de standaardtekstspatiëring ingesteld op **`s.t()`**.

   ![Handeling Beacon Adobe Analytics verzenden](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Sla de wijzigingen op. De **Pagina Geladen** regel zou nu de volgende configuratie moeten hebben:

   ![Definitieve opstartconfiguratie](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Luister naar de  `cmp:show` gebeurtenis.
   * **2.** Controleer of de gebeurtenis door een pagina is geactiveerd.
   * **3.** Analysevariabelen instellen voor  **paginanaam** en  **paginasjabloon**
   * **4.** Verzend het Beacon van de Mening van de Pagina van de Analyse
1. Sla alle wijzigingen op en maak uw opstartbibliotheek, waarbij u een upgrade uitvoert naar de juiste omgeving.

## Valideer de oproep Beacon en Analytics voor paginaweergave

Nu de **Pagina Geladen** regel het baken van Analytics verzendt, zou u de variabelen moeten kunnen zien die van Analytics gebruikend Foutopsporing van het Experience Platform volgen.

1. Open [WKND Site](https://wknd.site/us/en.html) in uw browser.
1. Klik op het pictogram Foutopsporing ![Ervaar het pictogram Foutopsporing op platform](assets/collect-data-analytics/experience-cloud-debugger.png) om Foutopsporing op Experience Platform te openen.
1. Zorg ervoor Debugger het bezit van de Lancering aan *uw* ontwikkelomgeving in kaart brengt, zoals vroeger beschreven en **Console het Registreren** wordt gecontroleerd.
1. Open het menu Analytics en controleer of de rapportsuite is ingesteld op *uw*-rapportsuite. De paginanaam moet ook worden ingevuld:

   ![Foutopsporing op het tabblad Analyse](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Schuif omlaag en vouw **Netwerkverzoeken** uit. U zou **evar** voor **Paginasjabloon** moeten kunnen vinden:

   ![Gebeurtenis en paginanaam ingesteld](assets/collect-data-analytics/evar-page-name-set.png)

1. Ga terug naar de browser en open de ontwikkelaarsconsole. Klik door **Carousel** bij de bovenkant van de pagina.

   ![Klikken door carrouselpagina](assets/collect-data-analytics/click-carousel-page.png)

1. Neem in de browser console de consoleverklaring waar:

   ![Voldoet niet aan voorwaarde](assets/collect-data-analytics/condition-not-met.png)

   De reden hiervoor is dat de Carousel een `cmp:show`-gebeurtenis *maar* activeert vanwege onze controle van het **Component Resource Type**, wordt geen gebeurtenis geactiveerd.

   >[!NOTE]
   >
   > Als u geen consolelogboeken ziet, zorg ervoor dat **Console het Registreren** onder **Lancering** in Foutopsporing van het Experience Platform wordt gecontroleerd.

1. Navigeer naar een artikelpagina zoals [Western Australia](https://wknd.site/us/en/magazine/western-australia.html). Bekijk de paginanaam en het sjabloontype worden gewijzigd.

## Gefeliciteerd!

U hebt net de gebeurtenisgestuurde Adobe Client Data Layer en het Experience Platform Launch gebruikt om gegevenspagina-gegevens van een AEM Site te verzamelen en deze naar Adobe Analytics te verzenden.

### Volgende stappen

Bekijk de volgende zelfstudie om te leren hoe u de gebeurtenisgestuurde Adobe Client Data-laag kunt gebruiken om kliks van specifieke-componenten op een Adobe Experience Manager-site ](track-clicked-component.md) bij te houden.[
