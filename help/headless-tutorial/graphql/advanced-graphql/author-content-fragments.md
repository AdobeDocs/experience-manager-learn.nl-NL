---
title: Inhoudsfragmenten van auteur - Geavanceerde concepten van AEM zonder kop - GraphQL
description: In dit hoofdstuk van Geavanceerde concepten van Adobe Experience Manager (AEM) Headless leert u werken met tabbladen, datum en tijd, JSON-objecten en fragmentverwijzingen in inhoudsfragmenten. Stel mapbeleid in om te beperken welke modellen van inhoudsfragmenten kunnen worden opgenomen.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
source-git-commit: 83e16ea87847182139982ea2378d8ff9f079c968
workflow-type: tm+mt
source-wordcount: '3015'
ht-degree: 0%

---

# Inhoudsfragmenten auteur

In de [vorige hoofdstuk](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md), hebt u vijf modellen van inhoudsfragmenten gemaakt: Persoon, Team, Plaats, Adres, en Info van het Contact. Dit hoofdstuk doorloopt u de stappen om Inhoudsfragmenten tot stand te brengen die op die modellen worden gebaseerd. Ook wordt uitgelegd hoe u mapbeleid kunt maken om te beperken welke modellen van inhoudsfragmenten in de map kunnen worden gebruikt.

## Vereisten {#prerequisites}

Dit document is onderdeel van een zelfstudie met meerdere onderdelen. Controleer of de vorige hoofdstukken zijn voltooid voordat u verdergaat met dit hoofdstuk.

## Doelstellingen {#objectives}

Leer in dit hoofdstuk hoe te:

* Mappen maken en limieten instellen met behulp van mapbeleid
* Fragmentverwijzingen rechtstreeks maken vanuit de Inhoudsfragmenteditor
* Gegevenstypen Tab, Date en JSON-object gebruiken
* Inhoud- en fragmentverwijzingen invoegen in de teksteditor met meerdere regels
* Meerdere fragmentverwijzingen toevoegen
* Inhoudsfragmenten nesten

## Voorbeeldinhoud installeren {#sample-content}

Installeer een AEM pakket dat meerdere mappen en voorbeeldafbeeldingen bevat die worden gebruikt om de zelfstudie te versnellen.

1. Downloaden [Advanced-GraphQL-Tutorial-Starter-Package-1.0.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.0.zip)
1. Navigeer in AEM naar **Gereedschappen** > **Implementatie** > **Pakketten** toegang **Pakketbeheer**.
1. Upload en installeer het pakket (ZIP-bestand) dat u in de vorige stap hebt gedownload.

   ![Pakket geüpload via pakketbeheer](assets/author-content-fragments/install-starter-package.png)

## Mappen maken en limieten instellen met behulp van mapbeleid

Selecteer in de AEM homepage de optie **Activa** > **Bestanden** > **WKND-site** > **Engels**. Hier ziet u de verschillende categorieën van het Fragment van de Inhoud, met inbegrip van avonturen en Medewerkers die in het vorige werden onderzocht [Zelfstudie GraphQL met meerdere stappen](../multi-step/overview.md).

### Mappen maken {#create-folders}

Navigeer in de **avonturen** map. U ziet dat er al mappen voor teams en locaties zijn gemaakt voor het opslaan van de fragmenten voor teams en locaties.

Creeer een omslag voor de Fragmenten van de Inhoud van Instructeurs die op het Model van het Fragment van de Inhoud van de Persoon gebaseerd zijn.

1. Selecteer op de pagina Adventures de optie **Maken** > **Map** in de rechterbovenhoek.

   ![Map maken](assets/author-content-fragments/create-folder.png)

1. Voer in het modaal voor het maken van mappen dat wordt weergegeven &quot;Instructors&quot; in het dialoogvenster **Titel** veld. Noteer de &#39;s&#39; aan het einde. Titels van de mappen die veel fragmenten bevatten, moeten meervoudig zijn. Selecteer **Maken**.

   ![Mapmodaal maken](assets/author-content-fragments/create-folder-modal.png)

   U hebt nu een map gemaakt voor de opslag van Adventure-instructeurs.

### Limieten instellen met behulp van mapbeleid

