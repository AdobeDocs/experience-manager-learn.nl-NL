---
title: Video-overzicht
description: Dynamic Media Classic wordt geleverd met automatische conversie van video tijdens het uploaden, videostreaming naar desktop- en mobiele apparaten en adaptieve videosets die zijn geoptimaliseerd voor afspelen op basis van apparaat en bandbreedte. Meer informatie over video in Dynamic Media Classic en meer informatie over videoconcepten en terminologie. Daarna leert u diep hoe u video kunt uploaden en coderen, videovoorinstellingen kunt kiezen voor het uploaden, toevoegen of bewerken van een videovoorinstelling, video's voorvertonen in een videoviewer, video kunt implementeren op websites en mobiele sites, bijschriften en hoofdstukmarkeringen aan video kunt toevoegen en videoviewers kunt publiceren voor gebruikers op het bureaublad en in mobiele apparaten.
sub-product: dynamic-media
feature: Dynamic Media Classic, Video Profiles, Viewer Presets
doc-type: tutorial
topics: development, authoring, configuring, videos, video-profiles
audience: all
activity: use
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '6234'
ht-degree: 0%

---


# Video-overzicht {#video-overview}

Dynamic Media Classic wordt geleverd met automatische conversie van video tijdens het uploaden, videostreaming naar desktop- en mobiele apparaten en adaptieve videosets die zijn geoptimaliseerd voor afspelen op basis van apparaat en bandbreedte. Een van de belangrijkste dingen van video is dat de workflow eenvoudig is — zo is ontworpen dat iedereen het kan gebruiken, zelfs als ze niet erg vertrouwd zijn met videotechnologie.

Aan het einde van deze sectie van de zelfstudie leert u hoe u het volgende kunt doen:

- Video uploaden en coderen (transcoderen) naar verschillende formaten en formaten
- Maak een keuze uit de beschikbare videovoorinstellingen voor uploaden
- Een voorinstelling voor videocodering toevoegen of bewerken
- Video&#39;s voorvertonen in een videoviewer
- Video distribueren naar websites en mobiele sites
- Bijschriften en hoofdstukmarkeringen aan video toevoegen
- Videoviewers aanpassen en publiceren voor gebruikers op het bureaublad en mobiele apparaten

>[!NOTE]
>
>Alle URL&#39;s in dit hoofdstuk dienen slechts ter illustratie; het zijn geen live links .

## Overzicht van Dynamic Media Classic Video

Laten we eerst een beter idee krijgen van de mogelijkheden voor video met Dynamic Media Classic.

### Functies en mogelijkheden

Het Dynamic Media Classic-videoplatform biedt alle onderdelen van de video-oplossing: het uploaden, converteren en beheren van video&#39;s. de mogelijkheid om bijschriften en hoofdstukmarkeringen aan een video toe te voegen; en de mogelijkheid om voorinstellingen te gebruiken voor een eenvoudige weergave.

Zo kunt u eenvoudig Adaptieve video van hoge kwaliteit publiceren voor streaming op meerdere schermen, zoals desktopcomputers, iOS, Android, Blackberry en mobiele Windows-apparaten. Een adaptieve videoreeks groepeert versies van de zelfde video die bij verschillende beetjetarieven en formaten zoals 400 kbps, 800 kbps, en 1000 kbps worden gecodeerd. De desktopcomputer of het mobiele apparaat detecteert de beschikbare bandbreedte.

Bovendien wordt de videokwaliteit automatisch dynamisch geschakeld als de netwerkomstandigheden veranderen op het bureaublad of op het mobiele apparaat. Als een klant de modus Volledig scherm op een desktopcomputer inschakelt, reageert de Adaptive Video Set met een betere resolutie, waardoor de kijkervaring van de klant wordt verbeterd. Met Adaptieve videosets kunt u de best mogelijke weergave bieden voor klanten die Dynamic Media Classic-video op meerdere schermen en apparaten afspelen.

### Videobeheer

Het werken met video kan complexer zijn dan het werken met stilstaande digitale beelden. Bij video hebt u te maken met een groot aantal indelingen en standaarden en met de onzekerheid of uw publiek de clips kan afspelen. Met Dynamic Media Classic kunt u eenvoudig met video werken en beschikt u over veel krachtige gereedschappen &quot;onder de kap&quot;, maar kunt u de complexiteit van het werken met deze gereedschappen wegwerken.

Dynamic Media Classic herkent en kan werken met veel verschillende beschikbare bronindelingen. Het lezen van de video is echter slechts een deel van de moeite — u moet de video ook converteren naar een webvriendelijke indeling. Dynamic Media Classic zorgt hiervoor doordat u video kunt omzetten in H.264-video.

Het omzetten van de video zelf kan zeer ingewikkeld worden met de vele professionele en liefdesgereedschappen die beschikbaar zijn. Met Dynamic Media Classic kunt u dit eenvoudig houden door eenvoudige voorinstellingen te bieden die zijn geoptimaliseerd voor verschillende kwaliteitsinstellingen. Als u echter iets meer wilt aanpassen, kunt u ook uw eigen voorinstellingen maken.

Als u veel video hebt, zult u begrijpen dat u al uw middelen samen met uw afbeeldingen en andere media kunt beheren in Dynamic Media Classic. U kunt uw elementen, waaronder video-elementen, ordenen, catalogiseren en doorzoeken met robuuste ondersteuning voor XMP metagegevens.

### Video afspelen

Net als bij het probleem van het converteren van video om deze webvriendelijk en toegankelijk te maken, is het probleem van het implementeren en implementeren van video op uw site. U kunt kiezen of u een speler wilt aanschaffen of zelf een speler wilt maken, zodat deze compatibel is voor verschillende apparaten en schermen en u uw spelers daarna gewoon kunt onderhouden.

Ook hier is de aanpak van Dynamic Media Classic zodanig dat u de voorinstelling en viewer kunt kiezen die aan uw behoeften voldoen. U hebt veel verschillende viewerkeuzen en een bibliotheek met vele voorinstellingen beschikbaar.

