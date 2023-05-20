---
title: Uitleg van Dispatcher-configuratiebestanden
description: Begrijp configuratiedossiers, noemende overeenkomsten en meer.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: ec8e2804-1fd6-4e95-af6d-07d840069c8b
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '1705'
ht-degree: 0%

---

# Uitleg van configuratiebestanden

[Inhoudsopgave](./overview.md)

[&lt;- Vorige: Basisbestandsindeling](./basic-file-layout.md)

In dit document worden de configuratiebestanden die zijn geïmplementeerd in een standaard geïntegreerde Dispatcher-server die is ingericht in Adobe Managed Services, uitgesplitst en uitgelegd. Het gebruik ervan, de naamgevingsconventie, enz.

## Naamgevingsconventie

De Server van het Web van Apache geeft eigenlijk niet wat de dossieruitbreiding van een dossier wanneer het richten van het met een dossier is `Include` of `IncludeOptional` instructie.  Als u deze namen correct benoemt met namen die conflicten en verwarring voorkomen, kan een <b>ton</b>. De namen die worden gebruikt, beschrijven het bereik van de toepassing van het bestand, waardoor het leven gemakkelijker wordt. Als alles een naam heeft `.conf` dit wordt echt verwarrend . We willen bestanden en extensies met een lage naam voorkomen.  Hieronder volgt een lijst met de verschillende aangepaste bestandsextensies en naamgevingsconventies die worden gebruikt in een standaard AMS geconfigureerde Dispatcher.

## Bestanden in conf.d/

| Bestand | Bestandsbestemming | Beschrijving |
| ---- | ---------------- | ----------- |
| FILENAME`.conf` | `/etc/httpd/conf.d/` | Een standaard Enterprise Linux-installatie gebruikt deze bestandsextensie en neemt de map op als een plaats om de instellingen te negeren die zijn gedeclareerd in httpd.conf. U kunt in Apache aanvullende functionaliteit toevoegen op algemeen niveau. |
| FILENAME`.vhost` | Staand: `/etc/httpd/conf.d/available_vhosts/`<br>Actief: `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Opmerking:</b> .vhost-bestanden hoeven niet naar de map enabled_hosts te worden gekopieerd, maar gebruiken symbolische koppelingen naar een relatief pad naar het bestand available_vhosts/\*.vhost</div></u><br><br> | \*.vhost-bestanden (virtuele host) zijn `<VirtualHosts>`  vermeldingen die overeenkomen met hostnamen en toestaan dat Apache elk domeinverkeer met verschillende regels behandelt. Van de `.vhost` bestand, andere bestanden, zoals `rewrites`, `whitelisting`, `etc` worden opgenomen. |
| FILENAME`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | `*_rewrite.rules` bestandsopslag `mod_rewrite` regels die expliciet door een `vhost` file |
| FILENAME`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` bestanden worden opgenomen vanuit de `*.vhost` bestanden. Het bevat IP regex of staat regels toe ontkennen om IP flits toe te staan. Als u probeert om het bekijken van een virtuele gastheer te beperken die op IP adressen wordt gebaseerd, zult u één van deze dossiers produceren en het van uw omvatten `*.vhost` file |

## Bestanden in conf.dispatcher.d/

