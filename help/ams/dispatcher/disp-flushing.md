---
title: AEM Dispatcher Flushing
description: Begrijp hoe AEM oude cachebestanden van de Dispatcher ongeldig maakt.
version: Experience Manager 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 461873a1-1edf-43a3-b4a3-14134f855d86
duration: 520
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2225'
ht-degree: 0%

---

# Dispatcher Vanity URL&#39;s

[Inhoudsopgave](./overview.md)

[&lt;- Vorige: Variabelen gebruiken en begrijpen](./variables.md)

In dit document wordt uitgelegd hoe flushing optreedt en wordt uitgelegd hoe het leegmaken en de ongeldigmaking van de cache worden uitgevoerd.


## Hoe werkt het

### Bewerkingsvolgorde

De typische workflow wordt het best beschreven wanneer de auteur van de inhoud een pagina activeert, wanneer de uitgever de nieuwe inhoud ontvangt, wordt een aanvraag voor uitspoelen naar de Dispatcher geactiveerd, zoals in het volgende diagram wordt getoond:
![ de auteur activeert inhoud, die uitgever teweegbrengt om een verzoek te verzenden om aan Dispatcher ](assets/disp-flushing/dispatcher-flushing-order-of-events.png " te flush-flushing-orde-van-gebeurtenissen ") te spoelen
Deze ketting van gebeurtenissen, benadrukt dat wij slechts punten spoelen wanneer zij nieuw of veranderd zijn.  Dit zorgt ervoor dat de inhoud door de uitgever vóór het ontruimen van het geheime voorgeheugen is ontvangen om rasvoorwaarden te vermijden waar het bloeden kon voorkomen alvorens de veranderingen van uitgever kunnen worden opgepikt.

## Replication Agents

Op auteur is er een replicatieagent die wordt gevormd om bij de uitgever te wijzen dat wanneer iets wordt geactiveerd het teweegbrengt om het dossier en alle gebiedsdelen van het naar de uitgever te verzenden.

Wanneer de uitgever het dossier ontvangt heeft het een replicatieagent die wordt gevormd om bij Dispatcher te richten die op de ontvang gebeurtenis teweegbrengt.  Vervolgens serialiseert de toepassing een aanvraag voor uitspoelen en plaatst deze op de Dispatcher.

### AUTEURREPLICATIE-AGENT

Hier zijn sommige voorbeeldschermafbeeldingen van een gevormde standaardreplicatieagent
![ schermafbeelding van standaard replicatieagent van AEM Web-pagina /etc/replication.html](assets/disp-flushing/author-rep-agent-example.png " auteur-rep-agent-voorbeeld ")

Er zijn typisch 1 of 2 replicatieagenten die op de auteur voor elke uitgever worden gevormd zij inhoud aan repliceren.

Eerst is de standaard replicatieagent die inhoudsactivering aan duwt.

Ten tweede is de reverse agent.  Dit is optioneel en is ingesteld om elke uitgever te controleren om na te gaan of er nieuwe inhoud is die in de auteur kan worden opgenomen als een omgekeerde replicatieactiviteit

### REPLICATIE-AGENT VOOR PUBLICATIE

Hier is een voorbeeld screenshots van een gevormde standaard flush replicatieagent
![ schermafbeelding van standaard flush replicatieagent van het Web-pagina van AEM /etc/replication.html](assets/disp-flushing/publish-flush-rep-agent-example.png " publish-flush-rep-agent-example ")

### DISPATCHER FLUSH REPLICATION ONTVANGT VIRTUELE HOST

De module van Dispatcher zoekt naar bepaalde kopballen om te weten wanneer een POST- verzoek iets is om tot AEM over te gaan geeft terug of als het in series wordt vervaardigd als flush verzoek en door de manager van Dispatcher zelf moet worden behandeld.

