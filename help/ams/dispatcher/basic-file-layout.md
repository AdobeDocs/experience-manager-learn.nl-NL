---
title: AMS Dispatcher Basic File Layout
description: Begrijp de basisbestandsindeling van Apache en Dispatcher.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 8a3f2bb9-3895-45c6-8bb5-15a6d2aac50e
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1161'
ht-degree: 1%

---

# Basisbestandsindeling

[Inhoudsopgave](./overview.md)

[&lt;- Vorige: wat is &quot;De verzender&quot;](./what-is-the-dispatcher.md)

In dit document wordt de standaardset configuratiebestanden van AMS beschreven en wordt de gedachte achter deze configuratiestandaard uitgelegd

## Standaardmapstructuur voor Enterprise Linux

In AMS gebruikt de basisinstallatie Enterprise Linux als het basisbesturingssysteem. Wanneer u Apache Webserver installeert, wordt standaard een installatiebestand geïnstalleerd. Hier zijn de standaardbestanden die worden geïnstalleerd door de basis-RPM&#39;s te installeren die door de yum-opslagplaats worden geleverd

```
/etc/httpd/ 
├── conf 
│   ├── httpd.conf 
│   └── magic 
├── conf.d 
│   ├── autoindex.conf 
│   ├── README 
│   ├── userdir.conf 
│   └── welcome.conf 
├── conf.modules.d 
│   ├── 00-base.conf 
│   ├── 00-dav.conf 
│   ├── 00-lua.conf 
│   ├── 00-mpm.conf 
│   ├── 00-proxy.conf 
│   ├── 00-systemd.conf 
│   └── 01-cgi.conf 
├── logs -> ../../var/log/httpd 
├── modules -> ../../usr/lib64/httpd/modules 
└── run -> /run/httpd
```

Wanneer het volgende en het naleven van het installatieontwerp/de structuur krijgen wij de volgende voordelen:

- Eenvoudiger ondersteuning voor een voorspelbare indeling
- Automatisch bekend bij iedereen die in het verleden aan Enterprise Linux HTTPD heeft gewerkt
- Hiermee worden patchcycli toegestaan die volledig worden ondersteund door het besturingssysteem zonder conflicten of handmatige aanpassingen
- Hiermee voorkomt u schendingen van SELinux van verkeerd gelabelde bestandskaders

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Opmerking:</b>
De Adobe Managed Services-serverimages hebben doorgaans kleine hoofdstations van het besturingssysteem.  Wij zetten onze gegevens in een afzonderlijk volume dat typisch in `/mnt"wordt opgezet dan gebruiken wij dat volume in plaats van de gebreken voor de volgende standaardfolders

`DocumentRoot`
- Standaard:`/var/www/html`
- AMS:`/mnt/var/www/html`

`Log Directory`
- Standaard: `/var/log/httpd`
- AMS: `/mnt/var/log/httpd`

Houd in mening dat de oude en nieuwe folders terug naar het originele onderstelpunt worden in kaart gebracht om verwarring te elimineren.
Het gebruik van een afzonderlijk volume is niet van vitaal belang, maar het is een notitie die het waard is
</div>

## AMS-invoegtoepassingen

AMS voegt een toevoeging toe aan de basisinstallatie van Apache Web Server.

### Documentrasters

AMS-standaardachterbasis voor documenten:
- Auteur:
   - `/mnt/var/www/author/`
- Publicatie:
   - `/mnt/var/www/html/`
- Onderhoud van alle vangsten en de health check
   - `/mnt/var/www/default/`

### Staging en Enabled VirtualHost Directories

Met de volgende mappen kunt u configuratiebestanden samenstellen met een parkeergebied dat u kunt bewerken aan bestanden en vervolgens alleen kunt inschakelen wanneer deze gereed zijn.
- `/etc/httpd/conf.d/available_vhosts/`
   - In deze map worden alle VirtualHost/-bestanden gehost die u noemt `.vhost`
- `/etc/httpd/conf.d/enabled_vhosts/`
   - Wanneer u klaar bent om te gebruiken `.vhost` bestanden, hebt u binnen de `available_vhosts` map die hen een relatief pad naar het `enabled_vhosts` directory

### Extra `conf.d` Mappen

Er zijn aanvullende onderdelen die veel voorkomen in Apache-configuraties en we hebben submappen gemaakt voor een schone manier om deze bestanden van elkaar te scheiden en niet alle bestanden in één directory te plaatsen

#### Map opnieuw schrijven

Deze map kan alle `_rewrite.rules` bestanden die u maakt en die uw standaard RewriteRulesyntax bevatten waarop Apache-webservers worden toegepast [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) module

- `/etc/httpd/conf.d/rewrites/`

#### Map Whitelists

Deze map kan alle `_whitelist.rules` bestanden die u maakt, die uw standaardbestanden bevatten `IP Allow` of `Require IP`syntaxis voor Apache-webservers [toegangsbesturingselementen](https://httpd.apache.org/docs/2.4/howto/access.html)

- `/etc/httpd/conf.d/whitelists/`

#### Directory voor variabelen

Deze map kan alle `.vars` bestanden die u maakt en variabelen bevatten die u in uw configuratiebestanden kunt gebruiken

- `/etc/httpd/conf.d/variables/`

### Dispatcher Module-specifieke configuratiemap

De Server van het Web van Apache is zeer verlengbaar en wanneer een module heel wat configuratiedossiers heeft is het beste praktijken om uw eigen configuratiefolder onder de installatielandfolder in plaats van het verwarren omhoog standaardte creëren.

We volgen de beste praktijken en creëerden onze eigen

#### Module configuratiebestand

- `/etc/httpd/conf.dispatcher.d/`

#### Staging en ingeschakelde kwekerij

Met de volgende mappen kunt u configuratiebestanden samenstellen met een parkeergebied dat u kunt bewerken aan bestanden en vervolgens alleen kunt inschakelen wanneer deze gereed zijn.
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - In deze map worden alle `/myfarm {` bestanden aangeroepen `_farm.any`
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - Wanneer u bereid bent om het landbouwbedrijfdossier te gebruiken, hebt u binnen de available_farm omslag symlink hen gebruikend een relatieve weg in de enabled_farm folder

### Extra `conf.dispatcher.d` Mappen

Er zijn extra stukken die subsecties zijn van de configuraties van het de landbouwbedrijfdossier van de Verzender en wij creeerden subfolders om voor een schone manier toe te staan om die dossiers te scheiden en niet alle dossiers in één folder te hebben

#### Cache Directory

Deze map bevat alle `_cache.any`, `_invalidate.any` bestanden die u maakt, bevatten uw regels voor de manier waarop u wilt dat de module caching-elementen van AEM en syntaxis voor validatieregels afhandelt.  Meer informatie over deze sectie vindt u hier [hier](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

- `/etc/httpd/conf.dispatcher.d/cache/`

#### Map met clientkoppen

Deze map kan alle `_clientheaders.any` de dossiers u creeert die lijsten van de Kopballen van de Cliënt bevatten u tot AEM wilt overgaan wanneer een verzoek binnen komt.  Meer informatie over deze sectie vindt u in [hier](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### Filtermap

Deze map kan alle `_filters.any` de dossiers u creeert die al uw filterregels bevatten om verkeer door de Verzender te blokkeren of toe te staan om AEM te bereiken

- `/etc/httpd/conf.dispatcher.d/filters/`

#### Map renderen

Deze map kan alle `_renders.any` bestanden die u maakt en die de connectiviteitsgegevens bevatten voor elke back-endserver waarvan de verzender inhoud gebruikt

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Vhosts-map

Deze map kan alle `_vhosts.any` dossiers u creeert die een lijst van de domeinnamen en wegen bevatten om met een bepaald landbouwbedrijf aan een bepaalde achterste deelserver aan te passen

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## Volledige mapstructuur

AMS heeft elk bestand gestructureerd met aangepaste bestandsextensies en met de bedoeling naamruimteproblemen/conflicten en verwarring te voorkomen.

Hier volgt een voorbeeld van een standaardbestandsset van een standaardimplementatie van AMS:

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
│   ├── 00-base.conf
│   ├── 00-dav.conf
│   ├── 00-lua.conf
│   ├── 00-mpm.conf
│   ├── 00-mpm.conf.old
│   ├── 00-proxy.conf
│   ├── 00-systemd.conf
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
├── logs -> ../../var/log/httpd
├── modules -> ../../usr/lib64/httpd/modules
└── run -> /run/httpd
```

## Het ideaal houden

Enterprise Linux heeft patchcycli voor het Apache Webserver Package (httpd).

Hoe minder geïnstalleerde standaardbestanden u hoe beter verandert, omdat als er oplossingen voor patches of configuratieverbeteringen zijn toegepast via de opdracht RPM/Yum, de correcties niet boven op een gewijzigd bestand worden toegepast.

In plaats daarvan wordt een `.rpmnew` naast het origineel.  Dit betekent u sommige veranderingen zult missen u zou kunnen gewild en meer huisvuil in uw configuratiemappen tot stand gebracht hebben.

dat wil zeggen: De RPM tijdens updateinstallatie zal bekijken `httpd.conf` als het in `unaltered` verklaren dat *vervangen* en u krijgt de essentiële updates.  Als de `httpd.conf` was `altered` dan *niet vervangen* het bestand en maakt een referentiebestand met de naam `httpd.conf.rpmnew` en de vele gewenste moeilijke situaties zullen in dat dossier zijn dat niet op de dienstopstarten van toepassing is.

Enterprise Linux is op de juiste wijze ingesteld om deze gebruikszaak op een betere manier te verwerken.  U krijgt gebieden waarin u de standaardinstellingen die u voor u instelt, kunt uitbreiden of overschrijven.  In de basisinstallatie van httpd vindt u het bestand `/etc/httpd/conf/httpd.conf`en bevat de syntaxis zoals:

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

Het idee is dat Apache wil dat u de modules en configuraties uitbreidt in het toevoegen van nieuwe bestanden aan de `/etc/httpd/conf.d/` en `/etc/httpd/conf.modules.d/` mappen met een bestandsextensie van `.conf`

Als perfect voorbeeld bij het toevoegen van de module Dispatcher aan Apache maakt u een module `.so` bestand in ` /etc/httpd/modules/` en neem het vervolgens op door een bestand toe te voegen in `/etc/httpd/conf.modules.d/02-dispatcher.conf` met de inhoud die uw module moet laden `.so` file

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Opmerking:</b>
we hebben geen bestaande Apache-bestanden gewijzigd.  In plaats daarvan hebben we gewoon onze directory's toegevoegd.
</div><br/>

Nu gebruiken we onze module in ons bestand <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> dat onze module initialiseert en het aanvankelijke module-specifieke configuratiedossier laadt

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Ook hier zult u zien dat we bestanden en modules hebben toegevoegd, maar geen originele bestanden hebben gewijzigd.  Dit geeft ons de gewenste functionaliteit en beschermt ons tegen ontbrekende gezochte flardmoeilijke situaties evenals het houden aan het hoogste niveau van verenigbaarheid met elke verbetering van het pakket.

[Volgende -> Uitleg van configuratiebestanden](./explanation-config-files.md)
