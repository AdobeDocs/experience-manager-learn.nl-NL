---
title: Geavanceerde concepten van AEM headless - GraphQL
description: Een end-to-end zelfstudie waarin geavanceerde concepten van Adobe Experience Manager (AEM) GraphQL API's worden geïllustreerd.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
source-git-commit: 83e16ea87847182139982ea2378d8ff9f079c968
workflow-type: tm+mt
source-wordcount: '926'
ht-degree: 0%

---

# Geavanceerde concepten van AEM headless

Deze end-to-end zelfstudie vervolgt de [basiszelfstudie](../multi-step/overview.md) die betrekking hadden op de basisbeginselen van Adobe Experience Manager (AEM) Headless en GraphQL. De geavanceerde zelfstudie illustreert diepgaande aspecten van het werken met de Modellen van het Fragment van de Inhoud, de Fragments van de Inhoud, en AEM GraphQL API, met inbegrip van het gebruiken van AEM GraphQL in een cliënttoepassing.

## Vereisten

Voltooi de [snelle installatie voor AEM as a Cloud Service](../quick-setup/cloud-service.md) om uw omgeving te configureren.

U wordt ten zeerste aangeraden het vorige [basiszelfstudie](../multi-step/overview.md) en [videoreeks](../video-series/modeling-basics.md) zelfstudies voordat u verdergaat met deze geavanceerde zelfstudie. Hoewel u de zelfstudie kunt voltooien met een lokale AEM, wordt in deze zelfstudie alleen ingegaan op de workflow voor AEM as a Cloud Service.

## Doelstellingen

Deze zelfstudie behandelt de volgende onderwerpen:

* Maak modellen van inhoudsfragmenten met behulp van validatieregels en meer geavanceerde gegevenstypen, zoals tijdelijke aanduidingen voor tabbladen, geneste fragmentverwijzingen, JSON-objecten en gegevenstypen voor datum en tijd.
* Inhoudsfragmenten schrijven terwijl u werkt met geneste inhoud en fragmentverwijzingen, en mapbeleid configureren voor het schrijven van inhoudsfragmenten.
* Ontdek AEM GraphQL API mogelijkheden gebruikend vragen GraphQL met variabelen en richtlijnen.
* Blijft VRAAG GraphQL met parameters in AEM en leer hoe te om cache-controle parameters met persisted vragen te gebruiken.
* Integreer verzoeken voor voortgezette query&#39;s in de voorbeeldtoepassing WKND GraphQL React met de AEM Headless JavaScript SDK.

## Geavanceerde concepten van AEM overzicht zonder kop

De volgende video biedt een overzicht op hoog niveau van de concepten die in deze zelfstudie worden behandeld. De zelfstudie bevat het definiëren van Content Fragment Models met geavanceerdere gegevenstypen, het nesten van Content Fragments en het voortduren van GraphQL-query&#39;s in AEM.

