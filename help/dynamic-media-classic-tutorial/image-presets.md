---
title: Voorinstellingen afbeelding
description: Voorinstellingen voor afbeeldingen in Dynamic Media Classic bevatten alle instellingen die nodig zijn om een afbeelding met een bepaalde grootte, indeling, kwaliteit en verscherping te maken. Voorinstellingen voor afbeeldingen vormen een belangrijk onderdeel van het dynamische formaat. Als u in Dynamic Media Classic naar een URL kijkt, kunt u gemakkelijk zien of een voorinstelling voor afbeeldingen in gebruik is. Meer informatie over voorinstellingen voor afbeeldingen, waarom ze zo nuttig zijn en hoe u ze kunt maken.
feature: Dynamic Media Classic, Image Presets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: e472db7c-ac3f-4f66-85af-5a4c68ba609e
duration: 127
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '648'
ht-degree: 0%

---

# Voorinstellingen afbeelding {#image-presets}

Een voorinstelling voor afbeeldingen is in feite een recept dat alle instellingen bevat die nodig zijn om een afbeelding te maken met een bepaalde grootte, indeling, kwaliteit en verscherping. Voorinstellingen voor afbeeldingen vormen een belangrijk onderdeel van het dynamische formaat.

Als u de URL&#39;s van ongeveer een Dynamic Media Classic-klant bekijkt, wordt waarschijnlijk een voorinstelling voor afbeeldingen in gebruik. Zoek alleen $name$ aan het einde van de URL (met woorden of woorden die de naam vervangen).

Met voorinstellingen voor afbeeldingen wordt de URL verkort. In plaats van meerdere instructies voor het leveren van afbeeldingen per aanvraag te schrijven, kunt u dus één voorinstelling voor afbeeldingen schrijven. Deze twee URL&#39;s produceren bijvoorbeeld dezelfde afbeelding van 300 x 300 JPEG met verscherping, maar de tweede URL gebruikt een voorinstelling voor afbeeldingen:

![afbeelding](assets/image-presets/image-preset-2.png)

De ware waarde van Beeld vooraf instelt is dat om het even welke Beheerder van het Bedrijf de definitie van dat Vooraf ingesteld Beeld kan bijwerken en elk beeld dat dat formaat gebruikt beïnvloeden — zonder enige Webcode te veranderen. U zult de resultaten van om het even welke verandering in een Vooraf ingesteld Beeld na het geheime voorgeheugen voor URL ontruimt zien.

>[!IMPORTANT]
>
>Wanneer u een afbeelding vergroot of verkleint, moet u de verhouding tussen de breedte en hoogte van de afbeelding altijd proportioneel houden, zodat de afbeelding niet wordt vervormd.

Een voorinstelling voor afbeelding heeft een dollarteken ($) aan weerszijden van de naam en volgt het vraagteken (?) scheidingsteken.

>[!TIP]
>
>Maak één voorinstelling voor afbeeldingen per unieke afbeeldingsgrootte op uw site. Als u bijvoorbeeld een afbeelding van 350 x 350 nodig hebt voor de pagina met productdetails, een afbeelding van 120 x 120 voor de pagina&#39;s Bladeren/Zoeken en een afbeelding van 90 x 90 voor een cross-sell/aanbevolen item, hebt u drie voorinstellingen voor afbeeldingen nodig, of het nu gaat om 500 of 500.000 afbeeldingen.

- Meer informatie over [Voorinstellingen afbeelding](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html).
- Leer hoe u [Een voorinstelling voor afbeeldingen maken](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset).

## Voorinstellingen voor afbeeldingen en Verscherpen

Voorinstellingen voor afbeeldingen vergroten/verkleinen doorgaans de grootte van een afbeelding. Als u de oorspronkelijke grootte van een afbeelding wijzigt, moet u de afbeelding verscherpen toevoegen. Dat komt omdat bij het vergroten of verkleinen veel pixels worden samengevoegd en in een kleinere ruimte overvloeien, waardoor de afbeelding er zacht en wazig uitziet. Met verscherpen wordt het contrast van randen en gebieden met veel contrast in een afbeelding vergroot.

We verwachten dat de afbeeldingen met hoge resolutie die u in Dynamic Media Classic uploadt, niet hoeven te worden verscherpt wanneer ze op volledige grootte worden weergegeven — wanneer u hierop inzoomt. Bij elke kleinere grootte is enige verscherping echter meestal gewenst.

>[!TIP]
>
>Altijd verscherpen wanneer het formaat van afbeeldingen wordt gewijzigd! Dat betekent dat u verscherping aan elke Voorinstelling Afbeelding (en Voorinstelling Viewer, die we later zullen bespreken) moet toevoegen.
>
>Als uw afbeeldingen er niet goed uitzien, is de kans groot dat ze moeten worden verscherpt of dat de kwaliteit misschien slecht was om mee te beginnen.

Hoeveel verscherping er moet worden toegevoegd is volledig subjectief. Sommige mensen houden van zachtere beelden, terwijl anderen van hen zeer scherp houden. U kunt een afbeelding eenvoudig verbeteren door een combinatie van verscherpingsfilters op een afbeelding uit te voeren. Het is echter ook gemakkelijk om een afbeelding overboord te zetten en te verscherpen.

In de volgende afbeelding ziet u drie verscherpingsniveaus. Van rechts naar links is er geen verscherping, alleen niet de juiste hoeveelheid en te veel.

![afbeelding](assets/image-presets/image-presets-1.jpg)

In Dynamic Media Classic kunt u drie typen verscherpen: Eenvoudig verscherpen, Nieuwe pixels berekenen en Onscherp masker.

Meer informatie over [Opties voor Dynamic Media Classic verscherpen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image).

## Aanvullende bronnen

[Handleiding voor voorinstellingen afbeelding](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf). Instellingen voor optimalisatie voor afbeeldingskwaliteit en laadsnelheid.

[Afbeelding is Alles Deel 2: Het is nooit alleen maar een vervaging — Kwaliteit versus snelheid](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). Een blogbericht waarin het gebruik van Voorinstellingen voor afbeeldingen wordt besproken voor het aanbieden van afbeeldingen van hoge kwaliteit die snel worden geladen.
