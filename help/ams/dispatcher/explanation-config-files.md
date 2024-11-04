---
title: Uitleg van Dispatcher-configuratiebestanden
description: Begrijp configuratiedossiers, noemende overeenkomsten en meer.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: ec8e2804-1fd6-4e95-af6d-07d840069c8b
duration: 379
source-git-commit: ef9c70e7895176e3cd535141a5de3c49886e666e
workflow-type: tm+mt
source-wordcount: '1694'
ht-degree: 0%

---

# Uitleg van configuratiebestanden

[Inhoudsopgave](./overview.md)

[&lt;- Vorige: basisbestandsindeling](./basic-file-layout.md)

Dit document zal onderbreken en elk van de configuratiedossiers verklaren die in een standaard gebouwde server van Dispatcher worden opgesteld die in Adobe Managed Services wordt voorzien. Het gebruik ervan, de naamgevingsconventie, enz.

## Naamgevingsconventie

Apache Web Server geeft eigenlijk niet om wat de bestandsextensie is van een bestand wanneer de extensie wordt toegewezen aan een `Include` - of `IncludeOptional` -instructie.  Namen hen behoorlijk met namen die conflicten en verwarring elimineren helpt a <b> ton </b>. De namen die worden gebruikt, beschrijven het bereik van de toepassing van het bestand, waardoor het leven gemakkelijker wordt. Als alles de naam `.conf` heeft, wordt dit echt verwarrend. We willen bestanden en extensies met een lage naam voorkomen.  Hieronder volgt een lijst met de verschillende aangepaste bestandsextensies en naamgevingsconventies die worden gebruikt in een standaard Dispatcher met AMS-configuratie.

## Bestanden in conf.d/

| Bestand | Bestandsbestemming | Beschrijving |
| ---- | ---------------- | ----------- |
| BESTANDSNAAM `.conf` | `/etc/httpd/conf.d/` | Een standaard Enterprise Linux-installatie gebruikt deze bestandsextensie en neemt de map op als een plaats om de instellingen te negeren die zijn gedeclareerd in httpd.conf. U kunt in Apache aanvullende functionaliteit toevoegen op algemeen niveau. |
| BESTANDSNAAM `.vhost` | Staged: `/etc/httpd/conf.d/available_vhosts/`<br> Actief: `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><b> Nota:</b> .vhost de dossiers moeten niet in de enabled_hosts omslag maar gebruiks symlinks aan een relatieve weg aan het available_vhosts/ worden gekopieerd \*.vhost dossier </u><br><br> | \*.vhost-bestanden (virtuele host) zijn `<VirtualHosts>`  vermeldingen die overeenkomen met hostnamen en toestaan dat Apache elk domeinverkeer met verschillende regels behandelt. Vanuit het `.vhost` -bestand worden andere bestanden, zoals `rewrites` , `whitelisting` `etc` , opgenomen. |
| BESTANDSNAAM `_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | In `*_rewrite.rules` files worden `mod_rewrite` -regels opgeslagen die expliciet door een `vhost` -bestand moeten worden opgenomen en verbruikt |
| BESTANDSNAAM `_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` -bestanden worden opgenomen vanuit de `*.vhost` -bestanden. Het bevat IP regex of staat regels toe ontkennen om IP flits toe te staan. Als u de weergave van een virtuele host wilt beperken op basis van IP-adressen, genereert u een van deze bestanden en neemt u deze op van uw `*.vhost` -bestand |

## Bestanden in conf.dispatcher.d/