Hier is een schermafbeelding van de configuratiepagina met deze waarden:
![ beeld van de belangrijkste montages van het configuratiescherm met het Type van Rangschikking dat wordt getoond om Dispatcher Flush ](assets/disp-flushing/disp-flush-agent1.png " disp-flush-agent1 ") te zijn

De standaardinstellingspagina toont de `Serialization Type` as `Dispatcher Flush` en stelt het foutniveau in

![ Screenshot van vervoerlusje van de replicatieagent.  Dit toont URI aan post flush verzoek aan.  /dispatcher/invalidate.cache](assets/disp-flushing/disp-flush-agent2.png " disp-flush-agent2 ")

Op het tabblad `Transport` ziet u dat `URI` is ingesteld om het IP-adres te wijzen van de Dispatcher die de verzoeken om uitspoelen ontvangt.  Het pad `/dispatcher/invalidate.cache` is niet hoe de module bepaalt als het een flush is het slechts een duidelijk eindpunt is u in het toegangslogboek kunt zien om het te weten een flush verzoek is.  Op het tabblad `Extended` bespreken we de dingen die er zijn om te kwalificeren dat dit een aanvraag is om de Dispatcher-module leeg te maken.

![ Schermafbeelding van het Uitgebreide lusje van de replicatieagent.  Noteer de kopballen die met het POST- verzoek worden verzonden om Dispatcher te vertellen om ](assets/disp-flushing/disp-flush-agent3.png " disp-flush-agent3 ") te spoelen

De `HTTP Method` for flush-aanvragen zijn slechts een `GET` -aanvraag met enkele speciale aanvraagheaders:
- CQ-actie
   - Dit gebruikt een variabele van AEM die op het verzoek wordt gebaseerd en de waarde is typisch *activeer of schrap*
- CQ-handler
   - Hierbij wordt een AEM-variabele gebruikt die is gebaseerd op de aanvraag. De waarde is doorgaans het volledige pad naar het item dat bijvoorbeeld wordt leeggemaakt. `/content/dam/logo.jpg`
- CQ-pad
   - Hierbij wordt een AEM-variabele gebruikt die is gebaseerd op de aanvraag. De waarde is doorgaans het volledige pad naar het item dat wordt verwijderd, bijvoorbeeld `/content/dam`
- Host
   - In dit geval wordt de header van `Host` gespoofd om een specifieke `VirtualHost` als doel in te stellen die is geconfigureerd op de Apache-webserver van de verzender (`/etc/httpd/conf.d/enabled_vhosts/aem_flush.vhost` ).  Het is een vaste gecodeerde waarde die overeenkomt met een item in de `aem_flush.vhost` -bestanden `ServerName` of `ServerAlias`

![ Scherm van een standaardreplicatieagent die toont dat de replicatieagent met reageert en teweegbrengt wanneer de nieuwe punten van een replicatiegebeurtenis van de auteur het publiceren inhoud ](assets/disp-flushing/disp-flush-agent4.png " zijn ontvangen disp-flush-agent4 ")

Op het tabblad `Triggers` nemen we nota van de schakelbare triggers die we gebruiken en van wat ze zijn

- `Ignore default`
   - Dit wordt toegelaten zodat wordt de replicatieagent niet teweeggebracht op een paginaactivering.  Dit is iets dat wanneer een auteursinstantie een verandering in een pagina zou aanbrengen een flush zou teweegbrengen.  Omdat dit een uitgever is die wij niet van dat type van gebeurtenis willen teweegbrengen.
- `On Receive`
   - Wanneer een nieuw bestand wordt ontvangen, willen we een uitlijneffect activeren.  Dus wanneer de auteur ons een bijgewerkt bestand stuurt, activeren we een aanvraag om het bestand leeg te maken en sturen we dit naar Dispatcher.
- `No Versioning`
   - We controleren dit om te voorkomen dat de uitgever nieuwe versies genereert omdat er een nieuw bestand is ontvangen.  We vervangen gewoon het bestand dat we hebben en vertrouwen erop dat de auteur de versies bijhoudt in plaats van de uitgever.

