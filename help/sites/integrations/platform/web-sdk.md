---
title: Experience Platform Web SDK integreren
description: Leer hoe te om AEM as a Cloud Service met het Web SDK van het Experience Platform te integreren. Deze basisstap is essentieel voor de integratie van Adobe Experience Cloud-producten, zoals Adobe Analytics, Target of recente innovatieve producten zoals Real-time Customer Data Platform, Customer Journey Analytics en Journey Optimizer.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
exl-id: b5182d35-ec38-4ffd-ae5a-ade2dd3f856d
source-git-commit: 32472c8591aeb47a7c6a7253afd7ad9ab0e45171
workflow-type: tm+mt
source-wordcount: '1340'
ht-degree: 0%

---

# Experience Platform Web SDK integreren

Leer hoe u AEM as a Cloud Service kunt integreren met Experience Platform [Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Deze basisstap is essentieel voor de integratie van Adobe Experience Cloud-producten, zoals Adobe Analytics, Target of recente innovatieve producten zoals Real-time Customer Data Platform, Customer Journey Analytics en Journey Optimizer.

U leert ook hoe u verzamelt en verzendt [WKND - voorbeeld Adobe Experience Manager-project](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) voorvertoningsgegevens in het dialoogvenster [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html).

Na voltooiing van deze opstelling, hebt u een stevige stichting uitgevoerd. Ook bent u bereid om de implementatie van het Experience Platform te bevorderen gebruikend toepassingen zoals [Real-time Customer Data Platform (Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html), [Customer Journey Analytics (CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html), en [Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html). De geavanceerde implementatie draagt bij aan een betere betrokkenheid van klanten door het web en de gegevens van klanten te standaardiseren.

## Vereisten

Het volgende wordt vereist wanneer het integreren van het Web SDK van het Experience Platform.

In **AEM als Cloud Service**:

+ Toegang van AEM beheerder tot AEM as a Cloud Service omgeving
+ Toegang tot Cloud Manager via Deployment Manager
+ Klonen en implementeren [WKND - voorbeeld Adobe Experience Manager-project](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) naar uw AEM as a Cloud Service omgeving.

In **Experience Platform**:

+ toegang tot de standaardproductie; **Prod** sandbox.
+ Toegang tot **Schemas** onder Gegevensbeheer
+ Toegang tot **Gegevenssets** onder Gegevensbeheer
+ Toegang tot **DataStreams** onder Gegevensverzameling
+ Toegang tot **Tags** (voorheen bekend als Launch) onder Gegevensverzameling

Als u niet de noodzakelijke toestemmingen hebt, gebruikt uw systeembeheerder [Adobe Admin Console](https://adminconsole.adobe.com/) kan de benodigde machtigingen verlenen.

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## XDM-schema maken - Experience Platform

Het schema van de Gegevens van de Ervaring Model (XDM) helpt u om de gegevens van de klantenervaring te standaardiseren. Om de **WKND-voorvertoning** gegevens, creeer een Schema XDM en gebruik de Adobe verstrekte gebiedsgroepen `AEP Web SDK ExperienceEvent` voor webgegevensverzameling.

Er zijn generieke en industrieën specifiek bijvoorbeeld Detailhandel, Financiële Diensten, Gezondheidszorg, en meer, reeks modellen van verwijzingsgegevens, zie [Overzicht bedrijfsgegevensmodellen](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/industries/overview.html) voor meer informatie .


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

Leer over het Schema XDM en verwante concepten zoals gebiedsgroepen, types, klassen, en gegevenstypes van [XDM System, overzicht](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

De [XDM System, overzicht](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) is een groot middel om over Schema XDM en verwante concepten zoals gebiedsgroepen, types, klassen, en gegevenstypes te leren. Het biedt een uitgebreid inzicht in het XDM-gegevensmodel en hoe u XDM-schema&#39;s kunt maken en beheren om gegevens in de hele onderneming te standaardiseren. Onderzoek het om een dieper inzicht in het schema te verkrijgen XDM en hoe het uw gegevensinzameling en beheersprocessen kan ten goede komen.

## DataStream maken - Experience Platform

Een DataStream instrueert het Netwerk van de Rand van het Platform waar te om de verzamelde gegevens te verzenden. Het kan bijvoorbeeld worden verzonden naar Experience Platform, Analytics of Adobe Target.


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

U vertrouwd maken met het concept van gegevensstromen en verwante onderwerpen zoals gegevensbeheer en configuratie door de [Overzicht gegevensstromen](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html) pagina.

## Tag-eigenschap maken - Experience Platform

Leer hoe u een tageigenschap (voorheen Launch genoemd) in Experience Platform maakt om de JavaScript-bibliotheek van de Web SDK aan de WKND-website toe te voegen. De nieuw gedefinieerde eigenschap tag heeft de volgende bronnen:

+ Tagextensies: [Kern](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) en [Adobe Experience Platform Web SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ Gegevenselementen: De gegevenselementen van het type van douanecode die pagina-naam, plaats-sectie, en gastheer-naam gebruikend de Laag van de Gegevens van de Cliënt van de Adobe van de plaats WKND halen. Ook, het het type van Objecten XDM gegevenselement dat met onlangs gecreeerd schema van WKND XDM bouwt vroeger in overeenstemming is [XDM-schema maken](#create-xdm-schema---experience-platform) stap.
+ Regel: Gegevens verzenden naar Platform Edge Network wanneer een WKND-webpagina wordt bezocht met behulp van de Adobe Client Data Layer geactiveerd `cmp:show` gebeurtenis.

Tijdens het maken en publiceren van de tagbibliotheek met de **Publishing Flow** kunt u de **Alle gewijzigde bronnen toevoegen** knop. Om alle middelen zoals het Element van Gegevens, Regel, en de Uitbreidingen van de Markering in plaats van het identificeren van en het plukken van een individuele middel te selecteren. Tijdens de ontwikkelingsfase kunt u de bibliotheek ook alleen publiceren naar de _Ontwikkeling_ milieu, dan verifieert en bevordert het aan _Werkgebied_ of _Productie_ milieu.

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>Het gegevenselement en regel-gebeurtenis code in de video wordt getoond is beschikbaar voor uw verwijzing die, **het element onder accordeon uitvouwen**. Nochtans, als u de Laag van Gegevens van de Cliënt NIET gebruikt, moet u de hieronder code wijzigen maar het concept van het bepalen van de Elementen van Gegevens en het gebruiken van hen in de definitie van de Regel blijft van toepassing.


+++ Gegevenselement en code voor regelgebeurtenissen

+ De `Page Name` Code gegevenselement.

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
       return event.component['dc:title'];
   }
   ```

+ De `Site Section` Code gegevenselement.

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

+ De `Host Name` Code gegevenselement.

   ```javascript
   if(window && window.location && window.location.hostname) {
       return window.location.hostname;
   }
   ```

+ De `all pages - on load` Code regelgebeurtenis

   ```javascript
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
   
       //Trigger the Launch Rule, passing in the new 'event' object
       // the 'event' obj can now be referenced by the reserved name 'event' by other Launch data elements
       // i.e 'event.component['someKey']'
       trigger(event);
       }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
       //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
       dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

+++


De [Overzicht van codes](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) biedt diepgaande kennis van belangrijke concepten zoals Data Elements, Rules en Extensions.

Voor extra informatie bij het integreren van AEM Componenten van de Kern met de Laag van de Gegevens van de Cliënt van Adobe raadpleegt u [De Adobe Client Data Layer gebruiken met AEM Core Components Guide](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html).

## Eigenschap van Connect-tag AEM

Ontdek hoe u de onlangs gemaakte tageigenschap koppelt aan AEM via Adobe IMS en Adobe Launch Configuration in AEM. Wanneer een AEM as a Cloud Service omgeving wordt ingesteld, worden automatisch verschillende configuraties van de technische account van Adobe IMS gegenereerd, waaronder Adobe Launch. Nochtans, voor AEM versie 6.5, moet u manueel vormen.

Nadat de eigenschap tag is gekoppeld, kan de WKND-site de JavaScript-bibliotheek van de tag-eigenschap op de webpagina&#39;s laden met de configuratie van de Adobe Launch-cloudservice.

### Eigenschappen van tag verifiëren bij laden van WKND

Adobe Experience Platform Debugger gebruiken [Chroom](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) of [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) controleren of de eigenschap tag wordt geladen op WKND-pagina&#39;s. U kunt verifiëren,

+ Eigenschappen van tags, zoals extensie, versie, naam en meer.
+ Platform Web SDK bibliotheekversie, DataStream ID
+ XDM-object als onderdeel `events` kenmerk in Experience Platform Web SDK

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## Dataset maken - Experience Platform

De gegevens van de voorproef die het gebruiken van Web SDK worden verzameld worden opgeslagen in de gegevens van het Experience Platform meer als datasets. De dataset is een opslag en beheersconstructie voor een inzameling van gegevens zoals een gegevensbestandlijst die een schema volgt. Leer hoe te om een Dataset tot stand te brengen en vroeger te vormen DataStream om gegevens naar het Experience Platform te verzenden.


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

De [Overzicht van gegevenssets](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html) verstrekt meer informatie over concepten, configuraties, en andere inspraakmogelijkheden.


## WKND-voorvertoningsgegevens in Experience Platform

Na de opstelling van SDK van het Web met AEM, in het bijzonder op de plaats WKND, is het tijd om verkeer te produceren door door de plaatspagina&#39;s te navigeren. Dan bevestig dat de voorproefgegevens in de Dataset van het Experience Platform worden opgenomen. Binnen de Dataset UI, worden diverse details zoals totale verslagen, grootte, en ingebedde partijen getoond samen met een visueel aantrekkelijke balkgrafiek.

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## Samenvatting

Geweldig werk! U hebt de opstelling van AEM met het Web SDK van het Experience Platform voltooid om gegevens van een website te verzamelen en in te voeren. Met deze basis kunt u nu verdere mogelijkheden verkennen om producten zoals Analytics, Target, Customer Journey Analytics (CJA) en vele andere te verbeteren en te integreren om rijke, gepersonaliseerde ervaringen voor uw klanten te creëren. Blijf leren en verkennen om het volledige potentieel van Adobe Experience Cloud te benutten.

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)


>[!AVAILABILITY]
>
>Als u de voorkeur geeft aan **end-to-end video** dat het volledige integratieproces in plaats van individuele opstellingsstapvideo&#39;s behandelt, kunt u klikken [hier](https://video.tv.adobe.com/v/3418905/) om toegang te krijgen tot het bestand.

## Aanvullende bronnen

+ [Het gebruiken van de Laag van de Gegevens van de Cliënt van Adobe met de Componenten van de Kern](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)
+ [Tags en AEM voor gegevensverzameling van Experience Platforms integreren](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [Overzicht van Adobe Experience Platform Web SDK en Edge Network](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [Tutorials voor gegevensverzameling](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Overzicht van Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
