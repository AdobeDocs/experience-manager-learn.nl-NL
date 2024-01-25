---
title: Modellen voor inhoudsfragmenten maken - Geavanceerde concepten van AEM headless - GraphQL
description: In dit hoofdstuk van Geavanceerde concepten van Adobe Experience Manager (AEM) Headless leert u hoe u een Content Fragment Model kunt bewerken door tabplaatsaanduidingen, datum en tijd, JSON-objecten, fragmentverwijzingen en inhoudsverwijzingen toe te voegen.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 2122ab13-f9df-4f36-9c7e-8980033c3b10
duration: 829
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1991'
ht-degree: 0%

---

# Modellen voor inhoudsfragmenten maken {#create-content-fragment-models}

Dit hoofdstuk doorloopt de stappen om vijf Modellen van het Fragment van de Inhoud tot stand te brengen:

* **Contactinfo**
* **Adres**
* **Persoon**
* **Locatie**
* **Team**

Met contentfragmentmodellen kunt u relaties tussen inhoudstypen definiëren en relaties als schema&#39;s voortzetten. Gebruik geneste fragmentverwijzingen, verschillende inhoudsgegevenstypen en het tabtype voor visuele inhoudsorganisatie. Geavanceerde gegevenstypen, zoals tabplaatsaanduidingen, fragmentverwijzingen, JSON-objecten en het datum- en tijdgegevenstype.

In dit hoofdstuk wordt ook beschreven hoe u de validatieregels voor inhoudsverwijzingen, zoals afbeeldingen, kunt verbeteren.

## Vereisten {#prerequisites}

Dit is een geavanceerde zelfstudie. Voordat u verdergaat met dit hoofdstuk, moet u ervoor zorgen dat u het [snelle installatie](../quick-setup/cloud-service.md). Zorg ervoor dat u ook de vorige [overzicht](../overview.md) hoofdstuk voor meer informatie over de opstelling voor het geavanceerde leerprogramma.

## Doelstellingen {#objectives}

* Modellen voor inhoudsfragmenten maken.
* Voeg tabplaatsaanduidingen, datum en tijd, JSON-objecten, fragmentverwijzingen en inhoudsverwijzingen toe aan de modellen.
* Validatie toevoegen aan inhoudsverwijzingen.

## Overzicht van het inhoudsfragmentmodel {#content-fragment-model-overview}

De volgende video biedt een korte inleiding tot Modellen van inhoudsfragmenten en hoe deze in deze zelfstudie worden gebruikt.

