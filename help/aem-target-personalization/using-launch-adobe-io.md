---
title: Adobe Experience Manager integreren met Adobe Target door tags en Adobe Developer te gebruiken
description: Stapsgewijze doorlichting over hoe u Adobe Experience Manager met Adobe Target kunt integreren met tags en Adobe Developer
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b1d7ce04-0127-4539-a5e1-802d7b9427dd
duration: 663
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '985'
ht-degree: 0%

---

# Tags gebruiken via Adobe Developer Console

## Vereisten

* [ AEM auteur en publiceer instantie ](./implementation.md#set-up-aem) lopend op localhost haven 4502 en 4503
* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud met de volgende oplossingen
      * [ de Inzameling van Gegevens ](https://experiencecloud.adobe.com)
      * [ Adobe Target ](https://experiencecloud.adobe.com)
      * [ Adobe Developer Console ](https://developer.adobe.com/console/)

     >[!NOTE]
     >U moet over de juiste machtigingen beschikken om omgevingen in gegevensverzameling te ontwikkelen, goed te keuren, te Publish, te beheren en te beheren. Als u geen van deze stappen kunt uitvoeren omdat de gebruikersinterfaceopties niet beschikbaar zijn, vraagt u de beheerder van het Experience Cloud om toegang. Voor meer informatie over markeringen toestemmingen, [ zie de documentatie ](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/user-permissions.html?lang=nl-NL).

* **browser van Chrome uitbreidingen**
   * Adobe Experience Cloud-foutopsporing (https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)

## Betrokken gebruikers

Voor deze integratie, moet het volgende publiek worden betrokken, en om sommige taken uit te voeren, zou u administratieve toegang kunnen nodig hebben.

* Developer
* AEM
* Beheerder Experience Cloud

## Inleiding

AEM biedt een out of the box integratie met tags. Dankzij deze integratie kunnen AEM beheerders eenvoudig tags configureren via een gebruiksvriendelijke interface, waardoor de moeite en het aantal fouten worden verminderd wanneer deze twee gereedschappen worden geconfigureerd. En door Adobe Target-extensie toe te voegen aan tags, kunnen we alle functies van Adobe Target op de AEM webpagina(&#39;s) gebruiken.

In deze sectie zouden de volgende integratiestappen worden behandeld:

* Tags
   * Een eigenschap voor tags maken
   * Doelextensie toevoegen
   * Een gegevenselement maken
   * Een paginalijn maken
   * Installatieomgevingen
   * Build and Publish
* AEM
   * Een Cloud Service maken
   * Maken

### Tags

#### Een eigenschap voor tags maken

Een eigenschap is een container die u vult met extensies, regels, gegevenselementen en bibliotheken wanneer u tags op uw site implementeert.

1. Ga aan uw organisaties [ Adobe Experience Cloud ](https://experiencecloud.adobe.com/) (`https://<yourcompany>.experiencecloud.adobe.com`)
1. Meld u aan met uw Adobe ID en zorg ervoor dat u zich in de juiste organisatie bevindt.
1. Van de oplossingsschakelaar, klik op **Experience Platform**, toen de **sectie van de Inzameling van Gegevens**, en selecteer **Markeringen**.

![ Experience Cloud - markeringen ](assets/using-launch-adobe-io/exc-cloud-launch.png)

1. Zorg ervoor dat u zich in de juiste organisatie bevindt en ga verder met het maken van een eigenschap voor tags.
   ![ Experience Cloud - markeringen ](assets/using-launch-adobe-io/launch-create-property.png)

   *voor meer informatie bij het creëren van eigenschappen, zie [ een Bezit ](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/companies-and-properties.html?lang=nl-NL#create-or-configure-a-property) in de productdocumentatie creëren.*
1. Klik op het **Nieuwe Bezit** knoop
1. Verstrek een naam voor uw bezit (bijvoorbeeld, *AEM Zelfstudie van het Doel*)
1. Als domein, ga *localhost.com* in aangezien dit het domein is waar de WKND demo plaats op loopt. Hoewel het &quot;*gebied van het Domein*&quot;wordt vereist, zal het markeringsbezit op om het even welk domein werken waar het wordt uitgevoerd. Het primaire doel van dit gebied is menuopties in de Bouwer van de Regel vooraf in te vullen.
1. Klik **sparen** knoop.

   ![ markeringen - Nieuw Bezit ](assets/using-launch-adobe-io/exc-launch-property.png)

1. Open de eigenschap die u zojuist hebt gemaakt en klik op het tabblad Extensies.

#### Doelextensie toevoegen

De Adobe Target-extensie ondersteunt client-side implementaties met de Target JavaScript SDK voor het moderne web, `at.js` . De klanten nog die de oudere bibliotheek van het Doel gebruiken, `mbox.js`, [ zouden aan at.js ](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html?lang=nl-NL) moeten bevorderen om markeringen te gebruiken.

De doelextensie bestaat uit twee hoofdonderdelen:

* De extensieconfiguratie die de kernbibliotheekinstellingen beheert
* Handelingen van de regel om het volgende te doen:
   * Doel laden (at.js)
   * Params toevoegen aan alle vakken
   * Params toevoegen aan Global Mbox
   * Globale standaardmap

1. Onder **Uitbreidingen**, kunt u de lijst van Uitbreidingen zien die reeds voor uw markeringsbezit geïnstalleerd zijn. ([ de Uitbreiding van de Kern van de Start van de Adobe ](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) wordt geïnstalleerd door gebrek)
2. Klik op de **optie van de Catalogus van de Uitbreiding**, en onderzoek naar Doel in de filter.
3. Selecteer de recentste versie van Adobe Target at.js en klik op **installeer** optie.
   ![ Markeringen - Nieuw Bezit ](assets/using-launch-adobe-io/launch-target-extension.png)

4. Klik op **vormen** knoop, en u kunt het configuratievenster met uw ingevoerde de rekeningsgeloofsbrieven van het Doel opmerken, en de versie at.js voor deze uitbreiding.
   ![ Doel - de Configuratie van de Uitbreiding ](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Als Doel wordt geïmplementeerd via asynchrone tags die insluiten-codes, moet u een voorverborgen fragment op uw pagina&#39;s vóór de tags insluiten-codes coderen om het flikkeren van de inhoud te beheren. We zullen later meer leren over de voorverborgen snipper. U kunt het pre-verborgen fragment [ hier downloaden ](assets/using-launch-adobe-io/prehiding.js)

5. Klik **sparen** om het toevoegen van de uitbreiding van het Doel aan uw markeringsbezit te voltooien, en u zou nu de uitbreiding moeten kunnen zien van het Doel onder de **Geïnstalleerde** lijst van uitbreidingen.

6. Herhaal bovenstaande stappen om te zoeken naar de extensie &quot;Experience Cloud-id-service&quot; en installeer deze.
   ![ Uitbreiding - de Dienst van identiteitskaart van het Experience Cloud ](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### Installatieomgevingen

1. Klik op het **Milieu** lusje voor uw plaatsbezit, en u kunt de lijst van milieu zien dat voor uw plaatseigenschap wordt gecreeerd. Standaard hebben we elk één instantie gemaakt voor ontwikkeling, staging en productie.

![ het Element van Gegevens - de Naam van de Pagina ](assets/using-launch-adobe-io/launch-environment-setup.png)

#### Build and Publish

1. Klik op het **Publiceren** lusje voor uw plaatsbezit, en laten een bibliotheek tot stand brengen om, onze veranderingen (gegevenselementen, regels) aan een ontwikkelomgeving te bouwen en op te stellen.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. Publish uw wijzigingen van de ontwikkelomgeving naar een testomgeving.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. Stel **bouwt voor het Opvoeren van optie** in werking.
4. Zodra de bouwstijl volledig is, stel **voor het Publiceren** in werking, die uw veranderingen van een het Opvoeren milieu aan een milieu van de Productie beweegt.
   ![ het Staging aan Productie ](assets/using-launch-adobe-io/build-staging.png)
5. Tot slot in werking stellen de **Bouwstijl en Publish aan productie** optie om uw veranderingen in productie te duwen.
   ![ bouwt en Publish aan Productie ](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Verleen de integratie van Adobe Developer de toegang tot uitgezochte werkruimten met de aangewezen [ rol om een centraal team toe te staan om API-gedreven veranderingen in slechts een paar werkruimten ](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html?lang=nl-NL) aan te brengen.

1. Maak IMS-integratie in AEM met behulp van referenties van Adobe Developer. (01:12 t/m 03:55)
2. Maak een eigenschap in Gegevensverzameling. (Bedekte [ hierboven ](#create-launch-property))
3. Met de IMS-integratie van Stap 1 maakt u de integratie van tags om uw eigenschap tags te importeren.
4. Wijs in AEM de integratie van tags toe aan een site met behulp van de browserconfiguratie. (05:28 t/m 06:14)
5. Integratie handmatig valideren. (06:15 t/m 06:33)
6. Adobe Experience Cloud Debugger-browserplug-in gebruiken. (06:51 t/m 07:22)

Op dit punt, hebt u met succes [ AEM met Adobe Target geïntegreerd gebruikend markeringen ](./using-aem-cloud-services.md#integrating-aem-target-options) zoals die in Optie 1 worden gedetailleerd.

Voor het gebruiken van AEM de aanbiedingen van het Fragment van de Ervaring om u verpersoonlijkingsactiviteiten te drijven, laat aan het volgende hoofdstuk verdergaan, en AEM met Adobe Target integreren gebruikend de erfeniswolkendiensten.
