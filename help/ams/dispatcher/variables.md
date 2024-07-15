---
title: Variabelen in uw AEM Dispatcher-configuratie gebruiken en begrijpen
description: Begrijp hoe u variabelen in uw configuratiebestanden van Apache- en Dispatcher-modules kunt gebruiken om deze naar het volgende niveau te brengen.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 299b32c3-7922-4eee-aa3a-56039a654f70
duration: 260
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 0%

---

# Variabelen gebruiken en begrijpen

[Inhoudsopgave](./overview.md)

[&lt;- Vorige: Cache begrijpen](./understanding-cache.md)

In dit document wordt uitgelegd hoe u de mogelijkheden van variabelen in Apache-webservers en in Dispatcher-moduleconfiguratiebestanden kunt benutten.

## Variabelen

Apache ondersteunt variabelen en sinds versie 4.1.9 van de module Dispather ondersteunt deze ook!

We kunnen deze gebruiken om een hoop nuttige dingen te doen zoals:

- Zorg ervoor om het even wat dat milieu specifiek is niet inline in de configuraties maar wordt gehaald om configuratiedossiers van te verzekeren dev werkt in prod met de zelfde functionele output.
- Met AMS kunt u functies in- en uitschakelen en logniveaus van onveranderbare bestanden wijzigen. U kunt deze ook wijzigen.
- Wijzigen wat op variabelen als `RUNMODE` en `ENV_TYPE` wordt gebruikt
- Kies dezelfde DNS-namen als `DocumentRoot` en `VirtualHost` tussen Apache-configuraties en moduleconfiguraties.

## Basislijnvariabelen gebruiken

Omdat basislijnbestanden van AMS alleen-lezen en onveranderlijk zijn, zijn er functies die kunnen worden in- en uitgeschakeld en geconfigureerd door de variabelen die ze verbruiken te bewerken.

### Basislijnvariabelen

Standaardvariabelen van AMS worden gedeclareerd in het bestand `/etc/httpd/conf.d/variables/ootb.vars` .  Dit bestand kan niet worden bewerkt, maar bestaat om er zeker van te zijn dat variabelen geen null-waarden hebben.  Deze worden eerst en daarna opgenomen dan we `/etc/httpd/conf.d/variables/ams_default.vars` gebruiken.  U kunt dat bestand bewerken om de waarden van deze variabelen te wijzigen of zelfs dezelfde variabelennamen en -waarden in uw eigen bestand op te nemen!

Hier volgt een voorbeeld van de inhoud van het bestand `/etc/httpd/conf.d/variables/ams_default.vars` :

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### Voorbeeld 1 - SSL forceren

De hierboven weergegeven variabelen `AUHOR_FORCE_SSL` of `PUBLISH_FORCE_SSL` kunnen op 1 worden ingesteld om regels voor herschrijven van tags in te voeren die eindgebruikers dwingen om op http-verzoek om te worden omgeleid naar https

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

De variabelen `DISP_LOG_LEVEL` kunnen worden gebruikt om te plaatsen wat u voor het logboekniveau wilt hebben dat eigenlijk in de lopende configuratie wordt gebruikt.

Hier is het syntaxisvoorbeeld dat in de dossiers van de de basislijnconfiguratie van ams bestaat:

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

Als u het Dispatcher-registratieniveau wilt verhogen, werkt u gewoon de `ams_default.vars` variabele `DISP_LOG_LEVEL` bij tot het gewenste niveau.

Voorbeeldwaarden kunnen een geheel getal of het woord zijn:

| Logboekniveau | Waarde van geheel getal | Woordwaarde |
| --- | --- | --- |
| Traceren | 4 | traceren |
| Foutopsporing | 3 | foutopsporing |
| Info | 2 | info |
| Waarschuwing | 1 | waarschuwen |
| Fout | 0 | fout |

### Voorbeeld 3 - Whitelists

