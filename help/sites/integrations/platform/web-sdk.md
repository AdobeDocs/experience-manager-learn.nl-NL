---
title: AEM Sites en Experience Platform Web SDK integreren
description: Leer hoe u AEM Sites as a Cloud Service kunt integreren met Experience Platform Web SDK. Deze fundamentele stap is essentieel voor de integratie van Adobe Experience Cloud-producten, zoals Adobe Analytics, Target of recente innovatieve producten zoals Real-time Customer Data Platform, Customer Journey Analytics en Journey Optimizer.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service" before-title="false"
exl-id: 47df99e6-6418-43c8-96fe-85e3c47034d6
duration: 1303
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1229'
ht-degree: 0%

---

# AEM Sites en Experience Platform Web SDK integreren

Leer hoe te om AEM as a Cloud Service met Experience Platform [ SDK van het Web ](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html) te integreren. Deze fundamentele stap is essentieel voor de integratie van Adobe Experience Cloud-producten, zoals Adobe Analytics, Target of recente innovatieve producten zoals Real-time Customer Data Platform, Customer Journey Analytics en Journey Optimizer.

U leert ook hoe te om [ WKND te verzamelen en te verzenden - het project van de steekproefAdobe Experience Manager ](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) voorproefgegevens in het [ Experience Platform ](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html).

