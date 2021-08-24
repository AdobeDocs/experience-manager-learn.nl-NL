---
title: Dynamic Media Classic Main Workflow en Previewing Assets
description: Leer meer over de hoofdworkflow in Dynamic Media Classic, die de drie stappen - Maken (en uploaden), Auteur (en Publiceren) en Leveren bevat. Leer vervolgens hoe u een voorvertoning van elementen kunt weergeven in Dynamic Media Classic.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, architecture, publishing
audience: all
activity: use
topic: Inhoudsbeheer
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '2714'
ht-degree: 0%

---


# Dynamic Media Classic Main Workflow en Previewing Assets {#main-workflow}

Dynamic Media ondersteunt het workflowproces Maken (en uploaden), Auteur (en Publiceren) en Leveren. U begint door activa te uploaden, dan iets met die activa te doen zoals het bouwen van een Reeks van het Beeld, en tenslotte te publiceren om hen levend te maken. De stap Build is optioneel voor bepaalde workflows. Als het bijvoorbeeld uw doel is om alleen dynamisch in te zoomen op afbeeldingen of video om te zetten en te publiceren voor streaming, zijn er geen benodigde stappen voor het maken van een build.

![afbeelding](assets/main-workflow/create-author-deliver.jpg)

De workflow in Dynamic Media Classic-oplossingen bestaat uit drie hoofdstappen:

1. SourceContent maken (en uploaden)
2. Author (and Publish) Assets
3. Elementen leveren

## Stap 1: Maken (en uploaden)

Dit is het begin van de workflow. In deze stap verzamelt of maakt u de broninhoud die past in de workflow die u gebruikt en uploadt u deze naar Dynamic Media Classic. Het systeem ondersteunt meerdere bestandstypen voor afbeeldingen, video en lettertypen, maar ook voor PDF, Adobe Illustrator en Adobe InDesign.

