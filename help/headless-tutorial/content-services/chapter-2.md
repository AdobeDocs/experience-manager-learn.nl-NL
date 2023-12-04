---
title: Hoofdstuk 2 - Fragmentmodellen voor gebeurtenisinhoud definiëren - Inhoudsservices
description: Hoofdstuk 2 van de AEM zelfstudie zonder koppen omvat het inschakelen en definiëren van modellen van inhoudsfragmenten die worden gebruikt om een genormaliseerde gegevensstructuur en ontwerpinterface te definiëren voor het maken van gebeurtenissen.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
duration: 472
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '807'
ht-degree: 0%

---

# Hoofdstuk 2 - Modellen voor inhoudsfragmenten gebruiken

AEM Content Fragment Models definieert inhoudsschema&#39;s die kunnen worden gebruikt om het maken van onbewerkte inhoud door AEM auteurs te optimaliseren. Deze aanpak lijkt op steigers of op formulieren gebaseerde ontwerpen. Het belangrijkste concept bij Content Fragments is dat de geschreven inhoud presentatie-agnostisch is, wat betekent zijn voorgenomen voor multi-kanaals gebruik waar de het verbruiken toepassing, namelijk AEM, één enkele paginatoepassing, of een Mobiele app, controleert hoe de inhoud aan de gebruiker wordt getoond.

Het belangrijkste doel van het inhoudsfragment is:

1. De juiste inhoud wordt verzameld van de auteur
2. De inhoud kan in een gestructureerde, goed begrepen formaat aan het verbruiken van toepassingen worden blootgesteld.

In dit hoofdstuk wordt beschreven hoe u modellen van inhoudsfragmenten kunt inschakelen en definiëren voor het definiëren van een genormaliseerde gegevensstructuur en ontwerpinterface voor het modelleren en maken van &quot;Gebeurtenissen&quot;.

## Modellen van inhoudsfragmenten inschakelen

Modellen van inhoudsfragmenten **moet** zijn ingeschakeld via **[AEM [!UICONTROL Configuration Browser]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)**.

Als modellen van inhoudsfragmenten bestaan **niet** die voor een configuratie worden toegelaten, **[!UICONTROL Create]>[!UICONTROL Content Fragment]** De knop wordt niet weergegeven voor de betreffende AEM.