AEM kunt u machtigingen en beleid voor de mappen met inhoudsfragmenten definiëren. Door toestemmingen te gebruiken, kunt u slechts bepaalde gebruikers (auteurs) of groepen auteurs toegang tot bepaalde omslagen verlenen. Met behulp van mapbeleid kunt u beperken welke modellen van inhoudsfragmenten auteurs in die mappen kunnen gebruiken. In dit voorbeeld, beperken wij een omslag tot de Persoon en de modellen van Info van het Contact. Een mapbeleid configureren:

1. Selecteer **Instructeurs** de map die u hebt gemaakt en selecteer vervolgens **Eigenschappen** in de bovenste navigatiebalk.

   ![Eigenschappen](assets/author-content-fragments/properties.png)

1. Selecteer **Beleid** tab, vervolgens de selectie opheffen **Overgenomen van /content/dam/wknd**. In de **Modellen van inhoudsfragmenten op pad toestaan** Selecteer het mappictogram.

   ![Mappictogram](assets/author-content-fragments/folder-icon.png)

1. Volg het pad in het dialoogvenster Pad selecteren dat wordt geopend **conf** > **WKND-site**. Het model van het Fragmentmodel van de Inhoud van de Persoon, dat in het vorige hoofdstuk wordt gecreeerd, bevat een verwijzing naar het Model van het Fragment van de Fragment van de Inhoud van het Contact van Info. Zowel moeten Person als de modellen van Info van het Contact in de omslag van Instructeurs worden toegestaan om een Fragment van de Inhoud van de Instructeur tot stand te brengen. Selecteren **Persoon** en **Contactinfo** en vervolgens drukt u op **Selecteren** om het dialoogvenster te sluiten.

   ![Pad selecteren](assets/author-content-fragments/select-path.png)

1. Selecteren **Opslaan en sluiten** en selecteert u **OK** in de succesdialoog die wordt weergegeven.

1. U hebt nu een omslagbeleid voor de omslag van Instructeurs gevormd. Navigeer in de **Instructeurs** map en selecteer **Maken** > **Inhoudsfragment**. De enige modellen die u nu kunt selecteren zijn **Persoon** en **Contactinfo**.

   ![Mapbeleid](assets/author-content-fragments/folder-policies.png)

## Inhoudsfragmenten auteur voor instructeurs

Navigeer in de **Instructeurs** map. Van hier, creëren wij een genestelde omslag om de contactinformatie van de Instructeurs op te slaan.