U kunt gemakkelijk video leveren aan het web en mobiele apparaten, aangezien Dynamic Media Classic HTML5-video ondersteunt. Dit betekent dat u gebruikers met verschillende browsers en Android- en iOS-platformgebruikers kunt besturen. Streaming video maakt een vloeiende weergave van langere of HD-inhoud mogelijk, terwijl voor progressieve HTML5-video voorinstellingen zijn geoptimaliseerd voor een klein scherm.

Voorinstellingen voor viewers voor video kunnen gedeeltelijk worden geconfigureerd, afhankelijk van het viewertype.

Net als alle viewers verloopt de integratie via één klassieke Dynamic Media-URL per viewer of video.

>[!NOTE]
>
>U kunt het beste Dynamic Media Classic HTML5-videoviewers gebruiken. De voorinstellingen die worden gebruikt in HTML5-videoviewers zijn robuuste videospelers. Door de combinatie in één speler van de capaciteit om de playbackcomponenten te ontwerpen gebruikend HTML5 en CSS, ingebedde playback te hebben, en adaptieve en progressieve het stromen afhankelijk van de mogelijkheden van de browser te gebruiken, breidt u het bereik van uw rijke media inhoud tot de Desktop, de tablet, en mobiele gebruikers uit, en verzekert een gestroomlijnde videoervaring.

Een laatste opmerking over Dynamic Media Classic-video die op sommige klanten van toepassing kan zijn: het is mogelijk dat niet alle bedrijven automatische conversie, streaming of videovoorinstellingen hebben ingeschakeld voor hun account. Als u om een of andere reden geen toegang hebt tot de URL&#39;s voor het streamen van video, kan dit de reden zijn. U kunt progressief gedownloade video nog wel uploaden en publiceren en u hebt toegang tot alle videoviewers. Als u echter de volledige Dynamic Media Classic-videomogelijkheden wilt benutten, neemt u contact op met uw accountmanager of Verkoopmanager om deze functies in te schakelen.

Meer informatie over [Video in Dynamic Media Classic](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/quick-start-video.html).

## Video 101

### Basisvideoconcepten en -terminologie

Voordat we aan de slag gaan, bespreken we enkele termen waarmee u vertrouwd moet zijn om met video te werken. Deze concepten zijn niet specifiek voor Dynamic Media Classic. Als u video&#39;s gaat beheren voor een professionele website, zou u er goed aan doen om wat verder te leren over dit onderwerp. We raden enkele bronnen aan het einde van deze sectie aan.

- **Coderen/transcoderen.** Codering is het proces waarbij videocompressie wordt toegepast om onbewerkte, niet-gecomprimeerde videogegevens om te zetten in een indeling waarmee u gemakkelijker kunt werken. Transcodering heeft, hoewel vergelijkbaar, betrekking op de conversie van de ene coderingsmethode naar de andere.

   - Master videobestanden die met videobewerkingssoftware zijn gemaakt, zijn vaak te groot en hebben niet de juiste indeling voor levering aan onlinebestemmingen. Ze zijn doorgaans gecodeerd voor snel afspelen op het bureaublad en voor bewerking, maar niet voor weergave op het web.
   - Om digitale video om te zetten in de juiste indeling en specificaties voor afspelen op verschillende schermen, worden videobestanden getranscodeerd naar een kleinere, efficiënte bestandsgrootte die optimaal is voor weergave op het web en op mobiele apparaten.

- **Videocompressie.** Het verminderen van de hoeveelheid gegevens die wordt gebruikt om digitale videobeelden te vertegenwoordigen, en is een combinatie van ruimtelijke beeldcompressie en tijdelijke bewegingscompensatie.

   - De meeste compressietechnieken zijn verliesgevend, wat betekent dat gegevens worden weggegooid om een kleinere grootte te bereiken.
   - DV-video wordt bijvoorbeeld relatief weinig gecomprimeerd en u kunt het bronbeeldmateriaal gemakkelijk bewerken, maar het is veel te groot om op het web te gebruiken of zelfs op een dvd te zetten.

