---
title: Bepaal de structuur van uw map en de naamgevingsconventie voor bestanden
description: Bestandsnaamgeving is wellicht de belangrijkste beslissing die u zult nemen bij het implementeren van Dynamic Media Classic. De mapstructuur is ook belangrijk. Leer waarom het zo belangrijk en mogelijk is om voor uw omslagstructuur en dossiernamen te kiezen.
sub-product: dynamic-media
feature: null
doc-type: tutorial
activity: develop
topics: development, authoring, configuring, architecture
audience: all
translation-type: tm+mt
source-git-commit: e7a02900b0582fe9b329e5f9bd568f3c54d8d63d
workflow-type: tm+mt
source-wordcount: '1211'
ht-degree: 0%

---


# Bepaal uw Omslagstructuur en Overeenkomst {#folder-structure-filenaming} van het Noemen van het Dossier

Voordat u naar binnen gaat en al uw inhoud gaat uploaden, is het verstandig om rekening te houden met de mapstructuur die u gebruikt en met name met de naamgevingsconventie voor bestanden. Het zal u waarschijnlijk tijd besparen en het moeten taken later opnieuw doen. Het is beter om deze besluiten over alle groepen te coördineren.

## Maphiërarchie en naamgevingsconventie voor bestanden

Bestandsnaamgeving is doorgaans de belangrijkste beslissing die u neemt met betrekking tot de implementatie van Dynamic Media Classic. Om te begrijpen waarom het belangrijk is, bespreken we eerst de mapstructuur.

### Maphiërarchie

Maphiërarchie is alleen voor u en uw bedrijf van belang voor organisatorische doeleinden. Uw URL&#39;s met dynamische media in de klassieke klasse verwijzen alleen naar de naam van het element, niet naar de map of het pad. Ongeacht waar u een bestand uploadt, zal de URL hetzelfde zijn. Dit is heel anders dan hoe de meeste mensen hun afbeeldingen en inhoud voor het web ordenen, maar met Dynamic Media Classic maakt het geen verschil.

Een andere belangrijke overweging is het aantal elementen of mappen dat in elke map moet worden opgeslagen. Als veel elementen in een map zijn opgeslagen, nemen de prestaties af bij het weergeven van elementen in Dynamic Media Classic. Sla duizenden bestanden niet op in een map. In plaats daarvan ontwikkelt u een organisatiehiërarchie met minder dan ongeveer 500 elementen of mappen binnen een bepaalde tak van uw hiërarchie. Dit is geen strikte eis, maar het zal helpen om aanvaardbare reactietijden te handhaven wanneer het bekijken van of het zoeken van activa. In feite is de aanbeveling om hiërarchieën te creëren die breed en oppervlakkig zijn in plaats van smal en diep.

De eenvoudigste manier om uw mappen te maken, is om de volledige mapstructuur te uploaden met behulp van FTP en de optie **Submappen opnemen** in te schakelen. Met deze optie maakt Dynamic Media Classic de mapstructuur op de FTP-site opnieuw in Dynamic Media Classic.

Voordat u met het uploaden van al uw bestanden begint, moet u rekening houden met de mapstructuur. Het is namelijk veel gemakkelijker om uw bestanden en mappen lokaal op uw computer te ordenen en te beheren dan in Dynamic Media Classic. U kunt bijvoorbeeld alleen bestanden, maar niet volledige mappen, slepen en neerzetten in Dynamic Media Classic.

### Mapstrategieën

Overweeg voor uw mapstrategie wat voor uw organisatie zinnig is. Hier volgen een aantal veelvoorkomende naamgevingsscenario&#39;s voor mappen:

- De website van de spiegel of productverdeling. Als u bijvoorbeeld kleding hebt verkocht, hebt u mogelijk mappen voor Men, Vrouwen en Bureau-accessoires en submappen voor Zijden en Shoes.
- Op SKU of product-id gebaseerde strategie. Bij detailhandelaren met duizenden items is het bijvoorbeeld handig SKU-nummers of product-id&#39;s als mapnamen te gebruiken.
- Merkstrategie. Fabrikanten die meerdere merken hebben, kunnen bijvoorbeeld hun merknaam kiezen als mappen op hoofdniveau.

## Naamgevingsconventie voor bestanden

Hoe u verkiest om uw dossiers te noemen is misschien het belangrijkste vroege besluit u met betrekking tot Dynamische Klassieke Media zult nemen. Dit komt doordat alle elementen in Dynamic Media Classic unieke namen moeten hebben, ongeacht de locatie van de elementen in de account.

Alle URL&#39;s en transacties in Dynamic Media Classic worden aangestuurd door een element-id, de unieke id van een element in de database. Wanneer u een bestand uploadt, wordt de element-id gemaakt door de bestandsnaam te nemen en de extensie te verwijderen. _896649.jpg_ krijgt bijvoorbeeld element _ID 896649_.

Regels betreffende ID&#39;s van activa:

- Geen twee elementen kunnen dezelfde naam hebben in Dynamic Media Classic, ongeacht in welke map de elementen zich bevinden.
- Namen zijn hoofdlettergevoelig. Met bijvoorbeeld Stoel.jpg, stoel.jpg en CHAIR.jpg kunt u drie verschillende id&#39;s voor middelen maken.
- Als beste praktijk, zou identiteitskaart van Activa geen lege ruimten of symbolen moeten bevatten. Het gebruik van spaties en symbolen maakt de implementatie moeilijker omdat u deze tekens via URL moet coderen. Een spatie &quot; &quot; wordt bijvoorbeeld &quot;%20&quot;.

Uw noemende overeenkomst is hoofdzakelijk hoe u met Dynamische Klassieke Media integreert. U integreert doorgaans uw back-office systemen niet in Dynamic Media Classic omdat het een gesloten systeem is. Het is een passieve partner, die op instructies in de vorm van URLs wacht.

De meeste gebruikers baseren hun noemende overeenkomst rond hun interne SKU of product IDs zodat wanneer een Web-pagina met informatie over die SKU omhoog wordt geroepen, de pagina automatisch naar een beeld kan zoeken dat een gelijkaardige naam heeft. Als er geen verbinding tussen het dossier - naam en SKU of identiteitskaart is, dan zal uw achterkantoorsysteem spoor van elke dossiernaam manueel moeten houden, en een persoon zal die verenigingen moeten handhaven - in het kort, veel werk voor zowel IT als inhoudsteams.

### Naamgevingsstrategieën voor bestanden

Uw naamgevingsstrategie moet flexibel zijn voor toekomstige uitbreidingen, zodat u niet hoeft te hernoemen nadat u de site hebt gestart. Hier volgen enkele typische naamgevingsstrategieën:

**Geen alternatieve afbeeldingen.** In dit scenario hebt u slechts één afbeelding per product en geen alternatieve of gekleurde weergaven. U geeft elke afbeelding uitsluitend een naam op basis van het unieke SKU- of product-id-nummer. Wanneer de pagina wordt geladen, roept het paginasjabloon de Asset ID aan met hetzelfde SKU-nummer.

| SKU/PID | Bestandsnaam | Element-id |
| ------- | ---------- | -------- |
| 896649 | 896649.jpg | 896649 |
| SKU123 | SKU123.png | SKU123 |

Dit is een heel eenvoudig systeem, en goed als u bescheiden behoeften hebt. Het is echter niet erg flexibel. Alleen al omdat u vandaag geen alternatieve afbeeldingen hebt, betekent dit niet dat u deze afbeeldingen morgen niet zult hebben. Het volgende scenario biedt meer flexibiliteit.

**Met de afbeelding, alternatieve weergaven, gekleurde versies en stalen.** Deze strategie maakt alternatieve weergaven en/of gekleurde weergaven mogelijk, als u ze hebt. In plaats van de afbeelding een naam te geven na alleen de SKU, voegt u een modifier zoals &quot;_1&quot; en &quot;_2&quot; toe voor alternatieve weergaven en een kleurcode &quot;_RED&quot; of &quot;_BLU&quot; voor gekleurde weergaven. Als u zowel gekleurde afbeeldingen als alternatieve weergaven voor hetzelfde product hebt, voegt u wellicht &quot;_RED_1&quot; en &quot;_RED_2&quot; toe voor de eerste en tweede weergave met rode kleuren. Stalen krijgen de naam SKU, kleurcode en de extensie &quot;_SW&quot;.

| SKU/PID | Categorie | Bestandsnaam | Element-id |
| ------- | ----------------------- | ------------------------------------------- | ------------------------------- |
| AA123 | Alt-weergaven | AA123_1.tif AA123_2.tif AA123_3.tif | AA123_1 AA123_2 AA123_3 |
|  | Gekleurde weergaven | AA123_BLU.tif AA123_RED.tif AA123_BROWN.tif | AA123_BLU AA123_RED AA123_BROWN |
|  | Stalen | AA123_BLU_SW.tif | AA123_BLU_SW |
|  | Afbeeldingsset of stalenset |  | AA123 of AA123_SET | — |

Wanneer u werkt met ingestelde verzamelingen, zoals Afbeeldingssets en Staalsets, moet de set zelf ook een unieke naam hebben. In dit geval kan de set dus de basis-SKU als naam krijgen, of de SKU met de extensie &quot;_SET&quot;.

### Naamgevingsconventie en automatisering

Nog een laatste woord over het belang van naamgeving. Als u sets wilt gebruiken (zoals Afbeeldingssets of Staalsets), kunt u de creatie ervan automatiseren met een conventie die voorspelbaar is voor de naamgeving. Eventuele scriptmethoden, zoals een voorinstelling Batch-set, waarover u in een afzonderlijk gedeelte van deze zelfstudie leert, kunnen worden uitgeschakeld met een naamgevingsconventie.

De andere methode is om uw sets handmatig te maken. Hoewel het handmatig maken van afbeeldingssets voor 200 afbeeldingen wellicht geen grote taak is, kunt u zich voorstellen dat u meer dan 100.000 afbeeldingen hebt. Dit is wanneer de vastgestelde schepingsautomatisering cruciaal wordt.
