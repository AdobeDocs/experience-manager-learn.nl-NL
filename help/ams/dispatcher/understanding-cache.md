---
title: Dispatcher begrijpen dat caching plaatsvindt
description: Begrijp hoe de Dispatcher module het geheime voorgeheugen in werking stelt.
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 66ce0977-1b0d-4a63-a738-8a2021cf0bd5
duration: 407
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1708'
ht-degree: 0%

---

# Inschakelen in cache

[Inhoudsopgave](./overview.md)

[&lt;- Vorige: uitleg van configuratiebestanden](./explanation-config-files.md)

In dit document wordt uitgelegd hoe Dispatcher caching plaatsvindt en hoe het kan worden geconfigureerd

## Caching Directories

Wij gebruiken de volgende standaardgeheim voorgeheugenfolders in onze basislijninstallaties

- Auteur
   - `/mnt/var/www/author`
- Uitgever
   - `/mnt/var/www/html`

Wanneer elke aanvraag de Dispatcher doorloopt, volgen de aanvragen de geconfigureerde regels om een versie in de lokale cache te behouden als reactie op in aanmerking komende items

>[!NOTE]
>
>We houden gepubliceerde werklasten bewust gescheiden van de werkbelasting van de auteur, omdat Apache bij het zoeken naar een bestand in de DocumentRoot niet weet van welke AEM instantie het afkomstig is. Dus zelfs als u cache in de auteurshandel hebt uitgeschakeld, als DocumentRoot van de auteur het zelfde is als uitgever zal het dossiers van het geheime voorgeheugen indien heden dienen. Dit betekent dat u ontwerpbestanden vanaf de gepubliceerde cache kunt bedienen en dat u voor uw bezoekers een zeer vreselijke mix-match-ervaring zult creëren.
>
>Het is ook een erg slecht idee om afzonderlijke DocumentRoot-mappen voor verschillende gepubliceerde inhoud te houden. U zult veelvoudige re-caching punten moeten tot stand brengen die niet tussen plaatsen zoals clientlibs verschillen evenals het moeten opstelling een replicatie spoelagent voor elke DocumentRoot u opstelling. Met activering van elke pagina wordt de mate van doorspoelen over de kop verhoogd. Vertrouw op naamruimte van bestanden en de bijbehorende paden in de volledige cache en vermijd meerdere DocumentRoot&#39;s voor gepubliceerde sites.

## Configuratiebestanden

Dispatcher bepaalt wat als cacheable in de `/cache {` sectie van om het even welk landbouwbedrijfdossier kwalificeert. 
In de de basislijnconfiguratielandbouwbedrijven van AMS, zult u onze hieronder getoonde omvat vinden:


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


Wanneer het creëren van de regels voor wat om al dan niet in het voorgeheugen onder te brengen, gelieve te verwijzen naar de documentatie [ hier ](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)


## Auteur in cache

Er zijn veel implementaties die we hebben gezien waar mensen geen auteursinhoud in cache plaatsen. 
Ze missen een enorme verbetering in prestaties en responsiviteit voor hun auteurs.

Laten wij over de strategie spreken die in het vormen van onze auteurshandel wordt genomen om behoorlijk in het voorgeheugen onder te brengen.

Hier volgt een sectie van de basis auteur `/cache {` van ons dossier van de auteurshandel:


```
/cache { 
    /docroot "/mnt/var/www/author" 
    /statfileslevel "2" 
    /allowAuthorized "1" 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
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
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any" 
    } 
}
```

Het is belangrijk om hier op te merken dat `/docroot` aan de geheim voorgeheugenfolder voor auteur wordt geplaatst.

>[!NOTE]
>
>Zorg ervoor dat de `DocumentRoot` in het `.vhost` -bestand van de auteur overeenkomt met de `/docroot` -parameter

De cacheregels bevatten een instructie die het bestand `/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any` bevat dat de volgende regels bevat:

