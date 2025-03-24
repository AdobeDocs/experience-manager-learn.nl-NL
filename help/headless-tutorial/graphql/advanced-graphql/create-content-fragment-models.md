---
title: Modellen voor inhoudsfragmenten maken - Geavanceerde concepten van AEM Headless - GraphQL
description: In dit hoofdstuk van de Geavanceerde concepten van Adobe Experience Manager (AEM) Headless, leer hoe te om een Model van het Fragment van de Inhoud uit te geven door lusjeplaceholders, datum en tijd, voorwerpen JSON, fragmentverwijzingen, en inhoudsverwijzingen toe te voegen.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 2122ab13-f9df-4f36-9c7e-8980033c3b10
duration: 757
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1991'
ht-degree: 0%

---

# Modellen voor inhoudsfragmenten maken {#create-content-fragment-models}

Dit hoofdstuk doorloopt de stappen om vijf Modellen van het Fragment van de Inhoud tot stand te brengen:

* **Info van het Contact**
* **Adres**
* **Persoon**
* **Plaats**
* **Team**

Met contentfragmentmodellen kunt u relaties tussen inhoudstypen definiëren en relaties als schema&#39;s voortzetten. Gebruik geneste fragmentverwijzingen, verschillende inhoudsgegevenstypen en het tabtype voor visuele inhoudsorganisatie. Geavanceerde gegevenstypen, zoals tabplaatsaanduidingen, fragmentverwijzingen, JSON-objecten en het datum- en tijdgegevenstype.

In dit hoofdstuk wordt ook beschreven hoe u de validatieregels voor inhoudsverwijzingen, zoals afbeeldingen, kunt verbeteren.

## Vereisten {#prerequisites}

Dit is een geavanceerde zelfstudie. Alvorens met dit hoofdstuk te werk te gaan, zorg ervoor dat u de [ snelle opstelling ](../quick-setup/cloud-service.md) hebt voltooid. Zorg ervoor dat u ook door het vorige [ overzicht ](../overview.md) hoofdstuk voor meer informatie over de opstelling voor het geavanceerde leerprogramma hebt gelezen.

## Doelstellingen {#objectives}

* Modellen voor inhoudsfragmenten maken.
* Voeg tabplaatsaanduidingen, datum en tijd, JSON-objecten, fragmentverwijzingen en inhoudsverwijzingen toe aan de modellen.
* Validatie toevoegen aan inhoudsverwijzingen.

## Overzicht van het inhoudsfragmentmodel {#content-fragment-model-overview}

De volgende video biedt een korte inleiding tot Modellen van inhoudsfragmenten en hoe deze in deze zelfstudie worden gebruikt.