| Bestand | Bestandsbestemming | Beschrijving |
| --- | --- | --- |
| FILENAME`.any` | `/etc/httpd/conf.dispatcher.d/` | De Apache Module van de AEM Dispatcher produceert het montages van `*.any` bestanden. Het standaard bovenliggende include-bestand is `conf.dispatcher.d/dispatcher.any` |
| FILENAME`_farm.any` | Staand: `/etc/httpd/conf.dispatcher.d/available_farms/`<br>Actief: `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Opmerking:</b> deze landbouwbedrijfdossiers moeten niet in worden gekopieerd `enabled_farms` map maar gebruik `symlinks` naar een relatief pad naar `available_farms/*_farm.any` file </div> <br/>`*_farm.any` bestanden worden opgenomen in de `conf.dispatcher.d/dispatcher.any` bestand. Deze ouderlandbouwbedrijfdossiers bestaan om modulegedrag voor elk te controleren teruggeeft of websitetype. Bestanden worden gemaakt in het dialoogvenster `available_farms` en ingeschakeld met een `symlink` in de `enabled_farms` directory.  <br/>Deze worden automatisch op naam opgenomen vanuit het menu `dispatcher.any` bestand.<br/><b>Basislijn</b> de landbouwbedrijfdossiers beginnen met `000_` om ervoor te zorgen dat ze eerst worden geladen.<br><b>Aangepast</b> de landbouwbedrijfdossiers zouden na moeten worden geladen door hun aantalregeling te beginnen bij `100_` om ervoor te zorgen dat het gedrag van include correct is. |
| FILENAME`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` bestanden worden opgenomen vanuit de `conf.dispatcher.d/enabled_farms/*_farm.any` bestanden. Elk landbouwbedrijf heeft een reeks regels die veranderen welk verkeer uit zou moeten worden gefiltreerd en niet het aan renderers maken. |
| FILENAME`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` bestanden worden opgenomen vanuit de `conf.dispatcher.d/enabled_farms/*_farm.any` bestanden. Deze bestanden zijn een lijst met hostnamen of uri-paden die door blob-overeenkomsten moeten worden aangepast om te bepalen welke renderer moet worden gebruikt om die aanvraag te bedienen |
| FILENAME`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` bestanden worden opgenomen vanuit de `conf.dispatcher.d/enabled_farms/*_farm.any` bestanden. Deze bestanden geven aan welke items in cache worden geplaatst en welke niet |
| FILENAME`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` bestanden worden opgenomen in de `conf.dispatcher.d/enabled_farms/*_farm.any` bestanden. Zij specificeren welke IP adressen worden toegestaan om gelijke en ongeldigingsverzoeken te verzenden. |
| FILENAME`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` bestanden worden opgenomen in de `conf.dispatcher.d/enabled_farms/*_farm.any` bestanden. Zij specificeren welke cliëntkopballen tot elke renderer zouden moeten worden overgegaan. |
| FILENAME`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` bestanden worden opgenomen in de `conf.dispatcher.d/enabled_farms/*_farm.any` bestanden. Zij specificeren IP, haven, en onderbrekingsmontages voor elke renderer. Een juiste renderer kan een levenscyclusserver of om het even welke AEM systemen zijn waar de Dispatcher de verzoeken van kan halen/volmacht |

## Vermijde problemen

Wanneer u de naamgevingsconventie volgt, kunt u bepaalde tamelijk eenvoudige fouten vermijden die rampzalige resultaten kunnen hebben.  We zullen enkele voorbeelden noemen.

### Probleemvoorbeeld

Als plaatsvoorbeeld voor ExampleCo werden twee configuratiedossiers gecreeerd door de ontwikkelaars van de configuraties van de Verzender.

<b>/etc/httpd/conf.d/exampleco.conf</b>

```
<VirtualHost *:80> 
    ServerName  "exampleco" 
    ServerAlias "www.exampleco.com" 
    .......... SNIP ............... 
    <IfModule mod_rewrite.c> 
        ReWriteEngine   on 
        LogLevel warn rewrite:trace1 
        Include /etc/httpd/conf.d/rewrites/exampleco.conf 
    </IfModule> 
</VirtualHost>
```

<b>/etc/httpd/conf.d/rewrites/exampleco.conf</b>

```
RewriteRule ^/$ /content/exampleco/en.html [PT,L] 
RewriteRule ^/robots.txt$ /content/dam/exampleco/robots.txt [PT,L]
```

#### `POTENTIAL DANGER - The file names are the same`

Als de `vhost` bestand wordt per ongeluk in het `rewrites` en de `rewrites file` wordt in de `vhosts` map.  De bestandsnaam van de toepassing lijkt correct te zijn, maar Apache geeft een *FOUT* en het probleem zal niet meteen duidelijk zijn .

<b>Hoe dit typisch een kwestie wordt</b>

Als de `two files` worden gedownload naar de `same` de locatie die ze kunnen kiezen `overwrite themselves` of maakt het van het ondeelbaar maken van het plaatsingsproces een nachtmerrie.

<b>De bestandsextensies zijn hetzelfde en worden automatisch opgenomen</b>

De bestandsextensies zijn gelijk en gebruiken de automatisch opgenomen extensie die Apache zal gebruiken `auto include` alle `.conf` bestanden in veel van de standaardmappen.

<b>Hoe dit typisch een kwestie wordt</b>

Als het vhost-bestand met de extensie `.conf` wordt in de `/etc/httpd/conf.d/` map wordt geprobeerd deze te laden in het geheugen van Apache. Dit is normaal gesproken OK, maar als het bestand met herschrijfregels wordt bijgewerkt met de extensie `.conf` wordt geplaatst in `/etc/httpd/conf.d/` automatisch worden opgenomen en worden globaal toegepast, wat leidt tot verwarring en ongewenste resultaten.

## Resolutie

Geef de bestanden een naam die is gebaseerd op wat ze doen en veilig buiten de naamruimte van regels voor automatisch opnemen.

Als het een virtueel gastheerdossier is - noem het met `.vhost` als de extensie.

Als het een bestand met herschrijfregels is, geeft u dit een naam met de site`_rewrite.rules` als achtervoegsel en extensie. Deze naamgevingsconventie zal duidelijk maken voor welke site het is en dat het een set herschrijfregels is.

Als het een IP whitelist regeldossier is, noem het Beschrijving`_whitelist.rules` als achtervoegsel en extensie. Deze noemende overeenkomst zal wat beschrijving van geven wat het is en dat het een reeks IP passende regels is.

Als u deze naamgevingsconventies gebruikt, worden problemen voorkomen als een bestand wordt verplaatst naar een map voor automatisch opnemen die niet bij het bestand hoort.

Bijvoorbeeld het plaatsen van een dossier genoemd met `.rules`, `.any`, of `.vhost` in de map autoInclude van `/etc/httpd/conf.d/` zou geen invloed hebben.

Als een verzoek van de plaatsingsverandering &quot;gelieve te stellen voorbeeco_rewrite.rules aan productieDispatchers&quot;zegt, kan de persoon die de veranderingen opstelt reeds weten dat zij geen nieuwe plaats toevoegen, zij enkel bijwerken herschrijft regels zoals die door filename worden vermeld.

### Volgorde opnemen

Bij het uitbreiden van functionaliteit en configuraties in Apache Webserver die op Onderneming Linux wordt geïnstalleerd, hebt u enkele belangrijke bevelen omvat u zult willen begrijpen

### Inclusief basislijn Apache

![](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

Zoals in het diagram boven het httpd binaire getal wordt gezien kijkt slechts aan het httpd.conf- dossier aangezien het configuratiedossier is.  Dat bestand bevat de volgende instructies:

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### Inclusief hoofdniveau AMS

Toen wij onze norm toepasten, voegden wij enkele extra dossiertypes en van onze toe.

Hier zijn de basislijnmappen van AMS en de bovenste include-bestanden op het niveau
![AMS Baseline omvat begin met een dispatcher_vhost.conf die om het even welk dossier met *.vhost van de folder /etc/httpd/conf.d/enabled_vhosts/ zal omvatten.  Items in de map /etc/httpd/conf.d/enabled_vhosts/ zijn symbolische koppelingen naar het feitelijke configuratiebestand dat zich in /etc/httpd/conf.d/available_vhosts/ bevindt](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "Apache-Webserver-AMS-Baseline-Includes")

Bij het samenstellen van de basislijn van Apache laten we zien hoe AMS aanvullende mappen en include-bestanden op hoofdniveau heeft gemaakt voor `conf.d` mappen en modulespecifieke mappen die onder `/etc/httpd/conf.dispatcher.d/`

Wanneer Apache wordt geladen, wordt deze `/etc/httpd/conf.modules.d/02-dispatcher.conf` en dat bestand bevat het binaire bestand `/etc/httpd/modules/mod_dispatcher.so` in staat van rennen.

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

Als u de module in onze `<VirtualHost />` wij laten vallen een configuratiedossier in `/etc/httpd/conf.d/` benoemd `dispatcher_vhost.conf` en binnen dit dossier zult u gebruiksopstelling de basisparameters nodig voor de module zien werken:

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Zoals u hierboven kunt zien, omvat dit het hoogste niveau `dispatcher.any` bestand voor onze Dispatcher-module om de configuratiebestanden van deze module op te halen `/etc/httpd/conf.dispatcher.d/dispatcher.any`

Let op de inhoud van dit bestand:

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

Het hoogste niveau `dispatcher.any` het dossier omvat alle toegelaten landbouwbedrijfdossiers die in leven `/etc/httpd/conf.dispatcher.d/enabled_farms/` met de bestandsnaam van `FILENAME_farm.any` volgens onze standaardnaamgevingsconventie.

Later in de `dispatcher_vhost.conf` het dossier vroeger vermeld doen wij ook omvat verklaring die elk toegelaten virtuele gastheerdossiers toelaat die in leven `/etc/httpd/conf.d/enabled_vhosts/` met de bestandsnaam van `FILENAME.vhost` volgens onze standaardnaamgevingsconventie.

```
IncludeOptional /etc/httpd/conf.d/enabled_vhosts/*.vhost
```

In elk van onze .vhost-bestanden ziet u dat de module Dispatcher is geïnitialiseerd als een standaardbestandshandler voor een map.  Hier volgt een voorbeeld van een .vhost-bestand met de syntaxis:

```
<VirtualHost *:80> 
 ServerName "weretail" 
 ServerAlias www.weretail.com weretail.com 
 <Directory /> 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
</VirtualHost>
```

Nadat het hoogste niveau omvat oplossen hebben zij andere sub omvat die het vermelden waard zijn.  Hier is een diagram op hoog niveau op hoe de landbouwbedrijven en de gastheren dossiers andere subelementen omvatten

### Inclusief virtuele AMS-host

![In deze afbeelding ziet u hoe een .vhost-bestand bestanden bevat van variabelen, whitelists en mappen opnieuw schrijven](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png "Apache-Webserver-AMS-Vhost-Includes")

Indien `.vhost` bestanden van `/etc/httpd/conf.d/availabled_vhosts/` directory wordt gesymkoppeld in de `/etc/httpd/conf.d/enabled_vhosts/` directory zij zullen in de lopende configuratie worden gebruikt.

De `.vhost` bestanden hebben subinclude-bestanden op basis van gemeenschappelijke gegevens die we hebben gevonden.  Dingen als variabelen, whitelisten en herschrijven van regels.

De `.vhost` het bestand bevat instructies voor elk bestand op basis van de locatie waar het bestand moet worden opgenomen in het `.vhost` bestand.  Hier volgt een voorbeeldsyntaxis van een `.vhost` bestand als een goede referentie:

```
Include /etc/httpd/conf.d/variables/weretail.vars 
<VirtualHost *:80> 
 ServerName "${MAIN_DOMAIN}" 
 <Directory /> 
  Include /etc/httpd/conf.d/whitelists/weretail*_whitelist.rules 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
 <IfModule mod_rewrite.c> 
  ReWriteEngine   on 
  LogLevel warn rewrite:trace1 
  Include /etc/httpd/conf.d/rewrites/weretail_rewrite.rules 
 </IfModule> 
</VirtualHost>
```

Zoals u in het bovenstaande voorbeeld kunt zien, is er een include-bestand voor de variabelen die nodig zijn in dit configuratiebestand die later worden gebruikt.

In het bestand `/etc/httpd/conf.d/variables/weretail.vars` we kunnen zien welke variabelen er zijn :

```
Define MAIN_DOMAIN dev.weretail.com
```

U kunt ook een regel zien die een lijst bevat met `_whitelist.rules` bestanden die bepalen wie deze inhoud kan bekijken op basis van verschillende whitelistcriteria.  Hiermee kunt u de inhoud van een van de bestanden in de witte lijst bekijken `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules`:

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

U kunt ook een regel zien die een set herschrijfregels bevat.  Laten we eens kijken naar de inhoud van de `weretail_rewrite.rules` bestand:

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### Omvat AMS-landbouwbedrijf

![<FILENAME>_farm.any zal sub.any dossiers omvatten om een landbouwbedrijfconfiguratie te voltooien.  In dit beeld kunt u zien dat een landbouwbedrijf elk hoofdniveau sectiedossiers geheim voorgeheugen, klantheaders, filters, teruggeeft en gastheren.om het even welke dossiers zal omvatten](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-Webserver-AMS-Farm-Includes")

Wanneer om het even welke FILENAME_farm.om het even welke dossiers van `/etc/httpd/conf.dispatcher.d/available_farms/` directory wordt gesymkoppeld in de `/etc/httpd/conf.dispatcher.d/enabled_farms/` directory zij zullen in de lopende configuratie worden gebruikt.

De landbouwbedrijfdossiers hebben sub omvat gebaseerd op [topniveausecties van het landbouwbedrijf](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#defining-farms-farms) zoals cache, clientheaders, filters, renders en hosts.

De `FILENAME_farm.any` de dossiers zullen verklaringen voor elk dossier omvatten dat op waar wordt gebaseerd zij in het landbouwbedrijfdossier moeten worden omvat.  Hier volgt een voorbeeldsyntaxis van een `FILENAME_farm.any` bestand als een goede referentie:

```
/weretailfarm {   
 /clientheaders { 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any" 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any" 
 } 
 /virtualhosts { 
  $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any" 
 } 
 /renders { 
  $include "/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any" 
 } 
 /filter { 
  $include "/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any" 
  $include "/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any" 
 } 
 ....SNIP.... 
 /cache { 
  ....SNIP.... 
  /rules { 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any" 
  } 
  ....SNIP.... 
  /allowedClients { 
   /0000 { 
    /glob "*.*.*.*" 
    /type "deny" 
   } 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any" 
  } 
 ....SNIP.... 
 } 
}
```

Aangezien u elke sectie voor het weretail landbouwbedrijf in plaats van het hebben van alle syntaxis nodig kunt zien het in plaats daarvan het gebruiken omvat verklaring.

Laten we eens kijken naar de syntaxis van een paar van deze include-bestanden om te zien hoe elke include-code eruit zou zien

`/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any`:

```
"brand1.weretail.com" 
"brand2.weretail.com" 
"www.weretail.comf"
```

Aangezien u kunt zien is het een nieuwe lijn gescheiden lijst van domeinnamen die van dit landbouwbedrijf over anderen zou moeten teruggeven.

Laten we nu eens kijken naar de `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`:

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[Volgende -> Cache begrijpen](./understanding-cache.md)