```
/0000 { 
 /glob "*" 
 /type "deny" 
} 
/0001 { 
 /glob "/libs/*" 
 /type "allow" 
} 
/0002 { 
 /glob "/libs/*.html" 
 /type "deny" 
} 
/0003 { 
 /glob "/libs/granite/csrf/token.json" 
 /type "deny" 
} 
/0004 { 
 /glob "/apps/*" 
 /type "allow" 
} 
/0005 { 
 /glob "/apps/*.html" 
 /type "deny" 
} 
/0006 { 
 /glob "/libs/cq/core/content/welcome.*" 
 /type "deny" 
}
```

In een auteurscenario, verandert de inhoud voortdurend en doelgericht. U wilt slechts punten in het voorgeheugen onderbrengen die niet vaak zullen veranderen.
We hebben regels om `/libs` in cache op te slaan, omdat ze onderdeel zijn van de basislijn AEM worden geïnstalleerd en worden gewijzigd totdat u een Service Pack, Cumulative Fix Pack, Upgrade of Hotfix hebt geïnstalleerd. Het in cache plaatsen van deze elementen heeft veel zin en heeft echt enorme voordelen van de ervaring van auteurs van eindgebruikers die de site gebruiken.

>[!NOTE]
>
>Onthoud dat deze regels ook <b>`/apps`</b> in cache plaatsen, dit is waar aangepaste toepassingscode zich bevindt. Als u uw code op deze instantie ontwikkelt dan zal het zeer verwarrend blijken te zijn wanneer u sparen uw dossier en niet ziet of in UI wegens het dienen van een caching exemplaar nadenken. De bedoeling is hier dat als u een plaatsing van uw code in AEM het doet het ook niet vaak zou zijn en een deel van uw plaatsingsstappen zou moeten zijn om het auteursgeheime voorgeheugen te ontruimen. Ook hier is het voordeel enorm, waardoor uw code sneller kan worden uitgevoerd voor de eindgebruikers.

## ServeOnStale (AKA Serve on Stale / SOS)

Dit is een van die uitingen van een kenmerk van de Dispatcher. Als de uitgever onder lading is of niet meer reageert zal het typisch een 502 of 503 http antwoordcode werpen. Als dat gebeurt en deze eigenschap wordt toegelaten zal Dispatcher worden geïnstrueerd om nog te dienen wat de inhoud nog in het geheime voorgeheugen als beste inspanning is zelfs als het geen vers exemplaar is. Het is beter om iets te dienen als u het eerder dan enkel een foutenmelding hebt tonen die geen functionaliteit aanbiedt.

>[!NOTE]
>
>Onthoud dat deze functie niet wordt geactiveerd wanneer de uitgeverrenderer een sockettime-out of een foutbericht van 500 heeft. Als AEM enkel onbereikbaar is, doet deze eigenschap niets

Dit plaatsen kan in om het even welk landbouwbedrijf worden geplaatst maar slechts steek houdt om het op toe te passen publiceert landbouwbedrijfdossiers. Hier is een syntaxisvoorbeeld van de eigenschap die in een landbouwbedrijfdossier wordt toegelaten:

```
/cache { 
    /serveStaleOnError "1"
```

## Pagina&#39;s met queryparams/argumenten in cache plaatsen

>[!NOTE]
>
>Een van de normale gedragingen van de Dispatcher-module is dat als een aanvraag een queryparameter in de URI heeft (wordt meestal weergegeven als `/content/page.html?myquery=value` ), deze het cachegeheugen van het bestand overslaat en rechtstreeks naar de AEM-instantie gaat. Het overweegt dit verzoek om een dynamische pagina en zou niet in het voorgeheugen ondergebracht moeten worden. Dit kan negatieve gevolgen voor geheim voorgeheugenefficiency veroorzaken.

Zie dit [ artikel ](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) tonen hoe de belangrijke vraagparameters uw plaatsprestaties kunnen beïnvloeden.

Standaard wilt u de `ignoreUrlParams` -regels zo instellen dat `*` wordt toegestaan.  Betekenis dat alle vraagparameters worden genegeerd en alle pagina&#39;s om ongeacht de gebruikte parameters in het voorgeheugen worden geplaatst.