Na voltooiing van deze opstelling, hebt u een stevige stichting uitgevoerd. Ook, bent u bereid om de implementatie van het Experience Platform te bevorderen gebruikend toepassingen zoals [ Real-time Customer Data Platform (Real-Time CDP) ](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html), [ Customer Journey Analytics (CJA) ](https://experienceleague.adobe.com/docs/customer-journey-analytics.html), en [ Adobe Journey Optimizer (AJO) ](https://experienceleague.adobe.com/docs/journey-optimizer.html). De geavanceerde implementatie draagt bij aan een betere betrokkenheid van klanten door het web en de gegevens van klanten te standaardiseren.

## Vereisten

Het volgende wordt vereist wanneer het integreren van het Web SDK van het Experience Platform.

In **AEM als Cloud Service**:

+ Toegang van beheerders tot de AEM as a Cloud Service-omgeving AEM
+ Toegang tot Cloud Manager via Deployment Manager
+ Kloon en stel [ WKND - het project van steekproefAdobe Experience Manager ](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) aan uw milieu van AEM as a Cloud Service op.

In **Experience Platform**:

+ Toegang tot de standaardproductie, **Prod** zandbak.
+ Toegang tot **Schema&#39;s** onder het Beheer van Gegevens
+ Toegang tot **Datasets** onder het Beheer van Gegevens
+ Toegang tot **gegevensstromen** onder de Inzameling van Gegevens
+ Toegang tot **Markeringen** onder de Inzameling van Gegevens

Voor het geval u noodzakelijke toestemmingen niet hebt, kan uw systeembeheerder die [ Adobe Admin Console ](https://adminconsole.adobe.com/) gebruiken noodzakelijke toestemmingen verlenen.

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## XDM-schema maken - Experience Platform

Het schema van de Gegevens van de Ervaring Model (XDM) helpt u om de gegevens van de klantenervaring te standaardiseren. Om de **WKND Pageiew** gegevens te verzamelen, creeer een Schema XDM en gebruik de Adobe verstrekte gebiedsgroepen `AEP Web SDK ExperienceEvent` voor de inzameling van Webgegevens.

Er zijn generische en industrieën specifiek voor bijvoorbeeld Detailhandel, Financiële Diensten, Gezondheid, en meer, reeks van verwijzingsgegevensmodellen, zie [ overzicht van de gegevensmodellen van de Industrie ](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/industries/overview.html) voor meer informatie.


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

Leer over het Schema XDM en verwante concepten zoals gebiedsgroepen, types, klassen, en gegevenstypes van [ XDM overzicht van het Systeem ](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

Het [ XDM overzicht van het Systeem ](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) is een groot middel om over het Schema XDM en verwante concepten zoals gebiedsgroepen, types, klassen, en gegevenstypes te leren. Het biedt een uitgebreid inzicht in het XDM-gegevensmodel en hoe u XDM-schema&#39;s kunt maken en beheren om gegevens in de hele onderneming te standaardiseren. Onderzoek het om een dieper inzicht in het schema te verkrijgen XDM en hoe het uw gegevensinzameling en beheersprocessen kan ten goede komen.

## DataStream maken - Experience Platform

Een DataStream instrueert de Edge Network van het Platform waar te om de verzamelde gegevens te verzenden. Het kan bijvoorbeeld worden verzonden naar Experience Platform, Analytics of Adobe Target.


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

Vertrouwt u met het concept van gegevensstromen en verwante onderwerpen zoals gegevensbeheer en configuratie door de [ overzichtspagina van de Gegevensstromen van Gegevensstromen ](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html) te bezoeken.

## Tag-eigenschap maken - Experience Platform

Leer hoe u een eigenschap tag maakt in Experience Platform om de Web SDK JavaScript-bibliotheek aan de WKND-website toe te voegen. De nieuw gedefinieerde eigenschap tag heeft de volgende bronnen:

+ De Uitbreidingen van de markering: [ Kern ](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) en [ SDK van het Web van Adobe Experience Platform ](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ Gegevenselementen: de gegevenselementen van het type van douanecode die pagina-naam, plaats-sectie, en gastheer-naam gebruikend de Laag van de Gegevens van de Cliënt van de Adobe van de plaats WKND halen. Ook, leidt het het type van Objecten XDM gegevenselement dat met onlangs gecreeerd schema van WKND XDM bouwt-binnen [ tot XDM Schema ](#create-xdm-schema---experience-platform) stap.
+ Regel: verzend gegevens naar de Edge Network van het Platform wanneer een WebND website wordt bezocht gebruikend de Adobe de  van de Gegevens van de Cliënt teweeggebrachte `cmp:show` gebeurtenis.

Terwijl het bouwen van en het publiceren van de markeringsbibliotheek die de **het Publiceren Stroom** gebruiken, kunt u **gebruiken voeg Alle Gewijzigde Middelen** knoop toe. Om alle middelen zoals het Element van Gegevens, Regel, en de Uitbreidingen van de Markering in plaats van het identificeren van en het plukken van een individuele middel te selecteren. Ook, tijdens de ontwikkelingsfase, kunt u de bibliotheek enkel aan het _milieu van de Ontwikkeling_ publiceren, dan het verifiëren en bevorderen aan het _Stadium_ of _milieu van de Productie_.

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>Het Element van Gegevens en regel-Gebeurtenis code in de video wordt getoond is beschikbaar voor uw verwijzing, **breidt hieronder accordeonelement** uit. Nochtans, als u NIET de Laag van Gegevens van de Cliënt van de Adobe gebruikt, moet u de hieronder code wijzigen maar het concept van het bepalen van de Elementen van Gegevens en het gebruiken van hen in de definitie van de Regel blijft van toepassing.


+++ Gegevenselement en code voor regelgebeurtenissen

+ De `Page Name` Data Element-code.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
      return event.component['dc:title'];
  }
  ```

+ De `Site Section` Data Element-code.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('repo:path')) {
  let pagePath = event.component['repo:path'];
  
  let siteSection = '';
  
  //Check of html String in URL.
  if (pagePath.indexOf('.html') > -1) { 
   siteSection = pagePath.substring(0, pagePath.lastIndexOf('.html'));
  
   //replace slash with colon
   siteSection = siteSection.replaceAll('/', ':');
  
   //remove `:content`
   siteSection = siteSection.replaceAll(':content:','');
  }
  
      return siteSection 
  }
  ```

+ De `Host Name` Data Element-code.

  ```javascript
  if(window && window.location && window.location.hostname) {
      return window.location.hostname;
  }
  ```

+ De `all pages - on load` regel-Event-code

  ```javascript
  var pageShownEventHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      // trigger tags Rule and pass event
      console.debug("cmp:show event: " + evt.eventInfo.path);
      var event = {
          // include the path of the component that triggered the event
          path: evt.eventInfo.path,
          // get the state of the component that triggered the event
          component: window.adobeDataLayer.getState(evt.eventInfo.path)
      };
  
      // Trigger the tags Rule, passing in the new 'event' object
      // the 'event' obj can now be referenced by the reserved name 'event' by other tags data elements
      // i.e 'event.component['someKey']'
      trigger(event);
      }
  }
  
  // set the namespace to avoid a potential race condition
  window.adobeDataLayer = window.adobeDataLayer || [];
  
  // push the event listener for cmp:show into the data layer
  window.adobeDataLayer.push(function (dl) {
      //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
      dl.addEventListener("cmp:show", pageShownEventHandler);
  });
  ```

+++


Het [ overzicht van Markeringen ](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) verstrekt diepgaande kennis over belangrijke concepten zoals de Elementen van Gegevens, Regels, en Uitbreidingen.

Voor extra informatie bij het integreren AEM de Componenten van de Kern met de Laag van de Gegevens van de Cliënt van de Adobe, verwijs naar [ Gebruikend de Laag van de Gegevens van de Cliënt van de Adobe met de gids van de Componenten van de Kern AEM ](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html).

## Eigenschap van Connect-tag AEM

Ontdek hoe u de onlangs gemaakte eigenschap tag koppelt aan AEM via Adobe IMS en -tags in Adobe Experience Platform Configuration in AEM. Wanneer een AEM as a Cloud Service-omgeving tot stand is gebracht, worden automatisch verschillende configuraties van de technische account van Adobe IMS gegenereerd, waaronder codes. Nochtans, voor AEM versie 6.5, moet u manueel vormen.

Nadat de eigenschap tag is gekoppeld, kan de WKND-site de JavaScript-bibliotheek van de eigenschap tag op de webpagina&#39;s laden met de tags in de configuratie van de Adobe Experience Platform-cloudservice.

### Eigenschappen van tag verifiëren bij laden van WKND

Gebruikend Adobe Experience Platform Debugger [ Chrome ](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) uitbreiding, verifieer als het markeringsbezit op WKND pagina&#39;s laadt. U kunt verifiëren,

+ Eigenschappen van tags, zoals extensie, versie, naam en meer.
+ Platform Web SDK bibliotheekversie, DataStream ID
+ XDM Object als part `events` -kenmerk in Experience Platform Web SDK

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## Dataset maken - Experience Platform

De gegevens van de voorproef die het gebruiken van Web SDK worden verzameld worden opgeslagen in de gegevens van het Experience Platform meer als datasets. De dataset is een opslag en beheersconstructie voor een inzameling van gegevens zoals een gegevensbestandlijst die een schema volgt. Leer hoe te om een Dataset tot stand te brengen en vroeger te vormen DataStream om gegevens naar het Experience Platform te verzenden.


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

Het [ overzicht van Datasets ](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html) verstrekt meer informatie over concepten, configuraties, en andere inspraakmogelijkheden.


## WKND-voorvertoningsgegevens in Experience Platform

Na de opstelling van SDK van het Web met AEM, in het bijzonder op de plaats WKND, is het tijd om verkeer te produceren door door de plaatspagina&#39;s te navigeren. Dan bevestig dat de voorproefgegevens in de Dataset van het Experience Platform worden opgenomen. Binnen de Dataset UI, worden diverse details zoals totale verslagen, grootte, en ingebedde partijen getoond samen met een visueel aantrekkelijke balkgrafiek.

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## Samenvatting

Geweldig werk! U hebt de opstelling van AEM met het Web SDK van het Experience Platform voltooid om gegevens van een website te verzamelen en in te voeren. Met deze stichting, kunt u verdere mogelijkheden onderzoeken om producten zoals Analytics, Doel, Customer Journey Analytics (CJA), en vele anderen te verbeteren en te integreren om rijke, gepersonaliseerde ervaringen voor uw klanten tot stand te brengen. Blijf leren en verkennen om het volledige potentieel van Adobe Experience Cloud te benutten.

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)


>[!AVAILABILITY]
>
>Als u de **video van begin tot eind** verkiest die het volledige integratieproces in plaats van individuele video&#39;s van de opstellingsstap behandelt, kunt u [ hier ](https://video.tv.adobe.com/v/3418905/) klikken om tot het toegang te hebben.

## Aanvullende bronnen

+ [ Gebruikend de Laag van de Gegevens van de Cliënt van de Adobe met de Componenten van de Kern ](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)
+ [ het Integreren de Markeringen en de AEM van de Inzameling van de Gegevens van het Experience Platform ](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [ SDK van het Web van Adobe Experience Platform en overzicht van de Edge Network ](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [ leerprogramma&#39;s van de Inzameling van Gegevens ](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [ overzicht van het Adobe Experience Platform Debugger ](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
