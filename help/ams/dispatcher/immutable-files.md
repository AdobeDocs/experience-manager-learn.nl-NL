---
title: AMS Dispatcher Read Only of Immuable Files
description: Begrijpen waarom sommige bestanden alleen-lezen of niet-bewerkbaar zijn en hoe u de gewenste functionele wijzigingen aanbrengt
version: Experience Manager 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 7be6b3f9-cd53-41bc-918d-5ab9b633ffb3
duration: 253
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '824'
ht-degree: 0%

---

# Alleen-lezen of Onveranderbare bestanden in AMS

[Inhoudsopgave](./overview.md)

[&lt;- Vorige: gemeenschappelijke logbestanden](./common-logs.md)

## Beschrijving

In dit document wordt beschreven in welke bestanden vergrendeld en niet gewijzigd moeten worden, en hoe u de gewenste configuratie-instellingen op de juiste wijze kunt instellen.

Wanneer AMS een systeem voorziet, ontwikkelen zij een basislijnconfiguratie die alles functioneel en veilig maakt.  Dit zijn dingen die AMS als basislijn van functionaliteit en veiligheid wil verzekeren.  Hiertoe worden sommige bestanden gemarkeerd als alleen-lezen en onveranderlijk om te voorkomen dat u ze wijzigt.

De lay-out verhindert u niet hun gedrag te veranderen en om het even welke veranderingen te schrappen u wenst.  In plaats van deze bestanden te wijzigen, bedekt u uw eigen bestand dat het origineel vervangt.

