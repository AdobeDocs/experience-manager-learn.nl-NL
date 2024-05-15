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

* [AEM auteur- en publicatieexemplaar](./implementation.md#set-up-aem) uitvoeren op respectievelijk localhost-poort 4502 en 4503
* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud met de volgende oplossingen
      * [Gegevensverzameling](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe Developer Console](https://developer.adobe.com/console/)

     >[!NOTE]
     >U moet over de juiste machtigingen beschikken om omgevingen in gegevensverzameling te ontwikkelen, goed te keuren, te publiceren, te beheren en te beheren. Als u geen van deze stappen kunt uitvoeren omdat de gebruikersinterfaceopties niet beschikbaar zijn, vraagt u de beheerder van het Experience Cloud om toegang. Voor meer informatie over tagmachtigingen: [zie de documentatie](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/user-permissions.html).

* **Chrome-browserextensies**
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
   * Samenstellen en publiceren
* AEM
   * Een Cloud Service maken
   * Maken

### Tags

#### Een eigenschap voor tags maken

Een eigenschap is een container die u vult met extensies, regels, gegevenselementen en bibliotheken wanneer u tags op uw site implementeert.

1. Ga naar uw organisaties [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`https://<yourcompany>.experiencecloud.adobe.com`)
1. Meld u aan met uw Adobe ID en zorg ervoor dat u zich in de juiste organisatie bevindt.
1. Klik vanuit de oplossingsschakelaar op **Experience Platform** en vervolgens de **Gegevensverzameling** en selecteert u **Tags**.

![Experience Cloud - tags](assets/using-launch-adobe-io/exc-cloud-launch.png)

1. Zorg ervoor dat u zich in de juiste organisatie bevindt en ga verder met het maken van een eigenschap voor tags.
   ![Experience Cloud - tags](assets/using-launch-adobe-io/launch-create-property.png)

   *Zie voor meer informatie over het maken van eigenschappen [Een eigenschap maken](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/companies-and-properties.html?lang=en#create-or-configure-a-property) in de productdocumentatie.*
1. Klik op de knop **Nieuwe eigenschap** knop
1. Geef een naam op voor uw eigenschap (bijvoorbeeld *Zelfstudie AEM*)
1. Als domein voert u *localhost.com* aangezien dit het domein is waar de WKND demo plaats loopt. Hoewel de &#39;*Domein*&#39; veld is vereist, werkt de eigenschap tags op elk domein waarin deze is geïmplementeerd. Het primaire doel van dit gebied is menuopties in de Bouwer van de Regel vooraf in te vullen.
1. Klik op de knop **Opslaan** knop.

   ![tags - Nieuwe eigenschap](assets/using-launch-adobe-io/exc-launch-property.png)

1. Open de eigenschap die u zojuist hebt gemaakt en klik op het tabblad Extensies.

#### Doelextensie toevoegen

De Adobe Target-extensie ondersteunt client-side implementaties met Target JavaScript SDK voor het moderne web. `at.js`. Klanten die nog steeds gebruikmaken van de oudere bibliotheek van Target, `mbox.js`, [moet worden bijgewerkt naar at.js](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html) om tags te gebruiken.

De doelextensie bestaat uit twee hoofdonderdelen:

* De extensieconfiguratie die de kernbibliotheekinstellingen beheert
* Handelingen van de regel om het volgende te doen:
   * Doel laden (at.js)
   * Params toevoegen aan alle vakken
   * Params toevoegen aan Global Mbox
   * Globale standaardmap

1. Onder **Extensies** kunt u de lijst met extensies zien die al zijn geïnstalleerd voor de eigenschap tags. ([Adobe Core-extensie starten](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) is standaard geïnstalleerd)
2. Klik op de knop **Extensiecatalogus** en zoek naar Doel in het filter.
3. Selecteer de nieuwste versie van Adobe Target om.js en klik op **Installeren** -optie.
   ![Tags - Nieuwe eigenschap](assets/using-launch-adobe-io/launch-target-extension.png)

4. Klikken op **Configureren** en u ziet het configuratievenster met de ingevoerde referenties van uw Target-account en de versie at.js voor deze extensie.
   ![Doel - Extension Config](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Als Doel wordt geïmplementeerd via asynchrone tags die insluiten-codes, moet u een voorverborgen fragment op uw pagina&#39;s vóór de tags insluiten-codes coderen om het flikkeren van de inhoud te beheren. We zullen later meer leren over de voorverborgen snipper. U kunt het voorverborgen fragment downloaden [hier](assets/using-launch-adobe-io/prehiding.js)

5. Klikken **Opslaan** om het toevoegen van de uitbreiding van het Doel aan uw markeringsbezit te voltooien, en u zou nu de uitbreiding van het Doel moeten kunnen zien onder **Geïnstalleerd** lijst met extensies.

6. Herhaal bovenstaande stappen om te zoeken naar de extensie &quot;Experience Cloud-id-service&quot; en installeer deze.
   ![Extension - Experience Cloud ID Service](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### Installatieomgevingen

1. Klik op de knop **Omgeving** voor uw site-eigenschap en u kunt de lijst met omgevingen zien die voor uw site-eigenschap worden gemaakt. Standaard hebben we elk één instantie gemaakt voor ontwikkeling, staging en productie.

![Data Element - Paginanaam](assets/using-launch-adobe-io/launch-environment-setup.png)

#### Samenstellen en publiceren

1. Klik op de knop **Publiceren** tabblad voor uw site-eigenschap. Laten we een bibliotheek maken om onze wijzigingen (gegevenselementen, regels) op te bouwen en in te voeren in een ontwikkelomgeving.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. Publiceer uw veranderingen van de Ontwikkeling aan een het Opvoeren milieu.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. Voer de **Optie Samenstellen voor stapelen**.
4. Zodra de bouwstijl volledig is, looppas **Goedkeuren voor publicatie**, die uw wijzigingen verplaatst van een testomgeving naar een productieomgeving.
   ![Staging naar productie](assets/using-launch-adobe-io/build-staging.png)
5. Als laatste voert u de **Samenstellen en publiceren voor productie** om uw wijzigingen in de productie door te voeren.
   ![Samenstellen en publiceren naar productie](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> De Adobe Developer-integratie toegang geven tot geselecteerde werkruimten met de juiste [rol om een centraal team toe te staan API-gedreven veranderingen in slechts een paar werkruimten aan te brengen](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html).

1. Maak IMS-integratie in AEM met behulp van referenties van Adobe Developer. (01:12 t/m 03:55)
2. Maak een eigenschap in Gegevensverzameling. (gedekt [boven](#create-launch-property))
3. Met de IMS-integratie van Stap 1 maakt u de integratie van tags om uw eigenschap tags te importeren.
4. Wijs in AEM de integratie van tags toe aan een site met behulp van de browserconfiguratie. (05:28 t/m 06:14)
5. Integratie handmatig valideren. (06:15 t/m 06:33)
6. Adobe Experience Cloud Debugger-browserplug-in gebruiken. (06:51 t/m 07:22)

U hebt nu de [AEM met Adobe Target met tags](./using-aem-cloud-services.md#integrating-aem-target-options) zoals beschreven in Optie 1.

Voor het gebruiken van AEM de aanbiedingen van het Fragment van de Ervaring om u verpersoonlijkingsactiviteiten te drijven, laat aan het volgende hoofdstuk verdergaan, en AEM met Adobe Target integreren gebruikend de erfeniswolkendiensten.