>[!VIDEO](https://video.tv.adobe.com/v/340037?quality=12&learn=on)

## Modellen voor inhoudsfragmenten maken {#create-models}

Laten we een aantal modellen van inhoudsfragmenten maken voor de WKND-app. Als u een basisinleiding aan het creëren van de Modellen van het Fragment van de Inhoud vereist, gelieve het aangewezen hoofdstuk in het [ basisleerprogramma ](../multi-step/content-fragment-models.md) te zien.

1. Navigeer aan **Hulpmiddelen** > **Algemene** > **Modellen van het Fragment van de Inhoud**.

   ![ de Weg van Modellen van het Fragment van de Inhoud ](assets/define-content-fragment-models/content-fragment-models-path.png)

1. Selecteer **Gedeelde WKND** om de lijst van bestaande Modellen van het Fragment van de Inhoud voor de plaats te bekijken.

### Contactinfo-model {#contact-info-model}

Maak vervolgens een model dat de contactgegevens van een persoon of locatie bevat.

1. Selecteer **creeer** in de hoger-juiste hoek.

1. Geef het model een titel van &quot;Info van het Contact&quot;, dan uitgezocht **creeer**. In het succesmodaal dat verschijnt, uitgezochte **Open** om het pas gecreëerde model uit te geven.

1. Begin door a **Enige gebied van de lijntekst** op het model te slepen. Geef het a **Etiket van het Gebied** van &quot;Telefoon&quot;in de **Eigenschappen** tabel. De eigenschapsnaam wordt automatisch ingevuld als `phone` . Selecteer checkbox om het gebied **Vereist** te maken.

1. Navigeer aan het **lusje van de Types van Gegevens**, dan voeg een ander **Enige lijntekst** gebied onder het &quot;Telefoon&quot;gebied toe. Geef het a **Etiket van het Gebied** van &quot;E-mail&quot;en plaats het ook aan **Vereiste**.

Adobe Experience Manager wordt geleverd met ingebouwde validatiemethoden. Met deze validatiemethoden kunt u beheerregels toevoegen aan specifieke velden in uw modellen van inhoudsfragmenten. In dit geval voegen we een validatieregel toe om ervoor te zorgen dat gebruikers alleen geldige e-mailadressen kunnen invoeren wanneer ze dit veld invullen. Onder het **drop-down Type van Bevestiging**, uitgezochte **E-mail**.

Het voltooide fragmentmodel voor inhoud moet er als volgt uitzien:

![ de modelweg van Info van het Contact ](assets/define-content-fragment-models/contact-info-model.png)

Zodra gedaan, uitgezocht **sparen** om uw veranderingen te bevestigen en de ModelRedacteur van het Fragment van de Inhoud te sluiten.

### Adresmodel {#address-model}

Maak vervolgens een model voor een adres.

1. Van **Gedeelde WKND**, uitgezocht **creeer** van de hoger-juiste hoek.

1. Ga een titel van &quot;Adres&quot;in en selecteer dan **creeer**. In het succesmodaal dat verschijnt, uitgezochte **Open** om het pas gecreëerde model uit te geven.

1. De belemmering en laat vallen a **Enige lijntekst** gebied op het model en geeft het a **Etiket van het Gebied** van &quot;Adres van de Straat.&quot; De eigenschapsnaam wordt vervolgens ingevuld als `streetAddress` . Selecteer **Vereiste** checkbox.

1. Herhaal bovenstaande stappen en voeg nog vier velden voor tekst uit één regel toe aan het model. Gebruik de volgende labels:

   * Plaats
   * Staat
   * Postcode
   * Land

1. Selecteer **sparen** om de veranderingen in het model van het Adres te bewaren.

   Het voltooide fragmentmodel &quot;Adres&quot; moet er als volgt uitzien:
   ![ model van het Adres ](assets/define-content-fragment-models/address-model.png)

### Persmodel {#person-model}

Maak vervolgens een model dat informatie over een persoon bevat.

1. In de hoger-juiste hoek, creeert de uitgezochte ****.

1. Geef het model een titel van &quot;Persoon&quot;, dan uitgezocht **creeer**. In het succesmodaal dat verschijnt, uitgezochte **Open** om het pas gecreëerde model uit te geven.

1. Begin door a **Enige gebied van de lijntekst** op het model te slepen. Geef het a **Etiket van het Gebied** van &quot;Volledige Naam&quot;. De eigenschapsnaam wordt automatisch ingevuld als `fullName` . Selecteer checkbox om het gebied **Vereist** te maken.

   ![ Volledige naamopties ](assets/define-content-fragment-models/full-name.png)

1. In andere modellen kunt u verwijzen naar modellen van inhoudsfragmenten. Navigeer aan het **lusje van de Types van Gegevens**, dan belemmering en laat vallen het **gebied van de Verwijzing van het Fragment** en geef het een etiket van &quot;Info van het Contact&quot;.

1. In het **lusje van Eigenschappen**, onder het **Toegestane modelgebied van het Fragment van de Inhoud**, selecteer het omslagpictogram en kies dan het **vroeger gecreeerd fragmentmodel van Info van het Contact**.

1. Voeg het gebied van de Verwijzing van de a **Inhoud** toe en geef het a **Etiket van het Gebied** van &quot;Beeld van het Profiel.&quot; Selecteer het omslagpictogram onder **Weg van de Wortel** om de modaal van de wegselectie te openen. Selecteer een wortelweg door **inhoud** te selecteren > **Assets**, dan selecterend checkbox voor **Gedeelde WKND**. Gebruik **Uitgezochte** knoop bij het hoogste recht om de weg te bewaren. Het uiteindelijke tekstpad moet `/content/dam/wknd-shared` lezen.

   ![ de wortelweg van de inhoudsverwijzing ](assets/define-content-fragment-models/content-reference-root-path.png)

1. Onder **keurt slechts gespecificeerde inhoudstypes** goed, uitgezochte &quot;Beeld&quot;.

   ![ de beeldopties van het Profiel ](assets/define-content-fragment-models/profile-picture.png)

1. Als u de grootte en afmetingen van afbeeldingsbestanden wilt beperken, bekijkt u enkele validatieopties voor het veld met inhoudsverwijzing.

   Onder **keurt slechts gespecificeerde dossiergrootte** goed, uitgezocht &quot;minder dan of gelijk aan&quot;, en de extra gebieden verschijnen hieronder.
   ![ keurt slechts gespecificeerde dossiergrootte ](assets/define-content-fragment-models/accept-specified-file-size.png) goed

1. Voor **Max**, ga &quot;5&quot;in, en voor **Uitgezochte Eenheid**, selecteer &quot;Megabytes (MB)&quot;. Bij deze validatie kunnen alleen afbeeldingen van de opgegeven grootte worden gekozen.

1. Onder **keurt slechts gespecificeerde beeldbreedte** goed, selecteer &quot;Maximale Breedte&quot;. Op het **Max (pixel)** gebied dat verschijnt, ga &quot;10000&quot;in. Selecteer de zelfde opties voor **goedkeuren slechts een gespecificeerde beeldhoogte**.

   Deze validaties zorgen ervoor dat toegevoegde afbeeldingen de opgegeven waarden niet overschrijden. De validatieregels moeten er nu als volgt uitzien:

   ![ de verwijzingsbevestigingsregels van de Inhoud ](assets/define-content-fragment-models/content-reference-validation.png)

1. Voeg a **Meerdere lijntekst** gebied toe en geef het a **Etiket van het Gebied** van &quot;Biografie&quot;. Verlaat het **StandaardType** dropdown als standaard &quot;Rijke Tekst&quot;optie.

   ![ de opties van de Biografie ](assets/define-content-fragment-models/biography.png)

1. Navigeer aan de **Types van Gegevens** tabel, dan sleep een **Opsomming** gebied onder &quot;Biografie&quot;. In plaats van het gebrek **geeft terug als** optie, uitgezocht **Dropdown** en geeft het a **Etiket van het Gebied** van &quot;het Niveau van de Ervaring van de Instructeur&quot;. Ga een selectie van de opties van het de ervaringsniveau van de instructeur zoals _Expert, Geavanceerd in, Midden_.

1. Daarna, sleep een ander **gebied van de Opsomming** onder &quot;het Niveau van de Ervaring van de Instructeur&quot;en kies &quot;checkboxes&quot;onder **teruggeven als** optie. Geef het a **Etiket van het Gebied** van &quot;Vaardigheden&quot;. Voer verschillende vaardigheden in, zoals het klimmen van rots, surfen, fietsen, skiën en backpackaging. Het label en de waarde van de optie moeten overeenkomen met de onderstaande waarden:

   ![ Opsomming van Vaardigheden ](assets/define-content-fragment-models/skills-enum.png)

1. Tot slot creeer een het gebiedsetiket van de &quot;Details van de Beheerder&quot;gebruikend a **multi-line tekst** gebied.

Selecteer **sparen** om uw veranderingen te bevestigen en de Redacteur van het Model van het Fragment van de Inhoud te sluiten.

### Locatiemodel {#location-model}

In het volgende inhoudsfragmentmodel wordt een fysieke locatie beschreven. Dit model gebruikt tabplaatsaanduidingen. Met tabplaatsaanduidingen kunt u de gegevenstypen in respectievelijk de modeleditor en de inhoud in de fragmenteditor ordenen door de inhoud te categoriseren. Elke plaatsaanduiding maakt een tabblad in de Content Fragment-editor, vergelijkbaar met een tabblad in een internetbrowser. Het locatiemodel moet twee tabbladen hebben: Locatiedetails en Locatie-adres.

1. Zoals eerder, creeer de uitgezochte **** om een ander Model van het Fragment van de Inhoud tot stand te brengen. Voer bij Modeltitel &quot;Locatie&quot; in. Selecteer **creeer** dat door **wordt gevolgd Open** in het succesmodaal dat verschijnt.

1. Voeg het gebied van Placeholder van het a **Lusje** aan het model toe en etiketteer het &quot;Details van de Plaats.&quot;

1. De belemmering en een daling a **Enige Tekst van de Lijn** en etiketteert het &quot;Naam.&quot; Onder dit gebiedsetiket, voeg a **multi-line tekst** gebied toe en etiketteer het &quot;Beschrijving.&quot;

1. Daarna, voeg het gebied van de Verwijzing van het a **Fragment** toe en etiketteer het &quot;Info van het Contact.&quot; In het eigenschappen lusje, onder **Toegestane Modellen van het Fragment van de Inhoud**, selecteer het **Pictogram van de Omslag** en kies het &quot;vroeger gecreeerd fragmentmodel van Info van het Contact&quot;.

1. Voeg het gebied van de Verwijzing van de a **Inhoud** onder &quot;Info van het Contact&quot; toe. Geef het label &quot;Locatieafbeelding&quot;. Het **Weg van de Weg van de Wortel** zou `/content/dam/wknd-shared.` onder **moeten zijn goedkeurt slechts gespecificeerde inhoudstypes**, selecteert &quot;Beeld&quot;.

1. Laten wij ook a **gebied van Objecten JSON** {onder het Beeld van de Plaats toevoegen.&quot; Aangezien dit gegevenstype flexibel is, kan het worden gebruikt om het even welke gegevens te tonen u in uw inhoud wilt omvatten. In dit geval wordt het JSON-object gebruikt om informatie over het weer weer weer te geven. Geef het JSON-object het label &#39;Weer op seizoen&#39;. In het **lusje van Eigenschappen**, voeg a **Beschrijving** toe zodat is het duidelijk aan de gebruiker welke gegevens hier zouden moeten zijn ingegaan: &quot;JSON- gegevens betreffende het weerweer van de gebeurtenisplaats door seizoen (Lente, Zomer, Herfst, Winter).&quot;

   ![ opties van Objecten JSON ](assets/define-content-fragment-models/json-object.png)

1. Om het lusje van het Adres van de Plaats tot stand te brengen, voeg Placeholder van het a **Lusje** gebied aan het model toe en etiketteer het &quot;Adres van de Plaats.&quot;

1. De belemmering en laat vallen het gebied van de Verwijzing van het a **en van het eigenschappen lusje, etiketteert het als &quot;Adres&quot;, en onder** Toegestane Modellen van het Fragment van de Inhoud **, selecteert het** Adres **model.**

1. Selecteer **sparen** om uw veranderingen te bevestigen en de Redacteur van het Model van het Fragment van de Inhoud te sluiten. Het voltooide model van de Plaats zou als hieronder moeten verschijnen:

   ![ de verwijzingsopties van de Inhoud ](assets/define-content-fragment-models/location-model.png)

### Teammodel {#team-model}

Tot slot creeer een model dat een team van mensen beschrijft.

1. Van de **Gedeelde WKND** pagina, uitgezocht **creeer** om een ander Model van het Fragment van de Inhoud te maken. Voer voor de modeltitel &quot;Team&quot; in. Zoals eerder, creeer uitgezochte **** die door **wordt gevolgd Open** in het succesmodaal dat verschijnt.

1. Voeg a **Meerdere lijntekst** gebied aan de vorm toe. Onder **Etiket van het Gebied**, ga &quot;Beschrijving&quot;in.

1. Voeg het gebied van de Datum en van de Tijd van de a **** aan het model toe en etiketteer het &quot;Team die Datum&quot;stichten. In dit geval, houd het standaard **Type** geplaatst aan &quot;Datum&quot;, maar merk op dat het ook mogelijk is om &quot;Datum &amp; Tijd&quot;of &quot;Tijd&quot;te gebruiken.

   ![ de opties van de Datum en van de tijd ](assets/define-content-fragment-models/date-and-time.png)

1. Navigeer aan de **Types van Gegevens** tabel. Onder het &quot;Team dat Datum&quot;oprichtt, voeg de Verwijzing van het a **Fragment** toe. In **geef terug als** dropdown, uitgezocht &quot;multifield&quot;. Voor **Etiket van het Gebied**, ga &quot;Leden van het Team&quot;in. Dit gebied verbindt met het _model van de Persoon_ eerder gecreeerd. Aangezien het gegevenstype een multi-gebied is, kunnen de veelvoudige fragmenten van de Persoon worden toegevoegd, toelatend de verwezenlijking van een team van mensen.

   ![ de verwijzingsopties van het Fragment ](assets/define-content-fragment-models/fragment-reference.png)

1. Onder **Toegestane Modellen van het Fragment van de Inhoud**, gebruik het omslagpictogram om de Uitgezochte modale Weg te openen, dan selecteer het **Person** model. Gebruik **Uitgezochte** knoop om de weg te bewaren.

   ![ Uitgezochte model van de Persoon ](assets/define-content-fragment-models/select-person-model.png)

1. Selecteer **sparen** om uw veranderingen te bevestigen en de Redacteur van het Model van het Fragment van de Inhoud te sluiten.

## Fragmentverwijzingen toevoegen aan het Adventure-model {#fragment-references}

Gelijkaardig aan hoe het model van het Team een fragmentverwijzing naar het model van de Persoon heeft, moeten de modellen van het Team en van de Plaats van het model van het Avontuur worden van verwijzingen voorzien om deze nieuwe modellen in WKND te tonen app.

1. Van de **Gedeelde WKND** pagina, selecteer het **model van het avontuur**, dan uitgezocht **geef** van de hoogste navigatie uit.

   ![ Adventure geeft weg uit ](assets/define-content-fragment-models/adventure-edit-path.png)

1. Bij de bodem van de vorm, onder &quot;wat om&quot;te brengen, voeg het gebied van de Verwijzing van het a **Fragment** toe. Ga het Etiket van het a **Gebied** van &quot;Plaats in&quot;. Onder **Toegestane Modellen van het Fragment van de Inhoud**, selecteer het **3} model van de Plaats {.**

   ![ het fragmentverwijzingsopties van de Plaats ](assets/define-content-fragment-models/location-fragment-reference.png)

1. Voeg één meer **gebied van de Verwijzing van het 0} fragment toe {en etiketteer het &quot;Team van de Instructeur&quot;.** Onder **Toegestane Modellen van het Fragment van de Inhoud**, selecteer het **Team** model.

   ![ de verwijzingsopties van het fragment van het Team ](assets/define-content-fragment-models/team-fragment-reference.png)

1. Voeg een ander **gebied van de Verwijzing van het 1} fragment toe** en etiketteer het &quot;Beheerder.&quot;

   ![ de verwijzingsopties van het fragment van de Beheerder ](assets/define-content-fragment-models/administrator-fragment-reference.png)

1. Selecteer **sparen** om uw veranderingen te bevestigen en de Redacteur van het Model van het Fragment van de Inhoud te sluiten.

## Aanbevolen procedures {#best-practices}

Er zijn een paar beste werkwijzen met betrekking tot het maken van Content Fragment Models:

* Creeer modellen die aan de componenten van UX in kaart brengen. De WKND-app heeft bijvoorbeeld Content Fragment Models voor avonturen, artikelen en locatie. U kunt ook kopteksten, promoties of disclaimers toevoegen. Elk van deze voorbeelden vormt een specifieke component UX.

* Maak zo weinig mogelijk modellen. Door het aantal modellen te beperken, kunt u hergebruik maximaliseren en inhoudsbeheer vereenvoudigen.

* Modellen van inhoudsfragmenten nesten zo diep als nodig maar alleen zo nodig. Herinneren dat het nesten met fragmentverwijzingen of inhoudsverwijzingen wordt verwezenlijkt. Neem maximaal vijf nestniveaus.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd! U hebt nu tabbladen toegevoegd, de gegevenstypen date en time voor JSON-objecten gebruikt en meer informatie gekregen over fragmentverwijzingen en inhoudsverwijzingen. U hebt ook validatieregels voor inhoudsverwijzingen toegevoegd.

## Volgende stappen {#next-steps}

Het volgende hoofdstuk in deze reeks zal [ auteursinhoudsfragmenten ](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md) van de modellen behandelen u in dit hoofdstuk creeerde. Leer hoe u de gegevenstypen gebruikt die in dit hoofdstuk zijn geïntroduceerd en mapbeleid maakt om te beperken welke modellen van inhoudsfragmenten in een elementenmap kunnen worden gemaakt.

Hoewel dit optioneel is voor deze zelfstudie, dient u alle inhoud te publiceren in situaties waarin de inhoud in de praktijk wordt geproduceerd. Raadpleeg voor een overzicht van auteur- en publicatieomgevingen in AEM de
[ AEM Headless en GraphQL videoreeks ](/help/headless-tutorial/graphql/video-series/author-publish-architecture.md).