Nu als wij kijken hoe een typisch spoelverzoek in de vorm van een `curl` bevel kijkt

```
$ curl \ 
-H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/dam/logo.jpg" \ 
-H "CQ-Path: /content/dam/" \ 
-H "Content-Length: 0" \  
-H "Content-Type: application/octect-stream" \ 
-H "Host: flush" \ 
http://10.43.0.32:80/dispatcher/invalidate.cache
```

In dit uitlijnvoorbeeld wordt het `/content/dam` -pad leeggemaakt door het `.stat` -bestand in die map bij te werken.

## Het `.stat` -bestand

Het spoelmechanisme is eenvoudig van aard en wij willen het belang verklaren van de `.stat` dossiers die in de documentwortel worden geproduceerd waar de geheim voorgeheugendossiers worden gecreeerd.

Binnen de `.vhost` en `_farm.any` dossiers vormen wij een richtlijn van de documentwortel om te specificeren waar het geheime voorgeheugen wordt gevestigd en waar te om dossiers op te slaan/te dienen van wanneer een verzoek van een eindgebruiker binnen komt.

Als u de volgende opdracht op uw Dispatcher-server zou uitvoeren, zou u `.stat` bestanden zoeken

```
$ find /mnt/var/www/html/ -type f -name ".stat"
```

Hier is een diagram van hoe deze dossierstructuur eruit zal zien wanneer u punten in het geheime voorgeheugen hebt en een spoelverzoek hebben die door de module van Dispatcher wordt verzonden en verwerkt

![ statfiles die met inhoud en data worden gemengd die met statusniveaus worden getoond ](assets/disp-flushing/dispatcher-statfiles.png " worden verzonden-statfiles ")

### BESTANDSNIVEAU STATEN

In elke map stond een `.stat` -bestand.  Dit is een indicator die een spoeling heeft plaatsgevonden.  In het voorbeeld boven werd de instelling `statfilelevel` ingesteld op `3` in het corresponderende configuratiebestand van de farm.

De instelling `statfilelevel` geeft aan hoeveel mappen er in de module staan die een `.stat` -bestand doorlopen en bijwerken.  Het .stat-bestand is leeg en is alleen een bestandsnaam met een datestamp. Het bestand kan zelfs handmatig worden gemaakt, maar u gebruikt de aanraakopdracht op de opdrachtregel van de Dispatcher-server.

Als de instelling voor het eerste bestandsniveau te hoog is, doorloopt elke aanvraag voor uitspoelen de mapstructuur die de statusbestanden aanraakt.  Dit kan een grote invloed hebben op de prestaties van grote cachestructuren en kan van invloed zijn op de algehele prestaties van uw Dispatcher.

Als u dit bestandsniveau te laag instelt, kan een aanvraag voor uitspoelen ertoe leiden dat er meer wordt gewist dan u had bedoeld.  Wat beurtelings het geheime voorgeheugen zou veroorzaken vaker met minder verzoeken worden gediend van geheim voorgeheugen en kan prestatieskwesties veroorzaken.

>[!BEGINSHADEBOX  &quot;Nota&quot;]

Stel de `statfilelevel` in op een redelijk niveau. Bekijk de mapstructuur en zorg ervoor dat deze zo is ingesteld dat beknopte opmaakelementen mogelijk zijn zonder dat u te veel mappen hoeft te doorlopen. Test het en zorg ervoor het aan uw behoeften tijdens een prestatietest van het systeem past.

Een goed voorbeeld is een site die talen ondersteunt. De typische inhoudsboom zou de volgende folders hebben:

`/content/brand1/en/us/`

In dit voorbeeld wordt een instelling 4 voor het basisbestandsniveau gebruikt. Zo weet u zeker dat wanneer u inhoud die zich in de map **`us`** bevindt, verwijdert, ook de taalmappen niet worden leeggemaakt.

>[!ENDSHADEBOX]

### TIJDSTEMPEL VAN BESTAND STAAN

