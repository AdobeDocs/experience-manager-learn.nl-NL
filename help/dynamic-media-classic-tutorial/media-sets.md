---
title: Afbeeldings-, staal-, centrifuge- en gemengde-mediasets
description: Een van de nuttigste en krachtigste mogelijkheden van Dynamic Media Classic is het maken van rijke mediasets, zoals Afbeelding, Staal, Draaien en Gemengde Mediasets. Leer wat elke rijke mediaset is en hoe u elk type maakt in Dynamic Media Classic. Meer informatie over voorinstellingen Batch-set, waarmee het maken van rich media-sets tijdens het uploaden wordt geautomatiseerd.
feature: Dynamic Media Classic, Image Sets, Mixed Media Sets, Spin Sets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 45c86ff2-d991-46a7-a8d1-25c9fec142d9
duration: 280
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1359'
ht-degree: 0%

---

# Afbeeldings-, staal-, centrifuge- en gemengde-mediasets {#media-sets}

Door Dynamic Media Classic ingestelde verzamelingen te verplaatsen naar meer dan één afbeelding voor dynamisch vergroten of verkleinen en zoomen, kunt u een rijkere online ervaring gebruiken. In dit gedeelte van de zelfstudie wordt uitgelegd hoe u de volgende rijke mediasets kunt maken in Dynamic Media Classic:

- Afbeeldingsset
- Staalset
- Set draaien
- Gemengde mediaset

Er wordt ook uitgelegd hoe u Voorinstellingen batchset kunt gebruiken om het maken van sets te automatiseren via een upload.

## Alles wat u altijd over sets wilde weten

Naast de standaard dynamische grootte en zoomfactor zijn sets waarschijnlijk het meest gebruikte Dynamic Media Classic-subproduct. Sets zijn in wezen &quot;virtuele&quot; elementen die geen werkelijke afbeeldingen bevatten, maar bestaan uit een reeks relaties met andere afbeeldingen en/of video. De belangrijkste aantrekkingskracht van sets is dat het mini-toepassingen zijn die klaar zijn &quot;van de plank.&quot; Door dat betekenen wij dat elke vastgestelde kijker zijn eigen logica en interface bevat zodat u slechts moet doen is vraag aan hen op de plaats. Bovendien vereisen ze alleen dat u één id voor middelen per set bijhoudt, in plaats van dat u zelf alle elementen en relaties van de leden moet beheren.

Wanneer u een set maakt, wordt die set beheerd als een afzonderlijk element dat moet worden gemarkeerd voor publiceren en gepubliceerd voordat deze via een URL kan worden verzonden. Alle activa van zijn leden moeten ook worden gepubliceerd.

### Typen sets

Meer informatie over de vier typen sets die u kunt maken in Dynamic Media Classic: Afbeelding, Staal, Draaien en Gemengde Mediasets.

## Afbeeldingsset

Dit is het meest voorkomende type set. Doorgaans gebruikt u deze voor alternatieve weergaven van hetzelfde item. Het bestaat uit meerdere afbeeldingen die u in de viewer laadt door op de bijbehorende miniatuur van die afbeelding te klikken.

![afbeelding](assets/media-sets/image-set-1.jpg)

_Voorbeeld van een Reeks van het Beeld_

De URL voor de bovenstaande afbeeldingsset kan er als volgt uitzien:

![afbeelding](assets/media-sets/image-set-url-1.png)

