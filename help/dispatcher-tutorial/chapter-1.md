---
title: Hoofdstuk 1 - Concepten, patronen en antipatronen van Dispatcher
description: In dit hoofdstuk wordt een korte inleiding gegeven over de Dispatcher-geschiedenis en -mechanica en wordt besproken hoe dit van invloed is op de manier waarop een AEM-ontwikkelaar zijn onderdelen zou ontwerpen.
feature: Dispatcher
topic: Architecture
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 3bdb6e36-4174-44b5-ba05-efbc870c3520
duration: 3855
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '17382'
ht-degree: 0%

---

# Hoofdstuk 1 - Concepten, patronen en antipatronen van Dispatcher

## Overzicht

In dit hoofdstuk wordt een korte inleiding gegeven over de Dispatcher-geschiedenis en -mechanica en wordt besproken hoe dit van invloed is op de manier waarop een AEM-ontwikkelaar zijn onderdelen zou ontwerpen.

## Waarom ontwikkelaars zich moeten bekommeren om infrastructuur

De Dispatcher is een essentieel onderdeel van de meeste, zo niet alle AEM-installaties. U vindt veel online artikelen over het configureren van de Dispatcher en over tips en trucs.

Deze stukken en stukken informatie beginnen echter altijd op een zeer technisch niveau - veronderstellend u reeds weet wat u wilt doen en zo slechts details verstrekken over hoe te om te bereiken wat u wilt. Wij hebben nooit gevonden om het even welke conceptuele teksten beschrijvend _wat is en waarom_ wanneer het op komt wat u kunt en niet met de verzender kunt doen.

### Antipatroon: Dispatcher als een afterthought

Dit gebrek aan basisinformatie leidt tot een aantal anti-patronen die we in een aantal AEM-projecten hebben gezien:

1. Aangezien Dispatcher in de server van het Web Apache wordt geïnstalleerd, is het de baan van de &quot;Unix gods&quot;in het project om het te vormen. Een &#39;mortale java-ontwikkelaar&#39; hoeft zich daar niet mee bezig te houden.

2. De Java-ontwikkelaar moet ervoor zorgen dat deze code werkt... de verzender zorgt er later magisch voor dat het snel gaat. De verzender is altijd een nagedachte. Dit werkt echter niet. Een ontwikkelaar moet zijn code met de verzender in mening ontwerpen. Hij moet zijn basisconcepten kennen om dat te doen.

### &quot;Eerst laat het werken - dan maak het snel&quot; Is niet altijd juist

U had het programmeeradvies kunnen horen _&quot;Maak eerst het werk - dan maak het snel.&quot;_. Het is niet helemaal fout. Zonder de juiste context wordt het echter vaak verkeerd geïnterpreteerd en niet correct toegepast.

Het advies moet voorkomen dat de ontwikkelaar voortijdig code optimaliseert, die misschien nooit wordt uitgevoerd - of zo zelden wordt uitgevoerd, dat een optimalisatie niet voldoende effect zou hebben om de inspanning in de optimalisering te rechtvaardigen. Bovendien zou optimalisatie kunnen leiden tot complexere code en zo insecten kunnen introduceren. Dus als je ontwikkelaar bent, besteed je niet teveel tijd aan microoptimalisering van elke regel code. Zorg gewoon dat u de juiste gegevensstructuren, algoritmen en bibliotheken hebt gekozen en wacht tot de hotspot-analyse van een analyseprogramma de algemene prestaties verhoogt door een grondiger optimalisatie te kiezen.

### Architectuurbeslissingen en artefacten

Het advies &quot;Eerst laat het werken - dan snel maken&quot; is echter volkomen verkeerd als het gaat om &quot;architecturale&quot; beslissingen. Wat zijn architectonische beslissingen? Eenvoudig gezegd, het zijn de beslissingen die duur, moeilijk en/of onmogelijk achteraf te veranderen zijn. Houd er rekening mee dat &quot;duur&quot; soms hetzelfde is als &quot;onmogelijk&quot;.  Als uw project bijvoorbeeld niet over voldoende middelen beschikt, kunnen dure wijzigingen niet worden doorgevoerd. Infrastructuurveranderingen zijn de allereerste soort veranderingen in die categorie die de meeste mensen in gedachten hebben. Maar er is ook een ander soort &#39;architectonische&#39; artefacten die erg vervelend kunnen worden om te veranderen:

1. Stukken code in het midden van een toepassing, waarop vele andere stukken vertrouwen. Als u deze opties wijzigt, moeten alle afhankelijkheden tegelijk worden gewijzigd en opnieuw worden getest.

2. Artefacten, die in één of ander asynchroon, timing-afhankelijk scenario betrokken zijn waar de input - en zo het gedrag van het systeem zeer willekeurig kan variëren. Wijzigingen kunnen onvoorspelbare effecten hebben en kunnen moeilijk te testen zijn.

3. Softwarepatronen die steeds opnieuw worden gebruikt en gebruikt in alle onderdelen en onderdelen van het systeem. Als het softwarepatroon suboptimaal blijkt te zijn, moeten alle artefacten die het patroon gebruiken opnieuw worden gecodeerd.

Herinner je? Bovenop deze pagina hebben we gezegd dat de Dispatcher een essentieel onderdeel is van een AEM-toepassing. De toegang tot een webtoepassing is zeer willekeurig - gebruikers komen en gaan op onvoorspelbare tijden. Uiteindelijk wordt (of moet) alle inhoud in de Dispatcher in het cachegeheugen opgeslagen. Dus als je goed oplette, had je je kunnen realiseren dat caching gezien kon worden als een &#39;architectonisch&#39; artefact en dus begrepen moest worden door alle leden van het team, ontwikkelaars en beheerders.

We zeggen niet dat een ontwikkelaar de Dispatcher eigenlijk moet configureren. Zij moeten de concepten kennen - vooral de grenzen - om ervoor te zorgen dat hun code ook door de Dispatcher kan worden gebruikt.

De Dispatcher verbetert de snelheid van de code niet magisch. Een ontwikkelaar moet zijn componenten met de Dispatcher in mening creëren. Daarom moet hij weten hoe het werkt.

## Dispatcher Caching - Basisbeginselen

### Dispatcher als Http in cache plaatsen - Balans laden

Wat is de Dispatcher en waarom heet het in de eerste plaats &quot;Dispatcher&quot;?

De Dispatcher is

* Eerst en vooral een cache

* Een reverse-proxy

* Een module voor de Apache httpd-webserver, die AEM-gerelateerde functies toevoegt aan de veelzijdigheid van de Apache en soepel samenwerkt met alle andere Apache-modules (zoals SSL of zelfs SSI zoals we die later zullen zien)

In de vroege dagen van het Web, zou u een paar honderd bezoekers aan een plaats verwachten. Bij de installatie van één Dispatcher &#39;verzonden&#39; of in evenwicht gebracht de lading aanvragen naar een aantal AEM-publicatieservers en die doorgaans voldoende was - dus de naam &#39;Dispatcher&#39;. Deze instelling wordt momenteel echter niet vaak meer gebruikt.

In dit artikel worden verschillende manieren weergegeven voor het instellen van Dispatcher- en Publish-systemen. Laten we beginnen met de basisbeginselen van http caching.

![ Basisfunctionaliteit van het Geheime voorgeheugen van Dispatcher ](assets/chapter-1/basic-functionality-dispatcher.png)

*Basisfunctionaliteit van het Geheime voorgeheugen van Dispatcher*

<br> 

De basisbeginselen van de verzender worden hier uitgelegd. De verzender is een eenvoudige reverse-proxy die in cache kan worden geplaatst en waarmee HTTP-aanvragen kunnen worden ontvangen en gemaakt. Een normale verzoek-/responscyclus gaat als volgt:

1. Een gebruiker vraagt een pagina aan
2. De Dispatcher controleert of er al een gerenderde versie van die pagina is. Laten we aannemen dat dit het allereerste verzoek voor deze pagina is en dat de Dispatcher geen lokale kopie in de cache kan vinden.
3. De Dispatcher vraagt de pagina aan via het publicatiesysteem
4. Op het publicatiesysteem wordt de pagina weergegeven door een JSP- of een HTML-sjabloon
5. De pagina wordt geretourneerd aan de Dispatcher
6. De Dispatcher plaatst de pagina in de cache
7. De Dispatcher geeft de pagina weer aan de browser
8. Als dezelfde pagina een tweede keer wordt opgevraagd, kan deze rechtstreeks worden aangeboden vanuit de Dispatcher-cache zonder dat deze opnieuw moet worden gerenderd op de instantie Publish. Hiermee bespaart u wachttijd voor de gebruiker en CPU cycli op de instantie Publiceren.

In de laatste sectie hadden we het over &quot;pagina&#39;s&quot;. Hetzelfde schema geldt echter ook voor andere bronnen, zoals afbeeldingen, CSS-bestanden, downloads van PDF&#39;s enzovoort.

#### Hoe gegevens in cache worden geplaatst

De Dispatcher-module maakt gebruik van de faciliteiten die de hostserver voor Apache biedt. Bronnen zoals HTML-pagina&#39;s, downloads en afbeeldingen worden als eenvoudige bestanden opgeslagen in het Apache-bestandssysteem. Zo eenvoudig is het.

De bestandsnaam wordt afgeleid door de URL van de gevraagde bron. Als u een bestand aanvraagt `/foo/bar.html` , wordt het opgeslagen onder /`var/cache/docroot/foo/bar.html` .

Als alle bestanden in het cachegeheugen worden opgeslagen en dus statisch in de Dispatcher worden opgeslagen, kunt u in principe de plug-in van het publicatiesysteem gebruiken en zou de Dispatcher fungeren als een eenvoudige webserver. Maar dit is slechts een illustratie van het principe. Het echte leven is ingewikkelder. U kunt niet alles in het cachegeheugen plaatsen en de cache is nooit volledig &quot;vol&quot;, omdat het aantal bronnen oneindig kan zijn vanwege de dynamische aard van het renderingsproces. Het model van een statisch bestandssysteem helpt een globaal beeld te genereren van de mogelijkheden van de verzender. En het helpt om de beperkingen van de verzender te verklaren.

#### De URL-structuur en bestandssysteemtoewijzing van AEM

Als u meer inzicht wilt krijgen in de Dispatcher, kunt u de structuur van een eenvoudige voorbeeld-URL herzien.  Kijk naar het onderstaande voorbeeld.

`http://domain.com/path/to/resource/pagename.selectors.html/path/suffix.ext?parameter=value&amp;otherparameter=value#fragment`

* `http` geeft het protocol aan

* `domain.com` is de domeinnaam

* `path/to/resource` is het pad waaronder de bron wordt opgeslagen in CRX en vervolgens in het bestandssysteem van de Apache-server.

Van hieruit verschillen de dingen enigszins tussen het AEM-bestandssysteem en het Apache-bestandssysteem.

In AEM:

* `pagename` is het resourcelabel

* `selectors` staat voor een aantal kiezers die in Sling worden gebruikt om te bepalen hoe de bron wordt weergegeven. Een URL kan een willekeurig aantal kiezers hebben. Ze worden gescheiden door een punt. Een sectie met kiezers kan bijvoorbeeld &#39;frans.mobile.fancy&#39; zijn. Kiezers mogen alleen letters, cijfers en streepjes bevatten.

* `html` als laatste van de &quot;kiezers&quot; wordt een extensie genoemd. In AEM/Sling bepaalt deze ook gedeeltelijk het renderscript.

* `path/suffix.ext` is een padachtige expressie die een achtervoegsel kan zijn van de URL.  Het kan in de manuscripten van AEM worden gebruikt om verder te controleren hoe een middel wordt teruggegeven. Later hebben we een volledige sectie over dit onderdeel. Vooralsnog zou het moeten volstaan om te weten u het als extra parameter kunt gebruiken. Achtervoegsels moeten een extensie hebben.

* `?parameter=value&otherparameter=value` is de querysectie van de URL. Het wordt gebruikt om willekeurige parameters door te geven aan AEM. URL&#39;s met parameters kunnen niet in de cache worden geplaatst en parameters moeten daarom worden beperkt tot gevallen waarin ze absoluut noodzakelijk zijn.

* `#fragment` , wordt het fragmentgedeelte van een URL niet doorgegeven aan AEM en wordt het alleen gebruikt in de browser. In JavaScript-frameworks wordt het fragmentgedeelte niet doorgegeven als &#39;routeparameters&#39; of om naar een bepaald deel van de pagina te gaan.

In Apache (*verwijzing het hieronder diagram*),

* `pagename.selectors.html` wordt gebruikt als de bestandsnaam in het bestandssysteem van de cache.

Als de URL een achtervoegsel `path/suffix.ext` heeft,

* `pagename.selectors.html` wordt gemaakt als een map

* `path` een map in de map `pagename.selectors.html`

* `suffix.ext` is een bestand in de map `path` . Opmerking: als het achtervoegsel geen extensie heeft, wordt het bestand niet in de cache opgeslagen.

![ lay-out van het Bestandssysteem na het krijgen van URLs van Dispatcher ](assets/chapter-1/filesystem-layout-urls-from-dispatcher.png)

*lay-out van het Bestandssysteem na het krijgen van URLs van Dispatcher*

<br> 

#### Basisbeperkingen

De toewijzing tussen een URL, de bron en de bestandsnaam is vrij eenvoudig.

Het is echter mogelijk dat u enkele overvullingen hebt opgemerkt.

1. URL&#39;s kunnen erg lang worden. Het toevoegen van het padgedeelte van een `/docroot` aan het lokale bestandssysteem kan de limieten van bepaalde bestandssystemen gemakkelijk overschrijden. Het runnen van Dispatcher in NTFS op Vensters kan een uitdaging zijn. U bent echter veilig met Linux.

2. URL&#39;s kunnen speciale tekens en umlauts bevatten. Dit is meestal geen probleem voor de verzender. Houd er echter rekening mee dat de URL op veel plaatsen in de toepassing wordt geïnterpreteerd. Vaak hebben we vreemd gedrag van een toepassing gezien - alleen om te weten te komen dat één stukje zelden gebruikte (aangepaste) code niet grondig is getest op speciale tekens. Indien mogelijk moet u deze vermijden. En als je dat niet kunt, plan dan voor grondig testen.

3. In CRX hebben bronnen subbronnen. Een pagina bevat bijvoorbeeld een aantal subpagina&#39;s. Dit kan niet worden gevonden in een bestandssysteem omdat bestandssystemen bestanden of mappen hebben.

#### URL&#39;s zonder extensie worden niet in de cache geplaatst

URL&#39;s moeten altijd een extensie hebben. Hoewel u URL&#39;s zonder extensies kunt bedienen in AEM. Deze URL&#39;s worden niet in de cache opgeslagen in de Dispatcher.

**Voorbeelden**

`http://domain.com/home.html` is **cacheable**

`http://domain.com/home` is **niet cacheable**

Dezelfde regel geldt wanneer de URL een achtervoegsel bevat. Het achtervoegsel moet een uitbreiding hebben om cacheable te zijn.

**Voorbeelden**

`http://domain.com/home.html/path/suffix.html` is **cacheable**

`http://domain.com/home.html/path/suffix` is **niet cacheable**

Je zou je kunnen afvragen: wat gebeurt er als het resource-part geen extensie heeft, maar het achtervoegsel wel. In dit geval heeft de URL helemaal geen achtervoegsel. Kijk naar het volgende voorbeeld:

**Voorbeeld**

`http://domain.com/home/path/suffix.ext`

De `/home/path/suffix` is het pad naar de bron... dus de URL bevat geen achtervoegsel.

**Conclusie**

Voeg altijd extensies toe aan zowel het pad als het achtervoegsel. SEO-bewuste mensen beweren soms dat dit je in zoekresultaten rangschikt. Maar een pagina zonder cache zou supertraag zijn en nog verder naar beneden gaan.

#### Conflicterende achtervoegsel-URL&#39;s

U hebt twee geldige URL&#39;s

`http://domain.com/home.html`

en

`http://domain.com/home.html/suffix.html`

Ze zijn absoluut geldig in AEM. U zou geen probleem zien op uw lokale ontwikkelcomputer (zonder een Dispatcher). Waarschijnlijk zult u ook geen probleem tegenkomen in UAT- of ladingstests. Het probleem waarmee we te maken hebben is zo subtiel dat het de meeste tests overslaat.  Het zal u hard raken wanneer u op piektijd bent en u bent beperkt in tijd om het te richten, waarschijnlijk heeft geen servertoegang, noch middelen om het te bevestigen. We zijn er geweest...

Dus... wat is het probleem?

`home.html` in een bestandssysteem kan een bestand of een map zijn. Niet beide tegelijk met AEM.

Als u `home.html` eerst aanvraagt, wordt deze gemaakt als een bestand.

Volgende aanvragen om `home.html/suffix.html` retourneren geldige resultaten, maar aangezien het bestand de positie in het bestandssysteem `home.html` &#39;blokkeert&#39;, kan `home.html` niet nogmaals als een map worden gemaakt en wordt `home.html/suffix.html` dus niet in de cache opgeslagen.

![ dossier dat positie in het filesystem blokkeert die sub-middelen verhinderen om in het voorgeheugen onder te brengen ](assets/chapter-1/file-blocking-position-in-filesystem.png)

*dossier dat positie in het filesystem blokkeert die sub-middelen verhinderen om in het voorgeheugen onder te brengen*

<br> 

Als u dit andersom doet, wordt eerst een `home.html/suffix.html` -aanvraag ingediend en wordt `suffix.html` eerst in de cache geplaatst onder een map `/home.html` . Deze map wordt echter verwijderd en vervangen door een bestand `home.html` wanneer u vervolgens `home.html` als bron aanvraagt.

![ Deleting a wegstructuur wanneer een ouder als middel ](assets/chapter-1/deleting-path-structure.png) wordt gehaald

*Deleting a wegstructuur wanneer een ouder als middel* wordt gehaald

<br> 

Het resultaat van wat in cache wordt geplaatst, is volledig willekeurig en afhankelijk van de volgorde van de binnenkomende aanvragen. Wat het nog lastiger maakt, is het feit dat je meestal meer dan één verzender hebt. De prestaties, de aanraaksnelheid en het gedrag van de cache kunnen per Dispatcher verschillen. Als u wilt weten waarom uw website niet reageert, moet u er zeker van zijn dat u naar de juiste Dispatcher kijkt in de ongelukkige volgorde van caching. Als je kijkt naar de Dispatcher die - gelukkig - een gunstiger aanvraagpatroon had, dan ben je verloren bij het zoeken naar het probleem.

