---
title: Het gebruiken van en het begrip van variabelen in uw Configuratie van de AEM Dispatcher
description: Begrijp hoe te om variabelen in uw Apache en de modules van de Verzender configuratiedossiers te gebruiken om hen aan het volgende niveau te nemen.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 299b32c3-7922-4eee-aa3a-56039a654f70
duration: 307
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1090'
ht-degree: 0%

---

# Variabelen gebruiken en begrijpen

[Inhoudsopgave](./overview.md)

[&lt;- Vorige: Cache begrijpen](./understanding-cache.md)

In dit document wordt uitgelegd hoe u de kracht van variabelen in Apache-webserver en in de configuratiebestanden van de module Dispatcher kunt benutten.

## Variabelen

Apache ondersteunt variabelen en sinds versie 4.1.9 van de module Dispather ondersteunt deze ook!

We kunnen deze gebruiken om een hoop nuttige dingen te doen zoals:

- Zorg ervoor om het even wat dat milieu specifiek is niet inline in de configuraties maar wordt gehaald om configuratiedossiers van te verzekeren dev werkt in prod met de zelfde functionele output.
- Met AMS kunt u functies in- en uitschakelen en logniveaus van onveranderbare bestanden wijzigen. U kunt deze ook wijzigen.
- Wijzigen wat voor gebruik op basis van variabelen als `RUNMODE` en `ENV_TYPE`
- Overeenkomst `DocumentRoot`s en `VirtualHost` DNS-namen tussen Apache-configuraties en moduleconfiguraties.

## Basislijnvariabelen gebruiken

Omdat basislijnbestanden van AMS alleen-lezen en onveranderlijk zijn, zijn er functies die kunnen worden in- en uitgeschakeld en geconfigureerd door de variabelen die ze verbruiken te bewerken.

### Basislijnvariabelen

Standaardvariabelen van AMS worden gedeclareerd in het bestand `/etc/httpd/conf.d/variables/ootb.vars`.  Dit bestand kan niet worden bewerkt, maar bestaat om er zeker van te zijn dat variabelen geen null-waarden hebben.  Ze worden eerst daarna opgenomen dan we ze opnemen `/etc/httpd/conf.d/variables/ams_default.vars`.  U kunt dat bestand bewerken om de waarden van deze variabelen te wijzigen of zelfs dezelfde variabelennamen en -waarden in uw eigen bestand op te nemen!

Hier volgt een voorbeeld van de inhoud van het bestand `/etc/httpd/conf.d/variables/ams_default.vars`:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### Voorbeeld 1 - SSL forceren

De hierboven weergegeven variabelen `AUHOR_FORCE_SSL`, of `PUBLISH_FORCE_SSL` kan aan 1 worden geplaatst om regels te coderen herschrijft die eind - gebruikers dwingen wanneer binnen op HTTP- verzoek om aan https worden opnieuw gericht

Hier is de syntaxis van het configuratiedossier die deze knevel om toestaat te werken:

```
</VirtualHost *:80> 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  <If "${PUBLISH_FORCE_SSL} == 1"> 
   Include /etc/httpd/conf.d/rewrites/forcessl_rewrite.rules 
  </If> 
 </IfModule> 
</VirtualHost>
```

Aangezien u de herschrijfregels kunt zien omvatten is wat de code heeft om eindgebruikers browser om te leiden, maar de variabele die aan 1 wordt geplaatst is wat het dossier om toestaat te gebruiken of niet

### Voorbeeld 2 - Logboekniveau

De variabelen `DISP_LOG_LEVEL` kan worden gebruikt om te plaatsen wat u voor het logboekniveau wilt hebben dat eigenlijk in de lopende configuratie wordt gebruikt.

Hier is het syntaxisvoorbeeld dat in de dossiers van de de basislijnconfiguratie van ams bestaat:

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

Als u het registratieniveau van de Dispatcher moet verhogen werkt u gewoon de `ams_default.vars` variabel `DISP_LOG_LEVEL` op het gewenste niveau.

Voorbeeldwaarden kunnen een geheel getal of het woord zijn:

| Logboekniveau | Waarde van geheel getal | Woordwaarde |
| --- | --- | --- |
| Traceren | 4 | traceren |
| Foutopsporing | 3 | foutopsporing |
| Info | 2 | info |
| Waarschuwing | 1 | waarschuwen |
| Fout | 0 | fout |

### Voorbeeld 3 - Whitelists

De variabelen `AUTHOR_WHITELIST_ENABLED` en `PUBLISH_WHITELIST_ENABLED` kan aan 1 worden geplaatst om in werking te stellen herschrijft regels die regels omvatten om eindgebruikersverkeer toe te staan of te ontkennen dat op IP adres wordt gebaseerd.  Als u deze functie inschakelt, moet u deze ook combineren met het maken van een bestand met whitelist-regels.

Hier volgen enkele syntaxisvoorbeelden van de manier waarop de variabele de opname van whitelist-bestanden en een whitelist-bestandsvoorbeeld inschakelt

`sample.vhost`:

```
<VirtualHost *:80> 
 <Directory /> 
  <If "${AUTHOR_WHITELIST_ENABLED} == 1"> 
   Include /etc/httpd/conf.d/whitelists/*_whitelist.rules 
  </If> 
 </Directory> 
</VirtualHost>
```

`sample_whitelist.rules`:

```
<RequireAny> 
  Require ip 10.43.0.10/24 
</RequireAny>
```

Zoals u kunt zien `sample_whitelist.rules` dwingt de IP beperking af maar het knepen van de variabele staat het toe om in te omvatten `sample.vhost`

## Waar moeten de variabelen worden geplaatst?