- Leer meer over de Reeksen van het Beeld met het [ Snelle Begin aan de Reeksen van het Beeld ](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/quick-start-image-sets.html).
- Leer hoe te [ een Reeks van het Beeld ](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/creating-image-set.html#creating-an-image-set) creëren.

### Staalset

Dit type set wordt doorgaans gebruikt om gekleurde weergaven van hetzelfde item weer te geven. Deze bestaat uit afbeeldingsparen en kleurenstalen.

Het belangrijkste verschil tussen een staal en een afbeeldingsset is dat stalensets een andere afbeelding gebruiken als een klikbaar staal, terwijl in Afbeeldingssets een miniatuurversie van de oorspronkelijke afbeelding wordt gebruikt waarop kan worden geklikt.

In stalensets worden afbeeldingen niet ingekleurd (een veel voorkomende misvatting). De afbeeldingen worden gewoon omgewisseld, net als in een Afbeeldingsset. De ministaalafbeeldingen hadden met Photoshop kunnen worden gemaakt, elke kleur had afzonderlijk kunnen worden gefotografeerd of met het gereedschap Uitsnijden in Dynamic Media Classic had een staal kunnen worden gemaakt van een van de gekleurde afbeeldingen.

![afbeelding](assets/media-sets/image-set-2.jpg)

_Voorbeeld van een Reeks van het Monster_

De URL voor de bovenstaande stalenset kan er als volgt uitzien:

![afbeelding](assets/media-sets/image-set_url.png)

- Leer meer over de Reeksen van het Monster met het [ Snelle Begin aan de Reeksen van het Monster ](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html).
- Leer hoe te [ een Reeks van het Monster ](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html#creating-a-swatch-set) creëren.

### Set draaien

Deze set wordt doorgaans gebruikt om een weergave van 360 graden van een item weer te geven. Net als Staalsets gebruiken centrifuges geen 3D-magie. Het echte werk is om veel foto&#39;s van een afbeelding van alle kanten te maken. Met de viewer kunt u eenvoudig schakelen tussen de afbeeldingen als een stop-motion-animatie.

Draaiingsets kunnen in één richting draaien langs één as of, indien ze afwisselend worden gemaakt als 2D-reeks, draaien op meerdere assen. Een auto kan bijvoorbeeld worden gedraaid terwijl alle wielen op de grond staan en vervolgens ook op de achterwielen worden &quot;gedraaid&quot;. Voor een correct ingestelde 2D-reeks voor draaien moet het aantal afbeeldingen per rij voor elke as gelijk zijn. Met andere woorden, als u op twee assen draait, hebt u twee keer zoveel afbeeldingen nodig als één hoek draaien.

![afbeelding](assets/media-sets/image-set-3.png)

_Voorbeeld van een Reeks van de Draai_

De URL voor de bovenstaande centrifugeset kan er als volgt uitzien:

![afbeelding](assets/media-sets/spin-set.png)

- Leer meer over de Reeksen van de Draai met het [ Snelle Begin om Reeksen ](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html) te draaien.
- Leer hoe te om [ een Reeks van de Draai ](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/creating-spin-set.html#creating-a-spin-set) te creëren.

## Gemengde mediaset

Dit is een combinatieset. Hiermee kunt u alle vorige sets combineren en video toevoegen in één viewer. In dit werkschema, creeert u om het even welke componentenreeksen eerst, en verenigt hen dan samen in een Gemengde Reeks van Media.

![afbeelding](assets/media-sets/image-set-4.png)

_Voorbeeld van een Gemengde Reeks van Media_

De URL voor de bovenstaande gemengde mediaset kan er als volgt uitzien:

![afbeelding](assets/media-sets/image-set-url-1.png)

- Leer meer over Gemengde Reeksen van Media met het [ Snelle Begin aan Gemengde Reeksen van Media ](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html).

- Leer hoe te [ een Gemengde Reeks van Media ](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html#creating-a-mixed-media-set) creëren.

Als u een afbeelding wilt weergeven voor zoomen, een set of een video op uw website, roept u deze aan in een Dynamic Media Classic-viewer. Dynamic Media Classic bevat viewers voor veelzijdige media-elementen, zoals stalensets, spelersets, video en vele andere.

Leer meer over [ Kijkers voor AEM Assets en Dynamic Media Classic ](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html).

## Voorinstellingen batchset

Tot nu toe hebben wij besproken hoe te om reeksen manueel te bouwen gebruikend de functie van de Bouwstijl van Dynamic Media Classic. Het is echter mogelijk om het maken van afbeeldingssets en centrifuges te automatiseren met behulp van een voorinstelling voor batchset, zolang u een standaardnaamgevingsconventie hebt.

Elke voorinstelling is een unieke, op zichzelf staande verzameling instructies die definiëren hoe de set moet worden samengesteld met afbeeldingen die overeenkomen met de gedefinieerde naamgevingsconventies. In de voorinstelling definieert u eerst naamgevingsconventies voor de elementen die u in een set wilt groeperen. Vervolgens kunt u een voorinstelling voor een batch-set maken die naar deze afbeeldingen verwijst.

Terwijl het mogelijk is om vooraf ingesteld tot stand te brengen zelf (zij worden gevonden onder **Opstelling > de Opstelling van de Toepassing > de Reeks van de Partij vooraf instelt** ), als beste praktijken u uw Consulting team of Technische Steun opstelling het voor u zou moeten hebben. Dit is de reden waarom:

- Voorinstellingen voor batchsets kunnen complex zijn om in te stellen. Ze worden aangedreven door reguliere expressies en deze syntaxis kan onbekend of verwarrend zijn, tenzij u een ontwikkelaar bent.
- Als deze eenmaal zijn gemaakt, worden ze standaard ingeschakeld. Er is geen functie &#39;ongedaan maken&#39;. Als u duizenden afbeeldingen begint te uploaden en uw voorinstelling onjuist is geconfigureerd, kan het zijn dat u uiteindelijk honderden of duizenden sets met breuken hebt, die u handmatig moet zoeken en verwijderen.

Een eenvoudige naamgevingsconventie is eerder voorgesteld, zodat u deze eenvoudig kunt opnemen in een voorinstelling voor batchsets. Omdat de voorinstellingen echter zeer flexibel zijn, kunnen ze complexe naamgevingsstrategieën gebruiken. Kortom, de afbeeldingen die deel uitmaken van een set moeten aan elkaar worden gekoppeld met een algemene naam. Dit is vaak het SKU-nummer of de product-id. In Dynamic Media Classic geeft u een standaardnaamgevingsconventie op voor alle afbeeldingen die voor een voorinstelling worden gebruikt, of u kunt meerdere voorinstellingen maken, elk met verschillende naamgevingsregels.

Voorinstellingen voor batchsets worden alleen tijdens het uploaden toegepast; ze kunnen niet worden uitgevoerd nadat de afbeeldingen zijn geüpload. Daarom is het belangrijk dat u de naamgevingsconventie indeelt en een voorinstelling maakt voordat u al uw afbeeldingen gaat laden.

Zodra vooraf instelt zijn gecreeerd, kan de Beheerder van het Bedrijf kiezen of zij actief of inactief zijn. De actieve middelen zullen zij op de uploadpagina onder **Opties van de Baan** verschijnen, terwijl inactieve voorinstellingen verborgen zullen blijven.

Leer hoe te [ een Reeks van de Partij creëren vooraf instelt ](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#creating-a-batch-set-preset).

### Batchvoorinstellingen gebruiken bij uploaden

Hieronder wordt beschreven hoe u voorinstellingen voor batchsets gebruikt bij het uploaden nadat deze zijn gemaakt:

1. Klik **uploaden** en kies of **van Desktop** of **via FTP**.
2. Klik **Opties van de Baan**.
3. Open de **Reeks van de Partij vooraf instelt** optie, en controleer of uncheck vooraf ingesteld om het met te gebruiken uploadt.
4. Nadat het uploaden is voltooid, kijkt u in uw map naar de voltooide sets.

Leer meer over [ Reeks van de Partij vooraf instelt ](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#batch-set-presets).