De variabelen `AUTHOR_WHITELIST_ENABLED` en `PUBLISH_WHITELIST_ENABLED` kunnen aan 1 worden geplaatst om herschrijfregels in werking te stellen die regels omvatten om eindgebruikersverkeer toe te staan of te verbieden dat op IP adres wordt gebaseerd.  Als u deze functie inschakelt, moet u deze ook combineren met het maken van een bestand met whitelist-regels.

Hier volgen enkele syntaxisvoorbeelden van de manier waarop de variabele de opname van whitelist-bestanden en een whitelist-bestandsvoorbeeld inschakelt

`sample.vhost` :

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

Zoals u kunt zien `sample_whitelist.rules` dwingt de IP-beperking af, maar als u de variabele in- of uitschakelt, kan deze worden opgenomen in de `sample.vhost`

## Waar moeten de variabelen worden geplaatst?

### Opstartargumenten webserver

AMS plaatst specifieke variabelen voor server/topologie in de opstartargumenten van het Apache-proces in het bestand `/etc/sysconfig/httpd`

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

>[!NOTE]
>
>Omdat dit bestand alleen wordt opgenomen wanneer de service wordt gestart.  U moet de service opnieuw starten om de wijzigingen op te halen.  Betekenis een herladen is niet genoeg maar een nieuw begin in plaats daarvan is nodig

### Variabelenbestanden (`.vars`)

Aangepaste variabelen die door de code worden verschaft, moeten in `.vars` bestanden in de map staan `/etc/httpd/conf.d/variables/`

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

Wanneer het creëren van uw eigen variabelendossiers noemen hen volgens hun inhoud en om de het noemen normen te volgen die in het handboek [ hier ](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention) worden verstrekt.  In het bovenstaande voorbeeld ziet u dat het variabelenbestand de verschillende DNS-vermeldingen bevat als variabelen die in de configuratiebestanden moeten worden gebruikt.

## Variabelen gebruiken

Nu u uw variabelen binnen uw variabelendossiers hebt bepaald zult u willen weten hoe te om hen behoorlijk binnen uw andere configuratiedossiers te gebruiken.

De voorbeeldbestanden van `.vars` hierboven gebruiken om een juiste toepassing te illustreren.

We willen alle omgevingsvariabelen opnemen die we maken `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

We weten dat wanneer de httpd-service wordt gestart deze de variabelen ophaalt die door AMS zijn ingesteld in `/etc/sysconfig/httpd` en de variabele set `ENV_TYPE` en `RUNMODE` heeft

Wanneer dit algemene `.conf` -bestand wordt opgehaald, wordt het vroeg opgehaald, omdat de volgorde waarin de bestanden in `conf.d` worden opgenomen gelijk is aan de numerieke laadvolgorde, betekent dit dat gemiddeld 000 in de bestandsnaam wordt geladen voordat de andere bestanden in de map worden geladen.

De instructie include gebruikt ook een variabele in de bestandsnaam.  Hierdoor kan worden gewijzigd welk bestand daadwerkelijk wordt geladen op basis van de waarde in de variabelen `ENV_TYPE` en `RUNMODE` .

Als de `ENV_TYPE` waarde `dev` is, wordt het volgende bestand gebruikt:

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

Als de `ENV_TYPE` waarde `stage` is, wordt het volgende bestand gebruikt:

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

Als de `RUNMODE` waarde `preview` is, wordt het volgende bestand gebruikt:

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

Als dat bestand wordt opgenomen, kunnen we de variabelenamen gebruiken die in dat bestand zijn opgeslagen.

In ons `/etc/httpd/conf.d/available_vhosts/weretail.vhost` -bestand kunnen we de normale syntaxis omwisselen die alleen voor dev werkte:

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

In ons `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` -bestand kunnen we de normale syntaxis omwisselen die alleen voor dev werkte:

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

Hoe de variabelen in uw gecompileerde configuratie van Dispatcher keken:

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

Gecompileerde Dispatcher-configuratie:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[Volgende -> Spoelen](./disp-flushing.md)