>[!NOTE]
>
>AEM configuraties vertegenwoordigen een set [context-bewuste huurdersconfiguraties](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) opgeslagen onder `/conf`. AEM configuraties correleren doorgaans met een bepaalde website die wordt beheerd in AEM Sites of een bedrijfseenheid die verantwoordelijk is voor een subset met inhoud (elementen, pagina&#39;s, enz.) in AEM.
>
>Als een configuratie een inhoudshiërarchie kan beïnvloeden, moet de configuratie via `cq:conf` eigenschap in die inhoudshiërarchie. (Dit wordt bereikt voor de [!DNL WKND Mobile] configuratie in **Stap 5** hieronder).
>
>Wanneer de `global` de configuratie wordt gebruikt, is de configuratie op alle inhoud van toepassing, en `cq:conf` hoeft niet te worden ingesteld.
>
>Zie de [[!UICONTROL Configuration Browser] documentatie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html) voor meer informatie .

1. Meld u aan bij AEM auteur als een gebruiker met de juiste machtigingen om de relevante configuratie te wijzigen.
   * Voor deze zelfstudie **admin** kan worden gebruikt.
1. Navigeren naar **[!UICONTROL Tool]> [!UICONTROL General] >[!UICONTROL Configuration Browser]**
1. Tik op de knop **mappictogram** naast **[!DNL WKND Mobile]** om te selecteren, en dan te tikken **[!UICONTROL Edit]knop** linksboven.
1. Selecteren **[!UICONTROL Content Fragment Models]** en tikken **[!UICONTROL Save & Close]** rechtsboven.

   Dit maakt modellen van inhoudsfragmenten op inhoudstructuren van de elementenmap mogelijk die de [!DNL WKND Mobile] toegepaste configuratie.

   >[!NOTE]
   >
   >Deze configuratieverandering is niet omkeerbaar van [!UICONTROL AEM Configuration] Webinterface. Deze configuratie ongedaan maken:
   >    
   >    1. Openen [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Navigeren naar `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Verwijder de `models` node
   >    
   >Alle bestaande modellen van inhoudsfragmenten die in deze configuratie zijn gemaakt, worden verwijderd en de definities ervan worden opgeslagen onder `/conf/wknd-mobile/settings/dam/cfm/models`.

1. Pas de **[!DNL WKND Mobile]** aan de **[!DNL WKND Mobile]Map Middelen** om toe te staan dat Content Fragments van Content Fragment Models binnen die de omslaghiërarchie van Elementen worden gecreeerd:

   1. Navigeren naar **[!UICONTROL AEM]> [!UICONTROL Assets] >[!UICONTROL Files]**
   1. Selecteer de **[!UICONTROL WKND Mobile]map**
   1. Tik op de knop **[!UICONTROL Properties]** in de bovenste actiebalk om te openen [!UICONTROL Folder Properties]
   1. In [!UICONTROL Folder Properties]tikt u op **[!UICONTROL Cloud Services]** tab
   1. Controleer de **[!UICONTROL Cloud Configuration]** veld is ingesteld op **/conf/wknd-mobile**
   1. Tikken **[!UICONTROL Save & Close]** in de rechterbovenhoek om wijzigingen aan te houden

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> __Modellen van inhoudsfragmenten__ zijn verplaatst van __Gereedschappen > Elementen__ tot __Gereedschappen > Algemeen__.

## Inzicht krijgen in het inhoudsfragmentmodel dat moet worden gemaakt

Voordat u ons model voor inhoudsfragmenten definieert, moet u de ervaring die we gaan gebruiken, bekijken om ervoor te zorgen dat we alle vereiste gegevenspunten vastleggen. Hiervoor bekijken we het ontwerp van mobiele toepassingen en wijzen we de ontwerpelementen toe aan content-to-collection.

De gegevenspunten die een gebeurtenis definiëren, kunnen als volgt worden verdeeld:

![Het model van het inhoudsfragment maken](assets/chapter-2/design-to-model-mapping.png)

Met de toewijzing kunnen we inhoudsfragment definiëren dat wordt gebruikt om de gebeurtenisgegevens te verzamelen en uiteindelijk beschikbaar te maken.

## Het model van het inhoudsfragment maken

1. Navigeren naar **[!UICONTROL Tools]> [!UICONTROL General] >[!UICONTROL Content Fragment Models]**.
1. Tik op de knop **[!DNL WKND Mobile]** te openen map.
1. Tikken **[!UICONTROL Create]** om de wizard Inhoudsfragmentmodel te openen.
1. Enter **[!DNL Event]** als de **[!UICONTROL Model Title]** *(beschrijving is optioneel)* en tikken **[!UICONTROL Create]** opslaan.

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## De structuur van het inhoudsfragmentmodel definiëren

1. Navigeren naar **[!UICONTROL Tools]> [!UICONTROL General] > [!UICONTROL Content Fragment Models] >[!DNL WKND]**.
1. Selecteer de **[!DNL Event]** Inhoudsfragmentmodel en tik **[!UICONTROL Edit]** in de bovenste actiebalk.
1. Van de **[!UICONTROL Data Types]tab** Sleep aan de rechterkant de **[!UICONTROL Single line text input]** in de linkerdropzone om de **[!DNL Question]** veld.
1. De nieuwe **[!UICONTROL Single line text input]** wordt links geselecteerd en **[!UICONTROL Properties]tab** is rechts geselecteerd. Vul de velden Eigenschappen als volgt in:

   * [!UICONTROL Render As] : `textfield`
   * [!UICONTROL Field Label] : `Event Title`
   * [!UICONTROL Property Name] : `eventTitle`
   * [!UICONTROL Max Length] : 25
   * [!UICONTROL Required] : `Yes`

Herhaal deze stappen met behulp van de invoerdefinities die hieronder zijn gedefinieerd om de rest van het gebeurtenisinhoudsfragmentmodel te maken.

>[!NOTE]
>
> De **Eigenschapnaam** velden MOETEN exact overeenkomen, aangezien de Android-toepassing is geprogrammeerd om deze namen uit te schakelen.

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

>[!VIDEO](https://video.tv.adobe.com/v/28335?quality=12&learn=on)

>[!NOTE]
>
>De **[!UICONTROL Property Name]** geeft de **beide** de naam van de JCR-eigenschap waar deze waarde wordt opgeslagen en de sleutel in het JSON-bestand. Dit moet een semantische naam zijn die niet wordt gewijzigd tijdens de levensduur van het inhoudsfragmentmodel.

Nadat u het inhoudsfragmentmodel hebt gemaakt, krijgt u een definitie die er als volgt uitziet:


![Fragmentmodel voor gebeurtenisinhoud](assets/chapter-2/event-content-fragment-model.png)

## Volgende stap

Installeer desgewenst de [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) inhoudspakket op AEM auteur via [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). Dit pakket bevat de configuraties en inhoud die in dit gedeelte van de zelfstudie worden beschreven.

* [Hoofdstuk 3 - Inhoudsfragmenten voor gebeurtenissen ontwerpen](./chapter-3.md)
