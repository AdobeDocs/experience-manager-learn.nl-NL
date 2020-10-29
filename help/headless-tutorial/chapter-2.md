---
title: Hoofdstuk 2 - Fragmentmodellen voor gebeurtenisinhoud definiëren
seo-title: Aan de slag met AEM Content Services - Hoofdstuk 2 - Modellen voor gebeurteniscontentfragmenten definiëren
description: Hoofdstuk 2 van de AEM zelfstudie zonder koppen omvat het inschakelen en definiëren van modellen van inhoudsfragmenten die worden gebruikt om een genormaliseerde gegevensstructuur en ontwerpinterface te definiëren voor het maken van gebeurtenissen.
seo-description: Hoofdstuk 2 van de AEM zelfstudie zonder koppen omvat het inschakelen en definiëren van modellen van inhoudsfragmenten die worden gebruikt om een genormaliseerde gegevensstructuur en ontwerpinterface te definiëren voor het maken van gebeurtenissen.
translation-type: tm+mt
source-git-commit: 1faf22f2e664b775c11e16cb1dfa18b363a7316b
workflow-type: tm+mt
source-wordcount: '863'
ht-degree: 1%

---


# Hoofdstuk 2 - Modellen voor inhoudsfragmenten gebruiken

AEM Content Fragment Models definieert inhoudsschema&#39;s die kunnen worden gebruikt om het maken van onbewerkte inhoud door AEM auteurs te optimaliseren. Deze aanpak lijkt op steigers of op formulieren gebaseerde ontwerpen. Het belangrijkste concept bij Content Fragments is dat de geschreven inhoud presentatie-agnostisch is, wat betekent zijn voorgenomen voor multi-kanaals gebruik waar de het verbruiken toepassing, namelijk AEM, één enkele paginatoepassing, of een Mobiele app, controleert hoe de inhoud aan de gebruiker wordt getoond.

Het belangrijkste doel van het inhoudsfragment is ervoor te zorgen dat:

1. De juiste inhoud wordt verzameld van de auteur
2. De inhoud kan in een gestructureerde, goed begrepen formaat aan het verbruiken van toepassingen worden blootgesteld.

In dit hoofdstuk wordt beschreven hoe u modellen van inhoudsfragmenten kunt inschakelen en definiëren voor het definiëren van een genormaliseerde gegevensstructuur en ontwerpinterface voor het modelleren en maken van &quot;Gebeurtenissen&quot;.

## Modellen van inhoudsfragmenten inschakelen

Modellen van inhoudsfragmenten **moeten** via **[AEM zijn ingeschakeld [!UICONTROL Configuration Browser]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html)**.

Als Modellen van het Fragment van de Inhoud **niet** voor een configuratie worden toegelaten, zal de **[!UICONTROL Create]>[!UICONTROL Content Fragment]** knoop niet voor de relevante AEM configuratie verschijnen.

