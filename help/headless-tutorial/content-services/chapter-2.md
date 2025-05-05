---
title: Hoofdstuk 2 - Fragmentmodellen voor gebeurtenisinhoud definiëren - Inhoudsservices
description: Hoofdstuk 2 van de AEM zelfstudie zonder koppen omvat het inschakelen en definiëren van modellen van inhoudsfragmenten die worden gebruikt om een genormaliseerde gegevensstructuur en ontwerpinterface te definiëren voor het maken van gebeurtenissen.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
duration: 378
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
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

De Modellen van het Fragment van de inhoud **moeten** worden toegelaten via **[AEM [!UICONTROL Configuration Browser] ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)**.

Als de Modellen van het Fragment van de Inhoud **&#x200B;**&#x200B;niet &lbrace;voor een configuratie worden toegelaten, **[!UICONTROL Create]>[!UICONTROL Content Fragment]** knoop zal niet voor de relevante AEM configuratie verschijnen.

>[!NOTE]
>
>AEM configuraties vertegenwoordigen een reeks [ context-bewuste huurdersconfiguraties ](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) die onder `/conf` worden opgeslagen. AEM configuraties correleren doorgaans met een bepaalde website die wordt beheerd in AEM Sites of een bedrijfseenheid die verantwoordelijk is voor een subset met inhoud (elementen, pagina&#39;s, enz.) in AEM.
>
>Een configuratie heeft alleen invloed op een inhoudshiërarchie als er naar de configuratie wordt verwezen via de eigenschap `cq:conf` in die inhoudshiërarchie. (Dit wordt bereikt voor de [!DNL WKND Mobile] configuratie in **Stap 5** hieronder).
>
>Wanneer de `global` -configuratie wordt gebruikt, geldt de configuratie voor alle inhoud en hoeft `cq:conf` niet te worden ingesteld.
>
>Zie de [[!UICONTROL Configuration Browser] documentatie ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html) voor meer informatie.

1. Meld u aan bij AEM auteur als een gebruiker met de juiste machtigingen om de relevante configuratie te wijzigen.
   * Voor dit leerprogramma, kan de **admin** gebruiker worden gebruikt.
1. Ga naar **[!UICONTROL Tool]> [!UICONTROL General] >[!UICONTROL Configuration Browser]**
1. Tik het **omslagpictogram** naast **[!DNL WKND Mobile]** om te selecteren, en dan de **[!UICONTROL Edit]knoop** in de hoogste linkerzijde te tikken.
1. Selecteer **[!UICONTROL Content Fragment Models]** en tik op **[!UICONTROL Save & Close]** rechtsboven.

   Hiermee schakelt u modellen van inhoudsfragmenten op de inhoudstructuren van de map Asset in waarop de [!DNL WKND Mobile] -configuratie is toegepast.

   >[!NOTE]
   >
   >Deze configuratiewijziging is niet omkeerbaar vanuit de [!UICONTROL AEM Configuration] webinterface. Deze configuratie ongedaan maken:
   >    
   >    1. Open [ CRXDE Lite ](http://localhost:4502/crx/de)
   >    1. Navigeren naar `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Het knooppunt `models` verwijderen
   >    
   >Alle bestaande modellen van inhoudsfragmenten die in deze configuratie zijn gemaakt, worden verwijderd en de definities ervan worden opgeslagen onder `/conf/wknd-mobile/settings/dam/cfm/models` .

1. Pas de **[!DNL WKND Mobile]** configuratie op de **[!DNL WKND Mobile]Omslag van Assets** toe om Inhoudsfragmenten van de Modellen van het Fragment van de Inhoud toe te staan om binnen die de omslaghiërarchie van Assets worden gecreeerd:

   1. Ga naar **[!UICONTROL AEM]> [!UICONTROL Assets] >[!UICONTROL Files]**
   1. Selecteer de **[!UICONTROL WKND Mobile]map**
   1. Tik op de knop **[!UICONTROL Properties]** in de bovenste actiebalk om het venster te openen [!UICONTROL Folder Properties]
   1. Tik in [!UICONTROL Folder Properties] op de tab **[!UICONTROL Cloud Services]**
   1. Controleer of het veld **[!UICONTROL Cloud Configuration]** is ingesteld op **/conf/wknd-mobile**
   1. Tik **[!UICONTROL Save & Close]** rechtsboven om de wijzigingen te behouden

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> __Modellen van het Fragment van de Inhoud__ zijn bewogen van __Hulpmiddelen > Assets__ aan __Hulpmiddelen > Algemeen__.

## Inzicht krijgen in het inhoudsfragmentmodel dat moet worden gemaakt

Voordat u ons model voor inhoudsfragmenten definieert, moet u de ervaring die we gaan gebruiken, bekijken om ervoor te zorgen dat we alle vereiste gegevenspunten vastleggen. Hiervoor bekijken we het ontwerp van mobiele toepassingen en wijzen we de ontwerpelementen toe aan content-to-collection.

De gegevenspunten die een gebeurtenis definiëren, kunnen als volgt worden verdeeld:

![ Creërend het Model van het Fragment van de Inhoud ](assets/chapter-2/design-to-model-mapping.png)

Met de toewijzing kunnen we inhoudsfragment definiëren dat wordt gebruikt om de gebeurtenisgegevens te verzamelen en uiteindelijk beschikbaar te maken.

## Het model van het inhoudsfragment maken

1. Navigeer naar **[!UICONTROL Tools]> [!UICONTROL General] >[!UICONTROL Content Fragment Models]** .
1. Tik op de map **[!DNL WKND Mobile]** die u wilt openen.
1. Tik op **[!UICONTROL Create]** om de wizard voor het maken van een inhoudsfragmentmodel te openen.
1. Voer **[!DNL Event]** in als de **[!UICONTROL Model Title]** *(beschrijving is optioneel)* en tik **[!UICONTROL Create]** om op te slaan.

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## De structuur van het inhoudsfragmentmodel definiëren

1. Navigeer naar **[!UICONTROL Tools]> [!UICONTROL General] > [!UICONTROL Content Fragment Models] >[!DNL WKND]** .
1. Selecteer het **[!DNL Event]** Inhoudsfragmentmodel en tik **[!UICONTROL Edit]** op de bovenste actiebalk.
1. Sleep vanuit de **[!UICONTROL Data Types]tab** aan de rechterkant de **[!UICONTROL Single line text input]** naar de linkerdropzone om het **[!DNL Question]** -veld te definiëren.
1. Zorg ervoor dat de nieuwe **[!UICONTROL Single line text input]** aan de linkerkant is geselecteerd en dat de **[!UICONTROL Properties]tab** aan de rechterkant is geselecteerd. Vul de velden Eigenschappen als volgt in:

   * [!UICONTROL Render As] : `textfield`
   * [!UICONTROL Field Label] : `Event Title`
   * [!UICONTROL Property Name] : `eventTitle`
   * [!UICONTROL Max Length] : 25
   * [!UICONTROL Required] : `Yes`

Herhaal deze stappen met behulp van de invoerdefinities die hieronder zijn gedefinieerd om de rest van het gebeurtenisinhoudsfragmentmodel te maken.

>[!NOTE]
>
> De **gebieden van de Naam van het Bezit** MOETEN precies aanpassen, aangezien de toepassing van Android aan sleutel van deze namen wordt geprogrammeerd.

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
>**[!UICONTROL Property Name]** wijst **zowel** aan JCR bezitsnaam waar deze waarde evenals sleutel in het JSON- dossier wordt opgeslagen. Dit moet een semantische naam zijn die niet wordt gewijzigd tijdens de levensduur van het inhoudsfragmentmodel.

Nadat u het inhoudsfragmentmodel hebt gemaakt, krijgt u een definitie die er als volgt uitziet:


![ Model van het Fragment van de Inhoud van de Gebeurtenis ](assets/chapter-2/event-content-fragment-model.png)

## Volgende stap

Naar keuze, installeer [ com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip ](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) inhoudspakket op AEM Auteur via [ AEM de Manager van het Pakket ](http://localhost:4502/crx/packmgr/index.jsp). Dit pakket bevat de configuraties en inhoud die in dit gedeelte van de zelfstudie worden beschreven.

* [Hoofdstuk 3 - Inhoudsfragmenten voor gebeurtenissen ontwerpen](./chapter-3.md)
