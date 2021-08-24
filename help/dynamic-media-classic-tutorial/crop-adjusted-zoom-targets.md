---
title: Uitsnijden, Aangepaste afbeeldingen en Zoomdoelen
description: De master afbeelding van Dynamic Media Classic ondersteunt het maken van afzonderlijke uitgesneden versies van elke afbeelding om details weer te geven of voor stalen zonder dat u afzonderlijke uitgesneden versies van elke afbeelding hoeft te maken. Leer hoe u afbeeldingen uitsnijdt in Dynamic Media Classic en opslaat als een nieuw master bestand of een virtuele afbeelding, virtuele aangepaste afbeeldingen opslaat en deze gebruikt in plaats van master elementen, en zoomdoelen maakt in uw afbeeldingen om de gemarkeerde details weer te geven.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Inhoudsbeheer
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '2659'
ht-degree: 0%

---


# Uitsnijden, Aangepaste afbeeldingen en Zoomdoelen {#crop-adjusted-zoom-targets}

Een van de belangrijkste sterke punten van het Dynamic Media Classic-concept voor master afbeeldingen is dat u het afbeeldingselement voor veel toepassingen kunt hergebruiken. Traditioneel moet u afzonderlijke, bijgesneden versies van elke afbeelding maken om details of stalen weer te geven. Wanneer u Dynamic Media Classic gebruikt, kunt u dezelfde taken uitvoeren op uw master computer en deze bijgesneden versies opslaan als nieuwe fysieke bestanden of als virtuele derivaten die geen opslagruimte innemen.

Aan het einde van deze sectie van de zelfstudie leert u hoe u het volgende kunt doen:

- Afbeeldingen uitsnijden in Dynamic Media Classic en opslaan als nieuwe master bestanden of als virtuele afbeeldingen. [Meer](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/cropping-image.html) informatie.
- Sla virtuele aangepaste afbeeldingen op en gebruik deze in plaats van master elementen. [Meer](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/adjusting-image.html) informatie.
- Maak Zoomdoelen op uw afbeeldingen om de hooglichten van de afbeeldingen te tonen. [Meer](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html) informatie.

## Uitsnijden

Dynamic Media Classic beschikt over enkele gereedschappen voor het bewerken van afbeeldingen die gemakkelijk in de gebruikersinterface beschikbaar zijn, waaronder het gereedschap Uitsnijden. U kunt uw master afbeelding om verschillende redenen uitsnijden in Dynamic Media Classic. Bijvoorbeeld:

- U hebt geen toegang tot het oorspronkelijke bestand. U wilt de afbeelding weergeven met een andere uitsnede of hoogte-breedteverhouding, maar u hebt het originele bestand niet op uw computer of u werkt thuis. In dit geval kunt u naar Dynamic Media Classic gaan, de afbeelding zoeken, uitsnijden en opslaan of deze opslaan als een nieuwe versie.
- Te veel witruimte verwijderen. De afbeelding is gefotografeerd met te veel witruimte, waardoor het product er klein uitziet. U wilt dat uw miniatuurafbeeldingen het canvas zoveel mogelijk vullen.
- Voor het maken van aangepaste afbeeldingen nemen virtuele kopieën van afbeeldingen zonder schijfruimte in beslag. Sommige bedrijven hebben bedrijfsregels die hen vereisen om afzonderlijke exemplaren van het zelfde beeld, maar met een verschillende naam te houden. Of misschien wilt u een bijgesneden en niet-bijgesneden versie van dezelfde afbeelding.
- Nieuwe afbeeldingen maken van een bronafbeelding. U kunt bijvoorbeeld kleurstalen of een detail van de hoofdafbeelding maken. U kunt dit in Adobe Photoshop doen en het gereedschap Uitsnijden afzonderlijk uploaden of het gereedschap Uitsnijden in Dynamic Media Classic gebruiken.

>[!NOTE]
>
>Alle URL&#39;s in de volgende discussies over uitsnijden dienen slechts ter illustratie; het zijn geen live links .

### Het gereedschap Uitsnijden gebruiken

U kunt het gereedschap Uitsnijden openen in Dynamic Media Classic vanaf de pagina Details voor een element of door op de knop **Bewerken** te klikken. U kunt het gereedschap op twee manieren uitsnijden:

- De standaarduitsnijdmodus waarin u de grepen van het uitsnijdvenster of de tekstwaarden in het vak Grootte sleept. Leer hoe u [Handmatig uitsnijden](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/cropping-image.html#select-an-area-to-crop).
- Bijsnijden. Gebruik deze optie om extra witruimte rond de afbeelding te verwijderen door het aantal pixels te berekenen dat niet overeenkomt met de afbeelding. Leer hoe u [Uitsnijden door bijsnijden](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/cropping-image.html#crop-to-remove-white-space-around-an-image).

### _Handmatig uitsnijden_

Als u een handmatig bijgesneden versie opslaat, wordt de afbeelding permanent bijgesneden. Dynamic Media Classic verbergt de pixels door een interne URL-optie toe te voegen om de afbeelding bij te snijden. Wanneer u publiceert, ziet u dat de afbeelding is uitgesneden. U kunt echter terugkeren naar de Crop Editor en de uitsnijding later verwijderen.

Vervolgens kunt u kiezen of u de afbeelding wilt opslaan als een nieuwe Master afbeelding of als een aanvullende weergave van de master afbeelding. Een nieuw master bestand is een nieuw fysiek bestand (zoals een TIFF- of JPEG-bestand) dat opslagruimte in beslag neemt. Een extra weergave is een virtuele afbeelding die geen serverruimte inneemt. We raden u niet aan Origineel vervangen te kiezen, omdat dat uw master overschrijft en het uitsnijden blijvend maakt. Als u opslaat als een nieuwe master of aanvullende weergave, moet u een nieuwe element-id kiezen. Net als andere id&#39;s voor bedrijfsmiddelen moet deze een unieke naam zijn in Dynamic Media Classic.

### _Uitsnijden bijsnijden_

Als u een afbeelding uploadt met teveel witruimte (extra canvas) rond het hoofdonderwerp van de afbeelding, ziet deze er veel kleiner uit op het web wanneer u de grootte wijzigt. Dit geldt met name voor miniatuurafbeeldingen van 150 pixels of kleiner. Het onderwerp van de foto gaat mogelijk verloren in alle extra ruimte eromheen.

Vergelijk deze twee versies van dezelfde afbeelding.

![afbeelding](assets/crop-adjusted-zoom-targets/trim-cropping.jpg)

De afbeelding aan de rechterkant wordt veel prominent gemaakt door de extra ruimte rondom het product te verwijderen. Het bijsnijden kan één afbeelding tegelijk worden uitgevoerd met het gereedschap Uitsnijden of tijdens het uploaden als een batchproces worden uitgevoerd. We raden u aan om als een batchproces uit te voeren als u wilt dat al uw afbeeldingen consistent worden bijgesneden tot de grenzen van het hoofdonderwerp. Snijdt bijsnijdingen bij naar het selectiekader: de rechthoek rondom de afbeelding.

>[!NOTE]
>
>Met Verkleinen wordt er geen transparantie rond de afbeelding gemaakt. Hiervoor moet u een uitknippad insluiten in de afbeelding en de uploadoptie **Masker maken van uitknippad** gebruiken.
>
>Ook, om een beeld aan zijn originele staat te herstellen nadat u het hebt bijgesneden wanneer u **sparen** optie hebt gebruikt, toon het beeld in het scherm van de Redacteur van het Gewas en selecteer **Terugstellen** knoop.

### _Uitsnijden bij uploaden_

Zoals eerder vermeld, kunt u de afbeeldingen ook uitsnijden terwijl u ze uploadt. Als u uitsnijden bij uploaden wilt gebruiken, klikt u op de knop **Taakopties** en kiest u **Bijsnijden** onder Opties voor uitsnijden.

Dynamic Media Classic onthoudt deze optie voor de volgende upload. Hoewel u afbeeldingen voor deze upload wilt uitsnijden, wilt u ze mogelijk niet voor elke upload bijsnijden. Een andere mogelijkheid is om een speciale geplande FTP-uploadtaak in te stellen en de uitsnijdopties daar te plaatsen. Op die manier voert u de taak alleen uit als u de afbeeldingen wilt uitsnijden.

>[!IMPORTANT]
>
>Als u een uitsnijding instelt voor het uploaden, plaatst Dynamic Media Classic een cookie om die instelling de volgende keer te onthouden. Als beste praktijken, klik **Terugstellen aan BedrijfsGebreken** knoop vóór uw volgende upload om het even welke gewassenopties te ontruimen die over van laatste upload worden verlaten; anders zou u per ongeluk de volgende reeks afbeeldingen kunnen uitsnijden.

### Uitsnijden op URL

Hoewel dit niet voor de hand ligt in Dynamic Media Classic, kunt u ook puur uitsnijden via de URL (of zelfs uitsnijden toevoegen aan een voorinstelling voor afbeeldingen).

Wanneer u het gereedschap Uitsnijden gebruikt, ziet u onder in het veld URL-waarden. U kunt deze waarden als URL-wijzigingstoetsen rechtstreeks op een afbeelding toepassen.

![De opdracht ](assets/crop-adjusted-zoom-targets/cropping-by-url.png)
_imageCrop wijzigt de optietoetsen onder aan de Editor voor uitsnijden_

![afbeelding](assets/crop-adjusted-zoom-targets/uncropped-cropped.png)

Omdat de grootte per afbeelding moet worden berekend wanneer u bijsnijden door bijsnijden gebruikt, kan deze niet worden geautomatiseerd via de URL. Uitsnijden met bijsnijden kan alleen worden uitgevoerd tijdens het uploaden of door er één afbeelding tegelijk op toe te passen.

### _Uitsnijden in de voorinstelling Afbeelding_

Voorinstellingen afbeelding hebben een veld waarin u opdrachten voor het leveren van extra afbeeldingen kunt toevoegen. Als u dezelfde uitsnijding als hierboven aan uw voorinstelling voor afbeeldingen wilt toevoegen, bewerkt u de voorinstelling en plakt of typt u de waarden in het veld URL-wijzigingstoetsen en slaat u deze op en publiceert u ze.

![](assets/crop-adjusted-zoom-targets/cropping-in-image-preset.jpg)
_imageVoeg uitsnijdopdrachten (of een opdracht) toe aan de URL-wijzigingstoetsen van de voorinstelling Afbeelding._

Het uitsnijden maakt nu deel uit van de voorinstelling Afbeelding en wordt automatisch toegepast telkens wanneer het wordt gebruikt. Natuurlijk hangt deze methode af van alle afbeeldingen die dezelfde hoeveelheid uitsnijding nodig hebben. Als niet alle afbeeldingen op dezelfde manier zijn opgenomen, werkt deze methode niet voor u.

## Aangepaste afbeeldingen

Als u het gereedschap Uitsnijden gebruikt, kunt u **Opslaan als extra weergave van Master**. Als u het bestand opslaat, wordt er een nieuw soort Klassiek Dynamic Media-element gemaakt: een aangepaste afbeelding. Een aangepaste afbeelding, ook wel een derivaat genoemd, is een virtuele afbeelding. Het is eigenlijk geen beeld; het is een gegevensbestandverwijzing (als een alias of een kortere weg) aan het fysieke master beeld.

### Zal de echte afbeelding opstaan`?`

Kunt u zien welke master afbeelding is en welke aangepaste afbeelding?

![afbeelding](assets/crop-adjusted-zoom-targets/real-image-stand-up.png)

U zou niet moeten kunnen vertellen zonder Dynamic Media Klassiek te bekijken en het activatype van &quot;Aangepast Beeld&quot;voor SBR_MAIN2 te zien.

Een aangepaste afbeelding gebruikt geen schijfruimte, omdat deze alleen bestaat als een regelitem in de database. Het is ook permanent verbonden met het oorspronkelijke actief; als het origineel wordt verwijderd, wordt de aangepaste afbeelding ook verwijderd. Het kan bestaan uit een volledige, niet-uitgesneden afbeelding of slechts een deel van een afbeelding (uitsnijden).

![afbeelding](assets/crop-adjusted-zoom-targets/adjusted-image.png)

Gewoonlijk maakt u aangepaste afbeeldingen met het gereedschap Uitsnijden. deze kunnen echter ook worden gemaakt met de andere afbeeldingseditors, de gereedschappen Aanpassen en Verscherpen.

Voor aangepaste afbeeldingen hebt u een unieke element-id nodig. Wanneer ze worden gepubliceerd (u moet publiceren zoals elk ander element), fungeren ze als elke andere afbeelding en worden ze via hun element-id op een URL aangeroepen. Op de detailpagina kunt u de aangepaste afbeeldingen die zijn gekoppeld aan een master afbeelding bekijken onder het tabblad **Samengesteld &amp; derivaten**.

![](assets/crop-adjusted-zoom-targets/derivatives.jpg)
_afbeeldingAangepaste weergaven voor master afbeelding ASIAN_BR_MAIN_

## Zoomdoelen

Zoomdoelen vindt u ook op de pagina **Bewerken** en **Details** van een afbeelding. Hiermee kunt u &quot;hotspots&quot; instellen om specifieke merchandising-functies van een zoomafbeelding te markeren. In plaats van afzonderlijke afbeeldingen te maken door een groot master uit te snijden, kan de zoomviewer de details boven op de afbeelding weergeven, samen met een kort label dat u maakt.

![afbeelding](assets/crop-adjusted-zoom-targets/arm-with-watch.jpg)

Omdat de Streefcijfers van het Gezoem hoofdzakelijk een verkoopsveranderende eigenschap zijn en kennis van de verkooppunten van een product vereisen, zouden zij typisch door een persoon in het Merchandising of team van het Product bij een bedrijf worden gecreeerd.

Het proces is heel eenvoudig: klik op de functie, geef deze een beschrijvende naam en sla deze op. Doelen kunnen van de ene afbeelding naar de andere worden gekopieerd als ze op elkaar lijken, maar het proces is handmatig. In Dynamic Media Classic is het niet mogelijk om het maken van Zoomdoelen te automatiseren, omdat elke afbeelding anders is en andere functies heeft.

U kunt ook zelf bepalen of u Zoomdoelen wilt gebruiken. Niet alle viewertypen kunnen zoomdoelen weergeven (deze worden bijvoorbeeld niet ondersteund door de uitvlieger).

Leer hoe u [Zoomdoelen maken](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html#creating-and-editing-zoom-targets).

![afbeelding](assets/crop-adjusted-zoom-targets/zoom-targets.jpg)

### Zoomdoel gebruiken

Hier volgt de workflow voor het maken van doelen in Dynamic Media Classic.

1. Blader naar de afbeelding, klik op de knop **Bewerken** en kies **Zoomdoelen**.
2. De Zoomdoeleditor wordt geladen. U ziet de afbeelding in het midden, een aantal knoppen bovenaan en een leeg doeldeelvenster rechts. Linksonder ziet u een voorinstelling voor de viewer die is geselecteerd. De standaardinstelling is &#39;&#39;Zoomen1-Met instructies&#39;&#39;.
3. Verplaats het rode vak met de muis en klik om een nieuw doel te maken.

   - Het rode vak is het doelgebied. Wanneer een gebruiker op dat doel klikt, wordt ingezoomd op het gebied binnen het vak.
   - De doelgrootte wordt bepaald door de weergavegrootte binnen de voorinstelling van de viewer. Hiermee bepaalt u de grootte van de hoofdzoomafbeelding. Zie _Weergavegrootte instellen_, verderop.

4. U ziet het doel dat u net hebt gemaakt blauw worden en rechts ziet u een miniatuurversie van dat doel en de standaardnaam &quot;target-0&quot;.
5. Als u de naam van het doel wilt wijzigen, klikt u op de bijbehorende miniatuur, typt u een nieuwe **Naam** en klikt u op **Enter** of **Tab** — als u gewoon wegklikt, wordt uw naam niet opgeslagen.
6. Als het doel is geselecteerd, bevat het vak groene onderbroken lijnen en kunt u het formaat en de verplaatsing wijzigen. Sleep de hoeken om het formaat te wijzigen of sleep het doelvak om het te verplaatsen.

   - Hiermee wordt de afbeelding geladen in de standaard aangepaste zoomviewer. Zorg ervoor dat de voorinstelling van de viewer zoomdoelen ondersteunt. In het algemeen zijn alle standaardvoorinstellingen met het woord &#39;-Met instructies&#39; ontworpen voor gebruik met zoomdoelen. Als u de doelen wilt gebruiken, houdt u de muisaanwijzer boven de doelminiatuur (of het hotspot-pictogram) om het label weer te geven en klikt u erop om te zien hoe de viewer in die functie inzoomt.
   - Net als bij alle andere werkzaamheden die u in Dynamic Media Classic uitvoert, moet u uw zoomdoelen publiceren om live te gaan op het web. Als u al een viewer gebruikt die doelen ondersteunt, worden deze direct weergegeven (nadat de cache is gewist). Als u echter geen zoomdoelviewer gebruikt, blijven deze verborgen.

      ![afbeelding](assets/crop-adjusted-zoom-targets/zoom-target-green-box.jpg)

7. Als u een doel moet verwijderen, selecteert u het door op de bijbehorende miniatuur te klikken en op de knop **Doel verwijderen** te drukken of op de toets DELETE op het toetsenbord te drukken.
8. Ga door met klikken om nieuwe doelen toe te voegen, de naam te wijzigen en/of het formaat te wijzigen nadat u een doel hebt toegevoegd.
9. Wanneer u wordt gedaan, klik **sparen** knoop, en dan **Voorproef**.

### De weergavegrootte instellen in de voorinstelling Zoomviewer

Laten we even vertellen waar de grootte van de Zoomdoelen vandaan komt. In de Viewer-voorinstelling voor uw zoomviewer is er een instelling die weergavegrootte wordt genoemd. De weergavegrootte is de grootte van de zoomafbeelding in de viewer. Het is anders dan de grootte van het werkgebied. Dit is de totale grootte van uw viewer, inclusief de UI-componenten en illustraties.

Wanneer u een nieuw doel maakt, worden de grootte en de hoogte-breedteverhouding afgeleid van de weergavegrootte. Als de weergavegrootte bijvoorbeeld 200 x 200 is, kunt u alleen vierkante doelen maken met een maximaal zoomgebied van 200 pixels. Uw doelen kunnen groter zijn dan 200 pixels, maar altijd vierkant. Maar dit betekent ook dat de afbeelding in de zoomviewer slechts 200 pixels is. De grootte van het zoomdoel is direct gerelateerd aan de grootte van de viewer. U moet dus eerst het ontwerp van uw viewer bepalen voordat u doelen instelt.

Standaard is de weergavegrootte echter leeg (ingesteld op 0 x 0), omdat de grootte van de hoofdweergaveafbeelding dynamisch is en automatisch wordt afgeleid op basis van de grootte van het werkgebied. Het probleem is dat als u niet expliciet een weergavegrootte instelt in uw voorinstelling, het gereedschap Zoomdoel niet weet in welke grootte de doelen moeten worden gemaakt.

Wanneer u het gereedschap Zoomdoel laadt, wordt de weergavegrootte weergegeven naast de naam van de voorinstelling. Vergelijk de weergavegrootte tussen de ingebouwde voorinstelling Zoom1 met instructies en de aangepaste voorinstelling ZT_AUTHORING.

![afbeelding](assets/crop-adjusted-zoom-targets/view-size.jpg)

U ziet dat de ingebouwde voorinstelling een grootte van 900 x 550 heeft, wat betekent dat het doel nooit kleiner kan worden dan dat formaat. Dat is waarschijnlijk te groot. Als u een afbeelding van 2000 pixels hebt, kunt u alleen een functie uitroepen die een minimale breedte van 900 pixels heeft. De gebruiker kan handmatig verder inzoomen, maar u kunt ze niet dichterbij plaatsen. Als u een weergavegrootte instelt op 350 x 350, kunnen doelen vrij dicht inzoomen of worden vergroot of verkleind. Als u echter een grotere zoomafbeelding in uw viewer wilt, moet u een nieuwe voorinstelling maken, omdat de voorinstelling is vergrendeld op 350 pixels.

### Een voorinstelling voor een viewer maken of bewerken die zoomdoelen ondersteunt

Als u de weergavegrootte wilt instellen, maakt of bewerkt u een voorinstelling voor de viewer die Zoomdoelen ondersteunt.

1. Ga in de Voorinstelling Viewer naar de optie **Zoominstellingen**.
2. Stel een breedte en hoogte in.
3. Sla de voorinstelling op en sluit deze af. Als u die voorinstelling op uw livesite wilt gebruiken, moet u deze later ook publiceren.
4. Ga naar het gereedschap Zoomdoel en kies de voorinstelling die u linksonder hebt bewerkt. De nieuwe weergavegrootte wordt meteen weerspiegeld in uw doelen.