#### Conflicterende URL&#39;s voorkomen

U kunt &quot;conflicterende URLs&quot;vermijden, waar een omslagnaam en filename &quot;concurreren&quot;voor de zelfde weg in het dossiersysteem, wanneer u een verschillende uitbreiding voor het middel gebruikt wanneer u een achtervoegsel hebt.

**Voorbeeld**

* `http://domain.com/home.html`

* `http://domain.com/home.dir/suffix.html`

Beide zijn perfect kacheable,

![](assets/chapter-1/cacheable.png)

Als u een achtervoegsel opvraagt of het achtervoegsel niet helemaal gebruikt, kiest u een toegewezen extensie &quot;dir&quot; voor een resource. Er zijn zeldzame gevallen waarin ze nuttig zijn. En het is gemakkelijk om deze gevallen correct uit te voeren.  Zoals we in het volgende hoofdstuk zullen zien wanneer we het hebben over cachevalidatie en flushing.

#### Niet-cachegeerbare verzoeken

Bekijk een korte samenvatting van het laatste hoofdstuk plus nog enkele uitzonderingen. De Dispatcher kan een URL in cache plaatsen als deze is geconfigureerd als cacheable en als het een GET-aanvraag is. Het kan niet in de cache worden geplaatst op grond van een van de volgende uitzonderingen.

**Cacheable Verzoeken**

* Verzoek is geconfigureerd om in de Dispatcher-configuratie cacheable te zijn
* Aanvraag is een aanvraag voor onbewerkte GET

**niet-Cacheable Verzoeken of Reacties**

* Verzoek dat caching door configuratie (Weg, Patroon, Type MIME) wordt ontkend
* Reacties die een header &quot;Dispatcher: no-cache&quot; retourneren
* Reactie die een &quot;Cache-Control: no-cache|private&quot; header retourneert
* Response that returns a &quot;Pragma: no-cache&quot; header
* Verzoek met vraagparameters
* URL zonder extensie
* URL met achtervoegsel dat geen extensie heeft
* Reactie die een andere statuscode dan 200 terugkeert
* POST-aanvraag

## De cache ongeldig maken en leegmaken

### Overzicht

In het laatste hoofdstuk wordt een groot aantal uitzonderingen vermeld, wanneer de Dispatcher een aanvraag niet in de cache kan plaatsen. Maar er zijn meer te overwegen dingen: Enkel omdat Dispatcher __ een verzoek kan in cache plaatsen, het niet noodzakelijk betekent dat het __ zou moeten.

Het punt is: In cache plaatsen is meestal eenvoudig. De Dispatcher moet alleen het resultaat van een reactie opslaan en deze de volgende keer retourneren wanneer precies hetzelfde verzoek wordt ontvangen. Toch? Verkeerd!

Het moeilijke deel is de _ongeldigverklaring_ of _flushing_ van het geheime voorgeheugen. De Dispatcher moet erachter komen wanneer een bron is gewijzigd - en moet opnieuw worden gerenderd.

Dit lijkt op het eerste gezicht een triviale taak... maar dat is het niet. Lees verder en u zult wat lastige verschillen tussen enige en eenvoudige middelen en pagina&#39;s ontdekken die zich op een hoogst gemanipuleerde structuur van veelvoudige middelen baseren.

### Eenvoudige bronnen en blozen

We hebben ons AEM-systeem zo ingesteld dat dynamisch een miniatuuruitvoering wordt gemaakt voor elke afbeelding wanneer deze wordt aangevraagd met een speciale &#39;duim&#39;-kiezer:

`/content/dam/path/to/image.thumb.png`

En - natuurlijk - wij verstrekken een URL om het originele beeld van een selecteurloze URL te dienen:

`/content/dam/path/to/image.png`

Als we beide, de miniatuur en de originele afbeelding downloaden, komen we terecht op een soort

```
/var/cache/dispatcher/docroot/content/dam/path/to/image.thumb.png

/var/cache/dispatcher/docroot/content/dam/path/to/image.png
```

in ons Dispatcher-bestandssysteem.

De gebruiker uploadt en activeert nu een nieuwe versie van dat bestand. Uiteindelijk wordt een verzoek tot nietigverklaring van AEM naar de Dispatcher gezonden,

```
GET /invalidate
invalidate-path:  /content/dam/path/to/image

<no body>
```

Ongeldigmaking is zo eenvoudig: een eenvoudig GET-verzoek naar een speciale URL voor &#39;&#39;invalidate&#39;&#39; op de Dispatcher. Een HTTP-hoofdtekst is niet vereist. De &#39;payload&#39; is alleen de header &#39;invalidate-path&#39;. Het invalidate-pad in de header is de bron die AEM kent, en niet het bestand of de bestanden die de Dispatcher in de cache heeft geplaatst. AEM weet alleen maar van bronnen. Extensies, kiezers en achtervoegsels worden tijdens runtime gebruikt wanneer een resource wordt aangevraagd. AEM voert geen boekhouding over welke selecteurs op een middel zijn gebruikt, zodat is de middelweg al het weet zeker wanneer het activeren van een middel.

Dat is in ons geval voldoende. Als een middel is veranderd, kunnen wij veilig veronderstellen, dat alle vertoningen van dat middel ook zijn veranderd. Als de afbeelding in ons voorbeeld is gewijzigd, wordt ook een nieuwe miniatuur weergegeven.

De Dispatcher kan de bron veilig verwijderen met alle uitvoeringen die in het cachegeheugen zijn opgeslagen. Het zal iets doen als:

`$ rm /content/dam/path/to/image.*`

verwijderen `image.png` en `image.thumb.png` en alle andere uitvoeringen die overeenkomen met dat patroon.

Supereenvoudig... zolang u maar één bron gebruikt om op een verzoek te reageren.

### Verwijzingen en verborgen inhoud

#### Probleem met verborgen inhoud

In tegenstelling tot afbeeldingen of andere binaire bestanden die naar AEM zijn geüpload, zijn HTML-pagina&#39;s geen afzonderlijke dieren. Ze leven in koppels en zijn in hoge mate met elkaar verbonden door hyperlinks en verwijzingen. De eenvoudige link is onschadelijk, maar het wordt lastig als we het hebben over verwijzingen naar inhoud. De alomtegenwoordige bovenste navigatie(s) op pagina&#39;s zijn verwijzingen naar inhoud.

#### Inhoudsverwijzingen en waarom dit een probleem is

Laten we naar een eenvoudig voorbeeld kijken. Een reisbureau heeft een webpagina waarop een reis naar Canada wordt gepromoot. Deze aanbieding wordt in de teassectie op twee andere pagina&#39;s weergegeven, op de pagina &quot;Home&quot; en op de pagina &quot;Winter Specials&quot;.

Aangezien op beide pagina&#39;s hetzelfde gummetje wordt weergegeven, is het onnodig de auteur te vragen het gummetje meerdere keren te maken voor elke pagina waarop het moet worden weergegeven. In plaats daarvan behoudt de doelpagina &quot;Canada&quot; een sectie in de pagina-eigenschappen om de informatie voor het taser weer te geven - of beter om een URL te bieden die het teaser helemaal rendert:

`<sling:include resource="/content/home/destinations/canada" addSelectors="teaser" />`

of

`<sling:include resource="/content/home/destinations/canada/jcr:content/teaser" />`

![](assets/chapter-1/content-references.png)

Alleen op AEM werkt dat als charm, maar als u een Dispatcher gebruikt op de instantie Publish, gebeurt er iets vreemds.

Stel je voor dat u uw website hebt gepubliceerd. De titel op je pagina in Canada is &quot;Canada&quot;. Wanneer een bezoeker uw homepage opvraagt - die een teasverwijzing naar die pagina heeft - geeft de component op de pagina &quot;Canada&quot; iets als

```
<div class="teaser">
  <h3>Canada</h3>
  <img …>
</div>
```

*in* de homepage. De startpagina wordt door de Dispatcher opgeslagen als een statisch HTML-bestand, inclusief het taser en de kop in het bestand.

Nu heeft de markator geleerd dat de teaskoppen actioneerbaar moeten zijn. Hij besluit dus de titel te wijzigen van &quot;Canada&quot; in &quot;Visit Canada&quot; en de afbeelding bij te werken.

Hij publiceert de bewerkte pagina &quot;Canada&quot; en reviseert de eerder gepubliceerde homepage om zijn wijzigingen te zien. Maar - daar is niets veranderd. Het geeft nog steeds de oude taser weer. Hij dubbelcheckt de &quot;Winter Special&quot;. Deze pagina is nog nooit eerder aangevraagd en is dus niet statisch in de Dispatcher opgeslagen. Deze pagina wordt dus nieuw weergegeven door Publiceren en deze pagina bevat nu de nieuwe taser &quot;Visit Canada&quot;.

![ Dispatcher die stale inbegrepen inhoud in de homepage opslaat ](assets/chapter-1/dispatcher-storing-stale-content.png)

*Dispatcher die stale inbegrepen inhoud in de homepage opslaat*

<br> 

Wat is er gebeurd? De Dispatcher slaat een statische versie op van een pagina die alle inhoud en opmaak bevat die uit andere bronnen is getekend tijdens het renderen.

De Dispatcher is een webserver op basis van bestandssystemen en is snel, maar ook relatief eenvoudig. Als een inbegrepen middel verandert realiseert het niet dat. De inhoud die aanwezig was toen de pagina voor opnemen werd weergegeven, blijft vastgeklonken.

De pagina &quot;Winter speciaal&quot; is nog niet weergegeven, dus er is geen statische versie op de Dispatcher en wordt dus weergegeven met het nieuwe taser, omdat deze op verzoek nieuw wordt weergegeven.

U zou kunnen denken, dat de Dispatcher spoor van elk middel zou houden het terwijl het teruggeven en het spoelen van alle pagina&#39;s die dit middel hebben gebruikt, wanneer dat middel verandert. Maar de Dispatcher geeft de pagina&#39;s niet weer. De rendering wordt uitgevoerd door het publicatiesysteem. De Dispatcher weet niet welke bronnen in een gerenderd .html-bestand worden gebruikt.

Nog steeds niet overtuigd? U zou kunnen denken *&quot;er een manier moet zijn om één of ander soort gebiedsdeel het volgen uit te voeren&quot;*. Goed daar is, of nauwkeuriger daar *was*. Communiqué 3 de overgrootvader van AEM had een gebiedsdeeltrekker die in de _wordt uitgevoerd zitting_ die werd gebruikt om een pagina terug te geven.

Tijdens een verzoek, werd elke middel dat via deze zitting werd verworven bijgehouden als gebiedsdeel van URL die momenteel werd teruggegeven.

Maar het bleek dat het bijhouden van de afhankelijkheden erg duur was. Mensen ontdekten al snel dat de website sneller is als ze de functie voor het bijhouden van afhankelijkheden volledig uitschakelen en afhankelijk zijn van het opnieuw weergeven van alle HTML-pagina&#39;s nadat één HTML-pagina is gewijzigd. Bovendien was die regeling ook niet perfect - er waren een aantal valkuilen en uitzonderingen onderweg. In sommige gevallen, gebruikte u niet de verzoeken standaardzitting om een middel te krijgen, maar een admin zitting om wat helper-middelen te krijgen om een verzoek terug te geven. Die gebiedsdelen werden gewoonlijk niet gevolgd en leidden tot hoofdpijnen en telefoongesprekken aan het ops-team die vragen om het geheime voorgeheugen manueel te spoelen. U had geluk als ze daarvoor een standaardprocedure hadden. Er waren meer gotchas onderweg, maar... laten we ophouden te herinneren. Dit leidt tot 2005. Uiteindelijk werd deze functie standaard gedeactiveerd in Communiqué 4 en maakte het niet terug in de opvolger van CQ5, die toen AEM werd.

### Automatische validatie

#### Wanneer volledig blozen goedkoper is dan afhankelijkheid-tracking

Sinds CQ5 vertrouwen we er volledig op dat de hele site min of meer ongeldig wordt gemaakt als slechts een van de pagina&#39;s verandert. Deze functie wordt &#39;Automatische validatie&#39; genoemd.

Maar nogmaals, hoe kan het zijn, dat het weggooien en opnieuw renderen van honderden pagina&#39;s goedkoper is dan het uitvoeren van een goede afhankelijkheidstracering en gedeeltelijke herrendering?

Er zijn twee belangrijke redenen:

1. Op een gemiddelde website wordt vaak slechts een kleine subset van de pagina&#39;s opgevraagd. Zelfs als u alle gerenderde inhoud weggooit, wordt er slechts een paar dozijn inhoud direct daarna aangevraagd. De weergave van de lange staart van pagina&#39;s kan worden verdeeld over een tijdsverloop, wanneer deze daadwerkelijk worden aangevraagd. In feite is de belasting bij het weergeven van pagina&#39;s dus niet zo hoog als u zou verwachten. Natuurlijk zijn er altijd uitzonderingen... wij zullen enkele trucs bespreken hoe te om gelijkelijk verdeelde lading op grotere websites met lege geheime voorgeheugens van Dispatcher te behandelen, later.

2. Alle pagina&#39;s zijn toch verbonden door de hoofdnavigatie. Dus bijna alle pagina&#39;s zijn uiteindelijk van elkaar afhankelijk. Dit betekent dat zelfs de slimste afhankelijkheidscontrole zal ontdekken wat wij reeds weten: Als één van de pagina&#39;s verandert, moet u alle anderen ongeldig maken.

Geloof je niet? Laat ons het laatste punt illustreren.

Wij gebruiken het zelfde argument zoals in het laatste voorbeeld met tellers die naar de inhoud van een verre pagina van verwijzingen voorzien. Alleen nu gebruiken we een extremer voorbeeld: een automatisch gerenderde Main-Navigation. Net als bij het gummetje wordt de navigatitel als inhoudsverwijzing van de gekoppelde of externe pagina getekend. De titels van de externe navigatie worden niet opgeslagen in de momenteel weergegeven pagina. Vergeet niet dat de navigatie op elke pagina van uw website wordt weergegeven. De titel van één pagina wordt dus steeds opnieuw gebruikt op alle pagina&#39;s met een hoofdnavigatie. En als u een navigatitel wilt veranderen, wilt u dat slechts eenmaal doen op de verre pagina - niet op elke pagina die naar de pagina verwijst.

In ons voorbeeld worden alle pagina&#39;s in de navigatie samengebracht met de navTitle van de doelpagina om een naam in de navigatie te renderen. De navigatitel voor &quot;IJsland&quot; wordt getekend op de pagina &quot;IJsland&quot; en weergegeven op elke pagina met een hoofdnavigatie.