### Opstartargumenten webserver

AMS zal server/topologie specifieke variabelen in de startargumenten van het Apache-proces in het dossier zetten `/etc/sysconfig/httpd`

Dit bestand bevat vooraf gedefinieerde variabelen, zoals hieronder wordt weergegeven:

```
AUTHOR_IP="10.43.0.59" 
AUTHOR_PORT="4502" 
AUTHOR_DOCROOT='/mnt/var/www/author' 
PUBLISH_IP="10.43.0.20" 
PUBLISH_PORT="4503" 
PUBLISH_DOCROOT='/mnt/var/www/html' 
ENV_TYPE='dev' 
RUNMODE='sites'
```

Dit is niet iets u kunt veranderen maar aan hefboomwerking in uw configuratiedossiers goed zijn

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Opmerking:</b>

Omdat dit bestand alleen wordt opgenomen wanneer de service wordt gestart.  U moet de service opnieuw starten om de wijzigingen op te halen.  Betekenis een herladen is niet genoeg maar een nieuw begin in plaats daarvan is nodig
</div>

### Variabelebestanden (`.vars`)

De variabelen van de douane die door uw code worden verstrekt zouden in moeten leven `.vars` bestanden in de map `/etc/httpd/conf.d/variables/`

Deze bestanden kunnen aangepaste variabelen bevatten die u wilt en enkele syntaxisvoorbeelden zijn te vinden in de volgende voorbeeldbestanden

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`:

```
Define WERETAIL_DOMAIN dev.weretail.com 
Define WERETAIL_ALT_DOMAIN dev.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`:

```
Define WERETAIL_DOMAIN stage.weretail.com
Define WERETAIL_ALT_DOMAIN stage.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_prod.vars`:

```
Define WERETAIL_DOMAIN www.weretail.com 
Define WERETAIL_ALT_DOMAIN www..weretail.net
```

Als u uw eigen variabelen maakt, geeft u deze bestanden een naam op basis van de inhoud en volgt u de naamgevingsstandaarden in de handleiding [hier](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention).  In het bovenstaande voorbeeld ziet u dat het variabelenbestand de verschillende DNS-vermeldingen bevat als variabelen die in de configuratiebestanden moeten worden gebruikt.

## Variabelen gebruiken

Nu u uw variabelen binnen uw variabelendossiers hebt bepaald zult u willen weten hoe te om hen behoorlijk binnen uw andere configuratiedossiers te gebruiken.

We gebruiken het voorbeeld `.vars` bestanden van bovenaf om een juiste gebruiksaanwijzing te illustreren.

We willen alle op de omgeving gebaseerde variabelen opnemen die we maken. `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

We weten dat wanneer de httpd-service wordt opgestart, deze de variabelen ophaalt die AMS in `/etc/sysconfig/httpd` en heeft de variabele set van `ENV_TYPE` en `RUNMODE`

Wanneer dit globaal `.conf` Het bestand wordt in het begin opgehaald, omdat de volgorde van de bestanden in het bestand `conf.d` is een alfanumerieke laadvolgorde, gemiddeld 000 in de bestandsnaam, zodat deze voor de andere bestanden in de map wordt geladen.

De instructie include gebruikt ook een variabele in de bestandsnaam.  Hiermee kunt u wijzigen welk bestand daadwerkelijk wordt geladen op basis van de waarde in het dialoogvenster `ENV_TYPE` en `RUNMODE` variabelen.

Als de `ENV_TYPE` waarde is `dev` het bestand dat wordt gebruikt, is:

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

Als de `ENV_TYPE` waarde is `stage` het bestand dat wordt gebruikt, is:

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

Als de `RUNMODE` waarde is `preview` het bestand dat wordt gebruikt, is:

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

Als dat bestand wordt opgenomen, kunnen we de variabelenamen gebruiken die in dat bestand zijn opgeslagen.

In onze `/etc/httpd/conf.d/available_vhosts/weretail.vhost` kunnen wij de normale syntaxis ruilen die slechts voor dev werkte:

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

Met een nieuwere syntaxis die de kracht van variabelen gebruikt om te werken voor ontwikkelen, werkgebied en staaf:

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

In onze `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` kunnen wij de normale syntaxis ruilen die slechts voor dev werkte:

```
"dev.weretail.com" 
"dev.weretail.net"
```

Met een nieuwere syntaxis die de kracht van variabelen gebruikt om te werken voor ontwikkelen, werkgebied en staaf:

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

Deze variabelen worden enorm hergebruikt om de instellingen te individualiseren zonder dat er verschillende bestanden per omgeving moeten worden geïmplementeerd.  U kunt uw configuratiebestanden in feite sjablonen met behulp van variabelen en bestanden opnemen op basis van variabelen.

## Waarden van variabelen weergeven

Soms wanneer het gebruiken van variabelen moeten wij zoeken om te zien wat de waarden in onze configuratiedossiers zouden kunnen zijn.  U kunt de opgeloste variabelen weergeven door de volgende opdrachten op de server uit te voeren:

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

Hoe de variabelen in uw gecompileerde configuratie Apache keken:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

Hoe de variabelen in uw gecompileerde configuratie van de Verzender keken:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

Van de output van de bevelen zult u de verschillen van de variabele in het config dossier tegenover de gecompileerde output zien.

Voorbeeldconfiguratie

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`:

```
<VirtualHost *:80> 
    DocumentRoot    ${PUBLISH_DOCROOT} 
```

Voer nu de opdrachten uit om de gecompileerde uitvoer te zien

Gecompileerde Apache-configuratie:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

Configuratieweergave gecompileerde verzendingen:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[Volgende -> Spoelen](./disp-flushing.md)