>[!NOTE]
>
>AEM configuraties vertegenwoordigen een reeks [context-bewuste huurdersconfiguraties](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) die onder worden opgeslagen `/conf`. AEM configuraties correleren doorgaans met een bepaalde website die wordt beheerd in AEM Sites of een bedrijfseenheid die verantwoordelijk is voor een subset met inhoud (elementen, pagina&#39;s, enz.) in AEM.
>
>Opdat een configuratie een inhoudshiërarchie beïnvloedt, moet de configuratie via het `cq:conf` bezit op die inhoudshiërarchie worden van verwijzingen voorzien. (Dit wordt bereikt voor de [!DNL WKND Mobile] configuratie in **Stap 5** hieronder).
>
>Wanneer de `global` configuratie wordt gebruikt, is de configuratie op alle inhoud van toepassing, en te hoeven niet `cq:conf` worden geplaatst.
>
>Raadpleeg de [[!UICONTROL Configuration Browser] documentatie](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html) voor meer informatie.

1. Meld u aan bij AEM-auteur als een gebruiker met de juiste machtigingen om de relevante configuratie te wijzigen.
   * Voor deze zelfstudie kan de gebruiker van de **beheerder** worden gebruikt.
1. Ga naar **[!UICONTROL Tool]> [!UICONTROL General] >[!UICONTROL Configuration Browser]**
1. Tik op het **mappictogram** naast **[!DNL WKND Mobile]** het pictogram en tik vervolgens linksboven op de **[!UICONTROL Edit]knop** .
1. Selecteer **[!UICONTROL Content Fragment Models]** en tik **[!UICONTROL Save & Close]** in de rechterbovenhoek.

   Dit laat van Modellen van het Fragment van de Inhoud op de inhoudbomen van de Omslag van Activa toe die de toegepaste [!DNL WKND Mobile] configuratie hebben.

   >[!NOTE]
   >
   >Deze configuratieverandering is niet omkeerbaar van het [!UICONTROL AEM Configuration] Web UI. Deze configuratie ongedaan maken:
   >    
   >    1. Open [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Ga naar `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Het `models` knooppunt verwijderen

   >    
   >Eventuele bestaande modellen van inhoudsfragmenten die in deze configuratie zijn gemaakt, worden verwijderd en de bijbehorende definities worden opgeslagen onder `/conf/wknd-mobile/settings/dam/cfm/models`.

1. Pas de **[!DNL WKND Mobile]** configuratie toe op de **[!DNL WKND Mobile]middelenmap** om toe te staan dat inhoudsfragmenten uit modellen van inhoudsfragmenten worden gemaakt in de maphiërarchie Elementen:

   1. Ga naar **[!UICONTROL AEM]> [!UICONTROL Assets] >[!UICONTROL Files]**
   1. Selecteer de **[!UICONTROL WKND Mobile]map**
   1. Tik op de **[!UICONTROL Properties]** knop in de bovenste actiebalk om deze te openen [!UICONTROL Folder Properties]
   1. Tik in [!UICONTROL Folder Properties]op de **[!UICONTROL Cloud Services]** tab
   1. Controleren of het **[!UICONTROL Cloud Configuration]** veld is ingesteld op **/conf/wknd-mobile**
   1. Tik **[!UICONTROL Save & Close]** rechtsboven om de wijzigingen aan te houden

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

## Inzicht krijgen in het inhoudsfragmentmodel dat moet worden gemaakt

Voordat u het model voor inhoudsfragmenten definieert, moeten we de ervaring die we gebruiken om ervoor te zorgen dat we alle benodigde gegevenspunten vastleggen, onder de loep nemen. Hiervoor bekijken we het ontwerp van mobiele toepassingen en wijzen we de ontwerpelementen toe aan content-to-collection.

De gegevenspunten die een gebeurtenis definiëren, kunnen als volgt worden verdeeld:

![Het model van het inhoudsfragment maken](assets/chapter-2/design-to-model-mapping.png)

Met de toewijzing kunnen we inhoudsfragment definiëren dat wordt gebruikt om de gebeurtenisgegevens te verzamelen en uiteindelijk beschikbaar te maken.

## Het model van het inhoudsfragment maken

1. Ga naar **[!UICONTROL Tools]> [!UICONTROL Assets] >[!UICONTROL Content Fragment Models]**.
1. Tik op de **[!DNL WKND Mobile]** map die u wilt openen.
1. Tik **[!UICONTROL Create]** om de ontwerpwizard Inhoudsfragmentmodel te openen.
1. Voer **[!DNL Event]** de velden in als **[!UICONTROL Model Title]** (beschrijving is optioneel) *en tik op* **[!UICONTROL Create]** om op te slaan.

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## De structuur van het inhoudsfragmentmodel definiëren

1. Ga naar **[!UICONTROL Tools]> [!UICONTROL Assets] > [!UICONTROL Content Fragment Models] >[!DNL WKND]**.
1. Selecteer het **[!DNL Event]** **[!UICONTROL Edit]** inhoudsfragmentmodel en tik in de bovenste actiebalk.
1. Sleep de **[!UICONTROL Data Types]tab** aan de rechterkant naar de linkerdropzone om het **[!UICONTROL Single line text input]** **[!DNL Question]** veld te definiëren.
1. Controleer of het nieuwe bestand links **[!UICONTROL Single line text input]** is geselecteerd en of het **[!UICONTROL Properties]tabblad** rechts is geselecteerd. Vul de velden Eigenschappen als volgt in:

   * [!UICONTROL Render As] : `textfield`
   * [!UICONTROL Field Label] : `Event Title`
   * [!UICONTROL Property Name] : `eventTitle`
   * [!UICONTROL Max Length] : 25
   * [!UICONTROL Required] : `Yes`

Herhaal deze stappen met de invoerdefinities die hieronder zijn gedefinieerd om de rest van het gebeurtenisinhoudsfragmentmodel te maken.

>[!NOTE]
>
> De velden **Eigenschapnaam** MOETEN exact overeenkomen, aangezien de Android-toepassing is geprogrammeerd om deze namen uit te schakelen.

### Gebeurtenisbeschrijving

* [!UICONTROL Data Type] : `Multi-line text`
* [!UICONTROL Field Label] : `Event Description`
* [!UICONTROL Property Name] : `eventDescription`
* [!UICONTROL Default Type] : `Rich text`

### Datum en tijd van gebeurtenis

* [!UICONTROL Data Type] : `Date and time`
* [!UICONTROL Field Label] : `Event Date and Time`
* [!UICONTROL Property Name] : `eventDateAndTime`
* [!UICONTROL Required] : `Yes`

### Type gebeurtenis

* [!UICONTROL Data Type] : `Enumeration`
* [!UICONTROL Field Label] : `Event Type`
* [!UICONTROL Property Name] : `eventType`
* [!UICONTROL Options] : `Art,Music,Performance,Photography`

### Ticketprijs

* [!UICONTROL Data Type] : `Number`
* [!UICONTROL Render As] : `numberfield`
* [!UICONTROL Field Label] : `Ticket Price`
* [!UICONTROL Property Name] : `eventPrice`
* [!UICONTROL Type] : `Integer`
* [!UICONTROL Required] : `Yes`

### Afbeelding van gebeurtenis

* [!UICONTROL Data Type] : `Content Reference`
* [!UICONTROL Render As] : `contentreference`
* [!UICONTROL Field Label] : `Event Image`
* [!UICONTROL Property Name] : `eventImage`
* [!UICONTROL Root Path] : `/content/dam/wknd-mobile/images`
* [!UICONTROL Required] : `Yes`

### Naam van doel

* [!UICONTROL Data Type] : `Single-line text`
* [!UICONTROL Render As] : `textfield`
* [!UICONTROL Field Label] : `Venue Name`
* [!UICONTROL Property Name] : `venueName`
* [!UICONTROL Max Length] : 20
* [!UICONTROL Required] : `Yes`

### Plaats van bestemming

* [!UICONTROL Data Type] : `Enumeration`
* [!UICONTROL Field Label] : `Venue City`
* [!UICONTROL Property Name] : `venueCity`
* [!UICONTROL Options] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335/?quality=12&learn=on)

>[!NOTE]
>
>In de **[!UICONTROL Property Name]** code wordt de naam van de JCR-eigenschap **zowel** aangegeven waar deze waarde wordt opgeslagen als de sleutel in het JSON-bestand. Dit moet een semantische naam zijn die niet wordt gewijzigd tijdens de levensduur van het inhoudsfragmentmodel.

Nadat u het inhoudsfragmentmodel hebt gemaakt, krijgt u een definitie die er als volgt uitziet:


![Fragmentmodel voor gebeurtenisinhoud](assets/chapter-2/event-content-fragment-model.png)

## Volgende stap

U kunt desgewenst het inhoudspakket [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) op AEM Author installeren via [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). Dit pakket bevat de configuraties en inhoud die in dit gedeelte van de zelfstudie worden beschreven.

* [Hoofdstuk 3 - Inhoudsfragmenten voor gebeurtenissen ontwerpen](./chapter-3.md)