- **Bestandsindelingen.** De indeling is een container, vergelijkbaar met een ZIP-bestand, die bepaalt hoe bestanden in het videobestand worden ingedeeld, maar doorgaans niet hoe ze worden gecodeerd.

   - Veelvoorkomende bestandsindelingen voor bronvideo zijn onder andere Windows Media (WMV), QuickTime (MOV), Microsoft AVI en MPEG. De indelingen die door Dynamic Media Classic worden gepubliceerd, zijn MP4.
   - Een videobestand bevat meestal meerdere tracks (een videotrack (zonder audio) en een of meer audiotracks (zonder video) die met elkaar verweven en gesynchroniseerd zijn.
   - De videobestandsindeling bepaalt hoe deze verschillende gegevenstracks en metagegevens worden ingedeeld.

- **Codec.** Een videocodec beschrijft het algoritme waardoor een video door het gebruik van compressie wordt gecodeerd. Audio wordt ook gecodeerd via een audiocodec.

   - Met codecs minimaliseert u de hoeveelheid informatie die nodig is om video af te spelen. In plaats van informatie over elk afzonderlijk frame wordt alleen informatie over de verschillen tussen het ene frame en het volgende opgeslagen.
   - Omdat de meeste video&#39;s weinig van het ene frame naar het andere veranderen, maken codecs hoge compressiesnelheden mogelijk, wat resulteert in kleinere bestanden.
   - Een videospeler decodeert de video volgens zijn codec en geeft vervolgens een reeks beelden, of kaders, op het scherm weer.
   - Veelvoorkomende video-codecs zijn H.264, On2 VP6 en H.263.

![afbeelding](assets/video-overview/bird-video.png)

- **Resolutie.** Hoogte en breedte van de video in pixels.

   - De grootte van de bronvideo wordt bepaald door de camera en de uitvoer van de bewerkingssoftware. Een HD-camera maakt doorgaans een video met een hoge resolutie van 1920 x 1080, maar om probleemloos op het web te kunnen worden afgespeeld, kunt u de camera downsamplen (vergroten of verkleinen) naar een kleinere resolutie, zoals 1280 x 720, 640 x 480 of lager.
   - De resolutie heeft een directe invloed op de bestandsgrootte en de bandbreedte die nodig is om de video af te spelen.

- **Hoogte-breedteverhouding weergeven.** Verhouding van de breedte van een video tot de hoogte van een video. Wanneer de hoogte-breedteverhouding van de video niet overeenkomt met de verhouding van de speler, ziet u mogelijk zwarte balken of lege ruimte. De volgende twee veelvoorkomende hoogte-breedteverhoudingen worden gebruikt voor het weergeven van video:

   - 4:3 (1,33:1). Deze indeling wordt gebruikt voor bijna alle tv-inhoud met standaarddefinitie.
   - 16:9 (1,78:1). Deze indeling wordt gebruikt voor vrijwel alle HDTV-inhoud (High-Definition TV) en films voor een groot scherm.

- **Bitsnelheid/gegevenssnelheid.** De hoeveelheid gegevens die wordt gecodeerd om één seconde video af te spelen (in kilobits per seconde).

   - Over het algemeen geldt dat hoe lager de bitsnelheid, des te wenselijker deze is voor het web omdat deze sneller kan worden gedownload. Dit kan echter ook betekenen dat de kwaliteit laag is als gevolg van compressieverlies.
   - Een goede codec zou lage beetjetarief met goede kwaliteit moeten in evenwicht brengen.

- **Framesnelheid (frames per seconde of FPS).** Het aantal frames, of stilstaande beelden, voor elke seconde van de video. Noord-Amerikaanse tv (NTSC) wordt doorgaans uitgezonden in 29,97 FPS; Europese en Aziatische tv (PAL) wordt uitgezonden in 25 FPS; en film (analoog en digitaal) hebben doorgaans een FPS-indeling van 24 (23,976).

   - Om dingen verwarrender te maken, zijn er ook progressieve en geïnterlinieerde kaders. Elk progressief frame bevat een volledig afbeeldingsframe, terwijl geïnterlinieerde frames elke andere rij pixels in een afbeeldingsframe bevatten. De frames worden dan heel snel afgespeeld en lijken te overvloeien. Film gebruikt een progressieve scanmethode, terwijl digitale video doorgaans geïnterlinieerd is.
   - Over het algemeen maakt het niet uit of uw bronbeeldmateriaal geïnterlinieerd is. Dynamic Media Classic behoudt de scanmethode in de omgezette video.
   - Streaming/progressieve levering. Videostreaming is het verzenden van media in een continue stream die kan worden afgespeeld wanneer deze wordt ontvangen, terwijl progressief gedownloade video wordt gedownload zoals elk ander bestand van een server en lokaal in de browser wordt opgeslagen.

Hopelijk helpt deze primer u de verschillende opties te begrijpen die betrokken zijn bij het gebruik van Dynamic Media Classic-video.

## Videoworkflow

Wanneer u met video werkt in Dynamic Media Classic, volgt u een standaardworkflow die lijkt op het werken met afbeeldingen.

![afbeelding](assets/video-overview/video-overview-2.png)

1. Start door videobestanden te uploaden naar Dynamic Media Classic. Hiervoor opent u het **menu Extra** onder aan het deelvenster Klassieke Dynamic Media-extensie en kiest u **Uploaden naar Dynamic Media Classic > Bestanden naar mapnaam** of **Uploaden naar Dynamic Media Classic > Mappen naar mapnaam**. De map &#39;Mapnaam&#39; is de map waarin u momenteel bladert met de extensie. Videobestanden kunnen groot zijn, dus raden we u aan om FTP te gebruiken voor het uploaden van grote bestanden. Als onderdeel van het uploaden kiest u een of meer videovoorinstellingen voor het coderen van uw video&#39;s. Video kan tijdens het uploaden naar MP4-video worden getranscodeerd. Zie het onderwerp Voorinstellingen video hieronder voor meer informatie over het gebruik en het maken van coderingsvoorinstellingen. Meer informatie over [Video&#39;s uploaden en coderen](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/uploading-encoding-videos.html).
2. Selecteer of selecteer en wijzig een videovoorinstelling van de viewer en bekijk een voorvertoning van uw video. U kunt een vooraf gebouwde voorinstelling voor de viewer kiezen of uw eigen voorinstelling aanpassen. Als u zich richt op mobiele gebruikers, hoeft u hier niets te doen omdat voor mobiele platforms geen viewer of voorinstelling is vereist. Meer informatie over het [voorvertonen van video&#39;s in een video-viewer](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/previewing-videos-video-viewer.html) en [Een voorinstelling voor een video-viewer toevoegen of bewerken](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/previewing-videos-video-viewer.html#adding-or-editing-a-video-viewer-preset).
3. Voer een video-publicatie uit, krijg de URL en integreer de video. Het belangrijkste verschil tussen deze stap voor de videoworkflow en de afbeeldingsworkflow is dat u een speciale publicatie van video uitvoert in plaats van (of misschien ook) de standaardpublicatie van Image Serving. De integratie van de videoviewer op het bureaublad werkt precies zoals de integratie van de beeldviewer, maar voor mobiele apparaten is het nog eenvoudiger — u hebt alleen de URL naar de video zelf nodig.

### Transcodering

Transcodering is eerder gedefinieerd als het converteren van de ene coderingsmethode naar de andere. In het geval van Dynamic Media Classic wordt uw bronvideo van de huidige indeling naar MP4 geconverteerd. Dit is vereist voordat uw video wordt weergegeven in de desktopbrowser of op een mobiel apparaat.

Dynamic Media Classic kan alle transcodering voor u afhandelen, een groot voordeel. U kunt de video zelf transcoderen en de bestanden uploaden die al zijn geconverteerd naar MP4, maar dat kan een complex proces zijn waarvoor geavanceerde software nodig is. Tenzij u weet wat u doet, zult u typisch geen goede resultaten op uw eerste poging krijgen.

Niet alleen zet Dynamic Media Classic de bestanden voor u om, het maakt het ook eenvoudig door het aanbieden van gebruiksvriendelijke voorinstellingen. Je hoeft echt niet veel te weten over de technische kant van dit proces. Je zou alleen maar moeten weten dat je grofweg de uiteindelijke grootte(s) hebt die je uit het systeem wilt halen en een idee van de bandbreedte die je eindgebruikers hebben.

Hoewel de vooraf gebouwde voorinstellingen handig zijn en aan de meeste behoeften voldoen, wilt u soms iets meer aan uw wensen aanpassen. In dat geval kunt u uw eigen coderingsvoorinstelling maken. In Dynamic Media Classic wordt een coderingsvoorinstelling een videovoorinstelling genoemd. Dit zal later in dit hoofdstuk worden uitgelegd.

### Informatie over streaming

Een andere belangrijke functie die het vermelden waard is, is videostreaming, een standaardfunctie van het Dynamic Media Classic-videoplatform. Streaming media worden voortdurend ontvangen door en gepresenteerd aan een eindgebruiker terwijl ze worden geleverd. Dit is om een aantal redenen belangrijk en wenselijk.

Streaming vereist doorgaans minder bandbreedte dan progressief downloaden, omdat alleen het gedeelte van de video dat wordt bekeken, daadwerkelijk wordt geleverd. De klassieke Dynamic Media-videostreamingserver en -viewers gebruiken automatische bandbreedtedetectie om de best mogelijke stream te leveren voor de internetverbinding van een gebruiker.

Bij streaming begint de video eerder te spelen dan bij gebruik van andere methoden. Het maakt ook efficiënter gebruik van netwerkmiddelen omdat slechts de delen van de video die worden bekeken naar de cliënt worden verzonden.

De andere leveringsmethode is progressieve download. In vergelijking met streaming video is er slechts één consistent voordeel voor progressief downloaden: u hebt geen streamingserver nodig om de video te leveren. En dit is natuurlijk waar Dynamic Media Classic binnenkomt — Dynamic Media Classic heeft een streamingserver ingebouwd in het platform, dus je hebt niet de gedoe of extra kosten nodig om dit speciale stuk hardware te onderhouden.

De progressieve downloadvideo kan van om het even welke normale Webserver worden gediend. Hoewel dit handig en potentieel kosteneffectief kan zijn, moet u er rekening mee houden dat progressieve downloads beperkte zoek- en navigatiemogelijkheden hebben en dat gebruikers toegang hebben tot uw inhoud en deze opnieuw kunnen gebruiken. In sommige situaties, zoals playback achter zeer strikte netwerkfirewalls, kan het stromen levering worden geblokkeerd; in deze gevallen kan het wenselijk zijn terug te keren naar progressieve levering .

Progressieve download is een goede keuze voor hobbyisten of websites met lage verkeersvereisten. als ze het niet erg vinden of hun inhoud in de cache is opgeslagen op de computer van een gebruiker; als zij slechts kortere lengte video&#39;s hoeven te leveren (minder dan 10 minuten); of als hun bezoekers om een of andere reden geen streaming video kunnen ontvangen.

U moet uw video streamen als u geavanceerde functies en controle over de levering van video nodig hebt, en/of als u video moet weergeven voor een groter publiek (bijvoorbeeld honderden gelijktijdige viewers), het gebruik van de video moet bijhouden en rapporteren of de kijkers de beste interactieve afspeelervaring wilt bieden.

Tot slot als u zich zorgen maakt over het beveiligen van uw media voor problemen met intellectuele eigendom of rechtenbeheer, zorgt streaming voor een veiligere levering van video, omdat de media bij het streamen niet worden opgeslagen in de cache van de client.

## Videovoorinstellingen

Wanneer u uw video uploadt, kiest u uit een of meer voorinstellingen die de instellingen bevatten voor het converteren van uw master video naar een webvriendelijke indeling via codering. Voorinstellingen voor video bestaan uit twee voorinstellingen voor video, adaptieve videovoorinstellingen en één voorinstelling voor codering.

Zie [Beschikbare videovoorinstellingen](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#video-presets-for-encoding-video-files).

Voorinstellingen voor adaptieve video worden standaard geactiveerd, wat betekent dat deze beschikbaar zijn voor codering. Als u één enkele het Coderen Vooraf ingestelde wenst te gebruiken, zal uw beheerder het moeten activeren om het in de lijst van Video vooraf instelt te verschijnen.

Leer hoe u [Voorinstellingen voor video activeren of deactiveren](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/uploading-encoding-videos.html#activating-or-deactivating-video-encoding-presets).

U kunt kiezen uit een van de vele voorinstellingen die bij Dynamic Media Classic worden geleverd, of u kunt zelf voorinstellingen maken. er zijn echter standaard geen voorinstellingen geselecteerd voor uploaden. Met andere woorden, **als u geen videovoorinstelling selecteert bij het uploaden, wordt uw video niet omgezet en kan deze mogelijk niet worden gepubliceerd**. U kunt de video echter zelf offline converteren en deze uploaden en publiceren. Voorinstellingen voor video zijn alleen vereist als u wilt dat Dynamic Media Classic de conversie voor u uitvoert.

Bij het uploaden selecteert u een videovoorinstelling door **Video-opties** te kiezen in het deelvenster Taakopties. Vervolgens kiest u of u wilt coderen voor Computer, Mobiel of Tablet.

- Computer is voor desktopgebruik. Hier vindt u doorgaans grotere voorinstellingen (zoals HD) die meer bandbreedte verbruiken.
- Mobiele apparaten en tablets maken MP4-video voor apparaten zoals iPhones en Android-smartphones. Het enige verschil tussen Mobile en Tablet is dat de tabletvoorinstellingen doorgaans een hogere bandbreedte hebben, omdat ze zijn gebaseerd op het WiFi-gebruik. Mobiele voorinstellingen zijn geoptimaliseerd voor trager 3G-gebruik.

### Vragen om uzelf te vragen voordat u een voorinstelling kiest

Wanneer u een voorinstelling kiest, moet u zowel het publiek als het bronbeeldmateriaal kennen. Wat weet u van uw klant? Hoe kijken ze naar de video — met een computermonitor of een mobiel apparaat?

Welke resolutie is uw video? Als u een voorinstelling kiest die groter is dan het origineel, krijgt u mogelijk een wazige of gepixeleerde video. De optie is alleen beschikbaar als uw video groter is dan de voorinstelling, maar u geen voorinstelling kiest die groter is dan uw bronvideo.

Wat is de hoogte-breedteverhouding? Als u zwarte balken rond de omgezette video ziet, kiest u de verkeerde hoogte-breedteverhouding. Dynamic Media Classic kan deze instellingen niet automatisch detecteren omdat het bestand eerst moet worden onderzocht voordat het kan worden geüpload.

### Onderverdeling video-opties

Met videovoorinstellingen bepaalt u hoe uw video wordt gecodeerd door deze instellingen op te geven. Als u niet vertrouwd met deze termijnen bent, te herzien gelieve het onderwerp Basis Video Concepten en Terminologie, hierboven.

![afbeelding](assets/video-overview/video-overview-3.jpg)

- **Hoogte-breedteverhouding.** Normaal 4:3 of breedbeeld16:9.
- **Grootte.** Dit is hetzelfde als de weergaveresolutie en wordt uitgedrukt in pixels. Dit is gerelateerd aan de hoogte-breedteverhouding. Bij een verhouding van 16:9 zal een video 432 x 240 pixels zijn, terwijl bij 4:3 de video 320 x 240 pixels is.
- **FPS.** Standaardframesnelheden zijn 30, 25 of 24 frames per seconde (fps), afhankelijk van de videostandaard — NTSC, PAL of Film. Deze instelling is niet van belang, omdat Dynamic Media Classic altijd dezelfde framesnelheid gebruikt als de bronvideo.
- **Format.** Dit is MP4.
- **Bandbreedte.** Dit is de gewenste verbindingssnelheid van de beoogde gebruiker. Hebben ze een snelle internetverbinding of een langzame verbinding? Werken ze meestal met desktopcomputers of mobiele apparaten? Dit houdt ook verband met resolutie (grootte), omdat hoe groter de video, hoe meer bandbreedte nodig is.

### De gegevenssnelheid of bitsnelheid voor uw video bepalen

Het berekenen van de bitsnelheid voor uw video is een van de minst begrepen factoren voor het weergeven van video op het web, maar mogelijk wel de belangrijkste omdat dit van directe invloed is op de gebruikerservaring. Als u de bitsnelheid te hoog instelt, hebt u een hoge videokwaliteit, maar slechte prestaties. Gebruikers met een tragere internetverbinding moeten wachten totdat de video tijdens het afspelen voortdurend wordt gepauzeerd. Maar als u het te laag stelt, zal de kwaliteit afnemen. In de Voorinstelling Video stelt Dynamic Media Classic een gegevensbereik voor afhankelijk van uw doelbandbreedte. Dat is een goede plek om te beginnen.

Nochtans als u het zelf wilt uitzoeken, zult u een calculator van het beetjetarief nodig hebben. Dit is een gereedschap dat veel wordt gebruikt door videoprofessionals en enthousiaste personen om te bepalen hoeveel gegevens in een bepaalde stream of mediaspeler (zoals een dvd) passen.

## Een aangepaste videovoorinstelling maken

Soms hebt u een speciale voorinstelling voor video nodig die niet overeenkomt met de instellingen van de ingebouwde coderingsvideovoorinstellingen. Dit kan gebeuren als u aangepaste video van een bepaalde grootte hebt, zoals een video die is gemaakt met 3D-animatiesoftware of een video die is bijgesneden van de oorspronkelijke grootte. Misschien wilt u experimenteren met verschillende bandbreedteinstellingen om video van hogere of lagere kwaliteit te bedienen. In elk geval moet u een aangepaste voorinstelling voor één coderingsvideo maken.

### Werkstroom videovoorinstellingen

1. Videovoorinstellingen bevinden zich onder **Setup > Application Setup > Video Presets**. Hier vindt u een lijst met alle coderingsvoorinstellingen die beschikbaar zijn voor uw bedrijf.

   - Elk streaming videoaccount heeft tientallen voorinstellingen uit het vak. Als u uw eigen aangepaste voorinstellingen maakt, worden deze ook hier weergegeven.
   - U kunt filteren op type gebruikend het drop-down menu. De voorinstellingen zijn onderverdeeld in Computer, Mobiel en Tablet.
      ![afbeelding](assets/video-overview/video-overview-4.jpg)

2. In de kolom Actief kunt u kiezen of u alle voorinstellingen tijdens het uploaden wilt weergeven, of alleen de voorinstellingen die u kiest. Als u in de VS werkt, wilt u mogelijk de Europese PAL-voorinstellingen uitschakelen en in het VK/EMEA schakelt u de NTSC-voorinstellingen uit.
3. Klik op de knop **Toevoegen** om een aangepaste voorinstelling te maken. Hiermee wordt het deelvenster Voorinstelling video toevoegen geopend. Het proces is hier vergelijkbaar met het maken van een voorinstelling voor afbeeldingen.
4. Geef de voorinstelling eerst de naam **Naam voorinstelling** om deze weer te geven in de lijst met voorinstellingen. In het bovenstaande voorbeeld is deze voorinstelling bedoeld voor video&#39;s van schermopnamen.
5. De **Beschrijving** is optioneel, maar geeft uw gebruikers knopinfo waarin het doel van deze voorinstelling wordt beschreven.
6. Het achtervoegsel **Encode File** wordt toegevoegd aan het einde van de naam van de video die u hier maakt. Onthoud dat u zowel een Master video als deze gecodeerde video zult hebben, die een derivaat van de master video is, en dat geen twee activa in de Klassiek van Dynamic Media de zelfde identiteitskaart van Activa kunnen hebben.
7. **Afspeelapparaten** waarop u de gewenste videobestandsindeling kiest (Computer, Mobiel of Tablet). Houd er rekening mee dat Mobile en Tablet dezelfde MP4-indeling hebben. Dynamic Media Classic moet alleen weten in welke categorie de voorinstelling wordt geplaatst. het theoretische verschil is echter dat voorinstellingen voor tablets doorgaans zijn bedoeld voor een snellere internetverbinding omdat alle toepassingen WiFi ondersteunen.
8. **De doelgegevensratio** moet u voor uzelf uitzoeken, maar u ziet hieronder een voorgesteld bereik in de afbeelding. U kunt de schuifregelaar ook naar de doelbandbreedte bij benadering slepen. Voor een preciezere figuur, gebruik een calculator van het beetjetarief. Het gaat om een beetje vallen en opstaan.

   ![afbeelding](assets/video-overview/video-overview-5.jpg)

9. Stel de **Verhouding** van het bronbestand in. Deze instelling is rechtstreeks gekoppeld aan de onderstaande grootte. Als u _Aangepast_ kiest, zult u zowel breedte als hoogte manueel moeten ingaan.
10. Als u een hoogte-breedteverhouding kiest, stelt u één waarde in voor **Resolutie-grootte** en Dynamic Media Classic vult de andere waarde automatisch in. Voor een aangepaste hoogte-breedteverhouding moet u beide waarden echter invullen. De grootte moet overeenkomen met de gegevenssnelheid. Als u een zeer lage gegevenssnelheid en een groot formaat instelt, verwacht u dat de kwaliteit slecht is.
11. Klik op **Opslaan** om de voorinstelling op te slaan. In tegenstelling tot elke andere voorinstelling hoeft u op dit punt geen voorinstellingen te publiceren, omdat deze alleen zijn bedoeld voor het uploaden van bestanden. Later moet u de gecodeerde video&#39;s publiceren, maar de voorinstellingen zijn alleen bedoeld voor intern gebruik door Dynamic Media Classic.
12. Als u wilt controleren of de videovoorinstelling in de uploadlijst staat, gaat u naar **Uploaden**.Kies **Taakopties** en vouwt u **Video-opties** uit. De voorinstelling wordt vermeld in de categorie voor het afspeelapparaat dat u hebt gekozen (Computer, Mobiel of Tablet).

Meer informatie over [Een videovoorinstelling toevoegen of bewerken](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/uploading-encoding-videos.html#adding-or-editing-a-video-encoding-preset).

## Bijschriften toevoegen aan uw video

In sommige gevallen is het handig om bijschriften aan uw video toe te voegen, bijvoorbeeld wanneer u de video in meerdere talen aan gebruikers moet leveren, maar u de audio niet in een andere taal wilt dupliceren of de video opnieuw in afzonderlijke talen wilt opnemen. Bovendien biedt het toevoegen van ondertitels een betere toegankelijkheid voor slechthorenden en gebruikers van ondertiteling. Met Dynamic Media Classic kunt u eenvoudig bijschriften toevoegen aan uw video&#39;s.

Leer hoe te [Bijschriften aan Video](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/adding-captions-video.html) toevoegen.

## Hoofdstukmarkeringen aan uw video toevoegen

Voor lange-formuliervideo&#39;s waarderen uw gebruikers waarschijnlijk de mogelijkheden en het gemak die worden geboden door in uw video te navigeren met hoofdstukmarkeringen. Met Dynamic Media Classic kunt u eenvoudig hoofdstukmarkeringen aan uw video toevoegen.

Leer hoe te [Voeg de Markeringen van het Hoofdstuk aan Video](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/adding-chapter-markers-video.html) toe.

## Onderwerpen over video-implementatie

### URL publiceren en kopiëren

De laatste stap in de Klassieke Dynamic Media-workflow is het publiceren van uw video-inhoud. Video heeft echter een eigen publicatietaak, genaamd Video Server publish, die u kunt vinden onder Geavanceerd.

![afbeelding](assets/video-overview/video-overview-6.jpg)

Leer hoe u [Uw video kunt publiceren](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#publishing-video).

Wanneer u een video-publicatie hebt uitgevoerd, kunt u een URL ophalen voor toegang tot uw video&#39;s en alle externe Dynamic Media Classic Viewer-voorinstellingen in een webbrowser. Nochtans als u aanpast of uw eigen Vooraf ingestelde VideoKijker creeert, zult u nog een afzonderlijke publicatie van de Server van het Beeld moeten in werking stellen.

- Leer hoe te [een URL met een Mobiele Plaats of een Website ](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#linking-a-video-url-to-a-mobile-site-or-a-website) verbinden.
- Leer hoe u [Video-viewer insluiten op een webpagina](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#embedding-the-video-viewer-on-a-web-page).

U kunt uw video ook implementeren met een externe of aangepaste ingebouwde videospeler.

Leer hoe te om [Video op te stellen die een Speler van de Video van de derde](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#deploying-video-using-a-third-party-video-player) gebruiken.

Bovendien als u ook de videoduimnagels wilt gebruiken — het beeld dat uit de video wordt gehaald — zult u ook een Server van het Beeld moeten in werking stellen publiceert. Dit is omdat het duimnagelbeeld voor de video op de Server van het Beeld verblijft, terwijl de video zelf op de Videoserver is. Videominiaturen kunnen worden gebruikt in de zoekresultaten van video en in afspeellijsten. Ze kunnen worden gebruikt als het eerste &quot;posterframe&quot; dat in de videoviewer wordt weergegeven voordat de video wordt afgespeeld.

Meer informatie over [Werken met videominiaturen](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#working-with-video-thumbnails).

### Een voorinstelling voor viewers selecteren en aanpassen

Het proces voor het selecteren en aanpassen van een voorinstelling voor viewers is precies hetzelfde als het proces voor afbeeldingen. U maakt een nieuwe voorinstelling of wijzigt een bestaande voorinstelling en slaat deze onder een andere naam op, bewerkt de voorinstelling en voert de publicatie van een afbeeldingsserver uit. Alle Viewer-voorinstellingen worden gepubliceerd naar de afbeeldingsserver en niet alleen naar voorinstellingen voor afbeeldingen. Daarom moet u een afbeelding publiceren om de nieuwe of gewijzigde voorinstellingen te kunnen zien.

>[!TIP]
>
>Voer een publicatiebewerking voor de afbeeldingsserver uit nadat de videoserver is gepubliceerd om eventuele miniatuurafbeeldingen te publiceren die aan uw video&#39;s zijn gekoppeld.

## Optimalisatie van de zoekmachine voor video

SEO (Search Engine Optimization, optimalisatie van zoekprogramma&#39;s) is het proces waarbij de zichtbaarheid van een website of webpagina in zoekprogramma&#39;s wordt verbeterd. Terwijl zoekmachines uitblinken in het verzamelen van informatie over op tekst gebaseerde inhoud, kunnen ze geen informatie over video adequaat verkrijgen tenzij deze informatie aan hen wordt verstrekt. Met Dynamic Media Classic Video SEO kunt u metagegevens gebruiken om zoekprogramma&#39;s beschrijvingen van uw video&#39;s te geven. Met de functie Video SEO kunt u video-items en Media RSS-feeds (mRSS) maken.

- **Video Sitemap**. Hiermee wordt Google precies aangegeven waar en wat de video-inhoud op een site is. Daarom kunnen video&#39;s volledig worden doorzocht op Google. Een videositemap kan bijvoorbeeld de actieve tijd en de categorieën video&#39;s opgeven.
- **mRSS-feed**. Wordt gebruikt door uitgevers van inhoud om mediabestanden naar Yahoo te sturen! Video zoeken. Google ondersteunt zowel het Video Sitemap- als Media RSS-feed (mRSS) voor het verzenden van informatie naar zoekmachines.

Wanneer u videositemaps en mRSS-feeds maakt, bepaalt u welke metagegevensvelden van videobestanden moeten worden opgenomen. Op deze manier beschrijft u uw video&#39;s aan zoekmachines zodat zoekprogramma&#39;s het verkeer naar video&#39;s op uw website nauwkeuriger kunnen sturen.

Nadat de Sitemap of feed is gemaakt, kunt u deze automatisch laten publiceren in Dynamic Media Classic, handmatig publiceren of gewoon een bestand genereren dat u later kunt bewerken. Bovendien kan Dynamic Media Classic dit bestand elke dag automatisch genereren en publiceren.

Aan het einde van het proces verzendt u het bestand of de URL naar uw zoekprogramma. Deze taak wordt uitgevoerd buiten Dynamic Media Classic; wij zullen het echter hieronder kort bespreken .

### Vereisten voor Sitemap/mRSS-bestanden

Google en andere zoekprogramma&#39;s kunnen uw bestanden alleen afwijzen als ze de juiste indeling hebben en bepaalde gegevens bevatten. Dynamic Media Classic genereert een bestand met de juiste indeling. als de informatie echter niet beschikbaar is voor sommige van uw video&#39;s, worden deze niet opgenomen in het bestand.

De vereiste velden zijn Openingspagina (de URL voor de pagina die de video weergeeft, niet de URL naar de video zelf), Titel en Beschrijving. Elke video moet een item voor deze items hebben, anders wordt deze niet opgenomen in het gegenereerde bestand. Optionele velden zijn Tags en Categorie.

Er zijn nog twee verplichte velden: de URL van de inhoud, de URL naar het video-element zelf en de miniatuur, een URL naar een miniatuurafbeelding van de video. Deze waarden worden echter automatisch door Dynamic Media Classic ingevuld.

De aanbevolen workflow bestaat uit het insluiten van deze gegevens in uw video&#39;s voordat ze worden geüpload met XMP metagegevens. Dynamic Media Classic haalt deze gegevens uit tijdens het uploaden. Met een toepassing als Adobe Bridge, die bij alle Adobe Creative Cloud-toepassingen wordt geleverd, kunt u de gegevens invullen in standaardmetagegevensvelden.

Als u deze methode volgt, hoeft u deze gegevens niet handmatig in te voeren met Dynamic Media Classic. U kunt echter ook Metagegevensvoorinstellingen gebruiken in Dynamic Media Classic als een snelle manier om dezelfde gegevens telkens in te voeren.

Zie [Metagegevens weergeven, toevoegen en exporteren](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html) voor meer informatie over dat onderwerp.

![afbeelding](assets/video-overview/video-overview-7.jpg)

Nadat de metagegevens zijn gevuld, kunt u deze weergeven in de Gedetailleerde weergave voor het desbetreffende video-element. Er kunnen ook trefwoorden aanwezig zijn, maar deze bevinden zich op het tabblad Trefwoorden.

- Meer informatie over [Trefwoorden toevoegen](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html#add-or-edit-keywords).
- Meer informatie over [Video SEO](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html).
- Meer informatie over [Instellingen voor video-SEO](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html#choosing-video-seo-settings).

#### Video-SEO instellen

Als u Video-SEO instelt, begint u met het kiezen van het gewenste type indeling, de generatiemethode en de metagegevensvelden die u in het bestand wilt opnemen.

1. Ga naar **Setup > Application Setup > Video SEO > Settings**.
2. Kies in het menu **Generatiemodus** een bestandsindeling. De standaardwaarde is Uit, zodat om het in te schakelen, kiest u Video Sitemap, mRSS of Beide.
3. Kies of u automatisch of handmatig wilt genereren. Voor eenvoud, adviseren wij u het aan **Automatische Wijze** plaatsen. Als u Automatisch kiest, stelt u ook de optie **Markeren voor publiceren** in, anders gaan de bestanden niet live. De Sitemap- en RSS-bestanden zijn typen in een XML-document en moeten net als andere elementen worden gepubliceerd. Gebruik een van de modi Handmatig als u niet alle informatie nu klaar hebt of slechts een eenmalige generatie wilt uitvoeren.
4. Vul de metagegevenstags in die in de bestanden worden gebruikt. Deze stap is niet optioneel. U dient minimaal de drie velden met een sterretje (\*) op te nemen: **Landing Page**, **Title** en **Description**. Als u de metagegevens voor deze taken wilt gebruiken, sleept u de velden vanuit het deelvenster Metagegevens aan de rechterkant naar een bijbehorend veld op het formulier. Dynamic Media Classic vult het veld voor plaatsaanduidingen automatisch met de feitelijke gegevens van elke video. U hoeft geen metagegevensvelden te gebruiken. In plaats daarvan kunt u hier statische tekst typen, maar voor elke video wordt dezelfde tekst weergegeven.
5. Nadat u informatie hebt ingevoerd in de drie vereiste velden, schakelt Dynamic Media Classic de knoppen **Opslaan** en **Opslaan en genereren** in. Klik op een pictogram om de instellingen op te slaan. Gebruik **Opslaan** als u in de automatische modus werkt en u wilt dat Dynamic Media Classic de bestanden later kan genereren. Gebruik **Opslaan en genereren** om het bestand direct te maken.

### Uw videositemap, mRSS-feed of beide bestanden testen en publiceren

Gegenereerde bestanden worden weergegeven in de basismap van uw account.

![afbeelding](assets/video-overview/video-overview-8.jpg)

Deze bestanden moeten worden gepubliceerd, omdat het gereedschap Video-SEO niet zelfstandig een publicatie kan uitvoeren. Zolang ze zijn gemarkeerd voor publicatie, worden ze de volgende keer dat een publicatie wordt uitgevoerd naar de publicatieservers verzonden.

Na publicatie zijn uw bestanden beschikbaar in deze URL-indeling.

![afbeelding](assets/video-overview/video-url-format.png)

Voorbeeld:

![afbeelding](assets/video-overview/video-format-example.png)

### Verzenden naar zoekmachines

De laatste stap van het proces is het verzenden van uw bestanden en/of URL&#39;s naar zoekprogramma&#39;s. Dynamic Media Classic kan deze stap niet voor u uitvoeren. als u echter de URL verzendt en niet het XML-bestand zelf, moet uw feed worden bijgewerkt wanneer het bestand de volgende keer wordt gegenereerd en er een publicatie plaatsvindt.

De methode voor het verzenden naar uw zoekmachine verschilt, maar voor Google gebruikt u Google Webmaster Tools. Als dat het geval is, gaat u naar **Site-configuratie > Sitemaps** en klikt u op **Sitemap** verzenden. Hier kunt u de klassieke Dynamic Media-URL plaatsen naar uw SEO-bestand(en).

### Video SEO-rapport

Dynamic Media Classic levert een rapport waarin u kunt zien hoeveel video&#39;s succesvol zijn opgenomen in de bestanden, en belangrijker nog, die niet zijn opgenomen als gevolg van fouten. Ga naar **Setup > Application Setup > Video SEO > Report** om het rapport te openen.

![afbeelding](assets/video-overview/video-overview-9.jpg)

## Mobiele implementatie voor MP4-video

Dynamic Media Classic bevat geen viewervoorinstellingen voor mobiele apparaten, omdat viewers video niet hoeven af te spelen op ondersteunde mobiele apparaten. Zolang u codeert naar de H.264 MP4-indeling (door het uploaden te activeren of door het vooraf coderen op uw bureaublad), kunnen ondersteunde tablets en smartphones uw video&#39;s afspelen zonder dat u een viewer nodig hebt. Dit wordt ondersteund op Android- en iOS-apparaten (iPhone en iPad).

Er is geen viewer vereist omdat beide platforms native H.264-ondersteuning hebben. U kunt de video insluiten in een HTML5-webpagina of de video insluiten in de toepassing zelf. De besturingssystemen Android en iOS bieden een controller voor het afspelen van de video.

Daarom geeft Dynamic Media Classic u geen URL naar een viewer voor mobiele apparaten, maar geeft u rechtstreeks een URL naar de video. In het voorvertoningsvenster voor een MP4-video zijn er koppelingen beschikbaar voor Desktop en Mobile. De URL voor mobiele apparaten verwijst naar de gepubliceerde video.

Een belangrijk aspect van gepubliceerde video is dat in de URL het volledige pad naar de video wordt vermeld en niet alleen de element-id. Wanneer u werkt met afbeeldingen, roept u de afbeelding aan met de element-id, ongeacht de mapstructuur. Voor video moet u echter ook de mapstructuur opgeven. In de bovenstaande URL&#39;s wordt de video opgeslagen in het pad:

![afbeelding](assets/video-overview/mobile-implement-1.png)

Dit kan ook worden uitgedrukt als bedrijfsnaam/mappad/naam van video.

### Methode #1: Afspelen browser — HTML5-code

Gebruik de HTML5-videotag om uw MP4-video in te sluiten in een webpagina.

![afbeelding](assets/video-overview/browser-playback.png)

Deze methode werkt ook voor het web op het bureaublad, maar u kunt problemen ondervinden met browserondersteuning. Niet alle webbrowsers op het bureaublad bieden native ondersteuning voor H.264-video, inclusief Firefox.

### Methode 2: App Playback op iOS — Media Player Framework

U kunt ook de Dynamic Media Classic MP4-video insluiten in uw mobiele toepassingscode. Hier volgt een algemeen voorbeeld voor iOS waarbij het Media Player-framework wordt gebruikt dat alleen ter illustratie wordt gegeven:

![afbeelding](assets/video-overview/app-playback.png)

## Aanvullende bronnen

Bekijk de [Dynamic Media Skill Builder: Video in Dynamic Media Classic](https://seminars.adobeconnect.com/p2ueiaswkuze) on-demand webinar voor meer informatie over het gebruik van de videofuncties in Dynamic Media Classic.
