---
title: Adobe Experience Manager integreren met Adobe Target door middel van Experience Platform Launch en Adobe I/O
seo-title: Integrating Adobe Experience Manager with Adobe Target using Experience Platform Launch and Adobe I/O
description: Stap voor stap door hoe u Adobe Experience Manager met Adobe Target kunt integreren met behulp van Experience Platform Launch en Adobe I/O
seo-description: Step by step walk-through on how to integrate Adobe Experience Manager with Adobe Target using Experience Platform Launch and Adobe I/O
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1064'
ht-degree: 1%

---


# Adobe Experience Platform Launch gebruiken via Adobe I/O Console

## Vereisten

* [AEM auteur en publiceer ](./implementation.md#set-up-aem) instormsessies op respectievelijk localhost port 4502 en 4503
* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud voorzien van de volgende oplossingen
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O-console](https://console.adobe.io)

      >[!NOTE]
      >U moet over de juiste machtigingen beschikken om in Launch omgevingen te ontwikkelen, goed te keuren, te publiceren, te beheren en te beheren. Als u geen van deze stappen kunt uitvoeren omdat de gebruikersinterfaceopties niet beschikbaar zijn, vraagt u de beheerder van de Experience Cloud om toegang. Voor meer informatie over de toestemmingen van de Lancering, [zie de documentatie](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/user-permissions.html).


* **Browserplug-ins**
   * Adobe Experience Cloud-foutopsporing ([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * Starten en DTM-switch ([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## Betrokken gebruikers

Voor deze integratie, moet het volgende publiek worden betrokken, en om sommige taken uit te voeren, zou u administratieve toegang kunnen nodig hebben.

* Developer
* AEM Admin
* Experience Cloud-beheerder

## Inleiding

AEM biedt een out of the box integratie met Experience Platform Launch. Deze integratie staat AEM beheerders toe om Experience Platform Launch via een gemakkelijk te gebruiken interface gemakkelijk te vormen, daardoor verminderend het niveau van inspanning en aantal fouten, wanneer het vormen van deze twee hulpmiddelen. En door Adobe Target-extensie toe te voegen aan Experience Platform Launch, kunnen we alle functies van Adobe Target op de AEM webpagina(&#39;s) gebruiken.

In deze sectie zouden de volgende integratiestappen worden behandeld:

* Starten
   * Een opstarteigenschap maken
   * Doelextensie toevoegen
   * Een gegevenselement maken
   * Een paginalijn maken
   * Installatieomgevingen
   * Samenstellen en publiceren
* AEM
   * Een Cloud Service maken
   * Maken

### Starten

#### Een opstarteigenschap maken

Een eigenschap is een container die u vult met extensies, regels, gegevenselementen en bibliotheken wanneer u tags op uw site implementeert.

1. Naar uw organisaties navigeren [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`https://<yourcompany>.experiencecloud.adobe.com`)
2. Meld u aan met uw Adobe ID en zorg ervoor dat u zich in de juiste organisatie bevindt.
3. Van de oplossingsschakelaar, klik op **Lancering** en selecteer dan **Ga naar Lancering** knoop.

   ![Experience Cloud - Starten](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. Zorg ervoor u in de juiste organisatie bent en ga dan met het creëren van een bezit van de Lancering te werk.
   ![Experience Cloud - Starten](assets/using-launch-adobe-io/launch-create-property.png)

   *Zie  [Eigenschappen maken in de ](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/companies-and-properties.html?lang=en#create-or-configure-a-property) productdocumentatie voor meer informatie over het maken van eigenschappen.*
5. Klik op de knop **Nieuwe eigenschap**
6. Geef een naam op voor uw eigenschap (bijvoorbeeld *AEM Zelfstudie doel*)
7. Als domein, ga *localhost.com* in aangezien dit het domein is waar de WKND demo plaats loopt. Hoewel het &quot;*Domein*&quot;gebied wordt vereist, zal het bezit van de Lancering aan om het even welk domein werken waar het wordt uitgevoerd. Het primaire doel van dit gebied is menuopties in de Bouwer van de Regel vooraf in te vullen.
8. Klik op de knop **Opslaan**.

   ![Starten - Nieuwe eigenschap](assets/using-launch-adobe-io/exc-launch-property.png)

9. Open de eigenschap die u zojuist hebt gemaakt en klik op het tabblad Extensies.

#### Doelextensie toevoegen

De Adobe Target-extensie ondersteunt client-side implementaties met de Target JavaScript SDK voor het moderne web, `at.js`. Klanten die nog gebruikmaken van de oudere bibliotheek Target, `mbox.js`, [dienen een upgrade uit te voeren naar at.js](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html) om Launch te gebruiken.

De doelextensie bestaat uit twee hoofdonderdelen:

* De extensieconfiguratie die de kernbibliotheekinstellingen beheert
* Handelingen van de regel om het volgende te doen:
   * Doel laden (at.js)
   * Params toevoegen aan alle vakken
   * Params toevoegen aan Global Mbox
   * Globale standaardmap

1. Onder **Extensies**, kunt u de lijst zien van Uitbreidingen die reeds voor uw bezit van de Lancering geïnstalleerd zijn. ([Experience Platform Launch Core Extension](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html) is standaard geïnstalleerd)
2. Klik op de optie **Extension Catalog** en zoek naar Doel in het filter.
3. Selecteer de nieuwste versie van Adobe Target at.js en klik op **Install** optie.
   ![Starten - Nieuwe eigenschap](assets/using-launch-adobe-io/launch-target-extension.png)

4. Klik op **Configure** knoop, en u kunt het configuratievenster opmerken met uw ingevoerde de rekeningsgeloofsbrieven van het Doel, en de versie at.js voor deze uitbreiding.
   ![Doel - Extension Config](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Wanneer Doel wordt geïmplementeerd via asynchrone insluitcodes voor Starten, moet u een voorverborgen fragment op uw pagina&#39;s vóór de insluitcodes voor Starten hard coderen om het flikkeren van de inhoud te beheren. We zullen later meer leren over de voorverborgen snipper. U kunt het voorverborgen fragment [hier](assets/using-launch-adobe-io/prehiding.js) downloaden

5. Klik **Save** om het toevoegen van de uitbreiding van het Doel aan uw bezit van de Lancering te voltooien, en u zou de uitbreiding van het Doel nu moeten kunnen zien onder **Geïnstalleerde** uitbreidingslijst wordt vermeld die.

6. Herhaal bovenstaande stappen om te zoeken naar de extensie &quot;Experience Cloud ID Service&quot; en installeer deze.
   ![Extension - Experience Cloud ID-service](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### Installatieomgevingen

1. Klik op het tabblad **Omgeving** voor uw site-eigenschap en u kunt de lijst met omgevingen zien die voor uw site-eigenschap worden gemaakt. Standaard hebben we elk één instantie gemaakt voor ontwikkeling, staging en productie.

![Gegevenselement - paginanaam](assets/using-launch-adobe-io/launch-environment-setup.png)

#### Samenstellen en publiceren

1. Klik op het **Publiceren** lusje voor uw plaatseigenschap, en laten wij een bibliotheek tot stand brengen om te bouwen, en onze veranderingen (gegevenselementen, regels) in een ontwikkelomgeving op te stellen.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. Publiceer uw veranderingen van de Ontwikkeling aan een het Opvoeren milieu.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. Voer **Build for Staging option** uit.
4. Zodra de bouwstijl volledig is, stel **goed voor het Publiceren** in werking, die uw veranderingen van een het Opvoeren milieu aan een milieu van de Productie beweegt.
   ![Staging naar productie](assets/using-launch-adobe-io/build-staging.png)
5. Tot slot stel **Bouwen en publiceren aan productie** optie in werking om uw veranderingen in productie te duwen.
   ![Samenstellen en publiceren naar productie](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Verleen de integratie van Adobe I/O de toegang tot uitgezochte werkruimten met de aangewezen [rol om een centraal team toe te staan om API-gedreven veranderingen in slechts een paar werkruimten aan te brengen](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html).

1. Maak IMS-integratie in AEM met behulp van referenties van Adobe I/O. (01:12 t/m 03:55)
2. Maak in Experience Platform Launch een eigenschap. (bedekt [boven](#create-launch-property))
3. Gebruikend de integratie IMS van Stap 1, creeer de integratie van het Experience Platform Launch om uw bezit van de Lancering in te voeren.
4. Wijs in AEM de integratie van het Experience Platform Launch aan een plaats toe gebruikend browser configuratie. (05:28 t/m 06:14)
5. Integratie handmatig valideren. (06:15 t/m 06:33)
6. Insteekmodule voor Starten/DTM-browser gebruiken. (06:34 t/m 06:50)
7. Adobe Experience Cloud Debugger-browserplug-in gebruiken. (06:51 t/m 07:22)

U hebt [AEM nu geïntegreerd met Adobe Target met Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options) zoals beschreven in Option 1.

Voor het gebruiken van AEM de aanbiedingen van het Fragment van de Ervaring om u verpersoonlijkingsactiviteiten te drijven, laat aan het volgende hoofdstuk verdergaan, en AEM met Adobe Target integreren gebruikend de erfeniswolkendiensten.
