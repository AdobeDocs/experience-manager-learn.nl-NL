---
title: Adobe Experience Manager integreren met Adobe Target door Experience Platform Launch en Adobe I/O te gebruiken
seo-title: Adobe Experience Manager integreren met Adobe Target door Experience Platform Launch en Adobe I/O te gebruiken
description: Stap voor stap doorlopen op hoe u Adobe Experience Manager met Adobe Target kunt integreren met behulp van Experience Platform Launch en Adobe I/O
seo-description: Stap voor stap doorlopen op hoe u Adobe Experience Manager met Adobe Target kunt integreren met behulp van Experience Platform Launch en Adobe I/O
translation-type: tm+mt
source-git-commit: 1209064fd81238d4611369b8e5b517365fc302e3
workflow-type: tm+mt
source-wordcount: '1098'
ht-degree: 1%

---


# Adobe Experience Platform Launch gebruiken via Adobe I/O-console

## Vereisten

* [AEM auteur- en publicatieexemplaar](./implementation.md#set-up-aem) dat wordt uitgevoerd op respectievelijk localhost-poort 4502 en 4503
* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - <https://>`<yourcompany>`.experienceCloud.adobe.com
   * Experience Cloud voorzien van de volgende oplossingen
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O-console](https://console.adobe.io)

      >[!NOTE]
      >U moet over de juiste machtigingen beschikken om in Launch omgevingen te ontwikkelen, goed te keuren, te publiceren, te beheren en te beheren. Als u geen van deze stappen kunt uitvoeren omdat de gebruikersinterfaceopties niet beschikbaar zijn, vraagt u de beheerder van de Experience Cloud om toegang. Raadpleeg de documentatie [](https://docs.adobelaunch.com/administration/user-permissions)voor meer informatie over opstartmachtigingen.


* **Browserplug-ins**
   * Adobe Experience Cloud Debugger ([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
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

1. Ga naar uw organisaties, [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (<https://>`<yourcompany>`.ExperienceCloud.adobe.com)
2. Meld u aan met uw Adobe ID en zorg ervoor dat u zich in de juiste organisatie bevindt.
3. Klik vanuit de oplossingsschakelaar op **Starten** en selecteer vervolgens de knop **Ga naar starten** .

   ![Experience Cloud - Starten](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. Zorg ervoor u in de juiste organisatie bent en ga dan met het creëren van een bezit van de Lancering te werk.
   ![Experience Cloud - Starten](assets/using-launch-adobe-io/launch-create-property.png)

   *Zie[Een eigenschap](https://docs.adobelaunch.com/administration/companies-and-properties#create-a-property)maken in de productdocumentatie voor meer informatie over het maken van eigenschappen.*
5. Klik op de knop **Nieuwe eigenschap**
6. Geef een naam op voor uw eigenschap (bijvoorbeeld *AEM zelfstudie* over het doel)
7. Als domein, ga *localhost.com* in aangezien dit het domein is waar de WKND demo plaats loopt. Hoewel het veld &#39;*Domein*&#39; is vereist, werkt de eigenschap Launch op elk domein waar het is geïmplementeerd. Het primaire doel van dit gebied is menuopties in de Bouwer van de Regel vooraf in te vullen.
8. Klik op de knop **Opslaan** .

   ![Starten - Nieuwe eigenschap](assets/using-launch-adobe-io/exc-launch-property.png)

9. Open de eigenschap die u zojuist hebt gemaakt en klik op het tabblad Extensies.

#### Doelextensie toevoegen

De Adobe Target-extensie ondersteunt client-side implementaties met Target JavaScript SDK voor het moderne web, `at.js`. Klanten die nog steeds gebruikmaken van de oudere bibliotheek Target, `mbox.js`moeten een upgrade uitvoeren naar at.js [](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/upgrading-from-atjs-1x-to-atjs-20.html) om Launch te gebruiken.

De doelextensie bestaat uit twee hoofdonderdelen:

* De extensieconfiguratie die de kernbibliotheekinstellingen beheert
* Handelingen van de regel om het volgende te doen:
   * Doel laden (at.js)
   * Params toevoegen aan alle vakken
   * Params toevoegen aan Global Mbox
   * Globale standaardmap

1. Onder **Uitbreidingen** ziet u de lijst Extensies die al zijn geïnstalleerd voor de eigenschap Launch. ([Experience Platform Launch Core Extension](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html) is standaard geïnstalleerd)
2. Klik op de optie **Extensiecatalogus** en zoek naar Doel in het filter.
3. Selecteer de meest recente versie van Adobe Target in.js en klik op de optie **Installeren** .
   ![Starten - Nieuwe eigenschap](assets/using-launch-adobe-io/launch-target-extension.png)

4. Klik op de knop **Configureren** en u ziet dat het configuratievenster met de geïmporteerde referenties van uw doelaccount en de versie at.js voor deze extensie.
   ![Doel - Extension Config](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Wanneer Doel wordt geïmplementeerd via asynchrone insluitcodes voor Starten, moet u een voorverborgen fragment op uw pagina&#39;s vóór de insluitcodes voor Starten hard coderen om het flikkeren van de inhoud te beheren. We zullen later meer leren over de voorverborgen snipper. U kunt het voorverborgen fragment [hier downloaden](assets/using-launch-adobe-io/prehiding.js)

5. Klik op **Opslaan** om het toevoegen van de extensie Doel aan de eigenschap Starten te voltooien. U moet nu de doelextensie kunnen zien in de lijst **Geïnstalleerde** extensies.

6. Herhaal bovenstaande stappen om te zoeken naar de extensie &quot;Experience Cloud ID Service&quot; en installeer deze.
   ![Extension - Experience Cloud ID-service](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### Installatieomgevingen

1. Klik op het tabblad **Omgeving** voor uw site-eigenschap en u kunt de lijst met omgevingen zien die voor uw site-eigenschap worden gemaakt. Standaard hebben we elk één instantie gemaakt voor ontwikkeling, staging en productie.

![Gegevenselement - paginanaam](assets/using-launch-adobe-io/launch-environment-setup.png)

#### Samenstellen en publiceren

1. Klik op het tabblad **Publiceren** voor uw site-eigenschap en maak een bibliotheek om onze wijzigingen (gegevenselementen, regels) te maken en in te voeren in een ontwikkelomgeving.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. Publiceer uw veranderingen van de Ontwikkeling aan een het Opvoeren milieu.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. Voer de optie **Samenstellen voor Staging uit**.
4. Zodra de bouwstijl volledig is, stel **Goedkeuren voor het Publiceren**in werking, die uw veranderingen van een het Opvoeren milieu aan een milieu van de Productie beweegt.
   ![Staging naar productie](assets/using-launch-adobe-io/build-staging.png)
5. Tot slot stel de optie **Bouwstijl en Publiceren aan productie** in werking om uw veranderingen in productie te duwen.
   ![Samenstellen en publiceren naar productie](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Verleen de Adobe I/O integratie de toegang tot uitgezochte werkruimten met de aangewezen [rol om een centraal team toe te staan API-gedreven veranderingen in slechts een paar werkruimten](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html)aan te brengen.

1. Maak IMS-integratie in AEM met behulp van referenties van Adobe I/O. (01:12 t/m 03:55)
2. Maak in Experience Platform Launch een eigenschap. ( [hierboven](#create-launch-property)behandeld)
3. Gebruikend de integratie IMS van Stap 1, creeer de integratie van het Experience Platform Launch om uw bezit van de Lancering in te voeren.
4. Wijs in AEM de integratie van het Experience Platform Launch aan een plaats toe gebruikend browser configuratie. (05:28 t/m 06:14)
5. Integratie handmatig valideren. (06:15 t/m 06:33)
6. Insteekmodule voor Starten/DTM-browser gebruiken. (06:34 t/m 06:50)
7. Adobe Experience Cloud Debugger-browserplug-in gebruiken. (06:51 t/m 07:22)

U hebt nu [AEM met Adobe Target geïntegreerd met Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options) , zoals beschreven in Optie 1.

Voor het gebruiken van AEM de aanbiedingen van het Fragment van de Ervaring om u verpersoonlijkingsactiviteiten te drijven, laat aan het volgende hoofdstuk verdergaan, en AEM met Adobe Target integreren gebruikend de erfeniswolkendiensten.
