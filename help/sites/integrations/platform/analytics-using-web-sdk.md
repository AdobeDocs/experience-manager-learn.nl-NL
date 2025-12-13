---
title: AEM Sites en Adobe Analytics integreren met Platform Web SDK
description: Integreer AEM Sites en Adobe Analytics met behulp van de moderne Web SDK-benadering van het Platform.
version: Experience Manager as a Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-25T00:00:00Z
jira: KT-13328
thumbnail: KT-13328.jpeg
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 0cc3d3bc-e4ea-4ab2-8878-adbcf0c914f5
duration: 2252
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1529'
ht-degree: 0%

---

# AEM Sites en Adobe Analytics integreren met Platform Web SDK

Leer de **moderne benadering** op hoe te om Adobe Experience Manager (AEM) en Adobe Analytics te integreren gebruikend SDK van het Web van het Platform. Deze uitvoerige zelfstudie begeleidt u door het proces om [&#x200B; WKND &#x200B;](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) paageview en CTA naadloos te verzamelen klikt gegevens. Vergroot waardevolle inzichten door de verzamelde gegevens te visualiseren in Adobe Analysis Workspace, waar u verschillende maatstaven en dimensies kunt verkennen. Ook, verken de Dataset van het Platform om de gegevens te verifiëren en te analyseren. Doe mee aan deze reis om de kracht van AEM en Adobe Analytics te benutten voor gegevensgestuurde besluitvorming.

## Overzicht

Het verzamelen van inzichten in gebruikersgedrag is een cruciaal doel voor elk marketingteam. Door te begrijpen hoe de gebruikers met hun inhoud in wisselwerking staan, kunnen de teams geïnformeerde besluiten nemen, strategieën optimaliseren, en betere resultaten drijven. Het WKND-marketingteam, een fictieve entiteit, heeft zich voorgenomen Adobe Analytics op hun website te implementeren om dit doel te bereiken. Het belangrijkste doel is het verzamelen van gegevens over twee belangrijke metriek: pagina&#39;s en homepage call-to-action (CTA) klikken.

Door pagina&#39;s bij te houden, kan het team analyseren welke pagina&#39;s de meeste aandacht van gebruikers krijgen. Bovendien biedt het volgen van homepage waarop CTA klikt waardevolle inzichten in de effectiviteit van de call-to-action-elementen van het team. Deze gegevens kunnen onthullen welke CTA&#39;s met gebruikers resoneren, welke degenen aanpassing nodig hebben, en potentieel nieuwe kansen ontdekken om gebruikersbetrokkenheid en aandrijvingsomzettingen te verbeteren.


>[!VIDEO](https://video.tv.adobe.com/v/3419872?quality=12&learn=on)

## Vereisten

Voor de integratie van Adobe Analytics met Platform Web SDK is het volgende vereist.

U hebt de opstellingsstappen van **[voltooid integreren het Web SDK van Experience Platform](./web-sdk.md)** leerprogramma.

In **AEM als Cloud Service**:

+ [&#x200B; toegang van de Beheerder van AEM tot het milieu van AEM as a Cloud Service &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/accessing/overview.html?lang=nl-NL)
+ Toegang tot Cloud Manager via Deployment Manager
+ Kloon en stel [&#x200B; WKND - het project van steekproefAdobe Experience Manager &#x200B;](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) aan uw milieu van AEM as a Cloud Service op.

In **Adobe Analytics**:

+ Toegang om **Reeks van het Rapport te creëren**
+ Toegang om **Analysis Workspace** te creëren

In **Experience Platform**:

+ Toegang tot de standaardproductie, **Prod** zandbak.
+ Toegang tot **Schema&#39;s** onder het Beheer van Gegevens
+ Toegang tot **Datasets** onder het Beheer van Gegevens
+ Toegang tot **gegevensstromen** onder de Inzameling van Gegevens
+ Toegang tot **Markeringen** onder de Inzameling van Gegevens

Voor het geval u noodzakelijke toestemmingen niet hebt, kan uw systeembeheerder die [&#x200B; Adobe Admin Console &#x200B;](https://adminconsole.adobe.com/) gebruiken noodzakelijke toestemmingen verlenen.

Alvorens in het integratieproces van AEM en Analytics te delving die het Web SDK van het Platform gebruiken, laten _de essentiële componenten en de belangrijkste elementen_ opnieuw samenvatten die in [&#x200B; werden gevestigd integreren SDK van het Web van Experience Platform &#x200B;](./web-sdk.md) leerprogramma. Het biedt een solide basis voor de integratie.

>[!VIDEO](https://video.tv.adobe.com/v/3419873?quality=12&learn=on)

Na de hervatting van het Schema XDM, DataStream, Dataset, het bezit van de Markering en, AEM en de verbinding van het markeringsbezit, laten wij aan de integratietraject beginnen.

## Document Analytics Solution Design Reference (SDR) definiëren

Als deel van het implementatieproces, wordt het geadviseerd om een document van de Verwijzing van het Ontwerp van de Oplossing (SDR) tot stand te brengen. Dit document speelt een cruciale rol als blauwdruk voor het bepalen van bedrijfsvereisten en het ontwerpen van efficiënte strategieën voor gegevensverzameling.

Het SDR-document biedt een uitgebreid overzicht van het uitvoeringsplan, zodat alle belanghebbenden op één lijn worden gebracht en de doelstellingen en de reikwijdte van het project worden begrepen.


>[!VIDEO](https://video.tv.adobe.com/v/3419874?quality=12&learn=on)

Voor meer informatie over concepten en diverse elementen die in het SDR- document zouden moeten worden omvat bezoek [&#x200B; creeer en handhaaf een Document van het Ontwerp van de Oplossing van de Verwijzing (SDR) &#x200B;](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/implementation/implementation-basics/creating-and-maintaining-an-sdr.html?lang=nl-NL). U kunt een malplaatje van steekproefExcel ook downloaden, nochtans is de WKND-Specifieke versie ook beschikbaar [&#x200B; hier &#x200B;](./assets/Initial-WKND-WebSDK-BRD-SDR.xlsx).

## Analyses instellen - rapportenpakket, Analysis Workspace

De eerste stap bestaat uit het instellen van Adobe Analytics, met name het rapporteren van een suite met conversievariabelen (of eVar) en succesgebeurtenissen. De omzettingsvariabelen worden gebruikt om oorzaak en effect te meten. De succesgebeurtenissen worden gebruikt om acties bij te houden.

In dit leerprogramma, `eVar5, eVar6, and eVar7` spoor _WKND de Naam van de Pagina, identiteitskaart van CTA WKND, en de Naam van CTA WKND_ respectievelijk, en `event7` wordt gebruikt om _WKND CTA te volgen klikt Gebeurtenis_.

Om die inzichten te analyseren, te verzamelen en te delen met anderen uit de verzamelde gegevens, wordt een project in Analysis Workspace gemaakt.

>[!VIDEO](https://video.tv.adobe.com/v/3419875?quality=12&learn=on)

Om meer over de opstelling en de concepten van Analytics te leren, worden de volgende middelen hoogst geadviseerd:

+ [&#x200B; Reeks van het Rapport &#x200B;](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite.html?lang=nl-NL)
+ [&#x200B; Variabelen van de Omzetting &#x200B;](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html?lang=nl-NL)
+ [&#x200B; Gebeurtenissen van het Succes &#x200B;](https://experienceleague.adobe.com/nl/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-event)
+ [&#x200B; Analysis Workspace &#x200B;](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html?lang=nl-NL)

## DataStream bijwerken - Analyseservice toevoegen

Een DataStream instrueert het Platform Edge Network waar te om de verzamelde gegevens te verzenden. In het [&#x200B; vorige leerprogramma &#x200B;](./web-sdk.md), wordt een DataStream gevormd om de gegevens naar Experience Platform te verzenden. Deze DataStream wordt bijgewerkt om de gegevens naar de het rapportreeks van Analytics te verzenden die in [&#x200B; hierboven &#x200B;](#setup-analytics---report-suite-analysis-workspace) stap werd gevormd.

>[!VIDEO](https://video.tv.adobe.com/v/3419876?quality=12&learn=on)

## XDM-schema maken

Met het XDM-schema (Experience Data Model) kunt u de verzamelde gegevens standaardiseren. In het [&#x200B; vorige leerprogramma &#x200B;](./web-sdk.md), wordt een schema XDM met `AEP Web SDK ExperienceEvent` een gebiedsgroep gecreeerd. Bovendien wordt met dit XDM-schema een Dataset gemaakt waarin de verzamelde gegevens worden opgeslagen in de Experience Platform.

Dat XDM-schema echter geen Adobe Analytics-specifieke veldgroepen heeft om de eVar, gebeurtenisgegevens te verzenden. Er wordt een nieuw XDM-schema gemaakt in plaats van het bestaande schema bij te werken om te voorkomen dat de eVar, gebeurtenisgegevens in het platform worden opgeslagen.

Het nieuwe XDM-schema heeft `AEP Web SDK ExperienceEvent` - en `Adobe Analytics ExperienceEvent Full Extension` -veldgroepen.

>[!VIDEO](https://video.tv.adobe.com/v/3419877?quality=12&learn=on)


## Tag bijwerken, eigenschap

In het [&#x200B; vorige leerprogramma &#x200B;](./web-sdk.md), wordt een markeringsbezit gecreeerd, heeft het de Elementen van Gegevens en een Regel om te verzamelen, in kaart te brengen, en de voorproefgegevens te verzenden. Het moet worden verbeterd voor:

+ De paginanaam toewijzen aan `eVar5`
+ Het teweegbrengen van de **paginaView** vraag van Analytics (of verzend baken)
+ CTA-gegevens verzamelen met de Adobe Client Data Layer
+ Wijs de CTA-id en -naam toe aan respectievelijk `eVar6` en `eVar7` . Ook, klikt CTA tellen aan `event7`
+ Het teweegbrengen van de **verbinding klikt** vraag van Analytics (of verzendt baken)


>[!VIDEO](https://video.tv.adobe.com/v/3419882?quality=12&learn=on)

>[!TIP]
>
>Het Element van Gegevens en regel-Gebeurtenis code in de video wordt getoond is beschikbaar voor uw verwijzing, **breidt hieronder accordeonelement** uit. Nochtans, als u NIET de Laag van Gegevens van de Cliënt van Adobe gebruikt, moet u de hieronder code wijzigen maar het concept van het bepalen van de Elementen van Gegevens en het gebruiken van hen in de definitie van de Regel blijft van toepassing.

+++ Gegevenselement en code voor regelgebeurtenissen

+ De `Component ID` Data Element-code.

  ```javascript
  if(event && event.path && event.path.includes('.')) {    
      // split on the `.` to return just the component ID for e.g. button-06bc532b85, tabs-bb27f4f426-item-cc9c2e6718
      return event.path.split('.')[1];
  }else {
      //return dummy ID
      return "WKND-CTA-ID";
  }
  ```

+ De `Component Name` Data Element-code.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // Return the Button, Link, Image, Tab name, for e.g. View Trips, Full Article, See Trips
      return event.component['dc:title'];
  }else {
      //return dummy ID
      return "WKND-CTA-Name";    
  }    
  ```

+ De `all pages - on load` **regel-voorwaarde** code

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type') && event.component.hasOwnProperty('xdm:template')) {
      return true;
  }else{
      return false;
  }    
  ```

+ De `home page - cta click` **regel-Gebeurtenis** code

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

+ De `home page - cta click` **regel-voorwaarde** code

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

Voor extra informatie bij het integreren van de Componenten van de Kern van AEM met de Laag van Gegevens van de Cliënt van Adobe, verwijs naar [&#x200B; Gebruikend de Laag van Gegevens van de Cliënt van Adobe met de gids van de Kern van AEM &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=nl-NL).


>[!INFO]
>
>Voor een uitvoerig begrip van het **Variabele 1&rbrace; dossier van het het lusjebezit details in het document van de Verwijzing van het Ontwerp van de Oplossing (SDR), heb toegang tot de voltooide WKND-Specifieke versie voor download** [&#x200B; hier.](./assets/Final-WKND-WebSDK-BRD-SDR.xlsx)



## De bijgewerkte eigenschap Tag op WKND verifiëren

Om ervoor te zorgen dat de bijgewerkte markeringseigenschap op de WKND-sitepagina&#39;s wordt gebouwd, gepubliceerd en correct werkt. Gebruik de de Webbrowser van Google Chrome [&#x200B; uitbreiding van Adobe Experience Platform Debugger &#x200B;](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob):

+ Om ervoor te zorgen dat het markeringsbezit de recentste versie is, controleer de bouwstijldatum.

+ Als u de XDM-gebeurtenisgegevens voor PageView en HomePage CTA Click wilt controleren, gebruikt u de menuoptie Experience Platform Web SDK in de extensie.

>[!VIDEO](https://video.tv.adobe.com/v/3419883?quality=12&learn=on)

## Webverkeer simuleren - seleniumautomatisering

Om een betekenisvolle hoeveelheid verkeer voor testdoeleinden te produceren, wordt een manuscript van de automatisering van Selenium ontwikkeld. Dit aangepaste script simuleert gebruikersinteracties met de WKND-website, zoals paginaweergave en het klikken op CTA&#39;s.

>[!VIDEO](https://video.tv.adobe.com/v/3419884?quality=12&learn=on)

## Dataset-verificatie - WKND-voorvertoning, CTA-gegevens

De dataset is een opslag en beheersconstructie voor een inzameling van gegevens zoals een gegevensbestandlijst die een schema volgt. De Dataset die in het [&#x200B; vorige leerprogramma &#x200B;](./web-sdk.md) wordt gecreeerd wordt opnieuw gebruikt om te verifiëren dat de voorproef en CTA klikgegevens in de Dataset van Experience Platform wordt opgenomen. Binnen de Dataset UI, worden diverse details zoals totale verslagen, grootte, en ingebedde partijen getoond samen met een visueel aantrekkelijke balkgrafiek.

>[!VIDEO](https://video.tv.adobe.com/v/3419885?quality=12&learn=on)

## Analyse - WKND-voorvertoning, CTA-gegevensvisualisatie

Analysis Workspace is een krachtig hulpmiddel in Adobe Analytics waarmee gegevens op een flexibele en interactieve manier kunnen worden verkend en weergegeven. Het verstrekt een belemmering-en-dalingsinterface om douanerapporten tot stand te brengen, geavanceerde segmentatie uit te voeren, en diverse gegevensvisualisaties toe te passen.

Laten wij het project van Analysis Workspace heropenen dat in de [&#x200B; Analytische &#x200B;](#setup-analytics---report-suite-analysis-workspace) stap van de Opstelling wordt gecreeerd. In de **Hoogste sectie van Pagina&#39;s**, onderzoek diverse metriek zoals bezoeken, unieke bezoekers, ingangen, stuittarief, en meer. Als u de prestaties van WKND-pagina&#39;s en CTA&#39;s van de homepage wilt beoordelen, sleept u de WKND-specifieke afmetingen (WKND-paginanaam, WKND CTA-naam) en de afmetingen (WKND CTA Click Event). Deze inzichten zijn van groot belang voor de marktpartijen om te begrijpen welke CTA&#39;s effectiever zijn en gegevensgestuurde beslissingen nemen, in overeenstemming met hun bedrijfsdoelstellingen.

Om gebruikersreizen visualiseren, gebruik de visualisatie van de Stroom, die met de **Naam van de Pagina WKND** begint en zich in diverse wegen uitbreidt.

>[!VIDEO](https://video.tv.adobe.com/v/3419886?quality=12&learn=on)

## Samenvatting

Geweldig werk! U hebt de installatie van AEM en Adobe Analytics voltooid met Platform Web SDK voor het verzamelen, analyseren van de gegevens voor de voorvertoning en CTA click.

Het implementeren van Adobe Analytics is van cruciaal belang voor marketingteams om inzicht te krijgen in gebruikersgedrag, geïnformeerde beslissingen te nemen, zodat ze hun inhoud kunnen optimaliseren en gegevensgestuurde beslissingen kunnen nemen.

Door de geadviseerde stappen uit te voeren en de verstrekte middelen, zoals het document van de Verwijzing van het Ontwerp van de Oplossing (SDR) te gebruiken en belangrijke concepten van Analytics te begrijpen, kunnen de marketers gegevens effectief verzamelen en analyseren.

>[!VIDEO](https://video.tv.adobe.com/v/3419888?quality=12&learn=on)


>[!AVAILABILITY]
>
>Als u de **video van begin tot eind** verkiest die het volledige integratieproces in plaats van individuele video&#39;s van de opstellingsstap behandelt, kunt u [&#x200B; hier &#x200B;](https://video.tv.adobe.com/v/3419889/) klikken om tot het toegang te hebben.


## Aanvullende bronnen

+ [&#x200B; integreren Experience Platform Web SDK &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform/web-sdk.html?lang=nl-NL)
+ [&#x200B; Gebruikend de Laag van Gegevens van de Cliënt van Adobe met de Componenten van de Kern &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=nl-NL)
+ [&#x200B; Integrating Experience Platform de Markeringen van de Inzameling van Gegevens en AEM &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html?lang=nl-NL)
+ [&#x200B; het Web SDK van Adobe Experience Platform en het overzicht van Edge Network &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html?lang=nl-NL)
+ [&#x200B; leerprogramma&#39;s van de Inzameling van Gegevens &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html?lang=nl-NL)
+ [&#x200B; overzicht van Adobe Experience Platform Debugger &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=nl-NL)