Wanneer een verzoek om inhoud in de zelfde routine komt gebeurt

1. Tijdstempel van het bestand `.stat` wordt vergeleken met de tijdstempel van het aangevraagde bestand
2. Als het `.stat` -bestand nieuwer is dan het gevraagde bestand, wordt de inhoud in de cache verwijderd en wordt er een nieuw bestand opgehaald uit AEM en in de cache geplaatst.  Vervolgens wordt de inhoud weergegeven
3. Als het `.stat` -bestand ouder is dan het gevraagde bestand, weet het dat het bestand vers is en kan het de inhoud leveren.

### CACHE HANDSHAKE - VOORBEELD 1

In het bovenstaande voorbeeld wordt een aanvraag voor de inhoud weergegeven `/content/index.html`

De tijd van het `index.html` -bestand is 2019-11-01 @ 6:21PM

De tijd van het dichtstbijzijnde `.stat` bestand is 2019-11-01 @ 12:22 PM

Als u begrijpt wat we hierboven hebben gelezen, ziet u dat het indexbestand nieuwer is dan het `.stat` -bestand en dat het bestand vanuit de cache wordt verzonden naar de eindgebruiker die het heeft aangevraagd

### CACHE HANDSHAKE - VOORBEELD 2

In het bovenstaande voorbeeld wordt een aanvraag voor de inhoud weergegeven `/content/dam/logo.jpg`

De tijd van het `logo.jpg` -bestand is 2019-10-31 @ 1:13PM

De tijd van het dichtstbijzijnde `.stat` bestand is 2019-11-01 @ 12:22 PM

Zoals u in dit voorbeeld kunt zien, is het bestand ouder dan het `.stat` -bestand. Het wordt verwijderd en er wordt een nieuw bestand uit AEM opgehaald om het in de cache te vervangen voordat het wordt verzonden naar de eindgebruiker die het heeft aangevraagd.

## Farm File Settings