Hier is een voorbeeld waarin iemand een diepgaande-koppelingsverwijzingsmechanisme voor sociale media heeft gebouwd dat de argumentverwijzing in URI gebruikt om te weten waar de persoon uit kwam.

*Genegeerde Voorbeeld:*

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

De pagina is 100% cacheable maar houdt geen cache omdat de argumenten aanwezig zijn. 
Als u `ignoreUrlParams` configureert als lijst van gewenste personen, kunt u dit probleem verhelpen:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

Nu de Dispatcher de aanvraag ziet, negeert deze het feit dat de aanvraag de `query` parameter of `?` reference heeft en toch de pagina in cache plaatst

<b> Dynamisch Voorbeeld:</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

Houd in mening dat als u vraagparameters hebt die een pagina maken het output teruggegeven maken dan zult u hen van uw genegeerde lijst moeten uitsluiten en de pagina uncacheable opnieuw maken.  Een zoekpagina die een queryparameter gebruikt, wijzigt bijvoorbeeld de weergegeven onbewerkte HTML.

Hier is de HTML-bron van elke zoekopdracht:

`/search.html?q=fruit` :

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Orange
        </div>
        <div class='result'>
            Apple
        </div>
        <div class='result'>
            Strawberry
        </div>
    </div>
</html>
```

`/search.html?q=vegetables`:

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Carrot
        </div>
        <div class='result'>
            Cucumber
        </div>
        <div class='result'>
            Celery
        </div>
    </div>
</html>
```

Als u `/search.html?q=fruit` eerst bezocht, zou het HTML met resultaten in de cache plaatsen die fruit tonen.

Vervolgens bezoekt u `/search.html?q=vegetables` seconde, maar geeft u resultaten van fruit.
De reden hiervoor is dat de queryparameter van `q` wordt genegeerd voor caching.  Om deze kwestie te vermijden zult u nota moeten nemen van pagina&#39;s die verschillende HTML die op vraagparameters worden gebaseerd teruggeven en caching voor die ontkennen.

Voorbeeld:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

Pagina&#39;s die queryparameters gebruiken via Javascript, werken nog steeds volledig zonder de parameters in deze instelling te negeren.  Omdat ze het HTML-bestand in rust niet wijzigen.  Ze gebruiken javascript om de browsers in realtime bij te werken in de lokale browser.  Betekenis dat als u de vraagparameters met javascript verbruikt het hoogst waarschijnlijk u deze parameter voor pagina caching kunt negeren.  Laat die pagina in cache plaatsen en geniet van de prestatiewinst!

>[!NOTE]
>
>Het bijhouden van deze pagina&#39;s vereist wel enige upkeep, maar is de prestatiewinst wel waard.  U kunt uw CSE vragen om een rapport op uw Websiteverkeer in werking te stellen om u een lijst van alle pagina&#39;s te geven die vraagparameters in de loop van de laatste 90 dagen voor u gebruiken om te analyseren en ervoor te zorgen u weet welke pagina&#39;s om te bekijken en welke vraagparameters om niet te negeren

## Responsheaders in cache plaatsen

Het is vrij duidelijk dat de Dispatcher `.html` pagina&#39;s en clientlibs (d.w.z. `.js`, `.css`) in cache plaatst, maar wist u dat het ook bepaalde antwoordheaders kan opslaan naast de inhoud in een bestand met dezelfde naam maar met een `.h` -bestandsextensie. Hierdoor is de volgende reactie niet alleen mogelijk op de inhoud, maar ook op de antwoordheaders die ermee moeten worden verbonden vanuit de cache.

AEM kan meer dan alleen UTF-8-codering verwerken

Soms hebben de punten speciale kopballen die helpen de het coderen van geheime voorgeheugenTTL details controleren, en laatst veranderde timestamps.

Deze waarden worden standaard verwijderd wanneer ze in de cache worden opgeslagen en de Apache httpd-webserver verwerkt het middel zelf met de normale bestandsafhandelingsmethoden. Normaal gesproken is dit beperkt tot Mime Type guessing op basis van bestandsextensies.