>[!VIDEO](https://video.tv.adobe.com/v/340035/?quality=12&learn=on)

## Projectinstelling

Het project van de Plaats WKND heeft alle noodzakelijke configuraties, zodat kunt u het leerprogramma beginnen net nadat u voltooit [snelle installatie](../quick-setup/cloud-service.md). In deze sectie worden alleen enkele belangrijke stappen gemarkeerd die u kunt gebruiken bij het maken van uw eigen AEM Headless-project.

### Een configuratie maken

De eerste stap aan aanvang van om het even welk nieuw project in AEM is zijn configuratie, als werkruimte tot stand te brengen en eindpunten te creëren GraphQL API. Als u een configuratie wilt weergeven of maken, navigeert u naar **Gereedschappen** > **Algemeen** > **Configuratiebrowser**.

![Navigeren naar de configuratiebrowser](assets/overview/create-configuration.png)

Merk op dat de configuratie van de Plaats WKND reeds voor het leerprogramma is gecreeerd. Om een configuratie voor uw eigen project tot stand te brengen, selecteer **Maken** in de rechterbovenhoek en vul het formulier in in de modus Configuratie maken die wordt weergegeven.

### GraphQL API-eindpunten maken

Daarna, moet u API eindpunten vormen om vragen te verzenden GraphQL naar. Als u bestaande eindpunten wilt bekijken of maken, navigeert u naar **Gereedschappen** > **Activa** > **GraphQL**.

![Eindpunten configureren](assets/overview/endpoints.png)

Merk op dat de globale en eindpunten WKND reeds zijn gecreeerd. Om een eindpunt voor uw project tot stand te brengen, selecteer **Maken** in de rechterbovenhoek en volg de workflow.

>[!NOTE]
>
> Na het bewaren van het eindpunt, zult u een modaal over het bezoeken van de Console van de Veiligheid zien, die u toestaat om veiligheidsmontages aan te passen als u wenst om toegang tot het eindpunt te vormen. De toestemmingen van de veiligheid zelf zijn buiten het werkingsgebied van dit leerprogramma, echter. Raadpleeg voor meer informatie de [AEM](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html).

### Een hoofdtaalmap voor uw project maken

Een hoofdmap voor de taal is een map met als naam de ISO-taalcode EN FR. Het AEM vertaalbeheersysteem gebruikt deze mappen om de primaire taal van uw inhoud en talen voor het vertalen van inhoud te definiëren.

Ga naar **Navigatie** > **Activa** > **Bestanden**.

![Naar bestanden navigeren](assets/overview/files.png)

Navigeer in de **WKND-site** map. Neem de map waar met de titel &quot;Engels&quot; en de naam &quot;EN&quot;. Deze map is de hoofdmap van de taal voor het WKND-siteproject.

![Engelse map](assets/overview/english.png)

Voor uw eigen project, creeer een omslag van de taalwortel binnen uw configuratie. Zie de sectie over [mappen maken](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#create-folders) voor meer informatie .

### Een configuratie toewijzen aan de geneste map

Tot slot moet u de configuratie van uw project aan de omslag van de taalwortel toewijzen. Deze toewijzing laat de verwezenlijking van de Fragmenten van de Inhoud toe die op de Modellen worden gebaseerd van het Fragment van de Inhoud in de configuratie van uw project wordt bepaald.

Als u de hoofdmap van de taal aan de configuratie wilt toewijzen, selecteert u de map en selecteert u **Eigenschappen** in de bovenste navigatiebalk.

![Eigenschappen selecteren](assets/overview/properties.png)

Navigeer vervolgens naar de **Cloud Services** en selecteert u het mappictogram in het dialoogvenster **Cloud Configuration** veld.

![Cloud Configuration](assets/overview/cloud-conf.png)

In modaal die verschijnt, selecteer uw eerder gecreeerde configuratie om de taalwortelomslag aan het toe te wijzen.

### Aanbevolen procedures

Hier volgt een overzicht van aanbevolen procedures voor het maken van uw eigen project in AEM:

* De maphiërarchie moet zijn gebaseerd op lokalisatie en vertaling. Met andere woorden, taalmappen moeten in configuratiemappen worden genest, zodat de inhoud in die configuratiemappen gemakkelijk kan worden vertaald.
* De maphiërarchie moet vlak en eenvoudig worden gehouden. Vermijd het later verplaatsen of hernoemen van mappen en fragmenten, vooral na publicatie voor live gebruik, omdat paden worden gewijzigd die fragmentverwijzingen en GraphQL-query&#39;s kunnen beïnvloeden.

## Starter- en oplossingspakketten

Twee AEM **pakketten** zijn beschikbaar en kunnen worden geïnstalleerd via [Pakketbeheer](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content)

* [Advanced-GraphQL-Tutorial-Starter-Package-1.0.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.0.zip) wordt later in de zelfstudie gebruikt en bevat voorbeeldafbeeldingen en mappen.
* [Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip) Bevat de gebeëindigde oplossing voor Hoofdstuk 1-4 met inbegrip van nieuwe Modellen van het Fragment van de Inhoud, de Fragmenten van de Inhoud, en de Persisted vragen GraphQL. Nuttig voor degenen die naar de [Integratie van clienttoepassingen](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md) hoofdstuk

Twee projecten van React JS zijn beschikbaar om met vragen te experimenteren [van een clienttoepassing zonder kop](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).

* [aem-guides-wknd-headless-start-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-start-tutorial.zip) - clienttoepassing starten die is voltooid in [Hoofdstuk 5 - Integratie van clienttoepassingen](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).
* [aem-guides-wknd-headless-oplossing-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) - voltooide clienttoepassing die gebruikmaakt van **blijvend** query&#39;s.


## Aan de slag

Ga als volgt te werk om aan de slag te gaan met deze geavanceerde zelfstudie:

1. Een ontwikkelomgeving instellen met [AEM as a Cloud Service](../quick-setup/cloud-service.md).
1. Het hoofdstuk met zelfstudie starten op [maken, modellen van inhoudsfragmenten](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md).