Voer de stappen uit die in de sectie over [mappen maken](#create-folders) om een map met de naam &quot;Contactinfo&quot; te maken. De geneste map neemt het mapbeleid van de bovenliggende map over. Voel vrij om specifieker beleid te vormen zodat laat de pas gecreëerde omslag slechts het model van Info van het Contact toe worden gebruikt.

### Een instructiefragment maken

Laten we vier mensen maken die aan een team van Adventure-instructeurs kunnen worden toegevoegd. De afbeeldingen en namen van de Contribute-inhoudsfragmenten die in de vorige versie zijn gemaakt, opnieuw gebruiken [Zelfstudie GraphQL met meerdere stappen](../multi-step/author-content-fragments.md). In de vorige zelfstudie werd uitgelegd hoe u elementaire inhoudsfragmenten kunt maken, maar deze zelfstudie richt zich op meer geavanceerde functies.

1. Maak in de map Instructors een nieuw inhoudsfragment op basis van het Person Content Fragment-model en geef dit de titel &quot;Jacob Wester&quot;.

   Het nieuwe inhoudsfragment ziet er als volgt uit:

   ![Inhoudsfragment persoon](assets/author-content-fragments/person-content-fragment.png)

1. Voer de volgende inhoud in de velden in:

   * **Volledige naam**: Jacob Wester
   * **Biografie**: Jacob Wester is al tien jaar een wandelende instructeur en heeft er elke minuut van gehouden! Hij is een avontuurzoeker met talent voor het klimmen en terugpakken van rots. Jacob is de winnaar van de beklimmingswedstrijden, waaronder de veetel van de bouldering-wedstrijd. Hij woont momenteel in Californië.
   * **Erviteitsniveau instructeur**: Expert
   * **Vaardigheden**: Rotsklimmen, surfen, rugzakken
   * **Beheerdergegevens**: Jacob Wester coördineert al drie jaar de opbergprojecten.

1. In de **Profielafbeelding** , voegt u een inhoudsverwijzing naar een afbeelding toe. Bladeren naar **WKND-site** > **Engels** > **Medewerkers** > **jacob_wester.jpg** om een pad naar de afbeelding te maken.

### Een nieuwe fragmentverwijzing maken vanuit de Inhoudsfragmenteditor {#fragment-reference-from-editor}

AEM kunt u een fragmentverwijzing rechtstreeks vanuit de editor voor inhoudsfragmenten maken. Laten we een verwijzing naar de contactgegevens van Jacob maken.

1. Selecteren **Nieuw inhoudsfragment** onder de **Contactinfo** veld.

   ![Nieuw inhoudsfragment](assets/author-content-fragments/new-content-fragment.png)

1. De modus Nieuw inhoudsfragment wordt geopend. Volg onder het tabblad Doel selecteren het pad **avonturen** > **Instructeurs** en selecteert u het selectievakje naast de **Contactinfo** map. Selecteren **Volgende** om door te gaan naar het tabblad Eigenschappen.

   ![Nieuw inhoudsfragment, modaal](assets/author-content-fragments/new-content-fragment-modal.png)

1. Voer onder het tabblad Eigenschappen &quot;Contactgegevens van jakob Wester&quot; in het dialoogvenster **Titel** veld. Selecteren **Maken** en vervolgens drukt u op **Openen** in de succesdialoog die wordt weergegeven.

   ![Eigenschappen, tabblad](assets/author-content-fragments/properties-tab.png)

   Er worden nieuwe velden weergegeven waarmee u het inhoudsfragment Contactinfo kunt bewerken.

   ![Inhoudsfragment contactpersoon](assets/author-content-fragments/contact-info-content-fragment.png)

1. Voer de volgende inhoud in de velden in:

   * **Telefoon**: 2009-888-0000
   * **E-mail**: jwester@wknd.com

   Selecteer **Opslaan**. U hebt nu een nieuw fragment met contactinfo gemaakt.

1. Om terug naar het Fragment van de Inhoud van de Instructeur te navigeren, selecteer **Jacob Wester** in de linkerbovenhoek van de editor.

   ![Teruggaan naar origineel inhoudsfragment](assets/author-content-fragments/back-to-jacob-wester.png)

   De **Contactinfo** bevat nu het pad naar het fragment Contactinfo waarnaar wordt verwezen. Dit is een geneste fragmentverwijzing. Het voltooide Fragment van de Inhoud van de Instructeur kijkt als dit:

   ![Jacob Wester-inhoudsfragment](assets/author-content-fragments/jacob-wester-content-fragment.png)

1. Selecteren **Opslaan en sluiten** om het inhoudsfragment op te slaan. U hebt nu een nieuw Fragment van de Inhoud van de Instructeur.

### Aanvullende fragmenten maken

Volg hetzelfde proces als in het dialoogvenster [vorige sectie](#fragment-reference-from-editor) om drie meer Fragments van de Inhoud van Instructeurs en drie Fragments van de Inhoud van Info van het Contact voor deze Instructeurs tot stand te brengen. Voeg de volgende inhoud in de fragmenten van Instructors toe:

**Stacey Roswells**

| Fields | Waarden |
| --- | --- |
| Titel van inhoudsfragment | Stacey Roswells |
| Volledige naam | Stacey Roswells |
| Contactinfo | /content/dam/wknd/nl/adventures/instructors/contact-info/stack-roswells-contact-info |
| Profielafbeelding | /content/dam/wknd/en/contributors/stacey-roswells.jpg |
| Biografie | Stacey Roswells is een volmaakte rockklimmer en alpenavonturer. Geboren in Baltimore, Maryland, Stacey is de jongste van zes kinderen. Haar vader was een luitenant-kolonel in de Amerikaanse marine en haar moeder was een moderne dansinstructeur. Haar familie verhuisde regelmatig met de taken van haar vader en ze nam haar eerste foto&#39;s toen hij in Thailand was gestationeerd. Dit is ook waar Stacey leerde klimmen. |
| Erviteitsniveau instructeur | Geavanceerd |
| Vaardigheden | Rotsklimmen | Skien | Achtergrond |

**Kumar Selvaraj**

| Velden | Waarden |
| --- | --- |
| Titel van inhoudsfragment | Kumar Selvaraj |
| Volledige naam | Kumar Selvaraj |
| Contactinfo | /content/dam/wknd/nl/adventures/instructors/contact-info/kumar-selvaraj-contact-info |
| Profielafbeelding | /content/dam/wknd/en/contributors/Kumar_Selvaraj.JPG |
| Biografie | Kumar Selvaraj is een ervaren AMGA-gecertificeerde professionele instructeur die als belangrijkste doel heeft studenten te helpen hun klimmen- en wandelvaardigheden te verbeteren. |
| Erviteitsniveau instructeur | Geavanceerd |
| Vaardigheden | Rotsklimmen | Achtergrond |

**Ayo Ogunsede**

| Velden | Waarden |
| --- | --- |
| Titel van inhoudsfragment | Ayo Ogunsede |
| Volledige naam | Ayo Ogunsede |
| Contactinfo | /content/dam/wknd/nl/adventures/instructors/contact-info/ayo-ogunsede-contact-info |
| Profielafbeelding | /content/dam/wknd/en/contributors/ayo-ogunseinde-237739.jpg |
| Biografie | Ayo Ogunsede is een professionele klimmer- en achtergrondinstructeur die in Fresno, Centraal-Californië woont. Haar doel is om de fietsers te begeleiden op hun meest epische avonturen in het nationale park. |
| Erviteitsniveau instructeur | Geavanceerd |
| Vaardigheden | Rotsklimmen | Fietsen | Achtergrond |

Laat de **Aanvullende informatie** veld leeg.

Voeg de volgende informatie toe aan de fragmenten Contactinfo:

| Titel van inhoudsfragment | Telefoon | E-mail |
| ------- | -------- | -------- |
| Contactinfo Stacey Roswells | 2009-888-0011 | sroswells@wknd.com |
| Contactinfo Kumar Selvaraj | 2009-888-0002 | kselvaraj@wknd.com |
| Contactinfo Ayo Ogunsede | 2009-888-0304 | aogunseinde@wknd.com |

U bent nu klaar om een Team te creëren!

## Inhoudsfragmenten voor auteur voor locaties

Navigeer in de **Locaties** map. Hier ziet u twee geneste mappen die al zijn gemaakt: Yosemite National Park en Yosemite Valley Lodge.

![Locatiemap](assets/author-content-fragments/locations-folder.png)

Negeer de map Yosemite Valley Lodge voorlopig. Wij zullen aan het later in deze sectie terugkeren wanneer wij een nieuwe plaats creëren die als Basis van het Huis voor ons team van instructeurs zal dienst doen.

Navigeer in de **Nationaal park Yosemite** map. Op dit moment bevat het slechts een foto van het Yosemite National Park. Laten we een nieuw inhoudsfragment maken met het Locatie-inhoudfragmentmodel en dit fragment de titel &quot;Yosemite National Park&quot; geven.

### Plaatsaanduidingen voor tabbladen

AEM kunt u tabplaceholders gebruiken om verschillende types van inhoud te groeperen en uw Fragments van de Inhoud te maken gemakkelijker te lezen en te beheren. In het vorige hoofdstuk hebt u tabplaatsaanduidingen toegevoegd aan het locatiemodel. Het fragment Locatie-inhoud heeft nu twee tabsecties: **Locatiedetails** en **Locatieadres**.

![Plaatsaanduidingen voor tabbladen](assets/author-content-fragments/tabs.png)

De **Locatiedetails** bevat de **Naam**, **Beschrijving**, **Contactinfo**, **Locatieafbeelding**, en **Weer na seizoen** velden, terwijl de **Locatieadres** bevat een verwijzing naar een fragment van de Inhoud van het Adres. Op de tabbladen wordt duidelijk aangegeven in welke typen inhoud de inhoud moet worden ingevuld. Het is dus gemakkelijker om inhoud te schrijven.

### JSON Object, gegevenstype

De **Weer na seizoen** Veld is een gegevenstype van het type JSON-object, wat betekent dat het gegevens accepteert in JSON-indeling. Dit gegevenstype is flexibel en kan worden gebruikt voor alle gegevens die u in de inhoud wilt opnemen.

U kunt de veldbeschrijving zien die in het vorige hoofdstuk is gemaakt door de muisaanwijzer op het informatiepictogram rechts van het veld te plaatsen.

![Info pictogram JSON-object](assets/author-content-fragments/json-object-info.png)

In dit geval moeten we het gemiddelde weer voor de locatie opgeven. Voer de volgende gegevens in:

```json
{
    "summer": "81 / 89°F",
    "fall": "56 / 83°F",
    "winter": "46 / 51°F",
    "spring": "57 / 71°F"
}
```

De **Weer na seizoen** het veld moet er nu als volgt uitzien :

![JSON-object](assets/author-content-fragments/json-object.png)

### Inhoud toevoegen

Voeg de rest van de inhoud aan het Fragment van de Inhoud van de Plaats toe om de informatie met GraphQL in het volgende hoofdstuk te vragen.

1. In de **Locatiedetails** voert u de volgende informatie in de velden in:

   * **Naam**: Nationaal park Yosemite
   * **Beschrijving**: Yosemite National Park bevindt zich in de Sierra Nevada-bergen van Californië. Het is beroemd om zijn prachtige watervallen, reusachtige aardbevingen en iconische weergaven van de kliffen El Capitan en Half Dome. Wandelen en kamperen zijn de beste manieren om Josemite te ervaren. Talloze sporen bieden eindeloze kansen voor avontuur en exploratie.

1. Van de **Contactinfo** , maakt u een nieuw inhoudsfragment op basis van het contactinfo-model en geeft u de titel &quot;Yosemite National Park Contact Info&quot;. Volg dezelfde procedure als in de vorige sectie over [een nieuwe fragmentverwijzing maken vanuit de editor](#fragment-reference-from-editor) en voert u de volgende gegevens in de velden in:

   * **Telefoon**: 2009-999-0000
   * **E-mail**: yosemite@wknd.com

1. Van de **Locatieafbeelding** veld, bladeren naar **avonturen** > **Locaties** > **Nationaal park Yosemite** > **yosemite-nationaal park.jpeg** om een pad naar de afbeelding te maken.

   Herinner dat in het vorige hoofdstuk u de beeldbevestiging vormde, zodat moeten de afmetingen van het beeld van de Plaats minder dan 2560 x 1800 zijn, en zijn dossiergrootte moet minder dan 3 MB zijn.

1. Met alle toegevoegde informatie, **Locatiedetails** tab ziet er nu als volgt uit:

   ![Het tabblad Locatiegegevens is voltooid](assets/author-content-fragments/location-details-tab-completed.png)

1. Navigeer in de **Locatieadres** tab. Van de **Adres** in het veld, maakt u een nieuw Content Fragment met de naam &quot;Yosemite National Park Address&quot; met behulp van het model voor het fragment Adresinhoud dat u in het vorige hoofdstuk hebt gemaakt. Volg hetzelfde proces als in de sectie over [een nieuwe fragmentverwijzing maken vanuit de editor](#fragment-reference-from-editor) en voert u de volgende gegevens in de velden in:

   * **Adres**: 9010 Curry Village Drive
   * **Plaats**: Yosemite Valley
   * **Staat**: CA
   * **Postcode**: 95389
   * **Land**: Verenigde Staten

1. De voltooide **Locatieadres** Het tabblad van het fragment Yosemite National Park ziet er als volgt uit:

   ![Tabblad Locatieadres is voltooid](assets/author-content-fragments/location-address-tab-completed.png)

1. Selecteren **Opslaan en sluiten**.

### Een extra fragment maken

1. Navigeer in de **Yosemite Valley Lodge** map. Maak een nieuw inhoudsfragment met het Locatie-inhoudsfragmentmodel en noem dit &#39;Yosemite Valley Lodge&#39;.

1. In de **Locatiedetails** voert u de volgende informatie in de velden in:

   * **Naam**: Yosemite Valley Lodge
   * **Beschrijving**: Yosemite Valley Lodge is een centrum voor groepsvergaderingen en allerlei activiteiten, zoals winkelen, dineren, vissen, fietsen en nog veel meer.

1. Van de **Contactinfo** een nieuw inhoudsfragment maken op basis van het contactinfo-model en dit fragment de titel &quot;Yosemite Valley Lodge Contact Info&quot; geven. Volg hetzelfde proces als in de sectie over [een nieuwe fragmentverwijzing maken vanuit de editor](#fragment-reference-from-editor) en voer de volgende gegevens in in de velden van het nieuwe inhoudsfragment:

   * **Telefoon**: 992-0000
   * **E-mail**: yosemitelodge@wknd.com

   Sla het nieuwe inhoudsfragment op.

1. Ga terug naar **Yosemite Valley Lodge** en ga naar de **Locatieadres** tab. Van de **Adres** in het veld, maakt u een nieuw Content Fragment met de naam &quot;Yosemite Valley Lodge Address&quot; met behulp van het Document Content Fragment Model dat u in het vorige hoofdstuk hebt gemaakt. Volg hetzelfde proces als in de sectie over [een nieuwe fragmentverwijzing maken vanuit de editor](#fragment-reference-from-editor) en voert u de volgende gegevens in de velden in:

   * **Adres**: 9006 Yosemite Lodge Drive
   * **Plaats**: Nationaal park Yosemite
   * **Staat**: CA
   * **Postcode**: 95389
   * **Land**: Verenigde Staten

   Sla het nieuwe inhoudsfragment op.

1. Ga terug naar **Yosemite Valley Lodge** selecteert u vervolgens **Opslaan en sluiten**. De **Yosemite Valley Lodge** De map bevat nu drie inhoudsfragmenten: Yosemite Valley Lodge, Yosemite Valley Lodge Contact Info en Yosemite Valley Lodge Address.

   ![De map Yosemite Valley Lodge](assets/author-content-fragments/yosemite-valley-lodge-folder.png)

## Auteur a Team Content Fragment

Bladeren naar mappen **Teams** > **Yosemite-team**. U ziet dat de map Yosemite Team momenteel alleen het teamlogo bevat.

![De map Yosemite Team](assets/author-content-fragments/yosemite-team-folder.png)

Laten we een nieuw inhoudsfragment maken met behulp van het model Inhoudsfragment voor team en dit de titel &quot;Yosemite Team&quot; geven.

### Inhoud- en fragmentverwijzingen in de teksteditor met meerdere regels

AEM staat u toe om inhoud en fragmentverwijzingen rechtstreeks in de multi-lijn tekstredacteur toe te voegen en hen later terug te winnen gebruikend vragen GraphQL. Laten we zowel inhoud als fragmentverwijzingen toevoegen aan de **Beschrijving** veld.

1. Voeg eerst de volgende tekst toe aan de **Beschrijving** veld: &quot;Het team van professionele avonturiers en wandelende instructeurs die in Yosemite National Park werken.&quot;

1. Als u een inhoudsverwijzing wilt toevoegen, selecteert u de optie **Element invoegen** in de werkbalk van de teksteditor met meerdere regels.

   ![Elementpictogram invoegen](assets/author-content-fragments/insert-asset-icon.png)

1. Selecteer in het modaal dat wordt weergegeven **team-yosemite-logo.png** en drukken **Selecteren**.

   ![Afbeelding selecteren](assets/author-content-fragments/select-image.png)

   De inhoudsverwijzing wordt nu toegevoegd aan de **Beschrijving** veld.

Vergeet niet dat u in het vorige hoofdstuk hebt toegestaan dat fragmentverwijzingen werden toegevoegd aan het dialoogvenster **Beschrijving** veld. Laten we er een toevoegen.

1. Selecteer **Inhoudsfragment invoegen** in de werkbalk van de teksteditor met meerdere regels.

   ![Pictogram Inhoudsfragment invoegen](assets/author-content-fragments/insert-content-fragment-icon.png)

1. Bladeren naar **WKND-site** > **Engels** > **avonturen** > **Locaties** > **Yosemite Valley Lodge** > **Yosemite Valley Lodge**. Druk **Selecteren** om het inhoudsfragment in te voegen.

   ![Inhoudsfragment modaal invoegen](assets/author-content-fragments/insert-content-fragment-modal.png)

   De **Beschrijving** Het veld ziet er nu als volgt uit:

   ![Beschrijving, veld](assets/author-content-fragments/description-field.png)

U hebt nu de inhoud en fragmentverwijzingen rechtstreeks toegevoegd aan de teksteditor met meerdere regels.

### Gegevenstype datum en tijd

Laten we eens kijken naar het gegevenstype Datum en tijd. Selecteer **Kalender** pictogram aan de rechterkant van het **Begindatum team** om de kalenderweergave te openen.

![Datumkalenderweergave](assets/author-content-fragments/date-calendar-view.png)

De datums in het verleden of in de toekomst kunnen worden ingesteld met de pijlen naar voren en naar achteren aan weerszijden van de maand. Laten we zeggen dat het team Yosemite op 24 mei 2016 is opgericht, dus we zullen de datum daarvoor vaststellen.

### Meerdere fragmentverwijzingen toevoegen

Laten wij Instructeurs aan de het fragmentverwijzing van de Leden van het Team toevoegen.

1. Selecteren **Toevoegen** in de **Teamleden** veld.

   ![Knop Toevoegen](assets/author-content-fragments/add-button.png)

1. Selecteer in het nieuwe veld dat wordt weergegeven het mappictogram om het modaal pad selecteren te openen. Bladeren door mappen naar **WKND-site** > **Engels** > **avonturen** > **Instructeurs** Schakel vervolgens het selectievakje naast **johannesbrood**. Druk **Selecteren** om het pad op te slaan.

   ![Fragmentverwijzingspad](assets/author-content-fragments/fragment-reference-path.png)

1. Selecteer **Toevoegen** nog driemaal. Gebruik de nieuwe gebieden om de resterende drie Instructeurs aan het team toe te voegen. De **Teamleden** het veld ziet er nu als volgt uit :

   ![Veld teamleden](assets/author-content-fragments/team-members-field.png)

1. Selecteren **Opslaan en sluiten** om het Fragment van de Inhoud van het Team te bewaren.

### Fragmentverwijzingen toevoegen aan een Adventure Content-fragment

Tot slot voegen onze pas gecreëerde Fragments van de Inhoud aan een Avontuur toe.

1. Navigeren naar **avonturen** > **Yosemite-achtergrondverpakking** en opent u het Yosemite Backpackaging Content Fragment. Onder aan het formulier ziet u de drie velden die u in het vorige hoofdstuk hebt gemaakt: **Locatie**, **Instructieteam**, en **Beheerder**.

1. De fragmentverwijzing toevoegen in het dialoogvenster **Locatie** veld. Het Locatiepad verwijst naar het Yosemite National Park Content Fragment dat u hebt gemaakt: `/content/dam/wknd/en/adventures/locations/yosemite-national-park/yosemite-national-park`.

1. De fragmentverwijzing toevoegen in het dialoogvenster **Instructieteam** veld. De weg van het Team zou het Fragment van de Inhoud van het Team van Yosemite moeten van verwijzingen voorzien dat u creeerde: `/content/dam/wknd/en/adventures/teams/yosemite-team/yosemite-team`. Dit is een geneste fragmentverwijzing. Het fragment van de Inhoud van het Team bevat een verwijzing naar het model van de Persoon dat verwijzingenInfo en de modellen van het Adres van het Contact. Daarom hebt u geneste inhoudsfragmenten drie niveaus omlaag.

1. De fragmentverwijzing toevoegen in het dialoogvenster **Beheerder** veld. Laten we zeggen dat Jacob Wester een beheerder is voor het Yosemite Backpackaging Adventure. Het pad moet leiden naar het Jacob Wester-inhoudsfragment en er als volgt uitzien: `/content/dam/wknd/en/adventures/instructors/jacob-wester`.

1. U hebt nu drie fragmentverwijzingen toegevoegd aan een Adventure Content-fragment. De velden zien er als volgt uit:

   ![Adventure-fragmentverwijzingen](assets/author-content-fragments/adventure-fragment-references.png)

1. Selecteren **Opslaan en sluiten** om uw inhoud op te slaan.

## Gefeliciteerd!

Gefeliciteerd! U hebt nu inhoudsfragmenten gemaakt op basis van de geavanceerde modellen van inhoudsfragmenten die in het vorige hoofdstuk zijn gemaakt. U hebt ook een mapbeleid gemaakt om te beperken welke modellen van inhoudsfragmenten in een map kunnen worden geselecteerd.

## Volgende stappen

In de [volgende hoofdstuk](/help/headless-tutorial/graphql/advanced-graphql/explore-graphql-api.md), zult u leren over het verzenden van geavanceerde vragen GraphQL gebruikend GraphiQL Geïntegreerde Ontwikkelomgeving (winde). Met deze query&#39;s kunnen we de gegevens bekijken die in dit hoofdstuk zijn gemaakt en deze query&#39;s uiteindelijk toevoegen aan de WKND-app.

Hoewel dit optioneel is voor deze zelfstudie, dient u alle inhoud te publiceren in situaties waarin de inhoud in de praktijk wordt geproduceerd. Meer informatie over de auteur- en publicatie-omgevingen vindt u in de [videoreeks zonder hoofd](/help/headless-tutorial/graphql/video-series/author-publish-architecture.md)