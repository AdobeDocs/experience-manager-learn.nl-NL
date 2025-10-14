---
title: Geavanceerde concepten van AEM Headless - GraphQL
description: Een end-to-end zelfstudie die geavanceerde concepten van Adobe Experience Manager (AEM) GraphQL APIs illustreert.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: daae6145-5267-4958-9abe-f6b7f469f803
duration: 441
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: tm+mt
source-wordcount: '1052'
ht-degree: 0%

---

# Geavanceerde concepten van AEM Headless

Dit leerprogramma van begin tot eind zet het [&#x200B; basisleerprogramma &#x200B;](../multi-step/overview.md) voort dat de grondbeginselen van de Hoofdtelefoon van Adobe Experience Manager (AEM) en GraphQL behandelde. De geavanceerde zelfstudie illustreert diepgaande aspecten van het werken met Modellen van het Fragment van de Inhoud, de Fragmenten van de Inhoud, en AEM GraphQL persisted vragen, met inbegrip van het gebruiken van GraphQL persisted vragen in een cliënttoepassing.

## Vereisten

Voltooi de [&#x200B; snelle opstelling voor AEM as a Cloud Service &#x200B;](../quick-setup/cloud-service.md) om uw milieu van AEM as a Cloud Service te vormen.

Het wordt hoogst geadviseerd dat u het vorige [&#x200B; basisleerprogramma &#x200B;](../multi-step/overview.md) voltooit en [&#x200B; videoreeks &#x200B;](../video-series/modeling-basics.md) leerprogramma&#39;s alvorens met dit geavanceerde leerprogramma te werk te gaan. Hoewel u de zelfstudie kunt voltooien met een lokale AEM-omgeving, is deze zelfstudie alleen van toepassing op de workflow voor AEM as a Cloud Service.

>[!CAUTION]
>
>Als u geen toegang tot het milieu van AEM as a Cloud Service hebt, kunt u [&#x200B; AEM Hoofdloze snelle opstelling voltooien gebruikend lokale SDK &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/quick-setup/local-sdk.html?lang=nl-NL). Het is echter belangrijk om op te merken dat sommige productpagina&#39;s van UI zoals de navigatie van de Fragmenten van de Inhoud verschillend zijn.



## Doelstellingen

Deze zelfstudie behandelt de volgende onderwerpen:

* Maak modellen van inhoudsfragmenten met behulp van validatieregels en meer geavanceerde gegevenstypen, zoals tijdelijke aanduidingen voor tabbladen, geneste fragmentverwijzingen, JSON-objecten en gegevenstypen voor datum en tijd.
* Inhoudsfragmenten schrijven terwijl u werkt met geneste inhoud en fragmentverwijzingen, en mapbeleid configureren voor governance voor het schrijven van inhoud.
* Ontdek de AEM GraphQL API-mogelijkheden met behulp van GraphQL-query&#39;s met variabelen en instructies.
* Blijft GraphQL vragen met parameters in AEM en leer hoe te om cache-controle parameters met persisted query&#39;s te gebruiken.
* Integreer aanvragen voor doorlopende query&#39;s in de voorbeeldtoepassing WKND GraphQL React met de AEM Headless JavaScript SDK.

## Geavanceerde concepten van AEM Headless-overzicht

De volgende video biedt een overzicht op hoog niveau van de concepten die in deze zelfstudie worden behandeld. De zelfstudie bevat het definiëren van modellen van inhoudsfragmenten met meer geavanceerde gegevenstypen, het nesten van inhoudsfragmenten en het voortduren van GraphQL-query&#39;s in AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3446133?quality=12&learn=on&captions=dut)

>[!CAUTION]
>
>In deze video (2:25) wordt gesproken over het installeren van de GraphiQL-query-editor via Package Manager om GraphQL-query&#39;s te verkennen. Nochtans in nieuwere versies van AEM als Cloud Service wordt de ingebouwde **Ontdekkingsreiziger GraphiQL verstrekt**, zodat wordt de pakketinstallatie niet vereist. Zie [&#x200B; Gebruikend GrahiQL winde &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html?lang=nl-NL) voor meer informatie.


## Projectinstelling

Het project van de Plaats WKND heeft alle noodzakelijke configuraties, zodat kunt u het leerprogramma beginnen net nadat u de [&#x200B; snelle opstelling &#x200B;](../quick-setup/cloud-service.md) voltooit. In deze sectie worden alleen enkele belangrijke stappen beschreven die u kunt gebruiken bij het maken van uw eigen AEM Headless-project.


### Bestaande configuratie controleren

De eerste stap naar het starten van elk nieuw project in AEM is het maken van de configuratie ervan als werkruimte en het maken van GraphQL API-eindpunten. Om een configuratie te herzien of tot stand te brengen, navigeer aan **Hulpmiddelen** > **Algemeen** > **Browser van de Configuratie**.

![&#x200B; ga aan Browser van de Configuratie &#x200B;](assets/overview/create-configuration.png)

Let erop dat de configuratie van de `WKND Shared` -site al is gemaakt voor de zelfstudie. Om een configuratie voor uw eigen project tot stand te brengen, creeert de uitgezochte **&#x200B;**&#x200B;in de hoger-juiste hoek en voltooit de vorm in Create de modale van de Configuratie die verschijnt.

![&#x200B; Overzicht WKND Gedeelde Configuratie &#x200B;](assets/overview/review-wknd-shared-configuration.png)

### GraphQL API-eindpunten controleren

Vervolgens moet u API-eindpunten configureren om GraphQL-query&#39;s te verzenden. Om bestaande eindpunten te herzien of te creëren, navigeer aan **Hulpmiddelen** > **Algemeen** > **GraphQL**.

![&#x200B; vorm eindpunten &#x200B;](assets/overview/endpoints.png)

Let op: `WKND Shared Endpoint` is al gemaakt. Om een eindpunt voor uw project tot stand te brengen, creeer **&#x200B;**&#x200B;in de hoger-juiste hoek en volg het werkschema.

![&#x200B; Gedeeld Eindpunt van het Overzicht WKND &#x200B;](assets/overview/review-wknd-shared-endpoint.png)

>[!NOTE]
>
> Na het bewaren van het eindpunt, zult u een modaal over het bezoeken van de Console van de Veiligheid zien, die u toestaat om veiligheidsmontages aan te passen als u wenst om toegang tot het eindpunt te vormen. De toestemmingen van de veiligheid zelf zijn buiten het werkingsgebied van dit leerprogramma, echter. Voor meer informatie, verwijs naar de [&#x200B; documentatie van AEM &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=nl-NL).

### WKND-inhoudsstructuur en taalhoofdmap controleren

Een goed gedefinieerde inhoudsstructuur is essentieel voor het succes van de AEM-implementatie zonder kop. Dit is handig voor schaalbaarheid, bruikbaarheid en toegangsbeheer van uw inhoud.

Een hoofdmap voor de taal is een map met als naam de ISO-taalcode EN FR. Het AEM-vertaalbeheersysteem gebruikt deze mappen om de primaire taal van uw inhoud en talen voor het vertalen van inhoud te definiëren.

Ga naar **Navigatie** > **Assets** > **Dossiers**.

![&#x200B; ga aan Dossiers &#x200B;](assets/overview/files.png)

Navigeer in de **Gedeelde WKND** omslag. Neem de map waar met de titel &quot;Engels&quot; en de naam &quot;EN&quot;. Deze map is de hoofdmap van de taal voor het WKND-siteproject.

![&#x200B; Engelse omslag &#x200B;](assets/overview/english.png)

Voor uw eigen project, creeer een omslag van de taalwortel binnen uw configuratie. Zie de sectie op [&#x200B; het creëren van omslagen &#x200B;](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#create-folders) voor meer details.

### Een configuratie toewijzen aan de geneste map

Tot slot moet u de configuratie van uw project aan de omslag van de taalwortel toewijzen. Deze toewijzing laat de verwezenlijking van de Fragmenten van de Inhoud toe die op de Modellen worden gebaseerd van het Fragment van de Inhoud in de configuratie van uw project wordt bepaald.

Om de omslag van de taalwortel aan de configuratie toe te wijzen, selecteer de omslag, dan selecteer **Eigenschappen** in de hoogste navigatiebar.

![&#x200B; Uitgezochte Eigenschappen &#x200B;](assets/overview/properties.png)

Daarna, navigeer aan het **lusje van de Diensten van de Wolk** en selecteer het omslagpictogram op het **gebied van de Configuratie van de Wolk**.

![&#x200B; Configuratie van de Wolk &#x200B;](assets/overview/cloud-conf.png)

In modaal die verschijnt, selecteer uw eerder gecreeerde configuratie om de taalwortelomslag aan het toe te wijzen.

### Aanbevolen procedures

Hier volgt een overzicht van aanbevolen procedures voor het maken van uw eigen project in AEM:

* De maphiërarchie moet zijn gebaseerd op lokalisatie en vertaling. Met andere woorden, taalmappen moeten in configuratiemappen worden genest, zodat de inhoud in die configuratiemappen gemakkelijk kan worden vertaald.
* De maphiërarchie moet vlak en eenvoudig worden gehouden. Vermijd het later verplaatsen of hernoemen van mappen en fragmenten, vooral na publicatie voor live gebruik, omdat paden worden gewijzigd die fragmentverwijzingen en GraphQL-query&#39;s kunnen beïnvloeden.

## Starter- en oplossingspakketten

Twee AEM **pakketten** zijn beschikbaar en kunnen via [&#x200B; de Manager van het Pakket &#x200B;](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content) worden geïnstalleerd

* [&#x200B; geavanceerd-GraphQL-zelfstudie-Starter-Pakket-1.1.zip &#x200B;](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip) wordt gebruikt later in het leerprogramma en bevat steekproefbeelden en omslagen.
* [&#x200B; geavanceerd-GraphQL-Tutorial-Oplossing-Pakket-1.2.zip &#x200B;](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip) bevat de gebeëindigde oplossing voor Hoofdstuk 1-4 met inbegrip van nieuwe Modellen van het Fragment van de Inhoud, de Fragmenten van de Inhoud, en de Gesterfde vragen van GraphQL. Nuttig voor hen die recht in het [&#128279;](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md) hoofdstuk van de Integratie van de Toepassing van de Cliënt 0&rbrace; willen overslaan.


Het [&#x200B; Reageer App - Geavanceerde Leerprogramma - WKND avonturen &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/advanced-tutorial/README.md) project is beschikbaar om de steekproeftoepassing te herzien en te onderzoeken. Deze voorbeeldtoepassing haalt de inhoud van AEM op door de voortgezette GraphQL-query&#39;s aan te roepen en rendert deze in een meeslepende ervaring.

## Aan de slag

Ga als volgt te werk om aan de slag te gaan met deze geavanceerde zelfstudie:

1. Opstelling een ontwikkelomgeving gebruikend [&#x200B; AEM as a Cloud Service &#x200B;](../quick-setup/cloud-service.md).
1. Begin het leerlingshoofdstuk op [&#x200B; creeer de Modellen van het Fragment van de Inhoud &#x200B;](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md).
