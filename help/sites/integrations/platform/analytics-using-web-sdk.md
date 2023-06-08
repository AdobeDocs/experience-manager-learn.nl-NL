---
title: Adobe Analytics integreren met Platform Web SDK
description: Leer de moderne benadering van hoe te om Adobe Experience Manager (AEM) en Adobe Analytics te integreren gebruikend het Web SDK van het Platform. Deze zelfstudie begeleidt u door het verzamelen van paginaweergave en het klikken van CTA gegevens om gegevensinzicht in de Werkruimte van Adobe Analytics te krijgen.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-25T00:00:00Z
jira: KT-13328
thumbnail: KT-13328.jpeg
source-git-commit: 542313c0da6f5eab5befe0da1b80ab38948156ac
workflow-type: tm+mt
source-wordcount: '1647'
ht-degree: 0%

---


# Adobe Analytics integreren met Platform Web SDK

Meer informatie over **moderne benadering** op hoe te om Adobe Experience Manager (AEM) en Adobe Analytics te integreren gebruikend het Web SDK van het Platform. Deze uitgebreide zelfstudie begeleidt u door het proces van naadloos verzamelen [WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) paginaweergave en CTA-klikgegevens. Vergroot waardevolle inzichten door de verzamelde gegevens te visualiseren in Adobe Analysis Workspace, waar u verschillende metriek en dimensies kunt verkennen. Onderzoek ook de Dataset van het Platform om de gegevens te verifiëren en te analyseren. Doe mee aan deze reis om de kracht van AEM en Adobe Analytics te benutten voor gegevensgestuurde besluitvorming.

## Overzicht

Het verzamelen van inzichten in gebruikersgedrag is een cruciaal doel voor elk marketingteam. Door te begrijpen hoe de gebruikers met hun inhoud in wisselwerking staan, kunnen de teams geïnformeerde besluiten nemen, strategieën optimaliseren, en betere resultaten drijven. Het WKND-marketingteam, een fictieve entiteit, heeft zich voorgenomen Adobe Analytics op hun website te implementeren om dit doel te bereiken. Het belangrijkste doel is het verzamelen van gegevens over twee belangrijke meeteenheden: De pagina&#39;s en homepage vraag-aan-actie (CTA) klikken.

Door pagina&#39;s bij te houden, kan het team analyseren welke pagina&#39;s de meeste aandacht van gebruikers krijgen. Ook, verstrekt het volgen van homepageCTA klikken waardevolle inzichten in de doeltreffendheid van vraag-aan-actie van het team elementen. Deze gegevens kunnen onthullen welke CTA&#39;s met gebruikers resoneren, welke degenen aanpassing nodig hebben, en potentieel nieuwe kansen ontdekken om gebruikersbetrokkenheid en aandrijvingsomzettingen te verbeteren.