Zie de volledige lijst met [Ondersteunde bestandstypen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#supported-asset-file-formats).

U kunt broninhoud op verschillende manieren uploaden:

- Direct vanaf uw bureaublad of lokaal netwerk. [Leer hoe](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-sps-desktop-application).
- Van een Classic FTP-server van Dynamic Media. [Leer hoe](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-via-ftp).

De standaardmodus is Van bureaublad, waar u naar bestanden op uw lokale netwerk bladert en het uploaden start.

![afbeelding](assets/main-workflow/upload.jpg)

>[!TIP]
>
>Voeg niet handmatig uw mappen toe. Voer in plaats daarvan een upload uit via FTP en gebruik de optie **Submappen opnemen** om de mapstructuur opnieuw te maken in Dynamic Media Classic.

De twee belangrijkste uploadopties worden toegelaten door gebrek - **Teken voor Publish**, die wij eerder hebben besproken, en **Overschrijf**. Overschrijven betekent dat als het bestand dat wordt geüpload dezelfde naam heeft als een bestand dat al in het systeem staat, het nieuwe bestand de bestaande versie vervangt. Als u deze optie uitschakelt, wordt het bestand mogelijk niet geüpload.

### Opties voor overschrijven bij uploaden van afbeeldingen

Er zijn vier variaties van de optie Afbeelding overschrijven die u voor het hele bedrijf kunt instellen. Deze variaties worden vaak verkeerd begrepen. Kortom, u stelt de regels zodanig in dat elementen met dezelfde naam vaker moeten worden overschreven of dat overschrijvingen minder vaak moeten plaatsvinden (in dat geval krijgt de nieuwe afbeelding een andere naam met de extensie &quot;-1&quot; of &quot;-2&quot;).

- **Overschrijf in de huidige map dezelfde naam/extensie** voor de basisafbeelding.
Deze optie is de strengste regel voor vervanging. Hiervoor moet u de vervangende afbeelding uploaden naar dezelfde map als het origineel en moet de vervangende afbeelding dezelfde bestandsnaamextensie hebben als het origineel. Als niet aan deze vereisten wordt voldaan, wordt een dubbel gecreeerd.

- **Overschrijven in huidige map, dezelfde naam van basiselement, ongeacht de extensie**.
U moet de vervangende afbeelding uploaden naar dezelfde map als het origineel, maar de bestandsnaamextensie kan afwijken van het origineel. bijvoorbeeld stoel.tif vervangt stoel.jpg.

- **Overschrijf in een willekeurige map dezelfde naam/extensie** voor basiselementen.
Vereist dat de vervangende afbeelding dezelfde bestandsnaamextensie heeft als de oorspronkelijke afbeelding (bijvoorbeeld stoel.jpg moet de naam stoel.jpg vervangen, niet stoel.tif ). U kunt de vervangende afbeelding echter naar een andere map uploaden dan het origineel. De bijgewerkte afbeelding staat in de nieuwe map; het bestand kan niet meer op de oorspronkelijke locatie worden gevonden.

- **Overschrijf in elke map dezelfde naam voor basiselementen, ongeacht de extensie**.
Deze optie is de meest inclusieve vervangingsregel. U kunt een vervangende afbeelding uploaden naar een andere map dan het origineel, een bestand met een andere bestandsnaamextensie uploaden en het oorspronkelijke bestand vervangen. Als het oorspronkelijke bestand zich in een andere map bevindt, bevindt de vervangende afbeelding zich in de nieuwe map waarnaar het is geüpload.

Meer informatie over [Afbeeldingen overschrijven Option](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#using-the-overwrite-images-option).

Hoewel dit niet verplicht is, kunt u tijdens het uploaden met een van de twee bovenstaande methoden taakopties voor die specifieke upload opgeven, bijvoorbeeld om een terugkerende upload te plannen, opties voor uitsnijden bij het uploaden instellen en vele andere. Deze kunnen voor sommige werkschema&#39;s waardevol zijn, zodat is het de moeite waard om te overwegen of zij voor van u kunnen zijn.

Meer informatie over [Taakopties](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-options).

Uploaden is de eerste noodzakelijke stap in een workflow, omdat Dynamic Media Classic niet kan werken met inhoud die zich nog niet in het systeem bevindt. Achter de schermen tijdens het uploaden registreert het systeem elk geüpload element met de gecentraliseerde Dynamic Media Classic-database, wijst het een id toe en kopieert het naar opslag. Bovendien zet het systeem afbeeldingsbestanden om in een indeling waarmee u op dynamische wijze kunt vergroten/verkleinen en zoomen en zet het videobestanden om in de webvriendelijke indeling van MP4.

### Concept: Dit is wat er gebeurt met afbeeldingen wanneer u deze uploadt naar Dynamic Media Classic

Wanneer u een afbeelding van een willekeurig type uploadt naar Dynamic Media Classic, wordt deze omgezet naar een master afbeeldingsindeling die een Piramid TIFF of P-TIFF wordt genoemd. Een P-TIFF is vergelijkbaar met de indeling van een gelaagde TIFF-bitmapafbeelding, behalve dat het bestand in plaats van verschillende lagen meerdere formaten (resoluties) van dezelfde afbeelding bevat.

![afbeelding](assets/main-workflow/pyramid-p-tiff.png)

Tijdens het omzetten van de afbeelding maakt Dynamic Media Classic gebruik van een &#39;momentopname&#39; van de volledige grootte van de afbeelding, schaalt deze met de helft en slaat deze op, schaalt deze nogmaals met de helft en slaat deze op, enzovoort, totdat deze wordt gevuld met even veelvouden van de oorspronkelijke grootte. Een P-TIFF van 2000 pixels beschikt bijvoorbeeld over een grootte van 1000, 500, 250 en 125 pixels (en kleiner) in hetzelfde bestand. Het P-TIFF-bestand is de indeling van een zogenaamde &quot;master afbeelding&quot; in Dynamic Media Classic.

Wanneer u om een bepaalde groottebeeld verzoekt, staat het creëren van P-TIFF de Server van het Beeld voor de Klassiek van Dynamic Media toe om de volgende grotere grootte snel te vinden en het neer te schrapen. Als u bijvoorbeeld een afbeelding van 2000 pixels uploadt en een afbeelding van 100 pixels aanvraagt, vindt Dynamic Media Classic de versie van 125 pixels en schaalt deze naar 100 pixels in plaats van te schalen van 2000 naar 100 pixels. Dit maakt de bewerking zeer snel. Wanneer u op een afbeelding zoomt, kan de zoomviewer bovendien alleen een tegel van de afbeelding waarop wordt ingezoomd aanvragen in plaats van de volledige afbeelding met volledige resolutie. Zo ondersteunt de master afbeeldingsindeling, het P-TIFF-bestand, zowel dynamische vergroting als vergroting.

Op dezelfde manier kunt u uw master bronvideo uploaden naar Dynamic Media Classic, en bij het uploaden kunt u de grootte van de video automatisch aanpassen en deze converteren naar de webvriendelijke indeling MP4.

### Miniatuurregels voor het bepalen van de optimale grootte voor de afbeeldingen die u uploadt

**Upload afbeeldingen van het grootste formaat dat u nodig hebt.**

- Als u wilt zoomen, uploadt u een afbeelding met hoge resolutie van 1500-2500 pixels in de langste dimensie. Houd rekening met de details die u wilt weergeven, de kwaliteit van de bronafbeeldingen en de grootte van het product dat wordt weergegeven. U kunt bijvoorbeeld een afbeelding van 1000 pixels uploaden voor een kleine ring, maar een afbeelding van 3000 pixels voor een hele ruimtekencène.
- Als u niet hoeft in te zoomen, uploadt u het bestand met het exacte formaat dat het wordt weergegeven. Als u bijvoorbeeld logo&#39;s of splash- en bannerafbeeldingen op uw pagina&#39;s wilt plaatsen, uploadt u deze precies bij de grootte 1:1 en roept u ze precies bij die grootte aan.

**Upsample uw afbeeldingen nooit of blaas ze op voordat u ze uploadt naar Dynamic Media Classic.** U moet bijvoorbeeld geen kleinere afbeelding upsamplen om er een afbeelding van 2000 pixels van te maken. Het zal er niet goed uitzien. Maak uw afbeeldingen zo perfect mogelijk voordat u gaat uploaden.

**Er is geen minimumgrootte voor zoomen, maar de viewers zoomen standaard niet verder dan 100%.** Als de afbeelding te klein is, wordt er niet op ingezoomd of wordt er slechts in een kleine hoeveelheid ingezoomd om te voorkomen dat de afbeelding er slecht uitziet.

**Hoewel er geen minimale afbeeldingsgrootte is, raden we u niet aan gigantische afbeeldingen te uploaden.** Een gigantische afbeelding kan worden beschouwd als meer dan 4000 pixels. Het uploaden van afbeeldingen van deze grootte kan mogelijke onvolkomenheden zoals stofkorrels of haren in de afbeelding laten zien. Dergelijke afbeeldingen nemen ook meer ruimte in beslag op de Dynamic Media Classic-server, waardoor u uw contractueel vastgelegde opslaglimiet kunt overschrijden.

Meer informatie over [Bestanden uploaden](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#uploading-your-files).

## Stap 2: Auteur (en publiceren)

Nadat u de inhoud hebt gemaakt en geüpload, gaat u nieuwe rich media-elementen van uw geüploade elementen maken door een of meer subworkflows uit te voeren. Dit omvat alle verschillende types van vastgestelde inzamelingen — Beeld, Monster, Spin, en Gemengde Reeksen van Media, evenals Malplaatjes. Het omvat ook video. We gaan later veel meer details bekijken over elk type verzameling afbeeldingen en video-rijke media. In bijna alle gevallen begint u echter met het selecteren van een of meer elementen (of hebt u geen elementen geselecteerd) en het kiezen van het type element dat u wilt maken. U kunt bijvoorbeeld een hoofdafbeelding en een paar weergaven van die afbeelding selecteren en een set afbeeldingen samenstellen, een verzameling alternatieve weergaven van hetzelfde product.

>[!IMPORTANT]
>
>Zorg ervoor dat al uw elementen zijn gemarkeerd voor publicatie. Hoewel standaard alle elementen automatisch worden gemarkeerd voor publicatie tijdens het uploaden, moeten nieuw geschreven elementen uit uw geüploade inhoud ook worden gemarkeerd voor publicatie.

Nadat u het nieuwe element hebt gemaakt, voert u een publicatietaak uit. U kunt dat handmatig doen of een publicatietaak plannen die automatisch wordt uitgevoerd. Door te publiceren wordt alle inhoud van de privésfeer, Dynamic Media Classic, gekopieerd naar het publiek, publiceert u de serversfeer van de vergelijking. Het product van een Dynamic Media-publicatietaak is een unieke URL voor elk gepubliceerd element.

De server waarnaar u publiceert, is afhankelijk van het type inhoud en workflow. Bijvoorbeeld, gaan alle beelden naar de Server van het Beeld en het stromen video aan de Server FMS. Voor het gemak zullen we het hebben over een &quot;publicatie&quot; als één gebeurtenis op één server.

Bij het publiceren wordt alle inhoud gepubliceerd die is gemarkeerd voor publicatie, niet alleen uw inhoud. Eén beheerder publiceert doorgaans namens iedereen in plaats van individuele gebruikers die een publicatie uitvoeren. De beheerder kan indien nodig publiceren of een terugkerende dagelijkse, wekelijkse of zelfs elke 10 minuten taak instellen die automatisch wordt gepubliceerd. Publiceer volgens een schema dat voor uw zaken zinvol is.

>[!TIP]
>
>Automatiseer uw publicatietaken en plant een Volledige publicatie die elke dag om 12.00 uur of om het even welke tijd laat in de avond wordt uitgevoerd.

### Concept: De klassieke Dynamic Media-URL begrijpen

Het uiteindelijke product van een Klassieke Dynamic Media-workflow is een URL die naar het element verwijst (of het nu gaat om een afbeeldingsset of een adaptieve videoset). Deze URL&#39;s zijn zeer voorspelbaar en volgen hetzelfde patroon. In het geval van afbeeldingen wordt elke afbeelding gegenereerd op basis van de master P-TIFF-afbeelding.

Hier volgt de syntaxis voor de URL van een afbeelding met een aantal voorbeelden:

![afbeelding](assets/main-workflow/dmc-url.jpg)

In de URL is alles links van het vraagteken het virtuele pad naar een specifieke afbeelding. Alles rechts van het vraagteken is een bepaling van de Server van het Beeld, een instructie voor hoe te om het beeld te verwerken. Wanneer u veelvoudige bepalingen hebt, worden zij gescheiden door ampersands.

In het eerste voorbeeld is het virtuele pad naar de afbeelding &quot;Backpack_A&quot; `http://sample.scene7.com/is/image/s7train/BackpackA`. De wijzigingstoetsen van de Server van het Beeld resize het beeld aan een breedte van 250 pixel (van wid=250) en resamples het beeld gebruikend het Lanczos interpolatiealgoritme, dat scherpt aangezien het resizes (van resMode=sharp2).

In het tweede voorbeeld wordt een zogenaamde &quot;voorinstelling afbeelding&quot; toegepast op dezelfde Backpack_A-afbeelding, zoals aangegeven door $!_template300$. De $-symbolen aan weerszijden van de expressie geven aan dat een voorinstelling voor een afbeelding, een set wijzigingstoetsen in pakketten, wordt toegepast op de afbeelding.

Als u eenmaal begrijpt hoe Dynamic Media Classic URL&#39;s worden samengesteld, begrijpt u hoe u deze programmatisch kunt wijzigen en hoe u ze dieper kunt integreren in uw site en back-endsystemen.

### Concept: De vertraging in cache

Nieuw geüploade en gepubliceerde elementen worden meteen zichtbaar, terwijl bijgewerkte elementen mogelijk worden vertraagd door caching binnen 10 uur. Standaard hebben alle gepubliceerde elementen minimaal 10 uur voordat ze verlopen. We zeggen een minimum, omdat elke keer dat de afbeelding wordt bekeken, deze begint met een klok die niet vervalt tot tien uur is verstreken en waarin niemand die afbeelding heeft bekeken. Deze periode van 10 uur is de &quot;Tijd om te leven&quot;voor activa. Nadat de cache voor dat element is verlopen, kan de bijgewerkte versie worden afgeleverd.

Dit is doorgaans geen probleem, tenzij er een fout is opgetreden en de afbeelding/het element dezelfde naam heeft als de eerder gepubliceerde versie, maar er is een probleem met de afbeelding. U hebt bijvoorbeeld per ongeluk een versie met een lage resolutie geüpload of uw art director heeft de afbeelding niet goedgekeurd. In dit geval wilt u de oorspronkelijke afbeelding terugroepen en deze vervangen door een nieuwe versie met dezelfde element-id.

Leer hoe te om [manueel het Geheime voorgeheugen voor URLs te ontruimen die moeten worden bijgewerkt](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/invalidate-cdn-cached-content.html).

>[!TIP]
>
>Om problemen met caching vertraging te vermijden, werk altijd vooruit — een avond, een dag, twee weken, enz. Stel uw werk in op tijd voor QA/acceptatie voor interne partijen voordat u het openbaar maakt. Zelfs als je een avond eerder werkt, kun je wijzigingen aanbrengen en die avond opnieuw publiceren. &#39;s Ochtends is de 10 uur verstreken en wordt de cache bijgewerkt met de juiste afbeelding.

- Meer informatie over [Een publicatietaak maken](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html#creating-a-publish-job).
- Meer informatie over [Publiceren](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html).

## Stap 3: Leveren

Vergeet niet dat het uiteindelijke product van een Klassieke Dynamic Media-workflow een URL is die naar het element verwijst. De URL verwijst mogelijk naar een afzonderlijke afbeelding, een Afbeeldingsset, een Spin-set of een andere verzameling of video in de Afbeeldingsset. U moet die URL gebruiken en er iets mee doen, zoals uw HTML bewerken zodat de `<IMG>`-tags verwijzen naar de klassieke Dynamic Media-afbeelding in plaats van naar een afbeelding die van uw huidige site afkomstig is.

In de stap Afleveren moet u deze URL&#39;s integreren in uw website, mobiele app, e-mailcampagne of een ander digitaal aanraakpunt waarop u het element wilt weergeven.

Voorbeeld van de integratie van de klassieke Dynamic Media-URL voor een afbeelding in een website:

![afbeelding](assets/main-workflow/example-url-1.png)

De rode URL is het enige element dat specifiek is voor Dynamic Media Classic.

Uw IT-team of integratiepartner kan het voortouw nemen bij het schrijven en wijzigen van code om Dynamic Media Classic URL&#39;s in uw site te integreren. Adobe beschikt over een consultancyteam dat u hierbij kan helpen door technische, creatieve of algemene begeleiding te bieden.

Voor complexere oplossingen, zoals zoomviewers of viewers die zoomen combineren met alternatieve weergaven, verwijst de URL doorgaans naar een viewer die wordt gehost door Dynamic Media Classic. Ook binnen die URL verwijst de URL naar een element-id.

Voorbeeld van een koppeling (in rood) waarmee een afbeeldingsset in een viewer wordt geopend in een nieuw pop-upvenster:

![afbeelding](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>U moet de klassieke Dynamic Media-URL&#39;s integreren in uw website, mobiele app, e-mail en andere digitale aanraakpunten — Dynamic Media Classic kan dat niet voor u!

## Elementen voorvertonen

U zult waarschijnlijk de activa willen voorproef u hebt geupload of creeert of uitgeeft om ervoor te zorgen zij verschijnen aangezien u wilt wanneer uw klanten hen bekijken. U kunt tot het venster van de Voorproef toegang hebben door om het even welke **knoop van de Voorproef**, of op de duimnagel van de activa, bij de bovenkant **te klikken doorbladert/bouwt Comité**, of door te gaan naar **Dossier > Voorproef**. In een browservenster wordt een voorvertoning weergegeven van het element dat zich momenteel in het deelvenster bevindt. Dit kan een afbeelding, video of samengesteld element zijn, zoals een Afbeeldingsset.

### Dynamische voorvertoning van grootte (voorinstellingen afbeelding)

U kunt een voorvertoning van uw afbeeldingen in meerdere formaten weergeven met de voorvertoning **Grootte**. Hiermee wordt een lijst geladen met de beschikbare voorinstellingen voor afbeeldingen. We bespreken de voorinstellingen voor afbeeldingen later, maar beschouwen deze als &#39;recepten&#39; voor het laden van uw afbeelding op een benoemde grootte met specifieke mate van verscherping en afbeeldingskwaliteit.

### Voorvertoning zoomen

U kunt de optie **Zoomen** ook gebruiken om een voorvertoning van uw afbeelding weer te geven in een van de vele vooraf gebouwde zoomvoorinstellingen, die zijn gebaseerd op verschillende inbegrepen zoomviewers.

Meer informatie over [Elementen voorvertonen](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/previewing-asset.html).
