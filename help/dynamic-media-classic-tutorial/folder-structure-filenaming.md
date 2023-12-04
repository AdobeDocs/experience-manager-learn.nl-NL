---
title: Bepaal de structuur van uw map en de naamgevingsconventie voor uw bestand
description: Bestandsnaamgeving is misschien wel de belangrijkste beslissing die u neemt bij het implementeren van Dynamic Media Classic. De mapstructuur is eveneens van belang. Leer waarom het zo belangrijk en mogelijk is om voor uw omslagstructuur en dossiernamen te kiezen.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 15121896-9196-4ce0-aff2-9178563326b4
duration: 330
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1202'
ht-degree: 0%

---

# Bepaal de structuur van uw map en de naamgevingsconventie voor uw bestand {#folder-structure-filenaming}

Voordat u naar binnen gaat en al uw inhoud gaat uploaden, is het verstandig om rekening te houden met de mapstructuur die u gebruikt en met name met de naamgevingsconventie voor bestanden. Het zal u waarschijnlijk tijd besparen en het moeten taken later opnieuw doen. Het is beter om deze besluiten over alle groepen te coördineren.

## Maphiërarchie en naamgevingsconventie voor bestanden

Bestandsnaamgeving is over het algemeen de belangrijkste beslissing die u neemt met betrekking tot de implementatie van Dynamic Media Classic. Om te begrijpen waarom het belangrijk is, bespreken we eerst de mapstructuur.

### Maphiërarchie

Maphiërarchie is alleen voor u en uw bedrijf van belang voor organisatorische doeleinden — uw Dynamic Media Classic-URL&#39;s verwijzen alleen naar de naam van het element, niet naar de map of het pad. Ongeacht waar u een bestand uploadt, is de URL hetzelfde. Dit is heel anders dan hoe de meeste mensen hun afbeeldingen en inhoud voor het web organiseren, maar met Dynamic Media Classic maakt het geen verschil.

Een andere belangrijke overweging is het aantal elementen of mappen dat in elke map moet worden opgeslagen. Als er veel middelen in een map zijn opgeslagen, neemt de prestatie af bij het weergeven van middelen in Dynamic Media Classic. Sla duizenden bestanden niet op in een map. In plaats daarvan, ontwikkelt een organisatorische hiërarchie met minder dan ongeveer 500 activa of omslagen binnen een bepaalde tak van uw hiërarchie. Dit is geen strikte voorwaarde, maar het helpt om aanvaardbare reactietijden te handhaven wanneer het bekijken van of het zoeken van activa. In feite is de aanbeveling om hiërarchieën te creëren die breed en oppervlakkig zijn in plaats van smal en diep.

De eenvoudigste manier om uw mappen te maken, is om de volledige mapstructuur te uploaden met behulp van FTP en de optie in te schakelen **Inclusief submappen**. Met deze optie maakt Dynamic Media Classic de mapstructuur opnieuw op de FTP-site in Dynamic Media Classic.

We willen dat u rekening houdt met uw mapstructuur voordat u al uw bestanden gaat uploaden, omdat het veel eenvoudiger is om uw bestanden en mappen lokaal op uw computer te ordenen en te beheren dan in Dynamic Media Classic. U kunt bijvoorbeeld alleen bestanden, maar niet volledige mappen, slepen en neerzetten in Dynamic Media Classic.

### Mapstrategieën

Overweeg voor uw omslagstrategie wat aan uw organisatie zinvol is. Hier volgen een aantal veelvoorkomende naamgevingsscenario&#39;s voor mappen:

- De website van de spiegel of productverdeling. Als u bijvoorbeeld kleding hebt verkocht, hebt u mogelijk mappen voor Men, Vrouwen en Bureau-accessoires en submappen voor Zijden en Shoes.
- Op SKU of product-id gebaseerde strategie. Bij detailhandelaren met duizenden items is het bijvoorbeeld handig SKU-nummers of product-id&#39;s als mapnamen te gebruiken.
- Merkstrategie. Fabrikanten die meerdere merken hebben, kunnen bijvoorbeeld hun merknaam kiezen als mappen op hoofdniveau.

## Overeenkomst bestandsnaamgeving

Hoe u ervoor kiest om uw bestanden een naam te geven, is wellicht de belangrijkste beslissing die u in een vroeg stadium zult nemen met betrekking tot Dynamic Media Classic. Dit komt doordat alle elementen in Dynamic Media Classic unieke namen moeten hebben, ongeacht de locatie van de elementen op de account.

Alle URL&#39;s en transacties in Dynamic Media Classic worden aangestuurd door een element-id, de unieke id van een element in de database. Wanneer u een bestand uploadt, wordt de element-id gemaakt door de bestandsnaam te nemen en de extensie te verwijderen. Bijvoorbeeld: _896649,jpg_ get-element _ID 89649_.

Regels betreffende ID&#39;s van activa:

- Geen twee elementen kunnen dezelfde naam hebben in Dynamic Media Classic, ongeacht in welke map de elementen zich bevinden.
- Namen zijn hoofdlettergevoelig. Met bijvoorbeeld Stoel.jpg, stoel.jpg en CHAIR.jpg kunt u drie verschillende id&#39;s voor middelen maken.
- Als beste praktijk, zou identiteitskaart van Activa geen lege ruimten of symbolen moeten bevatten. Het gebruik van spaties en symbolen maakt de implementatie moeilijker omdat u deze tekens via URL moet coderen. Een spatie &quot; &quot; wordt bijvoorbeeld &quot;%20&quot;.

Uw naamgevingsconventie is in feite hoe u met Dynamic Media Classic integreert. U integreert uw back-office systemen gewoonlijk niet in Dynamic Media Classic omdat het een gesloten systeem is. Het is een passieve partner, die op instructies in de vorm van URLs wacht.

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
|         | Kleurweergaven | AA123_BLU.tif AA123_RED.tif AA123_BROWN.tif | AA123_BLU AA123_RED AA123_BROWN |
|         | Stalen | AA123_BLU_SW.tif | AA123_BLU_SW |
|         | Afbeeldingsset of stalenset |                                             | AA123 of AA123_SET | — |

Wanneer u werkt met ingestelde verzamelingen, zoals Afbeeldingssets en Staalsets, moet de set zelf ook een unieke naam hebben. In dit geval kan de set dus de basis-SKU als naam krijgen, of de SKU met de extensie &quot;_SET&quot;.

### Naamgevingsconventie en automatisering

Nog een laatste woord over het belang van naamgeving. Als u sets wilt gebruiken (zoals Afbeeldingssets of Staalsets), kunt u de creatie ervan automatiseren met een conventie die voorspelbaar is voor de naamgeving. Eventuele scriptmethoden, zoals een voorinstelling Batch-set, waarover u in een afzonderlijk gedeelte van deze zelfstudie leert, kunnen worden uitgeschakeld met een naamgevingsconventie.

De andere methode is om uw sets handmatig te maken. Hoewel het handmatig maken van afbeeldingssets voor 200 afbeeldingen wellicht geen grote taak is, kunt u zich voorstellen dat u meer dan 100.000 afbeeldingen hebt. Dit is wanneer de vastgestelde schepingsautomatisering cruciaal wordt.