>[!VIDEO](https://video.tv.adobe.com/v/340037?quality=12&learn=on)

## Modellen voor inhoudsfragmenten maken {#create-models}

Laten we een aantal modellen van inhoudsfragmenten maken voor de WKND-app. Als u een basisinleiding nodig hebt voor het maken van Content Fragment Models, raadpleegt u het desbetreffende hoofdstuk in het dialoogvenster [basiszelfstudie](../multi-step/content-fragment-models.md).

1. Navigeren naar **Gereedschappen** > **Algemeen** > **Modellen van inhoudsfragmenten**.

   ![Pad voor modellen van inhoudsfragmenten](assets/define-content-fragment-models/content-fragment-models-path.png)

1. Selecteren **WKND gedeeld** Hiermee geeft u een lijst weer met bestaande modellen inhoudsfragmenten voor de site.

### Contactinfo-model {#contact-info-model}

Maak vervolgens een model dat de contactgegevens van een persoon of locatie bevat.

1. Selecteren **Maken** in de rechterbovenhoek.

1. Geef het model een titel &quot;Contactinfo&quot; en selecteer vervolgens **Maken**. Selecteer in het succesmodaal dat wordt weergegeven **Openen** om het nieuwe model te bewerken.

1. Begin door een **Tekst met één regel** op het model. Geef het een **Veldlabel** van &quot;Telefoon&quot; in de **Eigenschappen** tab. De eigenschapsnaam wordt automatisch ingevuld als `phone`. Schakel het selectievakje in om het veld te maken **Vereist**.

1. Ga naar de **Gegevenstypen** tab, en vervolgens een andere toevoegen **Tekst met één regel** onder het veld Telefoon. Geef het een **Veldlabel** van &quot;Email&quot; en ook instellen op **Vereist**.

Adobe Experience Manager wordt geleverd met ingebouwde validatiemethoden. Met deze validatiemethoden kunt u beheerregels toevoegen aan specifieke velden in uw modellen van inhoudsfragmenten. In dit geval voegen we een validatieregel toe om ervoor te zorgen dat gebruikers alleen geldige e-mailadressen kunnen invoeren wanneer ze dit veld invullen. Onder de **Validatietype** vervolgkeuzelijst, selecteren **E-mail**.

Het voltooide fragmentmodel voor inhoud moet er als volgt uitzien:

![Pad naar contactgegevensmodel](assets/define-content-fragment-models/contact-info-model.png)

Selecteer **Opslaan** om uw wijzigingen te bevestigen en de Content Fragment Model Editor te sluiten.

### Adresmodel {#address-model}

Maak vervolgens een model voor een adres.

1. Van de **WKND gedeeld**, selecteert u **Maken** in de rechterbovenhoek.

1. Voer een titel van &quot;Adres&quot; in en selecteer vervolgens **Maken**. Selecteer in het succesmodaal dat wordt weergegeven **Openen** om het nieuwe model te bewerken.

1. Sleep een **Tekst met één regel** op het model en geef het een **Veldlabel** van &quot;Straatadres&quot;. De eigenschapsnaam wordt vervolgens ingevuld als `streetAddress`. Selecteer de **Vereist** selectievakje.

1. Herhaal bovenstaande stappen en voeg nog vier velden voor tekst uit één regel toe aan het model. Gebruik de volgende labels:

   * Plaats
   * Staat
   * Postcode
   * Land

1. Selecteren **Opslaan** om de veranderingen in het model van het Adres te bewaren.

   Het voltooide fragmentmodel &quot;Adres&quot; moet er als volgt uitzien:
   ![Adresmodel](assets/define-content-fragment-models/address-model.png)

### Persmodel {#person-model}

Maak vervolgens een model dat informatie over een persoon bevat.

1. Selecteer in de rechterbovenhoek de optie **Maken**.

1. Geef het model een titel &quot;Persoon&quot; en selecteer vervolgens **Maken**. Selecteer in het succesmodaal dat wordt weergegeven **Openen** om het nieuwe model te bewerken.

1. Begin door een **Tekst met één regel** op het model. Geef het een **Veldlabel** van &quot;Volledige naam&quot;. De eigenschapsnaam wordt automatisch ingevuld als `fullName`. Schakel het selectievakje in om het veld te maken **Vereist**.

   ![Volledige-naamopties](assets/define-content-fragment-models/full-name.png)

1. In andere modellen kunt u verwijzen naar modellen van inhoudsfragmenten. Ga naar de **Gegevenstypen** en vervolgens de **Fragmentverwijzing** en geef deze het label &quot;Contactinfo&quot;.

1. In de **Eigenschappen** onder de **Modellen voor toegestane inhoudsfragmenten** veld, selecteert u het mappictogram en kiest u vervolgens de optie **Contactinfo** eerder gemaakt fragmentmodel.

1. Voeg een **Content Reference** veld en geef het een **Veldlabel** van &quot;Profielbeeld&quot;. Selecteer het mappictogram onder **Hoofdpad** om de padselectie te openen. Selecteer een hoofdpad door **content** > **Activa** en vervolgens het selectievakje voor **WKND gedeeld**. Gebruik de **Selecteren** klikt u rechtsboven om het pad op te slaan. Het uiteindelijke tekstpad moet worden gelezen `/content/dam/wknd-shared`.

   ![Hoofdpad van inhoudsverwijzing](assets/define-content-fragment-models/content-reference-root-path.png)

1. Onder **Alleen opgegeven inhoudstypen accepteren** selecteert u Afbeelding.

   ![Profielbeeldopties](assets/define-content-fragment-models/profile-picture.png)

1. Als u de grootte en afmetingen van afbeeldingsbestanden wilt beperken, bekijkt u enkele validatieopties voor het veld met inhoudsverwijzing.

   Onder **Alleen opgegeven bestandsgrootte accepteren**, selecteert u &quot;Kleiner dan of gelijk aan&quot; en worden hieronder extra velden weergegeven.
   ![Alleen opgegeven bestandsgrootte accepteren](assets/define-content-fragment-models/accept-specified-file-size.png)

1. Voor **Max**, typt u &quot;5&quot; en voor **Eenheid selecteren**, selecteert u &quot;Megabytes (MB)&quot;. Bij deze validatie kunnen alleen afbeeldingen van de opgegeven grootte worden gekozen.

1. Onder **Alleen opgegeven afbeeldingsbreedte accepteren**, selecteert u Maximumbreedte. In de **Max (pixels)** Voer &quot;10000&quot; in in het veld dat wordt weergegeven. Selecteer dezelfde opties voor **Alleen een opgegeven afbeeldingshoogte accepteren**.

   Deze validaties zorgen ervoor dat toegevoegde afbeeldingen de opgegeven waarden niet overschrijden. De validatieregels moeten er nu als volgt uitzien:

   ![Validatieregels voor verwijzingen naar inhoud](assets/define-content-fragment-models/content-reference-validation.png)

1. Voeg een **Tekst met meerdere regels** veld en geef het een **Veldlabel** van &quot;Biografie&quot;. Laat de **Standaardtype** vervolgkeuzelijst als de standaardoptie RTF.

   ![Opties voor biografie](assets/define-content-fragment-models/biography.png)

1. Ga naar de **Gegevenstypen** en sleep vervolgens een **Opsomming** veld onder &quot;Biografie&quot;. In plaats van de standaardinstelling **Renderen als** selecteert u **Vervolgkeuzelijst** en een **Veldlabel** van &quot;Erviteitsniveau instructeur&quot;. Ga een selectie van de opties van het de ervaringsniveau van de instructeur zoals in _Expert, geavanceerd, tussentijds_.

1. Vervolgens sleept u een andere **Opsomming** onder &quot;Niveau van de Ervaring van de Instructeur&quot; en kies &quot;checkboxes&quot;onder **Renderen als** -optie. Geef het een **Veldlabel** van &quot;Skills&quot;. Voer verschillende vaardigheden in, zoals het klimmen van rots, surfen, fietsen, skiën en backpackaging. Het label en de waarde van de optie moeten overeenkomen met de onderstaande waarden:

   ![Opsomming vaardigheden](assets/define-content-fragment-models/skills-enum.png)

1. Ten slotte maakt u een veldlabel &quot;Beheerder details&quot; met een **Tekst met meerdere regels** veld.

Selecteren **Opslaan** om uw wijzigingen te bevestigen en de Content Fragment Model Editor te sluiten.

### Locatiemodel {#location-model}

In het volgende inhoudsfragmentmodel wordt een fysieke locatie beschreven. Dit model gebruikt tabplaatsaanduidingen. Met tabplaatsaanduidingen kunt u de gegevenstypen in respectievelijk de modeleditor en de inhoud in de fragmenteditor ordenen door de inhoud te categoriseren. Elke plaatsaanduiding maakt een tabblad in de Content Fragment-editor, vergelijkbaar met een tabblad in een internetbrowser. Het locatiemodel moet twee tabbladen hebben: Locatiedetails en Locatie-adres.

1. Selecteer **Maken** om een ander inhoudsfragmentmodel te maken. Voer bij Modeltitel &quot;Locatie&quot; in. Selecteren **Maken** gevolgd door **Openen** in het succesmodaal dat verschijnt.

1. Voeg een **Tijdelijke aanduiding voor tab** aan het model en etiketteer het &quot;Details van de Plaats.&quot;

1. Slepen en neerzetten **Tekst met één regel** en geef deze de naam &quot;Naam&quot;. Voeg onder dit veldlabel een **tekst met meerdere regels** veld en label het veld &quot;Beschrijving&quot;.

1. Voeg vervolgens een **Fragmentverwijzing** veld en label &quot;Contactinfo&quot;. Op het tabblad Eigenschappen, onder **Modellen voor toegestane inhoudsfragmenten**, selecteert u de **Mappictogram** en kies het fragmentmodel &quot;Contactinfo&quot; dat u eerder hebt gemaakt.

1. Voeg een **Content Reference** veld onder &quot;Contactinfo&quot;. Geef het label &quot;Locatieafbeelding&quot;. De **Hoofdpad** moeten `/content/dam/wknd-shared.` Onder **Alleen opgegeven inhoudstypen accepteren** selecteert u Afbeelding.

1. Laten we ook een **JSON-object** veld onder &quot;Locatieafbeelding&quot;. Aangezien dit gegevenstype flexibel is, kan het worden gebruikt om het even welke gegevens te tonen u in uw inhoud wilt omvatten. In dit geval wordt het JSON-object gebruikt om informatie over het weer weer weer te geven. Geef het JSON-object het label &#39;Weer op seizoen&#39;. In de **Eigenschappen** tabblad, voegt u een **Beschrijving** het is dus duidelijk voor de gebruiker welke gegevens hier moeten worden ingevoerd : &quot; JSON - gegevens over het seizoen waarin de gebeurtenis plaatsvindt ( lente , zomer , herfst , winter ) &quot; .

   ![Opties voor JSON-object](assets/define-content-fragment-models/json-object.png)

1. Als u het tabblad Locatieadres wilt maken, voegt u een **Tijdelijke aanduiding voor tab** aan het model en etiketteer het &quot;Adres van de Plaats.&quot;

1. Sleep een **Fragmentverwijzing** veld en van het eigenschappentabblad, labelt u het als &quot;Adres&quot; en onder **Modellen voor toegestane inhoudsfragmenten**, selecteert u de **Adres** model.

1. Selecteren **Opslaan** om uw wijzigingen te bevestigen en de Content Fragment Model Editor te sluiten. Het voltooide model van de Plaats zou als hieronder moeten verschijnen:

   ![Opties voor inhoudsverwijzing](assets/define-content-fragment-models/location-model.png)

### Teammodel {#team-model}

Tot slot creeer een model dat een team van mensen beschrijft.

1. Van de **WKND gedeeld** pagina, selecteert u **Maken** om een ander inhoudsfragmentmodel te maken. Voer voor de modeltitel &quot;Team&quot; in. Selecteer **Maken** gevolgd door **Openen** in het succesmodaal dat verschijnt.

1. Voeg een **Tekst met meerdere regels** aan het formulier. Onder **Veldlabel**, typt u &quot;Beschrijving&quot;.

1. Voeg een **Datum en tijd** veld naar het model en label het &quot;Team Founding Date&quot;. In dit geval, houd het gebrek **Type** ingesteld op &quot;Datum&quot;, maar u kunt ook &quot;Datum en tijd&quot; of &quot;Tijd&quot; gebruiken.

   ![Datum- en tijdopties](assets/define-content-fragment-models/date-and-time.png)

1. Ga naar de **Gegevenstypen** tab. Onder de &quot;Datum van de Oprichting van het Team&quot;, voeg a toe **Fragmentverwijzing**. In de **Renderen als** vervolgkeuzelijst selecteert u &quot;multifield&quot;. Voor **Veldlabel**, voert u &quot;Teamleden&quot; in. Dit veld verwijst naar de _Persoon_ eerder gemaakt model. Aangezien het gegevenstype een multi-gebied is, kunnen de veelvoudige fragmenten van de Persoon worden toegevoegd, toelatend de verwezenlijking van een team van mensen.

   ![Fragmentverwijzingsopties](assets/define-content-fragment-models/fragment-reference.png)

1. Onder **Modellen voor toegestane inhoudsfragmenten**, gebruikt u het mappictogram om het modale pad selecteren te openen en selecteert u vervolgens het **Persoon** model. Gebruik de **Selecteren** om het pad op te slaan.

   ![Persoonsmodel selecteren](assets/define-content-fragment-models/select-person-model.png)

1. Selecteren **Opslaan** om uw wijzigingen te bevestigen en de Content Fragment Model Editor te sluiten.

## Fragmentverwijzingen toevoegen aan het Adventure-model {#fragment-references}

Gelijkaardig aan hoe het model van het Team een fragmentverwijzing naar het model van de Persoon heeft, moeten de modellen van het Team en van de Plaats van het model van het Avontuur worden van verwijzingen voorzien om deze nieuwe modellen in WKND te tonen app.

1. Van de **WKND gedeeld** pagina, selecteert u de **Adventure** model, selecteer dan **Bewerken** in de bovenste navigatie.

   ![Pad voor bewerken van avontuur](assets/define-content-fragment-models/adventure-edit-path.png)

1. Onder aan het formulier, onder &quot;Wat u wilt brengen&quot;, voegt u een **Fragmentverwijzing** veld. Voer een **Veldlabel** van &quot;Locatie&quot;. Onder **Modellen voor toegestane inhoudsfragmenten**, selecteert u de **Locatie** model.

   ![Referentieopties voor locatiefragment](assets/define-content-fragment-models/location-fragment-reference.png)

1. Voeg nog een toe **Fragmentverwijzing** veld en label it &quot;Instructor Team&quot;. Onder **Modellen voor toegestane inhoudsfragmenten**, selecteert u de **Team** model.

   ![Referentieopties teamfragment](assets/define-content-fragment-models/team-fragment-reference.png)

1. Nog een toevoegen **Fragmentverwijzing** veld en label &quot;Beheerder&quot;.

   ![Referentieopties voor fragment van beheerder](assets/define-content-fragment-models/administrator-fragment-reference.png)

1. Selecteren **Opslaan** om uw wijzigingen te bevestigen en de Content Fragment Model Editor te sluiten.

## Aanbevolen procedures {#best-practices}

Er zijn een paar beste werkwijzen met betrekking tot het maken van Content Fragment Models:

* Creeer modellen die aan de componenten van UX in kaart brengen. De WKND-app heeft bijvoorbeeld Content Fragment Models voor avonturen, artikelen en locatie. U kunt ook kopteksten, promoties of disclaimers toevoegen. Elk van deze voorbeelden vormt een specifieke component UX.

* Maak zo weinig mogelijk modellen. Door het aantal modellen te beperken, kunt u hergebruik maximaliseren en inhoudsbeheer vereenvoudigen.

* Modellen van inhoudsfragmenten nesten zo diep als nodig maar alleen zo nodig. Herinneren dat het nesten met fragmentverwijzingen of inhoudsverwijzingen wordt verwezenlijkt. Neem maximaal vijf nestniveaus.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd! U hebt nu tabbladen toegevoegd, de gegevenstypen date en time voor JSON-objecten gebruikt en meer informatie gekregen over fragmentverwijzingen en inhoudsverwijzingen. U hebt ook validatieregels voor inhoudsverwijzingen toegevoegd.

## Volgende stappen {#next-steps}

Het volgende hoofdstuk in deze reeks zal [inhoudsfragmenten ontwerpen](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md) van de modellen u in dit hoofdstuk creeerde. Leer hoe u de gegevenstypen gebruikt die in dit hoofdstuk zijn geïntroduceerd en mapbeleid maakt om te beperken welke modellen van inhoudsfragmenten in een elementenmap kunnen worden gemaakt.

Hoewel dit optioneel is voor deze zelfstudie, dient u alle inhoud te publiceren in situaties waarin de inhoud in de praktijk wordt geproduceerd. Raadpleeg voor een overzicht van de auteur- en publicatie-omgevingen in AEM de
[AEM headless en GraphQL videoreeks](/help/headless-tutorial/graphql/video-series/author-publish-architecture.md).