>[!VIDEO](https://video.tv.adobe.com/v/3419872?quality=12&learn=on)

## Vereisten

Het volgende wordt vereist wanneer het integreren van Adobe Analytics gebruikend het Web SDK van het Platform.

U hebt de installatiestappen uitgevoerd in het dialoogvenster **[Experience Platform Web SDK integreren](./web-sdk.md)** zelfstudie.

In **AEM als Cloud Service**:

+ [Toegang van AEM beheerder tot AEM as a Cloud Service omgeving](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/accessing/overview.html)
+ Toegang tot Cloud Manager via Deployment Manager
+ Klonen en implementeren [WKND - voorbeeld Adobe Experience Manager-project](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) naar uw AEM as a Cloud Service omgeving.

In **Adobe Analytics**:

+ Toegang tot maken **Rapportsuite**
+ Toegang tot maken **Analysis Workspace**

In **Experience Platform**:

+ toegang tot de standaardproductie; **Prod** sandbox.
+ Toegang tot **Schemas** onder Gegevensbeheer
+ Toegang tot **Gegevenssets** onder Gegevensbeheer
+ Toegang tot **DataStreams** onder Gegevensverzameling
+ Toegang tot **Tags** (voorheen bekend als Launch) onder Gegevensverzameling

Als u niet de noodzakelijke toestemmingen hebt, gebruikt uw systeembeheerder [Adobe Admin Console](https://adminconsole.adobe.com/) kan de benodigde machtigingen verlenen.

Alvorens in het integratieproces van AEM en Analytics te delving gebruikend het Web SDK van het Platform, laten wij _de essentiële onderdelen en de belangrijkste elementen opnieuw samenvatten_ die in het [Experience Platform Web SDK integreren](./web-sdk.md) zelfstudie. Het biedt een solide basis voor de integratie.

>[!VIDEO](https://video.tv.adobe.com/v/3419873?quality=12&learn=on)

Na het overzicht van het XDM-schema, DataStream, Dataset, Tag-eigenschap en, AEM en tageigenschapsverbinding, gaan we aan de integratietraject.

## Document Analytics Solution Design Reference (SDR) definiëren

Als deel van het implementatieproces, wordt het geadviseerd om een document van de Verwijzing van het Ontwerp van de Oplossing (SDR) tot stand te brengen. Dit document speelt een cruciale rol als blauwdruk voor het bepalen van bedrijfsvereisten en het ontwerpen van efficiënte strategieën voor gegevensverzameling.

Het SDR-document biedt een uitgebreid overzicht van het uitvoeringsplan, zodat alle belanghebbenden op één lijn worden gebracht en de doelstellingen en de reikwijdte van het project worden begrepen.


>[!VIDEO](https://video.tv.adobe.com/v/3419874?quality=12&learn=on)

Ga voor meer informatie over concepten en diverse elementen die in het SDR-document moeten worden opgenomen naar de [Creeer en handhaaf een Document van de Verwijzing van het Ontwerp van de Oplossing (SDR)](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/implementation/implementation-basics/creating-and-maintaining-an-sdr.html). U kunt een malplaatje van steekproefExcel ook downloaden, nochtans is de WKND-specifieke versie ook beschikbaar [hier](./assets/Initial-WKND-WebSDK-BRD-SDR.xlsx).

## Analyses instellen - rapportenpakket, Analysis Workspace

De eerste stap bestaat uit het instellen van Adobe Analytics, met name het rapporteren van een suite met conversievariabelen (of eVar) en succesgebeurtenissen. De omzettingsvariabelen worden gebruikt om oorzaak en effect te meten. De succesgebeurtenissen worden gebruikt om acties bij te houden.

In deze zelfstudie  `eVar5, eVar6, and eVar7` track  _WKND-paginanaam, WKND CTA-ID en WKND CTA-naam_ en `event7` wordt gebruikt om te volgen  _WKND CTA Click Event_.

Om die inzichten te analyseren, te verzamelen en te delen met anderen uit de verzamelde gegevens, wordt een project in Analysis Workspace gemaakt.

>[!VIDEO](https://video.tv.adobe.com/v/3419875?quality=12&learn=on)

Om meer over de opstelling en de concepten van Analytics te leren, worden de volgende middelen hoogst geadviseerd:

+ [Rapportsuite](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite.html)
+ [Conversievariabelen](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html)
+ [Gebeurtenissen geslaagd](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-events/success-event.html)
+ [Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html)

## DataStream bijwerken - Analyseservice toevoegen

Een DataStream instrueert het Netwerk van de Rand van het Platform waar te om de verzamelde gegevens te verzenden. In de [vorige zelfstudie](./web-sdk.md), wordt een DataStream gevormd om de gegevens naar het Experience Platform te verzenden. Deze DataStream wordt bijgewerkt om de gegevens naar de het rapportreeks van Analytics te verzenden die in [boven](#setup-analytics---report-suite-analysis-workspace) stap.

>[!VIDEO](https://video.tv.adobe.com/v/3419876?quality=12&learn=on)

## XDM-schema maken

Met het XDM-schema (Experience Data Model) kunt u de verzamelde gegevens standaardiseren. In de [vorige zelfstudie](./web-sdk.md), een XDM-schema met `AEP Web SDK ExperienceEvent` er wordt een veldgroep gemaakt. Ook, gebruikend dit schema XDM wordt een Dataset gecreeerd om de verzamelde gegevens in het Experience Platform op te slaan.

Dat XDM-schema echter geen Adobe Analytics-specifieke veldgroepen heeft om de eVar, gebeurtenisgegevens te verzenden. Er wordt een nieuw XDM-schema gemaakt in plaats van het bestaande schema bij te werken, zodat de eVar, gebeurtenisgegevens niet in het platform worden opgeslagen.

Het nieuwe XDM-schema heeft `AEP Web SDK ExperienceEvent` en `Adobe Analytics ExperienceEvent Full Extension` veldgroepen.

>[!VIDEO](https://video.tv.adobe.com/v/3419877?quality=12&learn=on)


## Tag bijwerken, eigenschap

In de [vorige zelfstudie](./web-sdk.md), er wordt een markeringseigenschap gemaakt, er zijn gegevenselementen en een regel voor het verzamelen, toewijzen en verzenden van de paginaweergavegegevens. Het moet worden verbeterd voor:

+ De paginanaam toewijzen aan `eVar5`
+ De **voorvertoning** Analytische aanroep (of verzendbaken)
+ Het verzamelen van gegevens CTA gebruikend de Laag van de Gegevens van de Cliënt van Adobe
+ De CTA-id en -naam toewijzen aan `eVar6` en `eVar7` respectievelijk. Ook, klikt CTA telling aan `event7`
+ De **koppelingsklik** Analytische aanroep (of verzendbaken)


>[!VIDEO](https://video.tv.adobe.com/v/3419882?quality=12&learn=on)

>[!TIP]
>
>Het gegevenselement en regel-gebeurtenis code in de video wordt getoond is beschikbaar voor uw verwijzing die, **het element onder accordeon uitvouwen**. Nochtans, als u de Laag van Gegevens van de Cliënt NIET gebruikt, moet u de hieronder code wijzigen maar het concept van het bepalen van de Elementen van Gegevens en het gebruiken van hen in de definitie van de Regel blijft van toepassing.

+++ Gegevenselement en code voor regelgebeurtenissen

+ De `Component ID` Code gegevenselement.

  ```javascript
  if(event && event.path && event.path.includes('.')) {    
      // split on the `.` to return just the component ID for e.g. button-06bc532b85, tabs-bb27f4f426-item-cc9c2e6718
      return event.path.split('.')[1];
  }else {
      //return dummy ID
      return "WKND-CTA-ID";
  }
  ```

+ De `Component Name` Code gegevenselement.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // Return the Button, Link, Image, Tab name, for e.g. View Trips, Full Article, See Trips
      return event.component['dc:title'];
  }else {
      //return dummy ID
      return "WKND-CTA-Name";    
  }    
  ```

+ De `all pages - on load` **Regel-voorwaarde** code

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type') && event.component.hasOwnProperty('xdm:template')) {
      return true;
  }else{
      return false;
  }    
  ```

+ De `home page - cta click` **Rule-Event** code

  ```javascript
  var componentClickedHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      //trigger Tag Rule and pass event
      console.log("cmp:click event: " + evt.eventInfo.path);
  
      var event = {
          //include the path of the component that triggered the event
          path: evt.eventInfo.path,
          //get the state of the component that triggered the event
          component: window.adobeDataLayer.getState(evt.eventInfo.path)
      };
  
      //Trigger the Tag Rule, passing in the new `event` object
      // the `event` obj can now be referenced by the reserved name `event` by other Tag Property data elements
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

+ De `home page - cta click` **Regel-voorwaarde** code

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type')) {
      //Check for Button Type OR Teaser CTA type
      if(event.component['@type'] === 'wknd/components/button' ||
      event.component['@type'] === 'wknd/components/teaser/cta') {
          return true;
      }
  }
  
  // none of the conditions are met, return false
  return false;    
  ```

+++

Voor extra informatie bij het integreren van AEM Componenten van de Kern met de Laag van de Gegevens van de Cliënt van Adobe raadpleegt u [De Adobe Client Data Layer gebruiken met AEM Core Components Guide](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html).


>[!INFO]
>
>Voor een volledig begrip van de **Variabele toewijzen** de details van het lusjebezit in het document van de Verwijzing van het Ontwerp van de Oplossing (SDR), hebben toegang tot de voltooide WKND-Specifieke versie voor download [hier](./assets/Final-WKND-WebSDK-BRD-SDR.xlsx).



## De bijgewerkte eigenschap Tag op WKND verifiëren

Om ervoor te zorgen dat de bijgewerkte markeringseigenschap op de WKND-sitepagina&#39;s wordt gebouwd, gepubliceerd en correct werkt. De Google Chrome-webbrowser gebruiken [Adobe Experience Platform Debugger-extensie](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob):

+ Om ervoor te zorgen dat het markeringsbezit de recentste versie is, controleer de bouwstijldatum.

+ Om de XDM gebeurtenisgegevens voor zowel PageView als HomePage te verifiëren klikt CTA, gebruik de het menuoptie van SDK van het Web van het Experience Platform binnen de uitbreiding.

>[!VIDEO](https://video.tv.adobe.com/v/3419883?quality=12&learn=on)

## Webverkeer simuleren - seleniumautomatisering

Om een betekenisvolle hoeveelheid verkeer voor testdoeleinden te produceren, wordt een manuscript van de automatisering van Selenium ontwikkeld. Dit aangepaste script simuleert gebruikersinteracties met de WKND-website, zoals paginaweergave en het klikken op CTA&#39;s.

>[!VIDEO](https://video.tv.adobe.com/v/3419884?quality=12&learn=on)

## Dataset-verificatie - WKND-voorvertoning, CTA-gegevens

De dataset is een opslag en beheersconstructie voor een inzameling van gegevens zoals een gegevensbestandlijst die een schema volgt. De gegevensset die is gemaakt in het dialoogvenster [vorige zelfstudie](./web-sdk.md) wordt opnieuw gebruikt om te verifiëren dat de de paginaamexemplaar en CTA klikgegevens in de Dataset van het Experience Platform worden opgenomen. Binnen de Dataset UI, worden diverse details zoals totale verslagen, grootte, en ingebedde partijen getoond samen met een visueel aantrekkelijke balkgrafiek.

>[!VIDEO](https://video.tv.adobe.com/v/3419885?quality=12&learn=on)

## Analyse - WKND-voorvertoning, CTA-gegevensvisualisatie

Analysis Workspace is een krachtig hulpmiddel in Adobe Analytics waarmee gegevens op een flexibele en interactieve manier kunnen worden verkend en weergegeven. Het verstrekt een belemmering-en-dalingsinterface om douanerapporten tot stand te brengen, geavanceerde segmentatie uit te voeren, en diverse gegevensvisualisaties toe te passen.

Laten we het Analysis Workspace-project dat is gemaakt in de [Analyses instellen](#setup-analytics---report-suite-analysis-workspace) stap. In de **Bovenste pagina&#39;s** verschillende meetgegevens te bekijken, zoals bezoeken, unieke bezoekers, berichten, stuitfrequentie en meer. Om de prestaties van WKND pagina&#39;s en homepageCTAs te beoordelen, sleep-en-daling de WKND-specifieke afmetingen (de Naam van de Pagina WKND, de Naam van WKND CTA) en metriek (de Gebeurtenis van de Klik van WKND CTA). Deze inzichten zijn van groot belang voor de marktpartijen om te begrijpen welke CTA&#39;s effectiever zijn en gegevensgestuurde beslissingen nemen, in overeenstemming met hun bedrijfsdoelstellingen.

Als u gebruikersreizen zichtbaar wilt maken, gebruikt u de stroomvisualisatie, te beginnen met de **WKND-paginanaam** en uitbreiden naar verschillende paden.

>[!VIDEO](https://video.tv.adobe.com/v/3419886?quality=12&learn=on)

## Samenvatting

Geweldig werk! U hebt de opstelling van AEM en Adobe Analytics gebruikend het Web SDK van het Platform voltooid om de voorproef en CTA klikgegevens te verzamelen, te analyseren.

Het implementeren van Adobe Analytics is van cruciaal belang voor marketingteams om inzicht te krijgen in gebruikersgedrag, geïnformeerde beslissingen te nemen, zodat ze hun inhoud kunnen optimaliseren en gegevensgestuurde beslissingen kunnen nemen.

Door de geadviseerde stappen uit te voeren en de verstrekte middelen, zoals het document van de Verwijzing van het Ontwerp van de Oplossing (SDR) te gebruiken en belangrijke concepten van Analytics te begrijpen, kunnen de marketers gegevens effectief verzamelen en analyseren.

>[!VIDEO](https://video.tv.adobe.com/v/3419888?quality=12&learn=on)


>[!AVAILABILITY]
>
>Als u de voorkeur geeft aan **end-to-end video** dat het volledige integratieproces in plaats van individuele opstellingsstapvideo&#39;s behandelt, kunt u klikken [hier](https://video.tv.adobe.com/v/3419889/) om toegang te krijgen tot het bestand.


## Aanvullende bronnen

+ [Experience Platform Web SDK integreren](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform/web-sdk.html)
+ [Het gebruiken van de Laag van de Gegevens van de Cliënt van Adobe met de Componenten van de Kern](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)
+ [Tags en AEM voor gegevensverzameling van Experience Platforms integreren](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [Overzicht van Adobe Experience Platform Web SDK en Edge Network](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [Tutorials voor gegevensverzameling](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Overzicht van Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