De documentatie is allen hier voor de volledige reeks configuratieopties: [ https://docs.adobe.com/content/help/nl-NL/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-dispatcher_configuring-the-dispatcher-cache-cache ](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=nl-NL)

We willen er een paar benadrukken die betrekking hebben op het leegmaken van de cache

### Flush-armen

Er zijn twee zeer belangrijke `document root` folders die dossiers van auteur en uitgeversverkeer in het voorgeheugen onder zullen brengen.  Als u deze mappen up-to-date wilt houden met nieuwe inhoud, moet u de cache leegmaken.  Die flush verzoeken willen niet verwikkeld raken met uw normale configuraties van het klantenverkeer die het verzoek zouden kunnen verwerpen of iets ongewenst doen.  In plaats daarvan bieden we twee uitlijningsboerderijen voor deze taak:

- `/etc/httpd.conf.d/available_farms/001_ams_author_flush_farm.any`
- `/etc/httpd.conf.d/available_farms/001_ams_publish_flush_farm.any`

Deze landbouwbedrijfdossiers doen niets behalve de folders van de documentwortel.

```
/publishflushfarm {  
    /virtualhosts {
        "flush"
    }
    /cache {
        /docroot "${PUBLISH_DOCROOT}"
        /statfileslevel "${DEFAULT_STAT_LEVEL}"
        /rules {
            $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any"
        }
        /invalidate {
            /0000 {
                /glob "*"
                /type "allow"
            }
        }
        /allowedClients {
            /0000 {
                /glob "*.*.*.*"
                /type "deny"
            }
            $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any"
        }
    }
}
```

### Hoofddocument

Deze configuratieingang leeft in de volgende sectie van het landbouwbedrijfdossier:

```
/myfarm { 
    /cache { 
        /docroot
```

U geeft de map op waarin u wilt dat de Dispatcher de cachemap vult en beheert.

>[!NOTE]
>
>Deze map moet overeenkomen met de hoofdinstelling van het Apache-document voor het domein waarvoor uw webserver is geconfigureerd.
>
>Het is om vele redenen niet erg om per bedrijf een geneste docroot-map te hebben die een submap van de hoofdmap van het Apache-document bevat.

### Bestandsniveau starten

Deze configuratieingang leeft in de volgende sectie van het landbouwbedrijfdossier:

```
/myfarm { 
    /cache { 
        /statfileslevel
```

Deze instelling bepaalt hoe diep `.stat` -bestanden moeten worden gegenereerd wanneer een aanvraag voor uitspoelen wordt weergegeven.

`/statfileslevel` ingesteld op het volgende getal met de hoofdmap van het document `/var/www/html/` heeft de volgende resultaten bij leegmaken `/content/dam/brand1/en/us/logo.jpg`

- 0 - De volgende statusbestanden worden gemaakt
   - `/var/www/html/.stat`
- 1 - De volgende statusbestanden worden gemaakt
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
- 2 - De volgende statusbestanden worden gemaakt
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
- 3 - De volgende statusbestanden worden gemaakt
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
- 4 - De volgende statusbestanden worden gemaakt
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/dam/brand1/en/.stat`
- 5 - De volgende statusbestanden worden gemaakt
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/damn/brand1/en/.stat`
   - `/var/www/html/content/damn/brand1/en/us/.stat`

>[!NOTE]
>
>Wanneer de tijdstempelhandshake plaatsvindt, zoekt deze naar het dichtstbijzijnde `.stat` -bestand.
>
>Als u alleen een `.stat` bestandsniveau 0 en een statusbestand op `/var/www/html/.stat` hebt, betekent dit dat inhoud die onder `/var/www/html/content/dam/brand1/en/us/` woont, naar het dichtstbijzijnde `.stat` -bestand zoekt en 5 mappen doorloopt om het enige `.stat` -bestand te zoeken dat op niveau 0 bestaat en datums met die gegevens vergelijkt. Betekent dat één spoeling bij dat hoge niveau van een niveau eigenlijk alle punten in het cachegeheugen ongeldig zou maken.

### Ongeldige validatie toegestaan

Deze configuratieingang leeft in de volgende sectie van het landbouwbedrijfdossier:

```
/myfarm { 
    /cache { 
        /allowedClients {
```

Binnen deze configuratie is waar u een lijst van IP adressen zet die worden toegestaan om flush verzoeken te verzenden.  Als een uitstelverzoek in de Dispatcher komt moet het van vertrouwde IP komen.  Als u dit misconfigured hebt of een flush verzoek van een niet vertrouwd IP adres verzendt zult u de volgende fout in het logboekdossier zien:

```
[Mon Nov 11 22:43:05 2019] [W] [pid 3079 (tid 139859875088128)] Flushing rejected from 10.43.0.57
```

### Invalidatieregels

Deze configuratieingang leeft in de volgende sectie van het landbouwbedrijfdossier:

```
/myfarm { 
    /cache { 
        /invalidate {
```

Deze regels geven meestal aan welke bestanden ongeldig mogen worden gemaakt met een aanvraag om de bestandsextensie te verwijderen.

Als u wilt voorkomen dat belangrijke bestanden ongeldig worden gemaakt met activering van een pagina, kunt u spelregels invoeren die aangeven welke bestanden ongeldig moeten worden gemaakt en welke bestanden handmatig ongeldig moeten worden gemaakt.  Hier volgt een voorbeeldset van configuraties waarmee alleen HTML-bestanden ongeldig kunnen worden gemaakt:

```
/invalidate { 
   /0000 { /glob "*" /type "deny" } 
   /0001 { /glob "*.html" /type "allow" } 
}
```

## Testen/problemen oplossen

Wanneer u een pagina activeert en het groene licht krijgt dat de pagina-activering is gelukt, moet u ook verwachten dat de inhoud die u hebt geactiveerd, uit de cache wordt verwijderd.

Je vernieuwt je pagina en ziet het oude spul! wat ? er was een groen licht ?!

Laten we een paar handmatige stappen door het spoelproces volgen om ons inzicht te geven in wat verkeerd zou kunnen zijn.  Voer vanuit de uitgevers-shell het volgende uitlijningsverzoek met behulp van krullen uit:

```
$ curl -H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/<PATH TO ITEM TO FLUSH>" \ 
-H "CQ-Path: /content/<PATH TO ITEM TO FLUSH>" \ 
-H "Content-Length: 0" -H "Content-Type: application/octet-stream" \ 
-H "Host: flush" \ 
http://<DISPATCHER IP ADDRESS>/dispatcher/invalidate.cache
```

Voorbeeld van een flush-aanvraag

```
$ curl -H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/customer/en-us" \ 
-H "CQ-Path: /content/customer/en-us" \ 
-H "Content-Length: 0" -H "Content-Type: application/octet-stream" \ 
-H "Host: flush" \ 
http://169.254.196.222/dispatcher/invalidate.cache
```

Zodra u het verzoekbevel aan Dispatcher hebt in brand gestoken zult u willen zien wat het in de logboeken en wat het met `.stat files` doet doet.  Tik op het logbestand en bekijk de volgende vermeldingen om te bevestigen dat de aanvraag voor uitspoelen in de Dispatcher-module terecht is gekomen

```
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Activation detected: action=Activate [/content/dam/logo.jpg] 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/dam/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] "GET /dispatcher/invalidate.cache" 200 purge [publishfarm/-] 0ms
```

Nu we de module zien oppakken en de aanvraag voor leegmaken erkennen, moeten we zien hoe deze de `.stat` -bestanden beïnvloedt.  Voer de volgende opdracht uit en bekijk de tijdstempels wanneer u een andere keer leegt:

```
$ watch -n 3 "find /mnt/var/www/html/ -type f -name ".stat" | xargs ls -la $1"
```

Zoals u kunt zien in de opdrachtuitvoer, worden de tijdstempels van de huidige `.stat` bestanden weergegeven

```
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/.stat
```

Als we de flush opnieuw uitvoeren, ziet u de tijdstempels bijwerken

```
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/.stat
```

Laten we onze tijdstempels voor inhoud vergelijken met onze tijdstempels voor `.stat` bestanden

```
$ stat /mnt/var/www/html/content/customer/en-us/.stat 
  File: `.stat' 
  Size: 0           Blocks: 0          IO Block: 4096   regular empty file 
Device: ca90h/51856d    Inode: 17154125    Links: 1 
Access: (0644/-rw-r--r--)  Uid: (   48/  apache)   Gid: (   48/  apache) 
Access: 2019-11-13 16:22:31.000000000 -0400 
Modify: 2019-11-13 16:22:31.000000000 -0400 
Change: 2019-11-13 16:22:31.000000000 -0400 
 
$ stat /mnt/var/www/html/content/customer/en-us/logo.jpg 
File: `logo.jpg' 
  Size: 15856           Blocks: 32          IO Block: 4096   regular file 
Device: ca90h/51856d    Inode: 9175290    Links: 1 
Access: (0644/-rw-r--r--)  Uid: (   48/  apache)   Gid: (   48/  apache) 
Access: 2019-11-11 22:41:59.642450601 +0000 
Modify: 2019-11-11 22:41:59.642450601 +0000 
Change: 2019-11-11 22:41:59.642450601 +0000
```

Als u naar een van de tijdstempels kijkt, ziet u dat de inhoud een nieuwere tijd heeft dan het `.stat` -bestand dat de module opgeeft het bestand te leveren vanuit de cache omdat het nieuwer is dan het `.stat` -bestand.

Plaats duidelijk iets bijgewerkt de tijdstempels van dit dossier die niet het om &quot;worden &quot;leeggemaakt&quot;of worden vervangen kwalificeren.

[Volgende -> Vanity URL&#39;s](./disp-vanity-url.md)