![ Belangrijkste navigatie onvermijdelijk makend inhoud van alle pagina&#39;s samen door hun &quot;NavTitles&quot;te trekken ](assets/chapter-1/nav-titles.png)

*Belangrijkste navigatie onvermijdelijk makend inhoud van alle pagina&#39;s samen door hun &quot;NavTitles&quot;te trekken*

<br> 

Als u de NavTitle op de IJslandse pagina wijzigt van &quot;IJsland&quot; in &quot;Mooi IJsland&quot;, verandert die titel direct in het hoofdmenu van alle andere pagina&#39;s. De pagina&#39;s die vóór die wijziging zijn gerenderd en in cache zijn geplaatst, worden dus allemaal verouderd en moeten ongeldig worden gemaakt.

#### Hoe automatisch ongeldig wordt gemaakt: Het .stat-bestand

Als u een grote site hebt met duizenden pagina&#39;s, duurt het behoorlijk lang om alle pagina&#39;s te doorlopen en fysiek te verwijderen. In die periode zou de Dispatcher onbedoeld vaste inhoud kunnen bedienen. Erger nog, zouden er sommige conflicten kunnen voorkomen terwijl de toegang tot van de geheim voorgeheugendossiers, misschien wordt een pagina gevraagd terwijl het enkel wordt geschrapt of een pagina opnieuw wordt geschrapt toe te schrijven aan een tweede ongeldigverklaring die na een directe verdere activering gebeurde. Bedenk wat een rommeltje dat zou zijn. Gelukkig gebeurt dit niet. De Dispatcher gebruikt een slimme truc om dat te vermijden: in plaats van honderden of duizenden bestanden te verwijderen, wordt een eenvoudig, leeg bestand in de hoofdmap van uw bestandssysteem geplaatst wanneer een bestand wordt gepubliceerd en alle afhankelijke bestanden daarom als ongeldig worden beschouwd. Dit bestand wordt het &quot;statfile&quot; genoemd. Het bestand statfile is een leeg bestand. Wat belangrijk is aan het bestand is de aanmaakdatum, alleen.

Alle bestanden in de dispatcher die een aanmaakdatum hebben die ouder is dan het bestand met de status, zijn gerenderd vóór de laatste activering (en ongeldigmaking) en worden daarom als &#39;ongeldig&#39; beschouwd. Ze zijn fysiek aanwezig in het bestandssysteem, maar de Dispatcher negeert ze. Ze zijn &#39;stale&#39;. Telkens wanneer een aanvraag voor een &#39;stale&#39; resource wordt gedaan, vraagt de Dispatcher het AEM-systeem om de pagina opnieuw te renderen. Die nieuw weergegeven pagina wordt vervolgens opgeslagen in het bestandssysteem - nu met een nieuwe aanmaakdatum en het is weer vers.

![ de datum van de Verwezenlijking van het .stat dossier bepaalt welke inhoud stale is en die vers ](assets/chapter-1/creation-date.png) is

*de datum van de Verwezenlijking van het .stat dossier bepaalt welke inhoud stale is en die vers* is

<br> 

U kunt zich afvragen waarom het &quot;.stat&quot; wordt genoemd? En misschien &quot;.invalidate&quot;? Goed, kunt u zich voorstellen, die dat dossier in uw filesystem hebben helpt Dispatcher bepalen welke middelen *statisch* zouden kunnen worden gediend - enkel als van een statische Webserver. Deze bestanden hoeven niet langer dynamisch te worden gerenderd.

De ware aard van de naam is echter minder metaforisch. Deze wordt afgeleid van de Unix-systeemaanroep `stat()` , die de wijzigingstijd van een bestand (onder andere) retourneert.

#### Eenvoudige en automatische validatie mixen

Maar wacht... eerder zeiden we dat afzonderlijke bronnen fysiek verwijderd zijn. Nu zeggen we dat een recentere status ze in de ogen van de Dispatcher vrijwel ongeldig zou maken. Waarom dan eerst de fysieke verwijdering?

Het antwoord is eenvoudig. Meestal gebruikt u beide strategieën parallel, maar voor verschillende soorten bronnen. Binaire elementen, zoals afbeeldingen, zijn op zichzelf staande elementen. Ze zijn niet verbonden met andere bronnen in die zin dat ze hun informatie nodig hebben om te worden weergegeven.

HTML-pagina&#39;s daarentegen zijn sterk onderling afhankelijk. Dus zou je automatische ongeldigmaking toepassen op die. Dit is de standaardinstelling in de Dispatcher. Alle bestanden die tot een ongeldig gemaakte bron behoren, worden fysiek verwijderd. Bovendien worden bestanden die eindigen met &quot;.html&quot; automatisch ongeldig gemaakt.

De Dispatcher beslist over de bestandsextensie, of het systeem voor automatische validatie wordt toegepast of niet.

De bestandseinden voor automatische validatie kunnen worden geconfigureerd. In theorie kunt u alle extensies opnemen voor automatische validatie. Maar houd er rekening mee dat dit een zeer hoge prijs heeft. Je ziet geen onbedoelde schaalbronnen, maar de leveringsprestaties nemen sterk af als gevolg van overvalidatie.

Stel dat u bijvoorbeeld een schema implementeert waarbij PNG&#39;s en JPG&#39;s dynamisch worden gerenderd en hiervoor afhankelijk zijn van andere bronnen. U wilt afbeeldingen met hoge resolutie opnieuw schalen naar een kleinere resolutie die compatibel is met het web. Als u dat wel hebt, wordt ook de compressiesnelheid gewijzigd. De resolutie en de compressiesnelheid in dit voorbeeld zijn geen vaste constanten maar configureerbare parameters in de component die de afbeelding gebruikt. Als deze parameter wordt gewijzigd, moet u de afbeeldingen ongeldig maken.

Geen probleem - we hebben net geleerd dat we afbeeldingen kunnen toevoegen aan automatische validatie en altijd vers gerenderde afbeeldingen kunnen hebben als er iets verandert.

#### De baby weggooien met het badwater

Dat klopt - en dat is een enorm probleem. Lees de laatste alinea opnieuw. &quot;...vers gerenderde afbeeldingen als er iets verandert.&quot;. Zoals u weet, wordt een goede website voortdurend gewijzigd. Er wordt hier nieuwe inhoud toegevoegd, een typefout gecorrigeerd en een gummetje ergens anders getweend. Dat betekent dat al uw afbeeldingen voortdurend ongeldig worden gemaakt en opnieuw moeten worden gerenderd. Onderschat dat niet. Het dynamisch renderen en overdragen van afbeeldingsgegevens werkt in milliseconden op uw lokale ontwikkelcomputer. Uw productieomgeving moet dat honderd keer vaker doen - per seconde.

En laten we hier duidelijk zijn, je JPG&#39;s moeten opnieuw worden weergegeven, wanneer een HTML-pagina verandert en andersom. Er is slechts één &#39;emmertje&#39; van bestanden die automatisch ongeldig worden gemaakt. Het wordt als geheel gespoeld. Zonder enige manier om in verdere gedetailleerde structuren te breken.

Er is een goede reden waarom de automatische ongeldigverklaring standaard op &quot;.html&quot; wordt gehouden. Het doel is dat emmertje zo klein mogelijk te houden. Gooi het kind niet met het badwater weg door alles ongeldig te maken - alleen maar om op de veilige kant te staan.

De zelfstandige middelen zouden op de weg van die middel moeten worden gediend. Dat helpt om veel validatie uit te voeren. Houd het eenvoudig, maak geen toewijzingsschema&#39;s zoals &quot;middel /a/b/c&quot;wordt gediend van &quot;/x/y/z&quot;. Laat uw componenten werken met de standaardinstellingen voor automatische validatie van Dispatcher. Probeer geen slecht ontworpen component te repareren met overvalidatie in de Dispatcher.

##### Uitzonderingen op automatische validatie: ResourceOnly-validatie

Het verzoek tot ongeldigmaking voor de Dispatcher wordt gewoonlijk geactiveerd van het publicatiesysteem of de publicatiesystemen door een replicatieagent.

Als u zich super zeker over uw gebiedsdelen voelt, kunt u proberen om uw eigen het ongeldig maken replicatieagent te bouwen.

Het zou een beetje verder gaan dan deze handleiding om op details in te gaan, maar we willen je tenminste een paar tips geven.

1. Weet echt wat je doet. Het is erg moeilijk om de validatie juist te krijgen. Dat is één van de redenen waarom de automatische ongeldigverklaring zo rigoureus is; om het leveren van verouderde inhoud te vermijden.

2. Als uw agent een HTTP-header `CQ-Action-Scope: ResourceOnly` verzendt, betekent dit dat dit ene validatieverzoek geen automatische ongeldigmaking activeert. Dit ( [ https://github.com/cqsupport/webinar-dispatchercache/tree/master/src/refetching-flush-agent/refetch-bundle ](https://github.com/cqsupport/webinar-dispatchercache/tree/master/src/refetching-flush-agent/refetch-bundle)) stuk van code zou een goed uitgangspunt voor uw eigen replicatieagent kunnen zijn.

3. `ResourceOnly` voorkomt alleen automatische ongeldigmaking. Om de noodzakelijke gebiedsdeel in feite te doen die en ongeldig maakt, moet u de ongeldigingsverzoeken teweegbrengen zelf. U kunt het pakket willen controleren Dispatcher spoelregels ([ https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html ](https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html)) voor inspiratie op hoe dat eigenlijk kon gebeuren.

Wij adviseren niet dat u een gebiedsdeeloplossend regeling bouwt. Er is gewoon te veel moeite en weinig winst - en zoals al eerder is gezegd, is er te veel dat u het mis zult hebben.

In plaats daarvan moet u nagaan welke bronnen niet afhankelijk zijn van andere bronnen en ongeldig kunnen worden gemaakt zonder automatische validatie. U hoeft echter geen aangepaste Replication Agent te gebruiken. Creëer enkel een douaneregel in uw Dispatcher config die deze middelen van auto ongeldigverklaring uitsluiten.

We hebben gezegd dat de belangrijkste navigatie of traasers een bron van afhankelijkheden zijn. Goed - als u de navigatie en leerkrachten asynchroon laadt of hen met een manuscript SSI in Apache omvat, zult u niet die afhankelijkheid hebben te volgen. Later in dit document zullen we verder werken aan het asynchroon laden van componenten wanneer we het hebben over ‘Sling Dynamic Includes’.

Hetzelfde geldt voor pop-upvensters of inhoud die in een lichtbak is geladen. Deze stukken hebben ook zelden navigaties (ook wel &quot;afhankelijkheden&quot; genoemd) en kunnen als één bron ongeldig worden gemaakt.

## Componenten samenstellen met de Dispatcher in gedachten

### Dispatcher-mechanisch toepassen in een Real-World-voorbeeld

In het laatste hoofdstuk legden we uit hoe de fundamentele mechanica van Dispatcher werkt, hoe het in het algemeen werkt en wat de beperkingen zijn.

We willen deze mechanica nu toepassen op een type onderdelen dat u waarschijnlijk zult vinden in de vereisten van uw project. We kiezen de component bewust om problemen te demonstreren die u vroeg of laat zult tegenkomen. Vrees niet - niet alle componenten hebben de hoeveelheid overweging nodig die we zullen presenteren. Maar als je ziet dat het nodig is om zo&#39;n component op te bouwen, dan weet je wel wat de gevolgen zijn en hoe ze moeten worden aangepakt.

### Het patroon van de component Spooling (anti)

#### De component Responsieve afbeelding

Laten we een gemeenschappelijk patroon (of antipatroon) van een component met onderling verbonden binaire elementen illustreren. We maken een component &quot;respi&quot; voor &quot;responsieve-image&quot;. Deze component moet de weergegeven afbeelding kunnen aanpassen aan het apparaat waarop deze wordt weergegeven. Op desktops en tablets toont het de volledige resolutie van het beeld, op telefoons met een kleinere versie met een smalle uitsnijding - of misschien zelfs een heel ander motief (dit wordt &quot;kunstrichting&quot;genoemd in de ontvankelijke wereld).

De activa worden geupload in het gebied DAM van AEM en slechts _van verwijzingen voorzien_ in de ontvankelijk-beeldcomponent.

De respi-component zorgt voor zowel het renderen van de markering als het leveren van de binaire afbeeldingsgegevens.

De manier waarop we het hier implementeren is een gemeenschappelijk patroon dat we hebben gezien in veel projecten en zelfs een van de kerncomponenten van AEM is op dat patroon gebaseerd. Daarom is het zeer waarschijnlijk dat u als ontwikkelaar dat patroon zou kunnen aanpassen. Het heeft zijn zoete vlekken in termen van inkapseling, maar het vereist veel inspanning om het Dispatcher-klaar te krijgen. Wij zullen verscheidene opties bespreken hoe te om het probleem later te verlichten.

We noemen het patroon dat hier wordt gebruikt het &quot;Spooler Pattern&quot;, omdat het probleem teruggaat naar de vroege dagen van Communiqué 3, waar een methode &#39;spool&#39; was die op een resource zou kunnen worden aangeroepen om het binaire raw-data te streamen naar de respons.

De oorspronkelijke term &#39;spooling&#39; verwijst eigenlijk naar gedeelde langzame offline randapparaten, zoals printers, zodat deze hier niet correct worden toegepast. Maar we vinden de term hoe dan ook leuk omdat hij zelden in de online wereld te onderscheiden is. Elk patroon moet toch een herkenbare naam hebben, toch? Het is aan jou om te beslissen of dit een patroon of een anti-patroon is.

#### Implementatie

Hieronder wordt beschreven hoe onze component responsieve beelden wordt geïmplementeerd:

De component heeft twee delen; het eerste deel geeft de HTML-opmaak van de afbeelding weer, het tweede deel &quot;spoolt&quot; de binaire gegevens van de afbeelding waarnaar wordt verwezen. Aangezien dit een moderne website is met een responsief ontwerp, renderen we geen eenvoudige `<img src"…">` -tag, maar een set afbeeldingen in `<picture/>` -tag. Voor elk apparaat uploaden we twee verschillende afbeeldingen naar de DAM en verwijzen we ernaar vanuit onze imagecomponent.

De component heeft drie renderingscripts (geïmplementeerd in JSP, HTML of als servlet) die elk worden geadresseerd met een toegewezen kiezer:

1. `/respi.jsp` - zonder kiezer om de HTML-markering te renderen
2. `/respi.img.java` om de bureaubladversie te renderen
3. `/respi.img.mobile.java` om de mobiele versie te renderen.


De component wordt geplaatst in parsys van de homepage. De resulterende structuur in de CRX wordt hieronder weergegeven.

![ middel-structuur van het ontvankelijke beeld in CRX ](assets/chapter-1/responsive-image-crx.png)

*middel-structuur van het ontvankelijke beeld in CRX*

<br> 

De componentenprijsverhoging wordt teruggegeven als dit,

```plain
  #GET /content/home.html

  <html>

  …

  <div class="responsive-image>

  <picture>
    <source src="/content/home/jcr:content/par/respi.img.mobile.jpg" …/>
    <source src="/content/home/jcr:content/par/respi.img.jpg …/>

    …

  </picture>
  </div>
  …
```

en... we zijn klaar met onze mooie ingekapselde component.

#### Responsieve afbeeldingscomponent in actie

Nu vraagt een gebruiker om de pagina - en de middelen via de Dispatcher. Dit resulteert in bestanden in het Dispatcher-bestandssysteem, zoals hieronder wordt weergegeven,

![ Caching structuur van de ingekapselde ontvankelijke beeldcomponent ](assets/chapter-1/cached-structure-encapsulated-image-comonent.png)

*Caching structuur van de ingekapselde ontvankelijke beeldcomponent*

<br> 

Overweeg een gebruiker een nieuwe versie van de twee bloemafbeeldingen te uploaden en in te schakelen naar de DAM. AEM stuurt een aanvraag voor validatie voor

`/content/dam/flower.jpg`

en

`/content/dam/flower-mobile.jpg`

aan de Dispatcher. Deze verzoeken zijn echter tevergeefs. De inhoud is in de cache geplaatst als bestanden onder de substructuur van de component. Deze bestanden zijn nu verouderd, maar worden nog steeds op verzoek afgehandeld.

![ de wanverhouding van de Structuur die aan de inhoud van de schaal ](assets/chapter-1/structure-mismatch.png) leidt

*de wanverhouding van de Structuur die aan de inhoud van de schaal* leidt

<br> 

Er is nog een voorbehoud bij deze aanpak. U kunt dezelfde flower.jpg op meerdere pagina&#39;s gebruiken. Vervolgens wordt hetzelfde element onder meerdere URL&#39;s of bestanden in de cache geplaatst,

```
/content/home/products/jcr:content/par/respi.img.jpg

/content/home/offers/jcr:content/par/respi.img.jpg

/content/home/specials/jcr:content/par/respi.img.jpg

…
```

Telkens wanneer een nieuwe pagina wordt opgevraagd die niet in de cache is opgeslagen, worden de elementen bij verschillende URL&#39;s opgehaald van AEM. Geen Dispatcher caching en geen browser caching kunnen de levering versnellen.

#### Waar het spoolerpatroon schittert

Er is één natuurlijke uitzondering, waar dit patroon zelfs in zijn eenvoudige vorm nuttig is: als het binaire getal in de component zelf - en niet in DAM wordt opgeslagen. Dit is echter alleen handig voor afbeeldingen die eenmaal op de website worden gebruikt en als u geen elementen opslaat in de DAM, hebt u een moeilijke tijd om uw middelen te beheren. Stel je voor dat je gebruikslicentie voor een bepaald middel is uitgeput. Hoe kunt u erachter komen op welke componenten u het middel hebt gebruikt?

Zie je? De &quot;M&quot; in DAM staat voor &quot;Management&quot; - zoals in Digital Asset Management. Je wilt die functie niet weggeven.

#### Conclusie

Vanuit het perspectief van de AEM-ontwikkelaar zag het patroon er superelegant uit. Maar als de Dispatcher in de vergelijking is opgenomen, zult u het met mij eens zijn dat de naïeve aanpak misschien niet voldoende is.

We laten het aan u over om te beslissen of dit nu een patroon of een antipatroon is. En misschien hebt u al een paar goede ideeën in gedachten hoe te om de hierboven beschreven kwesties te verlichten? Goed. Dan zou u graag zien hoe andere projecten deze kwesties hebben opgelost.

### Veelvoorkomende Dispatcher-problemen oplossen

#### Overzicht

Laten we het hebben over hoe dat een beetje meer cache-vriendelijk had kunnen worden geïmplementeerd. Er zijn verschillende opties. Soms kunt u niet de beste oplossing kiezen. Misschien kom je in een al lopend project en je hebt een beperkt budget om het &quot;cacheprobleem&quot; alleen maar op te lossen en niet genoeg om een volwaardig refactoring te doen. Of u hebt te maken met een probleem dat complexer is dan de voorbeeldafbeeldingscomponent.

Wij zullen de beginselen en de voorbehouden in de volgende paragrafen uiteenzetten.

Nogmaals, dit is gebaseerd op echte ervaring. We hebben al die patronen in het wild gezien, dus het is geen academische oefening. Daarom tonen we u een aantal antipatronen, zodat u de kans hebt te leren van fouten die anderen al hebben gemaakt.

#### Cachemoordenaar

>[!WARNING]
>
>Dit is een antipatroon. Niet gebruiken. Altijd.

Heb je wel eens queryparameters gezien zoals `?ck=398547283745` ? Ze worden &#39;cache-killer&#39; genoemd. Het idee is, dat als u om het even welke vraagparameter toevoegt het middel niet in het voorgeheugen onder wordt gebracht. Als u bovendien een willekeurig getal toevoegt als waarde van de parameter (zoals &quot;398547283745&quot;), wordt de URL uniek en zorgt u ervoor dat er geen andere cache is tussen het AEM-systeem en het scherm. Meestal zouden tussenliggende verdachten een &quot;Varnish&quot;-cache zijn voor de Dispatcher, een CDN of zelfs de browsercache. Nogmaals: doe dat niet. U wilt dat uw bronnen zo veel en zo lang mogelijk in cache worden opgeslagen. De cache is je vriend. Geen vrienden doden.

#### Automatische validatie

>[!WARNING]
>
>Dit is een antipatroon. Vermijd het gebruik voor digitale elementen. Probeer de standaardconfiguratie van de Dispatcher te behouden. Deze configuratie wordt alleen automatisch geannuleerd voor .html-bestanden.

Op korte termijn kunt u &quot;.jpg&quot; en &quot;.png&quot; toevoegen aan de configuratie voor automatische validatie in de Dispatcher. Dit betekent dat wanneer een validatie optreedt, alle &quot;.jpg&quot;, &quot;.png&quot; en &quot;.html&quot; opnieuw moeten worden gerenderd.

Dit patroon is supereenvoudig geïmplementeerd als bedrijfseigenaren klagen dat hun wijzigingen niet snel genoeg op de livesite worden doorgevoerd. Maar dit kan je slechts enige tijd kopen om met een verfijndere oplossing te komen.

Zorg ervoor dat u de enorme gevolgen voor de prestaties begrijpt. Hierdoor wordt uw website aanzienlijk vertraagd en kan zelfs de stabiliteit worden aangetast - als uw site een website met veel bezoekers is met veel wijzigingen - zoals een nieuwsportaal.

#### URL-vingerafdrukken

Een URL-vingerafdruk ziet eruit als een cache-moordenaar. Maar dat is het niet. Het is geen willekeurig getal maar een waarde die de inhoud van de bron karakteriseert. Dit kan een hash zijn van de inhoud van de bron of - nog eenvoudiger - een tijdstempel wanneer de bron werd geüpload, bewerkt of bijgewerkt.

Een Unix-timestamp is goed genoeg voor een real-world implementatie. Voor betere leesbaarheid gebruiken we een leesbaardere indeling in deze zelfstudie: `2018 31.12 23:59 or fp-2018-31-12-23-59` .

De vingerafdruk mag niet worden gebruikt als een queryparameter, als URL&#39;s met queryparameters   kan niet in cache worden geplaatst. U kunt een kiezer of het achtervoegsel voor de vingerafdruk gebruiken.

Stel dat het bestand `/content/dam/flower.jpg` een `jcr:lastModified` datum van 31 december in 2018 heeft, 23 :59 . De URL met de vingerafdruk is `/content/home/jcr:content/par/respi.fp-2018-31-12-23-59.jpg` .

Deze URL blijft stabiel, zolang het bestand resource waarnaar wordt verwezen (`flower.jpg`) niet wordt gewijzigd. Het kan dus voor onbepaalde tijd in cache worden opgeslagen en het is geen cachemoordenaar.

Deze URL moet worden gemaakt en bediend door de responsieve afbeeldingscomponent. Het is geen out-of-the-box AEM functionaliteit.

Dat is het basisconcept. Er zijn echter een paar details die gemakkelijk over het hoofd kunnen worden gezien.

In ons voorbeeld, werd de component teruggegeven en in het voorgeheugen ondergebracht bij 23 :59. Nu is het beeld veranderd om te zeggen bij 00 :00.  De component _zou_ een nieuwe vingergedrukte URL in zijn prijsverhoging produceren.

U zou kunnen denken het __.. maar het niet zou moeten. Aangezien alleen het binaire getal van de afbeelding is gewijzigd en de pagina met de inhoud niet is gewijzigd, hoeft de HTML-markering niet opnieuw te worden gerenderd. De Dispatcher bedient de pagina met de oude vingerafdruk, en dus de oude versie van de afbeelding.

![ component van het Beeld recenter dan referenced beeld, geen verse gerenderde vingerprint.](assets/chapter-1/recent-image-component.png)

*component van het Beeld recenter dan referenced beeld, geen verse gerenderde vingerprint.*

<br> 

Als u nu de startpagina (of een andere pagina van die site) opnieuw activeert, wordt het statusbestand bijgewerkt. De Dispatcher beschouwt dan de stijl home.html en geeft deze opnieuw met een nieuwe vingerafdruk in de afbeeldingscomponent.

Maar we hebben de homepage niet geactiveerd, toch? En waarom zouden we een pagina activeren die we toch niet aanraken? Bovendien hebben we misschien niet genoeg rechten om pagina&#39;s te activeren, of de goedkeuringswerkstroom is zo lang en tijdrovend dat we dat gewoon niet op korte termijn kunnen doen. Wat moet je doen?

#### Het gereedschap Lazy Admin - Statusniveaus verlagen

>[!WARNING]
>
>Dit is een antipatroon. Gebruik het alleen op korte termijn om wat tijd te kopen en met een verfijndere oplossing te komen.

De luie admin &quot;_plaatst auto-ongeldigverklaring aan jpgs en statfile-level aan nul - die altijd met caching kwesties van alle soorten_ helpt.&quot; U vindt dat advies in technische forums en het helpt bij uw validatieprobleem.

Tot nu toe hebben we het statfile-niveau niet besproken. Automatische validatie werkt in principe alleen voor bestanden in dezelfde substructuur. Het probleem is echter dat pagina&#39;s en elementen gewoonlijk niet in dezelfde substructuur leven. Pagina&#39;s bevinden zich ergens onder `/content/mysite` , terwijl elementen onder `/content/dam` leven.

Het &quot;statfile level&quot;bepaalt waar bij welke diepte de wortelknopen van sub-bomen zijn. In het bovenstaande voorbeeld is het niveau &quot;2&quot; (1=/content, 2=/mysite,dam)

Het idee van het &quot;verminderen&quot;het statfile niveau aan 0 eigenlijk is de gehele /content boom als één en enige subboom te bepalen om pagina&#39;s en activa in het zelfde auto ongeldigingsdomein te maken leven. Dus zouden we alleen op grote boomstructuur op niveau hebben (in de docroot &quot;/&quot;). Maar het doen dit automatisch-maakt alle plaatsen op de server onbruikbaar wanneer iets wordt gepubliceerd - zelfs op volledig verwante plaatsen. Vertrouw op ons: Dit is een slecht idee op de lange termijn, omdat u de totale aanraaksnelheid van de cache sterk zult verlagen. U kunt alleen maar hopen dat uw AEM-servers over voldoende vuurkracht beschikken om zonder cache te kunnen worden uitgevoerd.

U zult de volledige voordelen van diepere staatsdossierniveaus een beetje later begrijpen.

#### Een aangepaste validatieagent implementeren

Hoe dan ook - we moeten de Dispatcher op de een of andere manier vertellen, om de HTML-pagina&#39;s ongeldig te maken als &quot;.jpg&quot; of &quot;.png&quot; is gewijzigd, zodat het bestand opnieuw kan worden weergegeven met een nieuwe URL.

Wat wij in projecten hebben gezien is - bijvoorbeeld - speciale replicatieagenten op het publicatiesysteem die verzoeken om ongeldigverklaring van een plaats verzenden wanneer een beeld van die plaats wordt gepubliceerd.

Hier is het nuttig als u het pad van de site kunt afleiden van het pad van het element door de naamgevingsconventie te gebruiken.

Over het algemeen is het een goed idee om de sites en de middelenpaden als volgt op elkaar af te stemmen:

**Voorbeeld**

```
/content/dam/site-a
/content/dam/site-b

/content/site-a
/content/site-b
```

Op deze manier kan uw aangepaste Dispatcher Flushing-agent een aanvraag voor het verzenden en ongeldig maken naar /content/site-a eenvoudig verzenden wanneer er een wijziging optreedt in `/content/dam/site-a` .

Eigenlijk maakt het niet uit welk pad je de Dispatcher vertelt om ongeldig te maken - zolang het zich op dezelfde site bevindt, in dezelfde &#39;substructuur&#39;. U hoeft zelfs geen echt bronnenpad te gebruiken. Het kan ook &quot;virtueel&quot;zijn:

```
GET /dispatcher-invalidate
Invalidate-path /content/mysite/dummy
```

![](assets/chapter-1/resource-path.png)

1. Er wordt een listener op het publicatiesysteem geactiveerd wanneer een bestand in de DAM verandert

2. De listener verzendt een verzoek tot validatie naar de Dispatcher. Door auto-ongeldigmaking maakt het niet uit welk pad wij in auto-ongeldigverklaring verzenden, tenzij het onder de homepage van de plaats - of preciezer op het niveau van het plaatsenstatuut is.

3. Het statusbestand wordt bijgewerkt.

4. De volgende keer dat de startpagina wordt opgevraagd, wordt deze opnieuw weergegeven. De nieuwe vingerafdruk/-datum wordt als extra kiezer overgenomen van de eigenschap lastModified van de afbeelding

5. Hierdoor wordt impliciet een verwijzing naar een nieuwe afbeelding gemaakt

6. Als de afbeelding daadwerkelijk wordt aangevraagd, wordt een nieuwe uitvoering gemaakt en opgeslagen in de Dispatcher


#### Noodzaak van opschonen

Phew. Voltooid. Hurray!

Nou... nog niet helemaal.

Het pad,

`/content/mysite/home/jcr:content/par/respi.img.fp-2018-31-12-23-59.jpg`

heeft geen betrekking op een van de ongeldig gemaakte middelen. Herinner je? We hebben alleen een &#39;dummy&#39;-resource ongeldig gemaakt en zijn afhankelijk van automatische validatie om &#39;home&#39; als ongeldig te beschouwen. Het beeld zelf zou nooit _fysisch_ kunnen worden geschrapt. De cache zal dus groeien en groeien en groeien. Wanneer afbeeldingen worden gewijzigd en geactiveerd, krijgen ze nieuwe bestandsnamen in het Dispatcher-bestandssysteem.

Er zijn drie problemen waarbij u de bestanden in de cache niet fysiek verwijdert en voor onbepaalde tijd bewaart:

1. U verspilt opslagcapaciteit - heel duidelijk. Toegestane opslag is de afgelopen jaren goedkoper en goedkoper geworden. Maar beeldresoluties en bestandsgrootten zijn de afgelopen jaren ook toegenomen - met de komst van netvliesvertoningen die op zoek zijn naar kristalscherpe beelden.

2. Hoewel harde schijven goedkoper zijn geworden, is &quot;opslag&quot; mogelijk niet goedkoper geworden. We hebben een trend gezien dat uw datacenterprovider geen (goedkope) opslag voor bare metalen harde schijven heeft, maar virtuele opslag op een NAS verhuurt. Dit soort opslag is een beetje betrouwbaarder en scalable maar ook een beetje duurder. Misschien wilt u het niet verspillen door verouderde huisvuil op te slaan. Dit heeft niet alleen betrekking op de primaire opslag - denk ook aan back-ups. Als u een uit-van-de-doos reserveoplossing hebt kunt u niet de cache-folders kunnen uitsluiten. Uiteindelijk maakt u ook een back-up van afvalgegevens.

3. Nog erger: u hebt gebruikslicenties voor bepaalde afbeeldingen mogelijk slechts voor een beperkte periode gekocht - zolang u deze nodig hebt. Nu, als u het beeld nog opslaat nadat een vergunning is verlopen, zou dat als een auteursrechtschending kunnen worden gezien. Mogelijk gebruikt u de afbeelding niet meer op uw webpagina&#39;s, maar Google vindt ze wel.

Dus ten slotte, kom je met wat huiswerk om alle bestanden ouder te reinigen dan... laten we zeggen een week om dit soort zwerfafval onder controle te houden.

#### Bezig met verwijderen van vingerafdrukken van URL voor Denial of Service-aanvallen

Maar wacht, er is nog een fout in deze oplossing:

We misbruiken een kiezer als parameter: fp-2018-31-12-23-59 wordt dynamisch gegenereerd als een soort &#39;cache-killer&#39;. Maar misschien begint een verveeld kind (of een zoekmachine die wild is geworden) de pagina&#39;s aan te vragen:

```
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-00.jpg
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-01.jpg
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-02.jpg

…
```

Bij elke aanvraag wordt de Dispatcher overgeslagen, waardoor het laden van een instantie Publish plaatsvindt. En - erger nog - maak een bestand volgens de regels op de Dispatcher.

Dus... in plaats van de vingerafdruk alleen te gebruiken als een eenvoudige cache-moordenaar, zou u de jcr :lastModified datum van beeld moeten controleren en een 404 moeten terugkeren als dit niet de verwachte datum is. Dat kost wat tijd en CPU fietst op het publicatiesysteem... wat u in de eerste plaats wilde voorkomen.

#### Voorkomen van URL-vingerafdrukken in versies met hoge frequentie

U kunt het schema voor vingerafdrukken niet alleen gebruiken voor elementen die afkomstig zijn van de DAM, maar ook voor JS- en CSS-bestanden en verwante bronnen.

[ Versioned Clientlibs ](https://adobe-consulting-services.github.io/acs-aem-commons/features/versioned-clientlibs/index.html) is een module die deze benadering gebruikt.

Maar hier zou je een ander voorwendsel kunnen tegenkomen met URL-vingerafdrukken: het bindt de URL aan de inhoud. U kunt de inhoud niet wijzigen zonder ook de URL te wijzigen (de wijzigingsdatum wordt ook bijgewerkt). Daar zijn de vingerafdrukken in de eerste plaats voor bedoeld. Maar denk eraan dat u een nieuwe versie ontwikkelt, met nieuwe CSS- en JS-bestanden en dus nieuwe URL&#39;s met nieuwe vingerafdrukken. Al uw HTML-pagina&#39;s bevatten nog steeds verwijzingen naar de oude vingerafgedrukte URL&#39;s. Om de nieuwe release consistent te laten werken, moet u dus alle HTML-pagina&#39;s tegelijk invalideren om een nieuwe rendering met verwijzingen naar de nieuwe vingerafgedrukte bestanden te forceren. Als u meerdere sites hebt die op dezelfde bibliotheken vertrouwen, kan dat een aanzienlijke mate van renderen zijn - en hier kunt u de `statfiles` niet gebruiken. Wees daarom voorbereid om de pieken van de belasting op uw publicatiesystemen na een rollout te zien. U zou een blauw-groene plaatsing met geheim voorgeheugenopwarming kunnen overwegen of misschien een op TTL-Gebaseerde geheime voorgeheugen voor uw Dispatcher... de mogelijkheden zijn eindeloos.

#### Een korte onderbreking

Wow - Dat zijn nogal wat details die in overweging moeten worden genomen, toch? Het weigert gemakkelijk te begrijpen, te testen en te debuggen. En allemaal voor een schijnbaar elegante oplossing. Het is weliswaar elegant, maar alleen vanuit AEM-perspectief. Samen met de Dispatcher wordt het naakt.

En toch - het lost één basisvoorbehoud niet op, als een beeld veelvoudige tijden op verschillende pagina&#39;s wordt gebruikt, worden zij in het voorgeheugen onder die pagina&#39;s geplaatst. Er is niet veel synergie in de cache.

Doorgaans is het afdrukken van URL-vingerafdrukken een goed hulpmiddel in de toolkit, maar u moet deze met de nodige zorg toepassen, omdat het nieuwe problemen kan veroorzaken en slechts een paar bestaande problemen kan oplossen.

Dat was een lang hoofdstuk. Maar we hebben dit patroon zo vaak gezien, dat we het nodig vonden om je het hele beeld te geven met alle voor- en nadelen. De vingerafdruk URL lost een paar van het inherente probleem in het Patroon Spooler op, maar de inspanning om uit te voeren is vrij hoog en u moet ook andere - gemakkelijkere - oplossingen overwegen. Ons advies is altijd te controleren of u uw URL&#39;s kunt baseren op de beschikbare bronnenpaden en geen tussenliggende component hebt. In het volgende hoofdstuk zullen we hierop ingaan.

##### Resolutie van runtimeafhankelijkheid

De resolutie over runtimeafhankelijkheid is een concept dat we in één project hebben overwogen. Maar door het te denken werd het vrij ingewikkeld en we besloten het niet uit te voeren.

Dit is het basisidee:

De Dispatcher is niet op de hoogte van de afhankelijkheid van middelen. Het is gewoon een hoop losse bestanden met weinig semantiek.

AEM weet ook weinig over afhankelijkheden. Het ontbreekt aan juiste semantiek of een &quot;gebiedsdeelvolgster&quot;.

AEM is op de hoogte van een aantal referenties. Deze kennis wordt gebruikt om u te waarschuwen wanneer u een pagina of element waarnaar wordt verwezen probeert te verwijderen of te verplaatsen. Dit gebeurt door de interne zoekopdracht te starten wanneer u een element verwijdert. Inhoudsverwijzingen hebben een bepaalde vorm. Het zijn padexpressies die beginnen met &quot;/content&quot;. Ze kunnen dus gemakkelijk geïndexeerd worden in volledige tekst, waar nodig.

In ons geval, zouden wij een aangepaste replicatieagent op het Publish systeem nodig hebben, die een onderzoek naar een specifiek weg teweegbrengt wanneer dat weg is veranderd.

Laten we zeggen

`/content/dam/flower.jpg`

Is gewijzigd bij Publiceren. De agent zou een zoekopdracht naar &quot;/content/dam/flower.jpg&quot; uitvoeren en alle pagina&#39;s vinden die naar die afbeeldingen verwijzen.

Vervolgens kan het een aantal verzoeken tot ongeldigmaking doen toekomen aan de Dispatcher. Eén voor elke pagina die het element bevat.

In theorie zou dat moeten werken. Maar alleen voor afhankelijkheden op het eerste niveau. U wilt dat schema niet toepassen voor afhankelijkheden op meerdere niveaus, bijvoorbeeld wanneer u de afbeelding gebruikt op een ervaringsfragment dat op een pagina wordt gebruikt. Eigenlijk zijn we van mening dat deze aanpak te ingewikkeld is - en er kunnen ook uitvoeringsproblemen zijn. Over het algemeen is het beste advies om geen dure computertaken uit te voeren in gebeurtenishandlers. Vooral zoeken kan erg duur worden.

##### Conclusie

We hopen dat we het Spooler Pattern grondig genoeg hebben besproken om u te helpen beslissen wanneer u het gebruikt en niet in uw implementatie gebruiken.

## Dispatcher-problemen voorkomen

### Op bronnen gebaseerde URL&#39;s

Een veel elegantere manier om het afhankelijkheidsprobleem op te lossen is om helemaal geen afhankelijkheden te hebben. Vermijd kunstmatige afhankelijkheden die optreden wanneer één bron wordt gebruikt om gewoon een andere te proxy, zoals in het laatste voorbeeld. Probeer bronnen zo vaak mogelijk als &#39;afzonderlijke&#39; entiteiten te zien.

Ons voorbeeld is gemakkelijk op te lossen:

![ het Spooling van het beeld met servlet die aan het beeld, niet de component verbindend is.](assets/chapter-1/spooling-image.png)

*het Spooling van het beeld met servlet die aan het beeld, niet de component verbindend is.*

<br> 

We gebruiken de oorspronkelijke middelenpaden van de elementen om de gegevens te renderen. Als we de oorspronkelijke afbeelding zo moeten renderen, kunnen we alleen de standaard renderer van AEM&#39;s voor elementen gebruiken.

Als wij wat speciale verwerking voor een specifieke component moeten doen, zouden wij een specifieke servlet op die weg en selecteur registreren om de transformatie namens de component te doen. We hebben dat hier voorbeeldig gedaan met &quot;.respi&quot;. kiezer. Het is verstandig om de namen van kiezers die worden gebruikt in de algemene URL-ruimte (zoals `/content/dam` ) bij te houden en een goede naamgevingsconventie te gebruiken om naamgevingsconflicten te voorkomen.

Trouwens, we zien geen problemen met codeconcentratie. De servlet kan worden gedefinieerd in hetzelfde Java-pakket als het slingmodel voor componenten.

We kunnen zelfs extra kiezers gebruiken in de wereldwijde ruimte, zoals:

`/content/dam/flower.respi.thumbnail.jpg`

Eenvoudig, toch? Waarom komen mensen dan met gecompliceerde patronen zoals de Spooler?

Wel, konden wij het probleem oplossen vermijdend de interne inhoud-verwijzing omdat de buitencomponent weinig waarde of informatie aan de teruggave van de binnenmiddel toevoegde, dat het gemakkelijk in reeks statische selecteurs kon worden gecodeerd die de vertegenwoordiging van een solitaire middel controleren.

Maar er zijn één klasse gevallen die u niet gemakkelijk met een op bron-gebaseerde URL kunt oplossen. Wij roepen dit type van geval, &quot;Parameter Injecting Components&quot;, en bespreken hen in het volgende hoofdstuk.

### Parameter Injecting Components

#### Overzicht

De Spooler in het laatste hoofdstuk was slechts een dunne omslag rond een bron. Het veroorzaakte meer problemen dan hulp bij het oplossen van het probleem.

We kunnen die verpakking eenvoudig vervangen door een eenvoudige kiezer te gebruiken en een servlet toe te voegen om dergelijke verzoeken in te dienen.

Maar wat als de component &quot;respi&quot; meer is dan alleen een proxy. Wat gebeurt er als de component daadwerkelijk bijdraagt aan de rendering van de component?

Laten we een kleine uitbreiding introduceren van onze &quot;respi&quot;-component, dat is een beetje een spelwisselaar. Opnieuw zullen we eerst enkele naïeve oplossingen introduceren om de nieuwe uitdagingen aan te pakken en te laten zien waar ze tekortschieten.

#### De component Respi2

De component respi2 is een component die een responsieve afbeelding weergeeft, net als de component respi. Maar het heeft een kleine toevoeging,

![ structuur van CRX: component respi2 die een kwaliteitsbezit aan de levering toevoegt ](assets/chapter-1/respi2.png)

*structuur van CRX: component respi2 die een kwaliteitsbezit aan de levering toevoegt*

<br> 

De afbeeldingen zijn jpegs en jpegs kunnen worden gecomprimeerd. Wanneer u een JPEG-afbeelding comprimeert, exporteert u kwaliteit voor de bestandsgrootte. Compressie wordt gedefinieerd als een numerieke parameter &quot;quality&quot;, die loopt van &quot;1&quot; tot &quot;100&quot;. &quot;1&quot; betekent &quot;kleine maar slechte kwaliteit&quot;, &quot;100&quot; staat voor &quot;uitstekende kwaliteit maar grote bestanden&quot;. Wat is dan de perfecte waarde?

Zoals in alle IT-zaken, is het antwoord: &quot;Het is afhankelijk.&quot;

Hier hangt het af van het motief. Motivering met scherpe randen, zoals bijvoorbeeld geschreven tekst, foto&#39;s van gebouwen, illustraties, schetsen of foto&#39;s van productvakken (met scherpe contouren en tekst die erop zijn geschreven), valt gewoonlijk in die categorie. Verplaatsen met zachtere kleur- en contrastovergangen zoals landschappen of portretten kunnen een beetje meer worden gecomprimeerd zonder zichtbaar kwaliteitsverlies. Natuurfoto&#39;s vallen meestal onder die categorie.

Afhankelijk van waar de afbeelding wordt gebruikt, kunt u ook een andere parameter gebruiken. Een kleine miniatuur in een gummetje kan een betere compressie ondergaan dan dezelfde afbeelding die in een schermbrede hoofdbanner wordt gebruikt. Dat betekent dat de parameter quality niet in de afbeelding past, maar in de afbeelding en context. En naar de smaak van de auteur.

Kortom, er is geen perfecte instelling voor alle afbeeldingen. Er is geen standaard-alles. Het is het beste als de auteur beslist. Hij zal de parameter &quot;quality&quot;als bezit in de component aanpassen tot hij met de kwaliteit tevreden is en zal niet verder gaan om bandbreedte niet te offeren.

We hebben nu een binair bestand in de DAM en een component die een eigenschap quality biedt. Hoe moet de URL eruit zien? Welke component is verantwoordelijk voor het spoolen?

#### Naïeve aanpak 1: Eigenschappen doorgeven als zoekparameters

>[!WARNING]
>
>Dit is een antipatroon. Niet gebruiken.

In het laatste hoofdstuk zag de afbeeldings-URL die door de component werd gerenderd er als volgt uit:

`/content/dam/flower.respi.jpg`

Het enige dat ontbreekt, is de waarde voor de kwaliteit. De component weet welke eigenschap door de auteur is ingevoerd... Het kan gemakkelijk worden doorgegeven aan de server voor het renderen van afbeeldingen als een queryparameter wanneer de markering wordt gerenderd, zoals in `flower.respi2.jpg?quality=60` :

```plain
  <div class="respi2">
  <picture>
    <source src="/content/dam/flower.respi2.jpg?quality=60" …/>
    …
  </picture>
  </div>
  …
```

Dit is een slecht idee. Herinner je? Verzoeken met queryparameters kunnen niet in cache worden geplaatst.

#### Naïeve aanpak 2: geef aanvullende informatie door als kiezer

>[!WARNING]
>
>Dit kan een antipatroon worden. Gebruik het zorgvuldig.

![ het overgaan van de Eigenschappen van de Component als Kiezers ](assets/chapter-1/passing-component-properties.png)

*het overgaan van de Eigenschappen van de Component als Kiezers*

<br> 

Dit is een kleine wijziging van de laatste URL. Slechts dit keer gebruiken wij een selecteur om het bezit tot servlet over te gaan, zodat het resultaat cacheable is:

`/content/dam/flower.respi.q-60.jpg`

Dit is veel beter, maar onthoud die vervelende scriptkind uit het laatste hoofdstuk die op dergelijke patronen kijkt? Hij zou zien hoe ver hij kan komen met het herhalen van waarden:

```plain
  /content/dam/flower.respi.q-60.jpg
  /content/dam/flower.respi.q-61.jpg
  /content/dam/flower.respi.q-62.jpg
  /content/dam/flower.respi.q-63.jpg
  …
```

Hierdoor wordt de cache opnieuw overgeslagen en wordt het laden op het publicatiesysteem gestart. Het zou dus een slecht idee kunnen zijn. U kunt dit verminderen door slechts een kleine ondergroep van parameters te filtreren. U wilt alleen `q-20, q-40, q-60, q-80, q-100` toestaan.

#### Filteren van ongeldige verzoeken bij gebruik van kiezers

Het verkleinen van het aantal kiezers was een goed begin. Als vuistregel moet u het aantal geldige parameters altijd tot een absoluut minimum beperken. Als u dat slim doet, kunt u zelfs hefboomwerking een Firewall van de Toepassing van het Web buiten AEM gebruiken gebruikend een statische reeks filters zonder diepe kennis van het onderliggende systeem van AEM om uw systemen te beschermen:

```
Allow: /content/dam/(-\_/a-z0-9)+/(-\_a-z0-9)+
       \.respi\.q-(20|40|60|80|100)\.jpg
```

Als u geen Firewall van de Toepassing van het Web hebt moet u in Dispatcher of in AEM zelf filtreren. Als je het doet in AEM, zorg dan dat

1. Het filter wordt superefficiënt geïmplementeerd, zonder de CRX te veel te openen en geheugen en tijd te verspillen.

2. Het filter reageert op een foutbericht &quot;404 - Niet gevonden&quot;

Laten we nogmaals het laatste punt benadrukken. De HTTP-conversatie zou er als volgt uitzien:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 404 – Not found
  << empty response body >>
```

We hebben ook implementaties gezien, die ongeldige parameters filterden maar een geldige fallback rendering retourneerden wanneer een ongeldige parameter wordt gebruikt. Laten we aannemen dat we alleen parameters van 20 tot 100 toestaan. De tussenliggende waarden worden toegewezen aan de geldige waarden. Dus,

`q-41, q-42, q-43, …`

zou altijd op dezelfde afbeelding reageren als q-40 zou hebben:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 200 – OK
  << flower.jpg with quality = 40 >>
```

Die aanpak helpt helemaal niet. Deze verzoeken zijn eigenlijk geldige verzoeken.  Ze verbruiken verwerkingskracht en nemen ruimte in de cachemap op de Dispatcher.

Het is beter om een `301 – Moved permanently` terug te geven:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 301 – Moved permanently
  Location: /content/dam/flower.respi.q-40.jpg
```

Hier vertelt AEM de browser. &quot;Ik heb geen `q-41`. Maar ze - je kunt me vragen over `q-40` &quot;.

Dat voegt een extra verzoek-antwoord lijn aan het gesprek toe, wat een beetje overheadkosten is, maar het is goedkoper dan het doen van de volledige verwerking op `q-41`. En u kunt het bestand dat al in de cache is opgeslagen onder `q-40` gebruiken. U moet echter begrijpen dat 302 reacties niet in het cachegeheugen zijn opgeslagen in de Dispatcher, we hebben het over logica die wordt uitgevoerd in de AEM. Steeds opnieuw. Dus je kunt het beter slank en snel maken.

Persoonlijk vinden wij de 404 het meest geschikt. Het maakt het overduidelijk wat er gebeurt. En helpt u bij het analyseren van logbestanden fouten op uw website te detecteren. De 301s kan worden gebruikt, waarbij 404 altijd geanalyseerd en geëlimineerd moeten worden.

## Beveiliging - Excursie

### Filterverzoeken

#### Waar moet ik het beste filteren?

Aan het eind van het laatste hoofdstuk hebben we gewezen op de noodzaak om het inkomende verkeer voor bekende kiezers te filteren. Dan blijft de vraag: waar moet ik eigenlijk verzoeken filteren?

Nou, dat hangt ervan af. Hoe eerder hoe beter.

#### Firewalls voor webtoepassingen

Als u een toestel van de Firewall van de Toepassing van het Web of &quot;WAF&quot;hebt dat voor de Veiligheid van het Web wordt ontworpen, zou u absoluut van deze mogelijkheden moeten profiteren. Maar u zou kunnen weten, dat WAF wordt beheerd door mensen met slechts beperkte kennis van uw inhoudstoepassing en zij of filter geldige verzoeken of laat teveel schadelijke verzoeken overgaan. Misschien zult u ontdekken dat de mensen die de WAF in werking stellen aan een verschillende afdeling met verschillende verschuivingen en releaseschema&#39;s worden toegewezen, zou de communicatie niet zo strak kunnen zijn als met uw directe teamgenoten en u krijgt niet altijd de veranderingen in tijd, wat betekent dat uiteindelijk uw ontwikkeling en inhoudssnelheid lijden.

Je zou kunnen eindigen met een paar algemene regels of zelfs een lijst van gewezen personen, die volgens je gevoel van darmen zou kunnen worden aangescherpt.

#### Filteren op Dispatcher en Publiceren

In de volgende stap worden URL-filterregels toegevoegd in de Apache-kern en/of in de Dispatcher.

Hier hebt u alleen toegang tot URL&#39;s. U bent beperkt tot op patronen gebaseerde filters. Als u opstelling een meer op inhoud-gebaseerd filtreren (als het toestaan van dossiers slechts met correcte tijd-stempel) moet of u wat van het filtreren wilt dat op uw Auteur wordt gecontroleerd - zult u omhoog schrijvend iets zoals een douaneserlet filter.

#### Controle en foutopsporing

In de praktijk zult u op elk niveau wat veiligheid hebben. Maar zorg dat u middelen hebt om te weten te komen op welk niveau een verzoek wordt uitgefilterd. Zorg ervoor dat u rechtstreeks toegang hebt tot het publicatiesysteem, de Dispatcher en de logbestanden op de WAF om te zien welk filter in de keten aanvragen blokkeert.

### Kiezers en proliferatie van kiezers

De benadering die &quot;selecteur-parameters&quot;in het laatste hoofdstuk gebruikt is snel en gemakkelijk en kan de ontwikkelingstijd van nieuwe componenten versnellen, maar het heeft grenzen.

Het instellen van een eigenschap &quot;quality&quot; is slechts een eenvoudig voorbeeld. Maar laten we zeggen, de servlet verwacht ook dat parameters voor &quot;breedte&quot; veelzijdiger zijn.

U kunt het aantal geldige URL&#39;s verminderen door het aantal mogelijke selectiewaarden te verminderen. U kunt hetzelfde doen met de breedte:

kwaliteit = q-20, q-40, q-60, q-80, q-100

width = w-100, w-200, w-400, w-800, w-1000, w-1200

Maar alle combinaties zijn nu geldige URL&#39;s:

```
/content/dam/flower.respi.q-40.w-200.jpg
/content/dam/flower.respi.q-60.w-400.jpg
…
```

Nu hebben we al 5x6=30 geldige URL&#39;s voor één bron. Elke extra eigenschap vergroot de complexiteit. Er kunnen eigenschappen zijn die niet tot een redelijke hoeveelheid waarden kunnen worden gereduceerd.

Dus ook deze aanpak heeft grenzen.

#### Onbedoeld een API blootstellen

Wat gebeurt hier? Als we goed kijken, zien we dat we geleidelijk overgaan van een statisch weergegeven naar een zeer dynamische website. En we surfen per ongeluk op een API voor het renderen van afbeeldingen naar de browser van de klant die eigenlijk alleen bestemd was voor gebruik door auteurs.

De auteur die de pagina bewerkt, moet de kwaliteit en de grootte van een afbeelding instellen. Het hebben van de zelfde die mogelijkheden door servlet als eigenschap of als vector voor een Ontkenning van de aanval van de Dienst worden blootgesteld. Wat het eigenlijk is, hangt af van de context. Hoe bedrijfskritiek is de website? Hoeveel lading is op de servers? Hoeveel ruimte is er nog over? Hoeveel geld hebt u voor de uitvoering? Je moet deze factoren in evenwicht brengen. U moet op de hoogte zijn van de voor- en nadelen.

## Het Spooler-patroon - Gereviseerd en gerehabiliteerd

### Hoe Spooler het toegankelijk maken van de API voorkomt

We hebben het Spooler-patroon in het laatste hoofdstuk in diskrediet gebracht. Het is tijd om het te rehabiliteren.

![](assets/chapter-1/spooler-pattern.png)

Het patroon Spooler voorkomt de kwestie met het blootstellen van API die wij in het laatste hoofdstuk bespraken. De eigenschappen worden opgeslagen en ingekapseld in de component. Alles wat we nodig hebben om deze eigenschappen te openen, is het pad naar de component. We hoeven de URL niet als een medium te gebruiken om de parameters tussen opmaak en binaire rendering over te brengen:

1. De client rendert de HTML-opmaak wanneer de component wordt aangevraagd binnen de primaire aanvraaglus

2. Het pad van de component fungeert als achterverwijzing van de markering naar de component

3. Browser gebruikt deze backreference om het binaire getal te verzoeken

4. Aangezien het verzoek de component raakt, hebben wij alle eigenschappen in onze hand resize, comprimeren en spool de binaire gegevens

5. De afbeelding wordt via de component naar de clientbrowser verzonden

Het Spooler Pattern is toch niet zo slecht, daarom is het zo populair. Als het alleen waar niet zo omslachtig is als het gaat om invalidatie in de cache...

### De omgekeerde spooler - het beste van beide werelden?

Dat brengt ons bij de vraag. Waarom kunnen we niet gewoon het beste van beide werelden krijgen? De goede inkapseling van het Patroon Spooler en de aardige in het voorgeheugen onderbrengende eigenschappen van een Bron Gebaseerde URL?

We moeten toegeven dat we dat niet hebben gezien in een echt live project. Maar laten we hier toch een klein gedachte-experiment durven te doen - als uitgangspunt voor uw eigen oplossing.

Wij zullen dit patroon _Omgekeerde Spooler_ roepen. Inverted Spooler moet op de beeldmiddel worden gebaseerd, om alle aardige eigenschappen van de geheim voorgeheugenongeldigheid te hebben.

Maar het mag geen parameters blootstellen. Alle eigenschappen moeten in de component worden ingekapseld. Maar wij kunnen de componentenweg - als ondoorzichtige verwijzing naar de eigenschappen blootstellen.

Dit leidt tot een URL in het formulier:

`/content/dam/flower.respi3.content-mysite-home-jcrcontent-par-respi.jpg`

`/content/dam/flower` is het pad naar de bron van de afbeelding

`.respi3` is een kiezer die de juiste server selecteert om de afbeelding te leveren

`.content-mysite-home-jcrcontent-par-respi` is een extra kiezer. Het codeert het pad naar de component die de eigenschap opslaat die nodig is voor de afbeeldingstransformatie. Kiezers hebben slechts een kleiner aantal tekens dan paden. Het coderingsschema is hier slechts voorbeeldig. Het vervangt &quot;/&quot; door &quot;-&quot;. Er wordt geen rekening mee gehouden dat het pad zelf ook &quot;-&quot; kan bevatten. Een verfijnder coderingsschema zou in een echt voorbeeld worden geadviseerd. Base64 zou ok moeten zijn. Maar het maakt foutopsporing een beetje moeilijker.

`.jpg` is het achtervoegsel voor bestanden

### Conclusie

Wow... de discussie over de spooler werd langer en ingewikkelder dan verwacht. We zijn je een excuus schuldig. Maar wij vonden het nodig om u een groot aantal aspecten te presenteren - goede en slechte - zodat u enige intuïtie kunt ontwikkelen over wat goed werkt in Dispatcher-land en wat niet.

## StatusFile en StatusFile

### De basisbeginselen

#### Inleiding

Wij vermeldden reeds kort het _statfile_ voordien. Het heeft betrekking op automatische ongeldigmaking:

Alle cachebestanden in het Dispatcher-bestandssysteem die zijn geconfigureerd om automatisch ongeldig te worden gemaakt, worden als ongeldig beschouwd als de datum die het laatst is gewijzigd, ouder is dan de datum die het laatst is gewijzigd. `statfile's`

>[!NOTE]
>
>De datum die als laatste is gewijzigd, is de datum waarop het bestand in de cache is opgeslagen in de browser van de client en uiteindelijk in het bestandssysteem is gemaakt. Dit is niet de `jcr:lastModified` -datum van de bron.

De datum van de laatste wijziging van de status (`.stat`) is de datum waarop het verzoek tot nietigverklaring van AEM is ontvangen op de Dispatcher.

Als u meer dan één Dispatcher hebt, kan dit tot vreemde effecten leiden. Uw browser kan een recentere versie een Verzender (als u meer dan één Dispatcher hebt) hebben. Of een Dispatcher zou kunnen denken dat de versie van browser die door andere Dispatcher werd uitgegeven verouderd is en onnodig een nieuw exemplaar verzendt. Deze effecten hebben geen significante invloed op de prestaties of de functionele vereisten. En ze zullen uitstappen in de tijd, wanneer de browser de meest recente versie heeft. Het kan echter enigszins verwarrend zijn wanneer u het gedrag van het in cache plaatsen van de browser optimaliseert en er fouten in opspoort. Wees daarom gewaarschuwd.

#### Validatiedomeinen instellen met /statfileslevel

Toen wij auto-ongeldigverklaring en statfile introduceerden wij zeiden, dat *alle* dossiers als ongeldig worden beschouwd wanneer er om het even welke verandering is en dat alle dossiers hoe dan ook onderling afhankelijk zijn.

Dat is niet helemaal accuraat. Gewoonlijk zijn alle bestanden die een gemeenschappelijke hoofdnavigatie-hoofdmap delen onderling afhankelijk. Maar één instantie van AEM kan een aantal websites ontvangen - *onafhankelijke* websites. Geen gemeenschappelijke navigatie delen - in feite niets delen.

Zou het geen afval zijn om Site B ongeldig te maken omdat er een wijziging is in Site A? Ja, dat is het. Zo hoeft het niet te zijn.

De Dispatcher biedt een eenvoudige manier om de sites van elkaar te scheiden: De `statfiles-level` .

Het is een getal dat aangeeft vanaf welk niveau in het bestandssysteem twee substructuren als &#39;onafhankelijk&#39; worden beschouwd.

Laten we eens kijken naar het standaardgeval waarin het statfilesniveau 0 is.

![ /statfileslevel &quot;0&quot;: De __.stat_ _wordt gecreeerd in docroot. Het ongeldig makingsdomein overspant de volledige installatie met inbegrip van alle plaatsen ](assets/chapter-1/statfile-level-0.png)

`/statfileslevel "0":` Het `.stat` -bestand wordt gemaakt in de hoofdmap van het document. Het validatiedomein omvat de gehele installatie, inclusief alle sites.

Welk bestand ook ongeldig wordt gemaakt, het `.stat` -bestand helemaal boven aan de verzendersdocroot wordt altijd bijgewerkt. Wanneer u `/content/site-b/home` dus ongeldig maakt, worden ook alle bestanden in `/content/site-a` ongeldig, omdat ze nu ouder zijn dan het `.stat` -bestand in de hoofdmap. Het is duidelijk niet wat u nodig hebt, wanneer u `site-b` ongeldig maakt.

In dit voorbeeld stelt u de waarde `statfileslevel` in op `1` .

Als u nu publiceert - en dus `/content/site-b/home` of een andere onderliggende bron `/content/site-b` ongeldig maakt, wordt het `.stat` -bestand gemaakt op `/content/site-b/` .

De inhoud onder `/content/site-a/` wordt niet gewijzigd. Deze inhoud wordt vergeleken met een `.stat` -bestand op `/content/site-a/` . Er zijn twee verschillende validatiedomeinen gemaakt.

![ A statfileslevel &quot;1&quot;leidt tot verschillende invalidatiedomeinen ](assets/chapter-1/statfiles-level-1.png)

*A statfileslevel &quot;1&quot;leidt tot verschillende invalidatiedomeinen*

<br> 

De grote installaties zijn gewoonlijk een beetje complexer en dieper gestructureerd. Een gemeenschappelijke regeling is het structureren van sites per merk, land en taal. In dat geval kunt u het niveau van de statussen nog hoger instellen. _1_ zou tot ongeldigingsdomeinen per merk, _2_ per land en _3_ per taal leiden.

### Noodzaak van een homogene sitestructuur

Het statusniveau wordt op gelijke wijze toegepast op alle plaatsen in uw opstelling. Daarom moeten alle sites dezelfde structuur hebben en op hetzelfde niveau beginnen.

Stel dat uw portfolio bepaalde merken bevat die alleen op een paar kleine markten worden verkocht, terwijl andere wereldwijd worden verkocht. De kleine markten hebben toevallig slechts één lokale taal, terwijl er op de wereldmarkt landen zijn waar meer dan één taal wordt gesproken:

```plain
  /content/tiny-local-brand/finland/home
  /content/tiny-local-brand/finland/products
  /content/tiny-local-brand/finland/about
                              ^
                          /statfileslevel "2"
  …

  /content/tiny-local-brand/norway
  …

  /content/shiny-global-brand/canada/en
  /content/shiny-global-brand/canada/fr
  /content/shiny-global-brand/switzerland/fr
  /content/shiny-global-brand/switzerland/de
  /content/shiny-global-brand/switzerland/it
                                          ^
                                /statfileslevel "3"
  ..
```

De eerste zou a `statfileslevel` van _2_ vereisen, terwijl laatstgenoemde _3_ vereist.

Geen ideale situatie. Als u het aan _3_ plaatst, dan zou de auto ongeldigverklaring niet binnen de kleinere plaatsen tussen de sub-takken `/home`, `/products` en `/about` werken.

Het plaatsen van het aan _2_ betekent, dat in de grotere plaatsen u `/canada/en` en `/canada/fr` afhankelijk verklaart, die zij niet zouden kunnen zijn. Elke ongeldigmaking in `/en` zou dus ook de validatie van `/fr` ongedaan maken. Dit leidt tot een iets lagere aanraaksnelheid voor cache, maar is nog steeds beter dan het leveren van inhoud in een verouderde cache.

De beste oplossing is natuurlijk om de wortels van alle sites even diep te maken:

```
/content/tiny-local-brand/finland/fi/home
/content/tiny-local-brand/finland/fi/products
/content/tiny-local-brand/finland/fi/about
…
/content/tiny-local-brand/norway/no/home
                                 ^
                        /statfileslevel "3"
```

### Koppeling tussen sites

Wat is nu het juiste niveau? Dat hangt van het aantal gebiedsdelen af u tussen de plaatsen hebt. Opnamen die u oplost voor het weergeven van een pagina worden beschouwd als &#39;harde afhankelijkheden&#39;. Wij toonden zulk een _opneming_ toen wij de _3} component van het Taser {aan het begin van deze gids introduceerden._

_Hyperlinks_ zijn een zachtere vorm van gebiedsdelen. Het is zeer waarschijnlijk dat u hyperlinks binnen één website... en het is niet onwaarschijnlijk dat u koppelingen tussen uw websites hebt. Met eenvoudige hyperlinks worden meestal geen afhankelijkheden tussen websites gemaakt. Denk aan een externe koppeling die u van uw site naar Facebook hebt ingesteld... U hoeft uw pagina niet te renderen als er iets verandert op Facebook en andersom, toch?

Een afhankelijkheid treedt op wanneer u inhoud leest uit de gekoppelde bron (bijvoorbeeld de navigatitel). Dergelijke afhankelijkheden kunnen worden vermeden als u alleen vertrouwt op lokaal ingevoerde navigatitels en deze niet tekent vanaf de doelpagina (zoals bij externe koppelingen).

#### Onverwachte afhankelijkheid

Er kan echter een deel van uw installatie zijn, waar sites - zogenaamd onafhankelijk - samenkomen. Laten we eens kijken naar een echt scenario dat we in een van onze projecten hebben meegemaakt.

De klant had een site-structuur zoals die in het laatste hoofdstuk werd geschetst:

```
/content/brand/country/language
```

Bijvoorbeeld:

```
/content/shiny-brand/switzerland/fr
/content/shiny-brand/switzerland/de

/content/shiny-brand/france/fr

/content/shiny-brand/germany/de
```

Elk land had zijn eigen domein,

```
www.shiny-brand.ch

www.shiny-brand.fr

www.shiny-brand.de
```

Er waren geen navigeerbare verbindingen tussen de taalplaatsen en geen duidelijke opneming, zodat plaatsen wij het statfilesniveau aan 3.

Alle sites hadden eigenlijk dezelfde inhoud. Het enige grote verschil was de taal.

Zoekprogramma&#39;s zoals Google beschouwen dezelfde inhoud als op verschillende URL&#39;s als &#39;misleidend&#39;. Een gebruiker kan proberen hoger gerangschikt of vaker vermeld te worden door landbouwbedrijven te creëren die identieke inhoud dienen. Zoekprogramma&#39;s herkennen deze pogingen en plaatsen pagina&#39;s lager die gewoon inhoud recyclen.

U kunt voorkomen wordt onderaan door transparant te maken, dat u eigenlijk meer dan één pagina met de zelfde inhoud hebt, en dat u niet probeert om het systeem (zie [ &quot;Google over gelokaliseerde versies van uw pagina&quot;te &quot;vertellen&quot;](https://support.google.com/webmasters/answer/189077?hl=en)) door `<link rel="alternate">` markeringen aan elke verwante pagina in de kopbalsectie van elke pagina te plaatsen:

```
# URL: www.shiny-brand.fr/fr/home/produits.html

<head>

  <link rel="alternate" 
        hreflang="fr-ch" 
        href="http://www.shiny-brand.ch/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="de-ch" 
        href="http://www.shiny-brand.ch/de/home/produkte.html">
  <link rel="alternate" 
        hreflang="de-de" 
        href="http://www.shiny-brand.de/de/home/produkte.html">

</head>

----

# URL www.shiny-brand.de/de/home/produkte.html

<head>

  <link rel="alternate" 
        hreflang="fr-fr" 
        href="http://www.shiny-brand.fr/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="fr-ch" 
        href="http://www.shiny-brand.ch/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="de-ch"
         href="http://www.shiny-brand.ch/de/home/produits.html">

</head>
```

![ inter-verbindt allen ](assets/chapter-1/inter-linking-all.png)

*inter-verbindt allen*

<br> 

Sommige SEO-experts betogen zelfs dat dit reputatie of &quot;link-sap&quot; van een hooggeplaatste website in één taal zou kunnen overbrengen naar dezelfde website in een andere taal.

Met dit schema zijn niet alleen een aantal koppelingen tot stand gekomen, maar ook enkele problemen. Het aantal verbindingen die voor _p_ in _n_ talen worden vereist is _p x (n <sup> 2 </sup> - n)_: Elke paginakoppelingen aan elke andere pagina (_n x n_) behalve aan zich (_- n_). Dit schema wordt toegepast op elke pagina. Als wij een kleine plaats in 4 talen met 20 pagina&#39;s hebben, komt elk dit aan _240_ verbindingen.

Eerst wilt u niet dat een editor deze koppelingen handmatig moet onderhouden. Deze koppelingen moeten automatisch door het systeem worden gegenereerd.

Ten tweede moeten ze accuraat zijn. Wanneer het systeem een nieuw &quot;familielid&quot;ontdekt, wilt u het van alle andere pagina&#39;s met de zelfde inhoud (maar in verschillende taal) verbinden.

In ons project kwamen regelmatig nieuwe relatieve pagina&#39;s naar boven. Maar ze kwamen niet tot stand als &#39;alternatieve&#39; links. Toen de pagina `de-de/produkte` bijvoorbeeld werd gepubliceerd op de Duitse website, was deze niet meteen zichtbaar op de andere sites.

De reden was dat de sites in onze opzet onafhankelijk moesten zijn. Een wijziging op de Duitse website heeft dus geen aanleiding gegeven tot een ongeldigverklaring op de Franse website.

U kent al één oplossing om dat probleem op te lossen. Verlaag gewoon het statusniveau naar 2 om het validatiedomein uit te breiden. Dit verlaagt natuurlijk ook de slagverhouding van de cache, vooral wanneer er publicaties worden gepubliceerd, en maakt dus vaker ongeldig.

In ons geval was het nog ingewikkelder:

Hoewel we dezelfde inhoud hadden, waren de eigenlijke niet-merknamen in elk land verschillend.

`shiny-brand` werd `marque-brillant` in Frankrijk en `blitzmarke` in Duitsland aangeroepen:

```
/content/marque-brillant/france/fr
/content/shiny-brand/switzerland/fr
/content/shiny-brand/switzerland/de
/content/blitzmarke/germany/de
…
```

Dat zou betekenen om het `statfiles` -niveau in te stellen op 1 - wat zou hebben geleid tot een te groot validatiedomein.

Herstructurering van de locatie zou dat hebben opgelost. Alle merken samenvoegen tot één gemeenschappelijke basis. Maar we hadden toen niet de capaciteit gehad, en - dat zou ons slechts niveau 2 hebben gegeven.

We hebben besloten om vast te houden aan niveau 3 en de prijs te betalen voor het niet altijd hebben van actuele &quot;alternatieve&quot; links. Om het probleem te verhelpen, hadden we op de Dispatcher een &#39;goedkopere&#39;, &#39;cron job&#39; waarmee bestanden die ouder waren dan 1 week, hoe dan ook zouden worden opgeruimd. Uiteindelijk werden alle pagina&#39;s dus toch op een bepaald moment opnieuw weergegeven. Maar dat is een compromis dat in elk project afzonderlijk moet worden vastgesteld.

## Conclusie

We hebben enkele basisprincipes besproken over de manier waarop de Dispatcher in het algemeen werkt en we hebben u enkele voorbeelden gegeven waarbij u wellicht een beetje meer implementatieinspanningen moet doen om het goed te doen en waar u misschien compromissen wilt maken.

We zijn niet ingegaan op de manier waarop dat in de Dispatcher is geconfigureerd. Wij wilden u eerst de basisconcepten en de problemen begrijpen, zonder u aan de console te vroeg te verliezen. En - het daadwerkelijke configuratiewerk is goed gedocumenteerd - als u de basisconcepten begrijpt, zou u moeten weten waarvoor de diverse schakelaars worden gebruikt.

## Dispatcher Tips en trucs

We zullen het eerste deel van dit boek afsluiten met een willekeurige verzameling tips en trucs die nuttig kunnen zijn in een bepaalde situatie. Zoals we al eerder hebben gedaan, presenteren we de oplossing niet, maar het idee, zodat u de kans krijgt om het idee en concept te begrijpen en een link te leggen naar artikelen waarin de feitelijke configuratie nader wordt beschreven.

### Tijdstip voor validatie corrigeren

Als u een auteur van AEM installeert en uit de doos publiceert, is de topologie een beetje oneven. De auteur verzendt de inhoud naar de publicatiesystemen en het verzoek tot ongeldigmaking tegelijkertijd naar de Dispatchers. Aangezien zowel de Publish systemen als de Dispatcher, van de Auteur door rijen worden losgekoppeld kan de timing een beetje ongelukkig zijn. De Dispatcher kan het verzoek tot validatie van de auteur ontvangen voordat de inhoud wordt bijgewerkt op het publicatiesysteem.

Als een klant ondertussen om die inhoud verzoekt, zal de Dispatcher om de inhoud van de schaal vragen en opslaan.

Een meer betrouwbare opstelling verzendt het ongeldigingsverzoek van de Publish systemen _nadat_ zij de inhoud hebben ontvangen. Het artikel &quot;[ het Invalideren van het Geheime voorgeheugen van Dispatcher van een het Publiceren Instantie ](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html#InvalidatingDispatcherCachefromaPublishingInstance)&quot;beschrijft de details.

**Verwijzingen**

[ helpx.adobe.com - het Valideren van het Geheime voorgeheugen van Dispatcher van een het Publiceren Instantie ](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html#InvalidatingDispatcherCachefromaPublishingInstance)

### HTTP-koptekst en -koptekstcache

In de oude dagen had de Dispatcher gewoon bestanden zonder opmaak in het bestandssysteem opgeslagen. Als u HTTP-headers nodig had om aan de klant te worden geleverd, hebt u dit gedaan door Apache te configureren op basis van de kleine informatie die u van het bestand of de locatie had. Dat was vooral vervelend toen u een Webtoepassing in AEM uitvoerde die sterk op de kopballen van HTTP baseerde. Alles werkte prima in de AEM-enige instantie, maar niet toen je een Dispatcher gebruikte.

Gewoonlijk hebt u de ontbrekende headers opnieuw toegepast op de bronnen in de Apache-server met `mod_headers` door informatie te gebruiken die u zou kunnen afgeleid door het bronnenpad en het achtervoegsel. Maar dat was niet altijd voldoende.

Vooral het irriteren was, dat zelfs met Dispatcher de eerste _uncached_ reactie op browser uit het Publish systeem met een volledige waaier van kopballen kwam, terwijl de verdere reacties door Dispatcher met een beperkte reeks kopballen werden geproduceerd.

Vanaf Dispatcher 4.1.11 kan de Dispatcher kopteksten opslaan die zijn gegenereerd door de publicatiesystemen.

Zo voorkomt u dat u de headerlogica dupliceert in de Dispatcher en maakt u de volledige expressieve kracht van HTTP en AEM vrij.

**Verwijzingen**

* [ helpx.adobe.com - de Kopballen van de Reactie van het Caching ](https://helpx.adobe.com/experience-manager/kb/dispatcher-cache-response-headers.html)

### Uitzonderingen in cache afzonderlijk

Het is mogelijk dat u alle pagina&#39;s en afbeeldingen in het algemeen in cache wilt plaatsen, maar onder bepaalde omstandigheden moet u een uitzondering maken. U wilt bijvoorbeeld PNG-afbeeldingen in cache plaatsen, maar geen PNG-afbeeldingen met een captcha (die bij elke aanvraag moet worden gewijzigd). De Dispatcher herkent een captcha misschien niet als een captcha.. maar AEM zeker wel. De toepassing kan de Dispatcher vragen dat ene verzoek niet in de cache op te slaan door samen met het antwoord een header volgens te sturen:

```plain
  response.setHeader("Dispatcher", "no-cache");

  response.setHeader("Cache-Control: no-cache");

  response.setHeader("Cache-Control: private");

  response.setHeader("Pragma: no-cache");
```

Cache-Control en Pragma zijn officiële HTTP-headers, die worden doorgegeven aan en geïnterpreteerd door lagen in de cache, zoals een CDN. De header `Dispatcher` is slechts een tip voor de Dispatcher om geen cache te maken. Deze kan worden gebruikt om de Dispatcher de opdracht te geven geen cache op te slaan, terwijl de lagen in de cache nog steeds de mogelijkheid hebben dit te doen. Eigenlijk is het moeilijk om een geval te vinden waar dat nuttig kan zijn. Maar we zijn er zeker van dat er wat zijn, ergens.

**Verwijzingen**

* [ Dispatcher - geen Geheime voorgeheugen ](https://helpx.adobe.com/experience-manager/kb/DispatcherNoCache.html)

### Browsercaching

De snelste http-respons is de reactie die door de browser zelf wordt gegeven. Waar het verzoek en de reactie niet over een netwerk aan een webserver onder hoge lading moeten reizen.

U kunt de browser helpen bepalen wanneer de server om een nieuwe versie van het dossier moet vragen door een vervaldatum op een middel te plaatsen.

Gewoonlijk doet u dat statisch door Apache&#39;s `mod_expires` te gebruiken of door de Cache-Control en de header Expires die uit AEM komen op te slaan als u een meer individueel besturingselement nodig hebt.

Een document in de browser dat in de cache is opgeslagen, kan drie up-to-date niveaus hebben.

1. _Gegarandeerd vers_ - browser kan het caching document gebruiken.

2. _potentieel versleten_ - browser zou de server eerst moeten vragen als het caching document nog bijgewerkt is,

3. _Stale_ - browser moet de server voor een nieuwe versie vragen.

De eerste wordt gegarandeerd door de vervaldatum die door de server wordt geplaatst. Als een bron niet is verlopen, hoeft u de server niet opnieuw te vragen.

Als het document de vervaldatum bereikt heeft, kan het nog vers zijn. De vervaldatum wordt ingesteld wanneer het document wordt afgeleverd. Maar vaak weet u niet van tevoren wanneer er nieuwe inhoud beschikbaar is - dus dit is slechts een conservatieve schatting.

Om te bepalen of het document in de browsercache nog steeds hetzelfde is als het document dat bij een nieuwe aanvraag wordt geleverd, kan de browser de `Last-Modified` -datum van het document gebruiken. De browser vraagt de server:

&quot;_ik heb een versie van 10 juni... heb ik een update nodig?_&quot; en de server kan reageren met

&quot;_304 - Uw versie is nog bijgewerkt_&quot;zonder het middel opnieuw over te brengen, of de server kon antwoorden met

&quot;_200 - hier is een recentere versie_&quot;in de kopbal van HTTP en de daadwerkelijke recentere inhoud in het lichaam van HTTP.

Om dat tweede deel te laten werken, moet u de `Last-Modified` -datum naar de browser verzenden zodat deze een referentiepunt heeft om updates te vragen.

We hebben eerder uitgelegd dat wanneer de `Last-Modified` -datum door de Dispatcher wordt gegenereerd, deze tussen verschillende aanvragen kan verschillen, omdat het bestand in de cache - en de datum ervan - wordt gegenereerd wanneer het bestand door de browser wordt aangevraagd. Een alternatief zou zijn &quot;e-tags&quot; te gebruiken - dit zijn getallen die de werkelijke inhoud identificeren (bijvoorbeeld door een hash-code te genereren) in plaats van een datum.

&quot;[ Etag Steun ](https://adobe-consulting-services.github.io/acs-aem-commons/features/etag/index.html)&quot;van het _ACS Pakket van Commons_ gebruikt deze benadering. Dit komt echter met een prijs: Aangezien de E-markering als kopbal moet worden verzonden, maar de berekening van de knoeiboelcode vereist volledig lezend de reactie, moet de reactie volledig in het belangrijkste geheugen worden gebufferd alvorens het kan worden geleverd. Dit kan een negatief effect hebben op de latentie wanneer uw website waarschijnlijk over resources beschikt die niet in het cachegeheugen zijn opgeslagen en u natuurlijk het geheugen dat uw AEM-systeem gebruikt in de gaten moet houden.

Als u URL-vingerafdrukken gebruikt, kunt u zeer lange vervaldatums instellen. U kunt vingerafdrukbronnen altijd in de cache plaatsen in de browser. Een nieuwe versie wordt gemarkeerd met een nieuwe URL en oudere versies hoeven nooit te worden bijgewerkt.

We gebruikten URL-vingerafdrukken toen we het spoolerpatroon introduceerden. Statische bestanden die afkomstig zijn uit `/etc/design` (CSS, JS) worden zelden gewijzigd, waardoor ze ook geschikt zijn voor gebruik als vingerafdruk.

Voor gewone bestanden maken we meestal een vast schema, zoals elke 30 minuten opnieuw controleren op HTML, elke 4 uur afbeeldingen enzovoort.

Het in cache plaatsen van browsers is bijzonder handig op het Auteur-systeem. U wilt zoveel cache plaatsen als u in de browser kunt om de bewerkervaring te verbeteren. Helaas, de duurste middelen, de HTML- pagina&#39;s kunnen niet in het voorgeheugen worden opgeslagen... zij worden verondersteld om regelmatig op de auteur te veranderen.

De granitebibliotheken, die de gebruikersinterface van AEM vormen, kunnen voor een behoorlijk wat tijd in het voorgeheugen ondergebracht worden. U kunt uw sites ook in de cache plaatsen van statische bestanden (lettertypen, CSS en JavaScript) in de browser. Zelfs afbeeldingen in `/content/dam` kunnen doorgaans ongeveer 15 minuten in cache worden geplaatst, omdat ze niet zo vaak worden gewijzigd als tekst op de pagina&#39;s. Afbeeldingen worden niet interactief bewerkt in AEM. Ze worden eerst bewerkt en goedgekeurd voordat ze naar AEM worden geüpload. U kunt dus aannemen dat deze niet zo vaak als tekst veranderen.

In cache plaatsen van UI-bestanden, bestanden en afbeeldingen in uw sitemenu kan het opnieuw laden van pagina&#39;s aanzienlijk versnellen wanneer u in de bewerkingsmodus werkt.



**Verwijzingen**

*[ developer.mozilla.org - Caching ](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

* [ apache.org - Mod verloopt ](https://httpd.apache.org/docs/current/mod/mod_expires.html)

* [ ACS Commons - Etag Steun ](https://adobe-consulting-services.github.io/acs-aem-commons/features/etag/index.html)

### URL&#39;s afkappen

Uw bronnen worden opgeslagen onder

`/content/brand/country/language/…`

Maar dit is natuurlijk niet de URL die u aan de klant wilt laten zien. Voor esthetica, leesbaarheid en SEO redenen zou u het deel kunnen willen beknotten dat reeds in de domeinnaam wordt vertegenwoordigd.

Als u een domein hebt

`www.shiny-brand.fi`

het is meestal niet nodig om het merk en het land in de weg te leggen . In plaats van:

`www.shiny-brand.fi/content/shiny-brand/finland/fi/home.html`

u zou willen hebben,

`www.shiny-brand.fi/home.html`

U moet die toewijzing implementeren op AEM - omdat AEM moet weten hoe koppelingen moet worden gerenderd volgens de ingekorte notatie.

Vertrouw echter niet alleen op AEM. Als u dat doet, staan paden als `/home.html` in de hoofdmap van de cache. Is dat het &quot;huis&quot; voor de Fins, Duitsers of de Canadese website? En als er een bestand `/home.html` aanwezig is in de Dispatcher, hoe weet de Dispatcher dan dat dit ongeldig moet worden gemaakt als er een aanvraag tot nietigverklaring voor `/content/brand/fi/fi/home` wordt ingediend.

We hebben een project gezien dat voor elk domein aparte documenten had. Het was een nachtmerrie om fouten op te sporen en te onderhouden - en we hebben het eigenlijk nooit perfect gezien.

We kunnen de problemen oplossen door de cache te herstructureren. We hadden één docroot voor alle domeinen, en validatieverzoeken konden 1 :1 worden afgehandeld terwijl alle bestanden op de server begonnen met `/content` .

Het afbreekpunt was ook heel eenvoudig.  AEM heeft afgebroken koppelingen gegenereerd vanwege een configuratie volgens de instructies in `/etc/map` .

Wanneer een aanvraag `/home.html` de Dispatcher raakt, wordt eerst een herschrijfregel toegepast die het pad intern uitbreidt.

Die regel was opstelling statisch in elke gastheerconfiguratie. Eenvoudig gezegd, de regels leken er zo uit,

```plain
  # vhost www.shiny-brand.fi

  RewriteRule "^(.\*\.html)" "/content/shiny-brand/finland/fi/$1"
```

In het bestandssysteem hebben we nu eenvoudige, op `/content` gebaseerde paden, die ook op de Auteur en de Publish zouden worden gevonden - wat veel foutopsporing hielp. Om nog maar te zwijgen van de correcte ongeldigverklaring - dat was geen probleem meer.

Dat deden we alleen voor &#39;zichtbare&#39; URL&#39;s, URL&#39;s die worden weergegeven in de URL-sleuf van de browser. URL&#39;s voor afbeeldingen waren bijvoorbeeld nog steeds zuivere URL&#39;s met de naam &quot;/content&quot;. We zijn van mening dat het verfraaien van de &#39;hoofd&#39;-URL voldoende is voor optimalisatie van zoekprogramma&#39;s.

Het hebben van één gemeenschappelijke docroot had ook een andere aardige eigenschap. Als er iets fout ging in de Dispatcher, konden we de hele cache opruimen door uit te voeren,

`rm -rf /cache/dispatcher/*`

(iets wat u niet wilt doen bij hoge laadpieken).

**Verwijzingen**

* [ apache.org - Mod herschrijven ](https://httpd.apache.org/docs/2.4/mod/mod_rewrite.html)

* [ helpx.adobe.com - de Afbeelding van het Middel ](https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/resource-mapping.html)

### Foutafhandeling

In AEM-klassen leert u hoe u een fouthandler kunt programmeren bij Sling. Dit is niet zo anders dan het schrijven van een gewone sjabloon. Je schrijft gewoon een sjabloon in JSP of HTML, toch?

Ja, maar dit is alleen het AEM-deel. Onthoud: de Dispatcher slaat geen `404 – not found` - of `500 – internal server error` -reacties in cache op.

Als u deze pagina&#39;s dynamisch rendert bij elke (mislukte) aanvraag, hebt u een onnodige hoge belasting op de publicatiesystemen.

Wat wij nuttig vonden is om de volledige foutenpagina niet terug te geven wanneer een fout voorkomt maar slechts een super vereenvoudigde en kleine - zelfs statische versie van die pagina, zonder enige versieringen of logica.

Dit is natuurlijk niet wat de klant zag. In de Dispatcher hebben we `ErrorDocuments` als volgt geregistreerd:

```
ErrorDocument 404 "/content/shiny-brand/fi/fi/edocs/error-404.html"
ErrorDocument 500 "/content/shiny-brand/fi/fi/edocs/error-500.html"
```

Nu kon het AEM-systeem de Dispatcher laten weten dat er iets mis was, en de Dispatcher kon een glanzende en mooie versie van het foutdocument leveren.

Hier moeten twee dingen worden vermeld.

Ten eerste is `error-404.html` altijd dezelfde pagina. Zo, is er geen individueel bericht zoals &quot;Uw onderzoek naar &quot;_producten_&quot;gaf geen resultaat&quot;. Daar zouden we gemakkelijk mee kunnen leven.

Ten tweede... wel, als we een interne serverfout zien - of nog erger, als we een storing van het AEM-systeem tegenkomen, is er geen manier om AEM te vragen een foutpagina te genereren, toch? Het vereiste volgende verzoek zoals gedefinieerd in de instructie `ErrorDocument` zou ook mislukken. We hebben dit probleem omzeild door een uitsnijdtaak uit te voeren waarmee de foutpagina&#39;s periodiek via `wget` van de gedefinieerde locaties worden opgehaald en worden opgeslagen op statische bestandslocaties die zijn gedefinieerd in de instructie `ErrorDocuments` .

**Verwijzingen**

* [ apache.org - de Documenten van de Fout van de Douane ](https://httpd.apache.org/docs/2.4/custom-error.html)

### Beveiligde inhoud in cache plaatsen

De Dispatcher controleert geen toestemmingen wanneer het een middel door gebrek levert. Het wordt als dit op doel uitgevoerd - om uw openbare website te versnellen. Als u bepaalde bronnen wilt beschermen met een aanmelding, hebt u in principe drie opties:

1. Bescherm de bron voordat de aanvraag de cache raakt, dat wil zeggen door een SSO-gateway (Single Sign On) voor de Dispatcher of als een module op de Apache-server

2. Sluit gevoelige middelen van in het voorgeheugen ondergebracht worden uit en zo hen altijd levend van het Publish systeem.

3. Toestemmingsgevoelige caching in de Dispatcher gebruiken

En natuurlijk kun je je eigen mix van alle drie benaderingen toepassen.

**Optie 1**. Een &quot;SSO&quot;Gateway zou door uw organisatie hoe dan ook kunnen worden afgedwongen. Als uw toegangsregeling zeer grof gegraveerd is, kunt u geen informatie van AEM nodig hebben om te beslissen of om toegang tot een middel te verlenen of te ontkennen.

>[!NOTE]
>
>Dit patroon vereist a _Gateway_ dat _onderschept_ elk verzoek en de daadwerkelijke _vergunning_ uitvoert - het verlenen van of het ontkennen van verzoeken aan Dispatcher. Als uw systeem SSO een _authenticator_ is, die slechts de identiteit van een gebruiker vestigt u Optie 3 moet uitvoeren. Als u termen als &quot;SAML&quot;of &quot;OAauth&quot;in het handboek van uw SSO systeem leest - dat is een sterke indicator die u Optie 3 moet uitvoeren.


**Optie 2**. &quot;Niet in cache plaatsen&quot; is over het algemeen een slecht idee. Als u zo gaat, zorg ervoor de hoeveelheid verkeer en het aantal gevoelige middelen die worden uitgesloten klein zijn. Of zorg ervoor dat er een cache in het geheugen is geïnstalleerd in het publicatiesysteem, dat de publicatiesystemen de resulterende belasting kunnen verwerken - meer in die in deel III van deze reeks.

**Optie 3**. &#39;Machtigingsgevoelige caching&#39; is een interessante aanpak. De Dispatcher slaat een bron in de cache op - maar voordat het wordt geleverd, vraagt het het AEM-systeem of het dat mag doen. Dit leidt tot een extra verzoek van Dispatcher aan Publish - maar het spares het Publish systeem gewoonlijk tegen het opnieuw teruggeven van een pagina als het reeds in het voorgeheugen ondergebracht is. Deze aanpak vereist echter enige aangepaste implementatie. Vind details hier in het artikel [ Vertrouwelijke caching van de Toestemming ](https://helpx.adobe.com/experience-manager/dispatcher/using/permissions-cache.html).

**Verwijzingen**

* [ helpx.adobe.com - de Bevoegdheden gevoelige caching ](https://helpx.adobe.com/experience-manager/dispatcher/using/permissions-cache.html)

### De respijtperiode instellen

Als u regelmatig kort na elkaar ongeldig maakt - bijvoorbeeld door een boomactivering of uit eenvoudige noodzaak om uw inhoud up-to-date te houden, kan het gebeuren dat u de cache voortdurend leegmaakt en dat uw bezoekers bijna altijd een lege cache raken.

In het onderstaande diagram ziet u een mogelijke timing bij het openen van één pagina.  Het probleem wordt natuurlijk alleen maar groter als het aantal verschillende gevraagde pagina&#39;s groter wordt.

![ de Acties die van de Frequentie tot ongeldig geheime voorgeheugen voor het grootste deel van de tijd leiden ](assets/chapter-1/frequent-activations.png)

*de Acties die van de Frequentie tot ongeldig geheime voorgeheugen voor het grootste deel van de tijd leiden*

<br> 

Als u het probleem van deze onherstelbare storm in het cachegeheugen wilt verhelpen, zoals deze soms wordt genoemd, kunt u minder rigoureus zijn met betrekking tot de `statfile` -interpretatie.

U kunt de Dispatcher zo instellen dat deze een `grace period` voor automatische validatie gebruikt. Hierdoor wordt intern extra tijd toegevoegd aan de wijzigingsdatum van `statfiles` .

Stel dat uw `statfile` een wijzigingstijd heeft van vandaag 12 :00 en dat uw `gracePeriod` is ingesteld op 2 minuten. Dan worden alle auto-ongeldig gemaakte dossiers beschouwd als geldig bij 12 :01 en bij 12 :02. Zij worden opnieuw teruggegeven na 12 :02.

De referentieconfiguratie stelt om een goede reden een `gracePeriod` van twee minuten voor. Je zou kunnen denken: &quot;Twee minuten? Dat is bijna niets. Ik kan gemakkelijk 10 minuten wachten tot de inhoud verschijnt...&quot;.  Dus je zou geneigd kunnen zijn om een langere periode in te stellen - laten we zeggen 10 minuten, ervan uitgaande dat je inhoud tenminste na deze 10 minuten verschijnt.

>[!WARNING]
>
>Zo werkt `gracePeriod` niet. De aflossingsvrije periode is _niet_ de tijd waarna een document wordt gewaarborgd om ongeldig te worden verklaard, maar een tijd-kader geen ongeldigverklaring gebeurt. Elke verdere ongeldigverklaring die binnen dit kader _valt verlengt_ het tijdkader - dit kan oneindig lang zijn.

Laten we laten zien hoe `gracePeriod` eigenlijk werkt met een voorbeeld:

Stel dat u een mediasite gebruikt en dat uw bewerkingspersoneel om de vijf minuten regelmatig inhoud bijwerkt. U kunt instellen dat voor de respijtperiode 5 minuten nodig is.

Wij zullen met een snel voorbeeld bij 12 :00 beginnen.

12 :00 - Statfile wordt geplaatst aan 12 :00. Alle caching dossiers worden beschouwd als geldig tot 12 :05.

12 :01 - een ongeldigverklaring komt voor. Dit verlengt de rootetijd tot 12 :06

12 :05 - een andere redacteur publiceert zijn artikel - het verlengen van de aflossingstijd met een andere GracePeriod aan 12 :10.

En zo verder... de inhoud wordt nooit ongeldig gemaakt. Elke ongeldigverklaring *binnen* de GracePeriod verlengt effectief de aflossingstijd. De `gracePeriod` is ontworpen om de invalidatie-storm te doorstaan... maar u moet uiteindelijk de regen in gaan.............................................................................................................................................................................`gracePeriod`

#### Een deterministische respijtperiode

We willen graag een ander idee introduceren hoe je een invalidatiestorm kunt doorstaan. Het is slechts een idee. We hebben het niet in productie geprobeerd, maar we vonden het concept interessant genoeg om het idee met jullie te delen.

`gracePeriod` kan onvoorspelbaar lang worden, als uw regelmatige replicatieinterval korter is dan uw `gracePeriod`.

Het alternatieve idee is als volgt: Alleen ongeldig maken met vaste tijdsintervallen. De tussenliggende tijd betekent altijd dat u de inhoud van een schaal moet bedienen. De ongeldigverklaring zal uiteindelijk plaatsvinden, maar een aantal invalidaties worden verzameld bij één &quot;bulkvalidering&quot;, zodat de Dispatcher ondertussen de kans heeft om inhoud in de cache te bedienen en het publicatiesysteem wat lucht te geven om te ademen.

De implementatie zou er als volgt uitzien:

U gebruikt een &#39;Aangepast validatiescript&#39; (zie verwijzing) dat wordt uitgevoerd nadat de validatie is uitgevoerd. Dit script zou de `statfile's` laatste wijzigingsdatum lezen en deze afronden tot de volgende intervalstop. Met de UNIX-shell-opdracht `touch --time` geeft u een tijd op.

Als u bijvoorbeeld de respijtperiode instelt op 30 sec, zou de Dispatcher de laatste gewijzigde datum van het bestand afronden naar de volgende 30 sec. De verzoeken van de ongeldigverklaring die binnen tussen enkel plaatsen de zelfde volgende volledige 30 sec gebeuren.

![ het Postponing van de ongeldigverklaring aan volgende volledige 30 seconde verhoogt hit-rate.](assets/chapter-1/postponing-the-invalidation.png)

*het Postponing van de ongeldigverklaring aan volgende volledige 30 seconde verhoogt hit-rate.*

<br> 

De cache-treffers die tussen het validatieverzoek en de volgende ronde sleuf van 30 seconden plaatsvinden, worden dan als &#39;traag&#39; beschouwd. Er is een update voor Publiceren uitgevoerd, maar de Dispatcher heeft nog steeds oude inhoud.

Deze aanpak zou kunnen helpen bij het definiëren van langere aflossingsvrije perioden zonder dat men bang hoeft te zijn dat latere verzoeken de periode onbepaald verlengen. Zoals we al eerder hebben gezegd, is het slechts een idee en we hadden geen kans om het te testen.

**Verwijzingen**

[ helpx.adobe.com - de Configuratie van Dispatcher ](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)

### Automatisch opnieuw ophalen

Uw site heeft een bijzonder toegangspatroon. U hebt een hoge lading van inkomend verkeer en het grootste deel van het verkeer is geconcentreerd op een klein deel van uw pagina&#39;s. De homepage, uw campagne landende pagina&#39;s en uw meest kenmerkende productdetailpagina&#39;s ontvangen 90% van het verkeer. Of als u een nieuwe site gebruikt, hebben de recentere artikelen hogere verkeersaantallen dan oudere artikelen.

Deze pagina&#39;s worden waarschijnlijk in de Dispatcher in cache geplaatst, zoals ze zo vaak worden opgevraagd.

Er wordt een verzoek tot willekeurige ongeldigmaking naar de Dispatcher verzonden, waardoor alle pagina&#39;s, inclusief uw populairste pagina, ongeldig worden gemaakt.

Aangezien deze pagina&#39;s zo populair zijn, zijn er nieuwe binnenkomende aanvragen van verschillende browsers. Laten we de homepage als voorbeeld nemen.

Aangezien nu het geheime voorgeheugen ongeldig is, door:sturen alle verzoeken aan de homepage die tezelfdertijd binnenkomen aan het Publish systeem dat een hoge lading produceert.

![ Parallelle verzoeken aan zelfde middel op leeg geheime voorgeheugen: De verzoeken door:sturen aan Publish ](assets/chapter-1/parallel-requests.png)

*Parallelle verzoeken aan zelfde middel op leeg geheime voorgeheugen: De verzoeken door:sturen aan Publish*

Met automatisch opnieuw ophalen kunt u dat tot op zekere hoogte beperken. De meeste ongeldig gemaakte pagina&#39;s worden na automatische validatie fysiek opgeslagen op de Dispatcher. Zij worden slechts _beschouwd_ als verkoop. _Automatisch die_ opnieuw plaatsen betekent, dat u nog deze stapelpagina&#39;s voor een paar seconden terwijl het in werking stellen van _één enkel_ verzoek aan het publicatiesysteem aandient om de schaalinhoud opnieuw op te halen:

![ leverend stale inhoud terwijl het re-halen in de achtergrond ](assets/chapter-1/fetching-background.png)

*leverend stale inhoud terwijl het re-halen in de achtergrond*

<br> 

Als u het opnieuw ophalen wilt inschakelen, moet u de Dispatcher vertellen welke bronnen na een automatische ongeldigmaking opnieuw moeten worden opgehaald. Houd er rekening mee dat elke pagina die u activeert, automatisch alle andere pagina&#39;s valideert, inclusief de populaire pagina&#39;s.

Herhalen betekent eigenlijk dat je de Dispatcher in elk (!) verzoek tot ongeldigverklaring moet vertellen dat je de populairste opnieuw wilt ophalen - en dat zijn de populairste aanvragen.

Dit wordt bereikt door een lijst met bron-URL&#39;s (werkelijke URL&#39;s, niet alleen paden) te plaatsen in de hoofdtekst van de verzoeken tot ongeldigmaking:

```
POST /dispatcher/invalidate.cache HTTP/1.1

CQ-Action: Activate
CQ-Handle: /content/my-brand/home/path/to/some/resource
Content-Type: Text/Plain
Content-Length: 207

/content/my-brand/home.html
/content/my-brand/campaigns/landing-page-1.html
/content/my-brand/campaigns/landing-page-2.html
/content/my-brand/products/product-1.html
/content/my-brand/products/product-2.html
```

Wanneer de Dispatcher een dergelijk verzoek ziet, wordt automatische ongeldigverklaring op de gebruikelijke manier geactiveerd en worden onmiddellijk aanvragen voor het opnieuw ophalen van nieuwe inhoud van het publicatiesysteem in de wachtrij geplaatst.

Aangezien wij nu een verzoeklichaam gebruiken, moeten wij ook inhoud-type en inhoud-lengte volgens de norm van HTTP plaatsen.

De Dispatcher markeert de URL&#39;s ook intern, zodat ze weten dat ze deze bronnen rechtstreeks kunnen leveren, ook al worden ze door automatische ongeldigmaking als ongeldig beschouwd.

Alle vermelde URL&#39;s worden één voor één aangevraagd. U hoeft zich dus geen zorgen te maken over het maken van een te hoge belasting op de publicatiesystemen. Maar u zou ook niet teveel URLs in die lijst willen zetten. Uiteindelijk, moet de rij uiteindelijk in een begrensde tijd worden verwerkt om geen waliteit voor te lange inhoud te dienen. Neem gewoon de 10 meest geopende pagina&#39;s op.

Als u de cachemap van Dispatcher bekijkt, ziet u tijdelijke bestanden gemarkeerd met tijdstempels. Dit zijn de bestanden die momenteel op de achtergrond worden geladen.

**Verwijzingen**

[ helpx.adobe.com - het ongeldig maken van Cached Pagina&#39;s van AEM ](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html)

### Het publicatiesysteem beveiligen

De Dispatcher biedt een beetje extra veiligheid door het Publish systeem tegen verzoeken te beschermen die slechts voor onderhoudsdoeleinden bestemd zijn. U wilt bijvoorbeeld geen URL&#39;s `/crx/de` of `/system/console` openbaar maken.

Het heeft geen nadelige gevolgen als er een firewall voor webtoepassingen (WAF) op uw systeem is geïnstalleerd. Maar dat voegt een aanzienlijk deel van uw begroting toe en niet alle projecten bevinden zich in een situatie waarin ze zich kunnen veroorloven en - laten we het niet vergeten - een WAF kunnen exploiteren en onderhouden.

Wat we vaak zien, is een reeks Apache herschrijft regels in Dispatcher config die toegang van de kwetsbaardere middelen verhinderen.

Maar u zou ook een verschillende benadering kunnen overwegen:

Volgens de Dispatcher-configuratie is de Dispatcher-module gebonden aan een bepaalde directory:

```
<Directory />
  SetHandler dispatcher-handler
  …
</Directory>
```

Maar waarom bindt de manager aan het volledige docroot, wanneer u later moet omlaag filtreren?

U kunt de binding van de handler in de eerste plaats beperken. `SetHandler` bindt alleen een handler aan een map, zodat de handler aan een URL of aan een URL-patroon kan worden gekoppeld:

```
<LocationMatch "^(/content|/etc/design|/dispatcher/invalidate.cache)/.\*">
  SetHandler dispatcher-handler
</LocationMatch>

<LocationMatch "^/dispatcher/invalidate.cache">
  SetHandler dispatcher-handler
</LocationMatch>

…
```

Als u dit doet, vergeet niet altijd om de verzender-manager aan Dispatcher te binden ongeldig mak URL - anders zult u geen verzoeken van AEM tot bevestiging naar Dispatcher kunnen verzenden.

U kunt de Dispatcher ook als filter gebruiken door filterinstructies in te stellen in de `dispatcher.any`

```
/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url "/content\*"  }
```

Wij geven geen toestemming voor het gebruik van de ene richtlijn ten opzichte van de andere, maar wij bevelen een passende combinatie van alle richtlijnen aan.

Maar we stellen voor dat u overweegt de URL-ruimte zo vroeg mogelijk in de keten te verkleinen, zo veel als u nodig hebt, en dat zo eenvoudig mogelijk doet. Houd er echter rekening mee dat deze technieken een WAF op zeer gevoelige websites niet vervangen. Sommige mensen noemen deze technieken &#39;Arme&#39;s firewall&#39; - om een reden.

**Verwijzingen**

[ apache.org- sethandler richtlijn ](https://httpd.apache.org/docs/2.4/mod/core.html#sethandler)

[ helpx.adobe.com - het Vormen Toegang tot de Filter van de Inhoud ](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html#ConfiguringAccesstoContentfilter)

### Filteren met reguliere expressies en algemene weergaven

In de vroege dagen kon u alleen &#39;globs&#39; gebruiken, eenvoudige plaatsaanduidingen voor het definiëren van filters in de Dispatcher-configuratie.

Gelukkig is dat veranderd in de latere versies van de Dispatcher. Nu kunt u ook reguliere POSIX-expressies gebruiken en hebt u toegang tot verschillende onderdelen van een aanvraag om een filter te definiëren. Voor iemand die net met de Dispatcher is begonnen, wat vanzelfsprekend zou kunnen zijn. Maar als je gewend bent aan alleen globs, dan is het een verrassing en gemakkelijk te vergeten. Naast de syntaxis van globs en regexes is enkel te gelijkaardig. Laten we twee versies vergelijken die hetzelfde doen:

```
# Version A

/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url "/content\*"  }

# Version B

/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url '/content.\*'  }
```

Zie je het verschil?

Versie B gebruikt enige citaten `'` om a _regelmatige uitdrukkingspatroon_ te merken. &quot;Willekeurig teken&quot; wordt uitgedrukt door `.*` te gebruiken.

_het globberen patronen_, in contrast gebruiken dubbele citaten `"` en u kunt eenvoudige placeholders zoals `*` slechts gebruiken.

Als u dat verschil kent, is het triviaal - maar als niet, kunt u de citaten gemakkelijk mengen en een zonnige namiddag doorbrengen zuiverend uw configuratie. Nu wordt u gewaarschuwd.

&quot;Ik herken `'/url'` in de config... Maar wat is dat `'/glob'` in het filter dat je vraagt?

Die richtlijn vertegenwoordigt de volledige verzoekkoord, met inbegrip van de methode en de weg. Het kan goed zijn

`"GET /content/foo/bar.html HTTP/1.1"`

dit is de tekenreeks waarmee je patroon wordt vergeleken. Beginners vergeten meestal het eerste deel, de `method` (GET, POST, ...). Dus een patroon

`/0002  { /glob "/content/\*" /type "allow" }`

Zou altijd mislukken omdat &quot;/content&quot; niet overeenkomt met &quot;GET..&quot; van het verzoek.

Dus als je Globals wilt gebruiken,

`/0002  { /glob "GET /content/\*" /type "allow" }`

zou juist zijn.

Voor aanvankelijk ontkent regel, als

`/0001  { /glob "\*" /type "deny" }`

dat is prima . Maar voor de daaropvolgende toestemming is het beter en duidelijker expressiever en veel veiliger om de afzonderlijke delen van een verzoek te gebruiken:

```
/method
/url
/path
/selector
/extension
/suffix
```

Zo:

```
/005  {

  /type "allow"
  /method "GET"
  /extension '(css|gif|ico|js|png|swf|jpe?g)' }
```

Opmerking: u kunt regex- en glob-expressies op een regel combineren.

Een laatste woord over de &#39;regelnummers&#39; als `/005` vóór elke definitie,

Ze hebben helemaal geen zin! U kunt willekeurige noemers voor regels kiezen. Het gebruik van getallen vereist niet veel moeite om na te denken over een schema, maar houd er rekening mee dat de volgorde van belang is.

Als je honderden regels hebt, zoals:

```
/001
/002
/003
…
/100
…
```

en u wilt één tussen /001 en /002 opnemen wat met de verdere aantallen gebeurt? Verhoog je cijfers? Voegt u tussen getallen in?

```
/001
/001a
/002
/003
…
/100
…
```

Of wat gebeurt als u aan reorder /003 en /001 gaat veranderen gaat u de namen en hun identiteiten of bent u

```
/003
/002
/001
…
/100
…
```

Nummering lijkt op de eerste plaats een eenvoudige keuze die op de lange termijn zijn grenzen bereikt. Laten we eerlijk zijn, getallen als id&#39;s kiezen is toch een slechte programmeerstijl.

We willen een andere aanpak voorstellen: u zult waarschijnlijk geen betekenisvolle id&#39;s voor elke filterregel krijgen. Maar ze dienen waarschijnlijk een groter doel, zodat ze op een of andere manier kunnen worden gegroepeerd op basis van dat doel. Bijvoorbeeld &#39;basisopstelling&#39;, &#39;toepassingsspecifieke uitzonderingen&#39;, &#39;globale uitzonderingen&#39; en &#39;veiligheid&#39;.

U kunt de regels vervolgens een naam geven en groeperen en de lezer van de configuratie (uw beste collega) een of andere richting geven in het bestand:

```plain
  # basic setup:

  /filter {

    # basic setup

    /basic_01  { /glob "\*"             /type "deny"  }
    /basic_02  { /glob "/content/\*"    /type "allow" }
    /basic_03  { /glob "/etc/design/\*" /type "allow" }

    /basic_04  { /extension '(json|xml)'  /type "deny"  }
    …


    # login

    /login_01 { /glob "/api/myapp/login/\*" /type "allow" }
    /login_02 { … }

    # global exceptions

    /global_01 { /method "POST" /url '.\*contact-form.html' }
```


U zult waarschijnlijk een nieuwe regel toevoegen aan een van de groepen - of misschien zelfs een nieuwe groep maken. In dat geval is het aantal items dat moet worden hernoemd/hernummerd, beperkt tot die groep.

>[!WARNING]
>
>Geavanceerde instellingen splitsen filterregels in een aantal bestanden, die worden opgenomen in het hoofdconfiguratiebestand van `dispatcher.any` . Een nieuw bestand introduceert echter geen nieuwe naamruimte. Dus als je een regel &quot;001&quot; hebt in het ene bestand en &quot;001&quot; in het andere, krijg je een fout. Nog meer reden om semantisch sterke namen te bedenken.

**Verwijzingen**

[ helpx.adobe.com - het ontwerpen van Patronen voor globale Eigenschappen ](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html#DesigningPatternsforglobProperties)

### Protocol Specificatie

De laatste tip is geen echte tip, maar we vonden het toch de moeite waard om dit met u te delen.

AEM en Dispatcher werken meestal buiten de doos. U vindt dus geen uitgebreide Dispatcher-protocolspecificatie over het validatieprotocol om uw eigen toepassing bovenaan te bouwen. De informatie is openbaar, maar een beetje verspreid over een aantal middelen.

We proberen de kloof hier enigszins te dichten. Zo ziet een aanvraag voor validatie er uit:

```
POST /dispatcher/invalidate.cache HTTP/1.1
CQ-Action: <action>
CQ-Handle: <path-pattern>
[CQ-Action-Scope]
[Content-Type: Text/Plain]
[Content-Length: <bytes in request body>]

<newline>

<refetch-url-1>
<refetch-url-2>

…

<refetch-url-n>
```

`POST /dispatcher/invalidate.cache HTTP/1.1` - De eerste lijn is URL van het de controleeindpunt van Dispatcher en u zult waarschijnlijk niet het veranderen.

`CQ-Action: <action>` - Wat moet er gebeuren? `<action>` is ofwel:

* `Activate:` delete `/path-pattern.*`
* `Deactive:` delete `/path-pattern.*`
EN verwijderen `/path-pattern/*`
* `Delete:`   delete `/path-pattern.*`
EN verwijderen `/path-pattern/*`
* `Test:`   Retourneer &quot;ok&quot; maar doe niets

`CQ-Handle: <path-pattern>` - Het pad naar de bron van de inhoud dat ongeldig moet worden gemaakt. Opmerking: `<path-pattern>` is in feite een &#39;pad&#39; en geen &#39;patroon&#39;.

`CQ-Action-Scope: ResourceOnly` - Optioneel: als deze header is ingesteld, wordt het bestand `.stat` niet gewijzigd.

```
[Content-Type: Text/Plain]
[Content-Length: <bytes in request body>]
```

Stel deze koppen in als u een lijst met automatisch vernieuwde URL&#39;s definieert. `<bytes in request body>` is het aantal tekens in de hoofdtekst van HTTP

`<newline>` - Als u een aanvraagtekst hebt, moet deze met een lege rij van de koptekst worden gescheiden.

```
<refetch-url-1>
<refetch-url-2>
…
<refetch-url-n>
```

Geef een lijst weer van de URL&#39;s die u na de validatie direct opnieuw wilt ophalen.

## Aanvullende bronnen

Een goed overzicht en inleiding aan Dispatcher caching: [ https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher.html](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher.html)

De documentatie van Dispatcher met alle verklaarde richtlijnen: [ https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)

Sommige vaak gestelde vragen: [ https://helpx.adobe.com/experience-manager/using/dispatcher-faq.html](https://helpx.adobe.com/experience-manager/using/dispatcher-faq.html)

Opname van een webinar over Dispatcher optimalisering - hoogst geadviseerd: [ https://my.adobeconnect.com/p7th2gf8k43?proto=true ](https://my.adobeconnect.com/p7th2gf8k43?proto=true)

Presentatie &quot;de ondergewaardeerde macht van inhoudsafschrijving&quot;, &quot;adjustTo ()&quot;conferentie in Potsdam 2018 [ https://adapt.to/2018/en/schedule/the-underappreciated-power-of-content-invalidation.html ](https://adapt.to/2018/en/schedule/the-underappreciated-power-of-content-invalidation.html)

Het ongeldig maken van Cached Pagina&#39;s van AEM: [ https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html)

## Volgende stap

* [2 - Infrastructuurpatroon](chapter-2.md)