Als u het element en de gewenste koppen in de Dispatcher-cache hebt opgeslagen, kunt u de juiste ervaring beschikbaar maken en ervoor zorgen dat alle gegevens dit in de clientbrowser doen.

Hier is een voorbeeld van een landbouwbedrijf met de kopballen aan gespecificeerde geheime voorgeheugen:

```
/cache { 
 /headers { 
  "Cache-Control" 
  "Content-Disposition" 
  "Content-Type" 
  "Expires" 
  "Last-Modified" 
  "X-Content-Type-Options" 
 } 
}
```


In het voorbeeld hebben zij AEM gevormd om omhoog kopballen te dienen CDN zoekt om te weten wanneer om het cachegeheugen ongeldig te maken. Dit betekent dat AEM correct kan bepalen welke bestanden op basis van kopteksten ongeldig worden gemaakt.

>[!NOTE]
>
>Onthoud dat u reguliere expressies of overeenkomende glob-expressies niet kunt gebruiken. Het is een letterlijke lijst van kopballen aan geheime voorgeheugen. Plaats dit alleen in een lijst met de letterlijke koppen die u in de cache wilt plaatsen.

## Respijtperiode automatisch ongeldig maken

Op AEM systemen die veel activiteit hebben van auteurs die veel paginanavigaties uitvoeren, kunt u een zeldzame situatie hebben waarin herhaalde validaties voorkomen. Zware herhaalde verzoeken om uitspoelen zijn niet nodig en u kunt enige tolerantie gebruiken om een uitspoeling niet te herhalen totdat de respijtperiode is gewist.

### Voorbeeld van hoe dit werkt:

Als u 5 verzoeken hebt om `/content/exampleco/en/` ongeldig te maken, gebeurt dit allemaal binnen een periode van 3 seconden.

Met deze functie zou u de cachemap vijf keer ongeldig maken `/content/exampleco/en/`

Met deze eigenschap op en reeks aan 5 seconden zou het de geheim voorgeheugenfolder `/content/exampleco/en/` <b> eens </b> ongeldig maken

Hier is een voorbeeldsyntaxis van deze eigenschap die voor 5 tweede graadperiode wordt gevormd:

```
/cache { 
    /gracePeriod "5"
```

## Op TTL gebaseerde validatie

Een nieuwere functie van de Dispatcher-module was op `Time To Live (TTL)` gebaseerde validatieopties voor items in de cache. Wanneer een item in de cache wordt geplaatst, zoekt het naar de aanwezigheid van cachebeheerkoppen en genereert het een bestand in de cachemap met dezelfde naam en een extensie `.ttl` .

Hier is een voorbeeld van de eigenschap die in het dossier van de landbouwbedrijfconfiguratie wordt gevormd:

```
/cache { 
    /enableTTL "1"
```

>[!NOTE]
>
>Houd in mening dat AEM nog moet worden gevormd om de kopballen van TTL voor Dispatcher te verzenden om hen te respecteren. Als u deze functie inschakelt, kan de Dispatcher alleen weten wanneer de bestanden worden verwijderd waarvoor AEM cachebeheerkoppen heeft verzonden. Als AEM niet begint met het verzenden van TTL-headers, zal Dispatcher hier niets speciaals doen.

## Regels voor filter Cache

Hier is een voorbeeld van een basislijnconfiguratie waarvoor elementen op een uitgever in het voorgeheugen onderbrengen:

```
/cache{ 
    /0000 { 
        /glob "*" 
        /type "allow" 
    } 
    /0001 { 
        /glob "/libs/granite/csrf/token.json" 
        /type "deny" 
    }
```

We willen onze gepubliceerde site zo hebzuchtig mogelijk maken en alles in de cache plaatsen.

Als er elementen zijn die de ervaring breken wanneer caching u regels kunt toevoegen om de optie te verwijderen om dat punt in het voorgeheugen onder te brengen. Zoals u in het voorbeeld hierboven ziet, mogen de csrf-tokens nooit in de cache worden geplaatst en zijn deze uitgesloten. De verdere details bij het schrijven van deze regels kunnen [ hier worden gevonden ](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

[Volgende -> Variabelen gebruiken en begrijpen](./variables.md)