Op deze manier kunt u er zeker van zijn dat wanneer AMS de Dispatcher patches met de meest recente correcties en beveiligingsverbeteringen patcheert, deze uw bestanden niet zullen wijzigen.  Dan kunt u blijven profiteren van de verbeteringen en slechts de veranderingen goedkeuren u zou willen.
![&#x200B; toont een bowlingbaan met een bal die onderaan de weg rolt.  De bal heeft een pijl met het woord dat je toont.  De tegenstempels van de tussenruimte worden opgevoed en er staan de woorden onveranderlijke bestanden boven.](assets/immutable-files/bowling-file-immutability.png " bowling-dossier-onveranderbaarheid ")
Zoals uit bovenstaande afbeelding blijkt, blokkeren onveranderlijke bestanden je niet om de game te spelen.  Ze houden je er gewoon van om je prestatiekracht te schaden en je in de rij te houden.  Met deze methode kunnen we de weinige sleutelfuncties gebruiken:

- Aanpassingen worden verwerkt in hun eigen veilige ruimten
- De bedekking van aangepaste wijzigingen weerspiegelt de bedekkingsmethoden in AEM
- De patchconfiguraties van AMS kunnen worden gedaan zonder aanpassingen te veranderen
- De testbasis installeert versus aangepaste configuraties kunnen gelijktijdig worden gedaan helpen als de kwesties door aanpassingen of iets anders worden veroorzaakt Welke Dossiers ontdekken?


Hier volgt een lijst met bestanden die worden geïmplementeerd met een Dispatcher:

```
/etc/httpd/
├── conf
│   ├── httpd.conf
│   └── magic
├── conf.d
│   ├── 000_init_ootb_vars.conf
│   ├── 001_init_ams_vars.conf
│   ├── README
│   ├── autoindex.conf
│   ├── available_vhosts
│   │   ├── 000_unhealthy_author.vhost
│   │   ├── 000_unhealthy_publish.vhost
│   │   ├── aem_author.vhost
│   │   ├── aem_flush.vhost
│   │   ├── aem_flush_author.vhost
│   │   ├── aem_health.vhost
│   │   ├── aem_publish.vhost
│   │   └── ams_lc.vhost
│   ├── dispatcher_vhost.conf
│   ├── enabled_vhosts
│   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
│   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
│   │   ├── aem_health.vhost -> /etc/httpd/conf.d/available_vhosts/aem_health.vhost
│   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
│   ├── logformat.conf
│   ├── mimetypes3d.conf
│   ├── remoteip.conf
│   ├── rewrites
│   │   ├── base_rewrite.rules
│   │   └── xforwarded_forcessl_rewrite.rules
│   ├── security.conf
│   ├── userdir.conf
│   ├── variables
│   │   ├── ams_default.vars
│   │   └── ootb.vars
│   ├── welcome.conf
│   └── whitelists
│       └── 000_base_whitelist.rules
├── conf.dispatcher.d
│   ├── available_farms
│   │   ├── 000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any
│   │   ├── 002_ams_lc_farm.any
│   │   └── 002_ams_publish_farm.any
│   ├── cache
│   │   ├── ams_author_cache.any
│   │   ├── ams_author_invalidate_allowed.any
│   │   ├── ams_publish_cache.any
│   │   └── ams_publish_invalidate_allowed.any
│   ├── clientheaders
│   │   ├── ams_author_clientheaders.any
│   │   ├── ams_common_clientheaders.any
│   │   ├── ams_lc_clientheaders.any
│   │   └── ams_publish_clientheaders.any
│   ├── dispatcher.any
│   ├── enabled_farms
│   │   ├── 000_ams_catchall_farm.any -> ../available_farms/000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any -> ../available_farms/001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any -> ../available_farms/001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any -> ../available_farms/002_ams_author_farm.any
│   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
│   ├── filters
│   │   ├── ams_author_filters.any
│   │   ├── ams_lc_filters.any
│   │   └── ams_publish_filters.any
│   ├── renders
│   │   ├── ams_author_renders.any
│   │   ├── ams_lc_renders.any
│   │   └── ams_publish_renders.any
│   └── vhosts
│       ├── ams_author_vhosts.any
│       ├── ams_lc_vhosts.any
│       └── ams_publish_vhosts.any
├── conf.modules.d
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
└── modules - ../../usr/lib64/httpd/modules
    └── mod_dispatcher.so
```

Als u wilt bepalen welke bestanden onveranderlijk zijn, kunt u de volgende opdracht op een Dispatcher uitvoeren om te zien:

```
$ lsattr -Rl /etc/httpd 2>/dev/null | grep Immutable
```

Hier volgt een voorbeeldreactie waarvan bestanden onveranderlijk zijn:

```
/etc/httpd/conf/httpd.conf   Immutable
/etc/httpd/conf.d/available_vhosts/aem_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_health.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/ams_lc.vhost Immutable
/etc/httpd/conf.d/rewrites/base_rewrite.rules Immutable
/etc/httpd/conf.d/rewrites/xforwarded_forcessl_rewrite.rules Immutable
/etc/httpd/conf.d/whitelists/000_base_whitelist.rules Immutable
/etc/httpd/conf.d/variables/ootb.vars Immutable
/etc/httpd/conf.d/dispatcher_vhost.conf Immutable
/etc/httpd/conf.d/logformat.conf Immutable
/etc/httpd/conf.d/security.conf Immutable
/etc/httpd/conf.d/mimetypes3d.conf Immutable
/etc/httpd/conf.d/remoteip.conf Immutable
/etc/httpd/conf.d/000_init_ootb_vars.conf Immutable
/etc/httpd/conf.d/001_init_ams_vars.conf Immutable
/etc/httpd/conf.modules.d/02-dispatcher.conf Immutable
/etc/httpd/conf.dispatcher.d/available_farms/000_ams_catchall_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_author_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_publish_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_author_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_lc_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_publish_farm.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_author_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_lc_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_author_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_lc_filters.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_author_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_lc_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_author_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_lc_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/dispatcher.any Immutable
```

## Wijzigingen aanbrengen

### Variabelen

Met variabelen kunt u functionele wijzigingen doorvoeren zonder de configuratiebestanden zelf te wijzigen.  Bepaalde elementen van de configuratie kunnen worden aangepast door de waarden van variabelen aan te passen.  Hier wordt een voorbeeld van het bestand `/etc/httpd/conf.d/dispatcher_vhost.conf` weergegeven dat we kunnen markeren:

```
Include /etc/httpd/conf.d/variables/ams_default.vars
IfModule disp_apache2.c
    DispatcherConfig conf.dispatcher.d/dispatcher.any
    DispatcherLog    logs/dispatcher.log
    DispatcherLogLevel ${DISP_LOG_LEVEL}
    DispatcherDeclineRoot 0
    DispatcherUseProcessedURL 1
/IfModule
```

Zie hoe de DispatcherLogLevel-instructie een variabele van `DISP_LOG_LEVEL` heeft in plaats van de normale waarde die u daar ziet.  Boven die sectie van code zult u ook een omvat verklaring aan een veranderingendossier zien.  In het variabele bestand `/etc/httpd/conf.d/variables/ams_default.vars` wilt u het volgende zoeken.  Hier volgt de inhoud van het variabelenbestand:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define LIVECYCLE_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
Define LIVECYCLE_FORCE_SSL 1
```

Hierboven ziet u dat de huidige waarde van `DISP_LOG_LEVEL` variable `info` is.  Wij kunnen dit aanpassen aan spoor of zuiveren, of de aantalwaarde/het niveau van uw keus.  Nu overal die het logboekniveau controleert automatisch zal aanpassen.

### Bedekkingsmethode

Houd rekening met de include-bestanden op hoofdniveau, omdat dit de beginplaats is voor het maken van aanpassingen.  Om met een eenvoudig voorbeeld te beginnen, hebben we een scenario waarin we een nieuwe domeinnaam willen toevoegen die we op deze Dispatcher willen aanwijzen.  Het domeinvoorbeeld dat we gebruiken is we-retail.adobe.com.  We beginnen met het kopiëren van een bestaand configuratiebestand naar een nieuw bestand waar we onze wijzigingen kunnen toevoegen:

```
$ cp /etc/httpd/conf.d/available_vhosts/aem_publish.vhost /etc/httpd/conf.d/available_vhosts/weretail_publish.vhost
```

We hebben het bestaande bestand aem_publish.vhost gekopieerd omdat het al bevat wat we nodig hebben om dingen te laten werken en we geen reeds sterke start opnieuw willen uitvinden.  Nu bewerken we het nieuwe bestand weretail.vhost en brengen we de benodigde wijzigingen aan.

Voor:

```
VirtualHost *:80
    ServerName    publish
    ServerAlias    ${PUBLISH_DEFAULT_HOSTNAME}
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

Na:

```
VirtualHost *:80
    ServerName    weretail-publish
    ServerAlias    we-retail.adobe.com
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "werteail-publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

Nu hebben we onze `ServerName` en `ServerAlias` bijgewerkt zodat deze overeenkomen met de nieuwe domeinnamen en andere headers van de broodkruimel bijgewerkt.  Laten we nu het nieuwe bestand inschakelen zodat apache weet dat het ons nieuwe bestand gebruikt:

```
$ cd /etc/httpd/conf.d/enabled_vhosts/; ln -s ../available_vhosts/weretail_publish.vhost .
```

Nu weet apache webserver dat domein iets is waarvoor verkeer moet worden opgewekt, maar we moeten de Dispatcher-module nog steeds informeren dat deze een nieuwe domeinnaam heeft die moet worden gerespecteerd.  We maken eerst een nieuw `*_vhost.any` bestand `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` en plaatsen in dat bestand de domeinnaam die we willen respecteren:

```
"we-retail.adobe.com"
```

Nu moeten wij een nieuw landbouwbedrijfdossier maken dat ons nieuwe dossier van de gastheeringang zal gebruiken, en wij zullen beginnen door een sterk begindossier aan onze eigen nieuwe te kopiëren.

```
$ cp /etc/httpd/conf.dispatcher.d/available_farms/999_ams_publish_farm.any /etc/httpd/conf.dispatcher.d/available_farms/400_weretail_publish_farm.any
```

Laat de veranderingen tonen die wij aan dit landbouwbedrijfdossier zullen moeten maken

Voor:

```
/publishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any"
    }
........SNIP.........
}
```

Na:

```
/weretailpublishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any"
    }
........SNIP.........
}
```

Nu hebben wij de landbouwbedrijfnaam bijgewerkt, en omvat het gebruikt in de `/virtualhosts` sectie van de landbouwbedrijfconfiguratie.  Wij moeten dit nieuwe landbouwbedrijfdossier toelaten zodat kan het het in de lopende configuratie gebruiken:

```
$ cd /etc/httpd/conf.dispatcher.d/enabled_farms/; ln -s ../available_farms/400_weretail_publish_farm.any .
```

Nu kunnen we alleen de webserverservice opnieuw laden en ons nieuwe domein gebruiken!

>[!NOTE]
>
>Merk op wij slechts de stukken veranderden die wij nodig hadden om de bestaande omvat en de code te gebruiken die met de dossiers van de basislijnconfiguratie kwam.  We hoeven alleen maar af te wijken van het element dat we moeten veranderen.  Maakt het veel makkelijker en maakt het ons mogelijk minder code te handhaven

[Volgende -> Dispatcher Health Check](./health-check.md)