| Bestand | Bestandsbestemming | Beschrijving |
| --- | --- | --- |
| BESTANDSNAAM `.any` | `/etc/httpd/conf.dispatcher.d/` | De AEM Dispatcher Apache Module biedt de instellingen van `*.any` bestanden. Het standaard bovenliggende include-bestand is `conf.dispatcher.d/dispatcher.any` |
| BESTANDSNAAM `_farm.any` | Staand: `/etc/httpd/conf.dispatcher.d/available_farms/`<br> Actief: `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><b> Nota:</b> deze landbouwbedrijfdossiers moeten niet in de `enabled_farms` omslag worden gekopieerd maar gebruik `symlinks` aan een relatieve weg aan de `available_farms/*_farm.any` dossier <br/>`*_farm.any` dossiers zijn inbegrepen binnen het `conf.dispatcher.d/dispatcher.any` dossier. Deze ouderlandbouwbedrijfdossiers bestaan om modulegedrag voor elk terug te geven of websitetype te controleren. Bestanden worden gemaakt in de map `available_farms` en ingeschakeld met een `symlink` in de map `enabled_farms` .  <br/> het auto-omvat hen door naam van het `dispatcher.any` dossier.</b> de landbouwbedrijfdossiers van 0} Basislijn {beginnen met `000_` om ervoor te zorgen zij eerst worden geladen.<br/><b></b> het landbouwbedrijfdossiers van de Douane <br><b> zouden na door hun aantalregeling bij `100_` te beginnen moeten worden geladen om behoorlijk te verzekeren omvat gedrag. |
| BESTANDSNAAM `_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` -bestanden worden opgenomen vanuit de `conf.dispatcher.d/enabled_farms/*_farm.any` -bestanden. Elk landbouwbedrijf heeft een reeks regels die veranderen welk verkeer uit zou moeten worden gefiltreerd en niet het aan renderers maken. |
| BESTANDSNAAM `_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` -bestanden worden opgenomen vanuit de `conf.dispatcher.d/enabled_farms/*_farm.any` -bestanden. Deze bestanden zijn een lijst met hostnamen of uri-paden die door blob-overeenkomsten moeten worden aangepast om te bepalen welke renderer moet worden gebruikt om die aanvraag te bedienen |
| BESTANDSNAAM `_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` -bestanden worden opgenomen vanuit de `conf.dispatcher.d/enabled_farms/*_farm.any` -bestanden. Deze bestanden geven aan welke items in cache worden geplaatst en welke niet |
| BESTANDSNAAM `_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` -bestanden worden opgenomen in de `conf.dispatcher.d/enabled_farms/*_farm.any` -bestanden. Zij specificeren welke IP adressen worden toegestaan om gelijke en ongeldigingsverzoeken te verzenden. |
| BESTANDSNAAM `_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` -bestanden worden opgenomen in de `conf.dispatcher.d/enabled_farms/*_farm.any` -bestanden. Zij specificeren welke cliëntkopballen tot elke renderer zouden moeten worden overgegaan. |
| BESTANDSNAAM `_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` -bestanden worden opgenomen in de `conf.dispatcher.d/enabled_farms/*_farm.any` -bestanden. Zij specificeren IP, haven, en onderbrekingsmontages voor elke renderer. Een juiste renderer kan een livecycle-server of een AEM zijn waar de Dispatcher de aanvragen van |

## Vermijde problemen

Wanneer u de naamgevingsconventie volgt, kunt u bepaalde tamelijk eenvoudige fouten vermijden die rampzalige resultaten kunnen hebben.  We zullen enkele voorbeelden noemen.

### Probleemvoorbeeld

Als plaatsvoorbeeld voor ExampleCo werden twee configuratiedossiers gecreeerd door de ontwikkelaars van de configuraties van Dispatcher.

<b> /etc/httpd/conf.d/exampleco.conf</b>

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

<b> /etc/httpd/conf.d/rewrites/exampleco.conf</b>

```
RewriteRule ^/$ /content/exampleco/en.html [PT,L] 
RewriteRule ^/robots.txt$ /content/dam/exampleco/robots.txt [PT,L]
```

#### `POTENTIAL DANGER - The file names are the same`

Als het `vhost` -bestand per ongeluk in de `rewrites` -map wordt geplaatst en de `rewrites file` in de `vhosts` -map wordt geplaatst.  Het zou om door filename behoorlijk worden opgesteld, nog zal Apache een *FOUT* werpen en het probleem zal niet onmiddellijk duidelijk zijn.

<b> hoe dit typisch een kwestie </b> wordt

Als de `two files` naar de `same` -locatie wordt gedownload, kunnen ze dit `overwrite themselves` doen of het onherkenbaar maken. Hierdoor wordt het implementatieproces een nachtmerrie.

<b> de dossieruitbreidingen zijn het zelfde en auto-omvat prone </b>

De bestandsextensies zijn gelijk en gebruiken een automatisch opgenomen extensie die door Apache wordt `auto include` weergegeven in `.conf` -bestanden in veel van de standaardmappen van Apache.

<b> hoe dit typisch een kwestie </b> wordt

Als het hostbestand met de extensie `.conf` in de `/etc/httpd/conf.d/` -map wordt geplaatst, probeert het dit bestand te laden in het geheugen van Apache. Dit is normaal gesproken OK, maar als het bestand met herschrijfregels met de extensie `.conf` in de `/etc/httpd/conf.d/` -map wordt geplaatst, wordt het automatisch opgenomen en wordt het globaal toegepast, wat verwarrende en ongewenste resultaten oplevert.

## Resolutie

Geef de bestanden een naam die is gebaseerd op wat ze doen en veilig buiten de naamruimte van regels voor automatisch opnemen.

Als het een virtueel gastheerdossier is noem het met `.vhost` als uitbreiding.

Als het een herschrijf regeldossier is, noem het met plaats `_rewrite.rules` als achtervoegsel en uitbreiding. Deze naamgevingsconventie zal duidelijk maken voor welke site het is en dat het een set herschrijfregels is.

Als het een IP whitelist regeldossier is, noem het beschrijving `_whitelist.rules` als achtervoegsel en uitbreiding. Deze noemende overeenkomst zal wat beschrijving van geven wat het is en dat het een reeks IP passende regels is.

Als u deze naamgevingsconventies gebruikt, worden problemen voorkomen als een bestand wordt verplaatst naar een map voor automatisch opnemen die niet bij het bestand hoort.

Als u bijvoorbeeld een bestand met de naam `.rules` , `.any` of `.vhost` in de map auto-include van `/etc/httpd/conf.d/` plaatst, heeft dat geen invloed.

Als een verzoek van de plaatsingsverandering &quot;gelieve te stellen voorbeeco_rewrite.rules aan productieDispatchers&quot;zegt, kan de persoon die de veranderingen opstelt reeds weten dat zij geen nieuwe plaats toevoegen, zij enkel bijwerken herschrijft regels zoals die door filename worden vermeld.

### Volgorde opnemen

Bij het uitbreiden van functionaliteit en configuraties in Apache Webserver die op Onderneming Linux wordt geïnstalleerd, hebt u enkele belangrijke bevelen omvat u zult willen begrijpen

### Inclusief basislijn Apache

![ de Web van HTTPD- Webserver basislijn omvat ](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

Zoals in het diagram boven het httpd binaire getal wordt gezien kijkt slechts aan het httpd.conf- dossier aangezien het configuratiedossier is.  Dat bestand bevat de volgende instructies:

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### Inclusief hoofdniveau AMS

Toen wij onze norm toepasten, voegden wij enkele extra dossiertypes en van onze toe.

Hier zijn de basislijnmappen van AMS en de bovenste include-bestanden op het niveau
![ de Basislijn van AMS omvat begin met een dispatcher_vhost.conf die om het even welk dossier met *.vhost van de folder /etc/httpd/conf.d/enabled_vhosts/ zal omvatten.  De punten in /etc/httpd/conf.d/enabled_vhosts/ folder zijn symlinks naar het daadwerkelijke configuratiedossier dat in /etc/httpd/conf.d/available_vhosts/](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png " Apache-Webserver-AMS-Baseline-Includes ") leeft

Aan de hand van de basislijn van Apache laten we zien hoe AMS aanvullende mappen en include-bestanden op hoofdniveau heeft gemaakt voor `conf.d` -mappen en specifieke mappen die zijn genest onder `/etc/httpd/conf.dispatcher.d/`

Wanneer Apache wordt geladen, wordt het `/etc/httpd/conf.modules.d/02-dispatcher.conf` opgehaald en wordt het binaire bestand `/etc/httpd/modules/mod_dispatcher.so` opgenomen in de actieve staat.

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

Als u de module wilt gebruiken in onze `<VirtualHost />` , zet u een configuratiebestand neer in `/etc/httpd/conf.d/` named `dispatcher_vhost.conf` en in dit bestand ziet u hoe u de basisparameters die nodig zijn om de module te laten werken, instelt:

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Zoals u hierboven kunt zien, bevat dit het bestand op hoofdniveau `dispatcher.any` voor onze Dispatcher-module om de configuratiebestanden van deze module op te halen uit `/etc/httpd/conf.dispatcher.d/dispatcher.any`

Let op de inhoud van dit bestand:

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

Het bestand op hoofdniveau `dispatcher.any` bevat alle ingeschakelde landbouwhuisbestanden die in `/etc/httpd/conf.dispatcher.d/enabled_farms/` met de bestandsnaam `FILENAME_farm.any` wonen en die voldoen aan onze standaardnaamgevingsconventie.

Later in het eerder vermelde `dispatcher_vhost.conf` bestand voeren we ook een include-instructie uit om elk ingeschakeld virtueel hostbestand in te schakelen dat in `/etc/httpd/conf.d/enabled_vhosts/` met de bestandsnaam van `FILENAME.vhost` woont. Deze instructie volgt de standaardnaamgevingsconventie.

```
IncludeOptional /etc/httpd/conf.d/enabled_vhosts/*.vhost
```

In elk van onze .vhost-bestanden ziet u dat de Dispatcher-module wordt geïnitialiseerd als een standaardbestandshandler voor een map.  Hier volgt een voorbeeld van een .vhost-bestand met de syntaxis:

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

![ Dit beeld toont hoe één.vhost- dossier dossiers van variabelen, whitelists omvat, en omslagen ](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png " Apache-Webserver-AMS-Vhost-Omvat ") herschrijft

Wanneer een `.vhost` -bestand uit de map `/etc/httpd/conf.d/availabled_vhosts/` wordt gesymboliseerd in de map `/etc/httpd/conf.d/enabled_vhosts/` , wordt het gebruikt in de actieve configuratie.

De `.vhost` -bestanden bevatten subinclude-bestanden op basis van gemeenschappelijke items die we hebben gevonden.  Dingen als variabelen, whitelisten en herschrijven van regels.

Het `.vhost` -bestand bevat instructies voor elk bestand op basis van de locatie waar ze in het `.vhost` -bestand moeten worden opgenomen.  Hier volgt een voorbeeld van de syntaxis van een `.vhost` -bestand als een goede referentie:

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

In het bestand `/etc/httpd/conf.d/variables/weretail.vars` kunnen we zien welke variabelen zijn gedefinieerd:

```
Define MAIN_DOMAIN dev.weretail.com
```

U kunt ook een regel zien die een lijst bevat met `_whitelist.rules` -bestanden die bepalen wie deze inhoud kan bekijken op basis van verschillende whitelistcriteria.  Hiermee kunt u de inhoud van een van de bestanden in de witte lijst bekijken `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules` :

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

U kunt ook een regel zien die een set herschrijfregels bevat.  Bekijk de inhoud van het bestand `weretail_rewrite.rules` :

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### Omvat AMS-landbouwbedrijf

![<FILENAME>_farm.any zal sub.any dossiers omvatten om een landbouwbedrijfconfiguratie te voltooien.  In dit beeld kunt u zien dat een landbouwbedrijf elk geheim voorgeheugen van hoogste niveausectiedossiers, klantheaders, filters, teruggeeft, en gastheren.any- dossiers ](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png " Apache-Webserver-AMS-Farm-omvat ")

Wanneer om het even welke FILENAME_farm.om het even welke dossiers van `/etc/httpd/conf.dispatcher.d/available_farms/` folder in de `/etc/httpd/conf.dispatcher.d/enabled_farms/` folder worden gesymboliseerd zullen zij in de lopende configuratie worden gebruikt.

De landbouwbedrijfdossiers hebben sub omvat gebaseerd op [ hoogste niveausecties van het landbouwbedrijf ](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#defining-farms-farms) als geheime voorgeheugen, klantheaders, filters, teruggeeft, en gastheren.

De `FILENAME_farm.any` -bestanden bevatten instructies voor elk bestand op basis van de locatie waar de bestanden in het landbouwbedrijfsbestand moeten worden opgenomen.  Hier volgt een voorbeeld van de syntaxis van een `FILENAME_farm.any` -bestand als een goede referentie:

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

`/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any` :

```
"brand1.weretail.com" 
"brand2.weretail.com" 
"www.weretail.comf"
```

Aangezien u kunt zien is het een nieuwe lijn gescheiden lijst van domeinnamen die van dit landbouwbedrijf over anderen zou moeten teruggeven.

Kijk nu naar de `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any` :

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[Volgende -> Cache begrijpen](./understanding-cache.md)
