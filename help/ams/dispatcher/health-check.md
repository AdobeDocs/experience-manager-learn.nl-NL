---
title: AMS Dispatcher Health Check
description: AMS biedt een cgi-bin-script voor de health check die de taakverdelingsmechanisme voor de cloud uitvoert om te zien of AEM gezond is en in dienst moet blijven voor het openbaar verkeer.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: d6b7d63ba02ca73d6c1674d90db53c6eebab3bd2
workflow-type: tm+mt
source-wordcount: '1136'
ht-degree: 1%

---

# AMS Dispatcher Health Check

[Inhoudsopgave](./overview.md)

[&lt;- Vorige: Alleen-lezen bestanden](./immutable-files.md)

Wanneer u een AMS-basislijnverzendingsprogramma hebt geïnstalleerd, wordt deze geleverd met een paar freebies.  Een van deze functies is een set scripts voor health check.
Met deze scripts kan het taakverdelingsmechanisme waarmee de AEM stapel wordt voorafgegaan, weten welke poten gezond zijn en in bedrijf blijven.

![Geanimeerd GIF dat de verkeersstroom toont](assets/load-balancer-healthcheck/health-check.gif "Stappen voor health check")

## Standaard taakverdelingscontrole

Wanneer het klantenverkeer door Internet komt om uw AEM instantie te bereiken zullen zij door een ladingsverdelingsmechanisme gaan

![Afbeelding toont verkeersstroom van internet naar een taakverdeler](assets/load-balancer-healthcheck/load-balancer-traffic-flow.png "load-balancer-traffic-flow")

Elke aanvraag die via het taakverdelingsmechanisme wordt ontvangen, leidt elke aanvraag tot een verwijzing.  Het taakverdelingsmechanisme beschikt over een mechanisme voor gezondheidscontrole dat is ingebouwd om ervoor te zorgen dat het verkeer naar een gezonde host stuurt.

De standaardcontrole is typisch een havencontrole om te zien of luisteren de servers die in het taakverdelingsmechanisme worden gericht op het havenverkeer binnen komt (d.w.z. TCP 80 en 443)

> `Note:` Terwijl dit werkt, heeft het geen echte maatstaf als AEM gezond is.  Het test alleen of de Dispatcher (Apache-webserver) actief is.

## AMS Health Check

Om te voorkomen dat verkeer naar een gezonde verzender wordt gestuurd die een ongezonde AEM-instantie voorgaat, heeft AMS enkele extra&#39;s gemaakt die de gezondheid van de poot evalueren en niet alleen de verzender.

![De afbeelding toont de verschillende onderdelen voor de health check om te werken](assets/load-balancer-healthcheck/health-check-pieces.png "proefstukken voor gezondheidscontrole")

De gezondheidscontrole bestaat uit de volgende stukken:
- 1 `Load balancer`
- 1 `Apache web server`
- 3 `Apache *VirtualHost* config files`
- 5 `CGI-Bin scripts`
- 1 `AEM instance`
- 1 `AEM package`

Wij zullen behandelen wat elk stuk aan opstelling en hun belang is

### AEM

Om aan te geven of AEM functioneert, hebt u het nodig om een basispagina-compilatie uit te voeren en de pagina te bedienen.  Adobe Managed Services heeft een basispakket gemaakt dat de testpagina bevat.  De pagina test dat de bewaarplaats omhoog is en dat de middelen en paginasjabloon kunnen teruggeven.

![Afbeelding toont het AMS-pakket in CRX pakketbeheer](assets/load-balancer-healthcheck/health-check-package.png "gezondheidscontrole-pakket")

Hier is de pagina.  De opslagplaats-id van de installatie wordt weergegeven

![Afbeelding toont de pagina AMS Regent](assets/load-balancer-healthcheck/health-check-page.png "health check-page")

> `Note:` We zorgen ervoor dat de pagina niet in cache kan worden geplaatst.  Het zou niet de daadwerkelijke status controleren als het elke keer enkel een caching pagina terugkwam!

Dit is het lichtgewichteindpunt dat we kunnen testen om te zien dat AEM in gebruik is.

### Configuratie taakverdelingsmechanisme

Wij vormen de ladingsbalansen om aan een CGI-BIN eindpunt in plaats van het gebruiken van een havencontrole te richten.

![Afbeelding toont de configuratie van de AWS-taakverdelingscontrole](assets/load-balancer-healthcheck/aws-settings.png "aws-lb-settings")

![Afbeelding toont de Azure load balancer health check configuratie](assets/load-balancer-healthcheck/azure-settings.png "azure-lb-instellingen")

### Virtuele hosts voor Apache Health Check

#### Virtuele host CGI-BIN `(/etc/httpd/conf.d/available_vhosts/ams_health.vhost)`

Dit is het `<VirtualHost>` Apache-configuratiebestand waarmee de CGI-Bin-bestanden kunnen worden uitgevoerd.

```
Listen 81
<VirtualHost *:81>
    ServerName	"health"
    ...SNIP...
    ScriptAlias /health/ "/var/www/cgi-bin/health/"
</VirtualHost>
```

> `Note:` cgi-bin-bestanden zijn scripts die kunnen worden uitgevoerd.  Dit kan een kwetsbare aanvalsvector zijn en deze manuscripten die AMS gebruikt zijn niet openbaar toegankelijk slechts beschikbaar aan het taakverdelingsmechanisme om tegen te testen.


#### Virtuele hosts voor ongezond onderhoud

- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost`
- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost`

Deze bestanden krijgen een naam `000_` als voorvoegsel.  Het is opzettelijk gevormd om de zelfde domeinnaam te gebruiken zoals de levende plaats.  Het is de bedoeling dat dit bestand wordt ingeschakeld wanneer de health check vaststelt dat er een probleem is met een van de AEM achtergronden.  Geef vervolgens een foutpagina op in plaats van alleen een 503 HTTP-antwoordcode zonder pagina.  Het zal verkeer van het normale stelen `.vhost` bestand omdat het voor dat bestand is geladen `.vhost` bestand delen terwijl u hetzelfde deelt `ServerName` of `ServerAlias`.  Resulterend in pagina&#39;s die voor een bepaald domein worden bestemd om naar de ongezonde gastheer in plaats van het gebrek te gaan het normale verkeersstromen door is.

Wanneer de scripts voor de health check worden uitgevoerd, melden ze hun huidige gezondheidsstatus af.  Eenmaal per minuut wordt er een concronjob uitgevoerd op de server die zoekt naar ongezonde items in het logbestand.  Als het ontdekt dat de auteur AEM instantie niet gezond is zal het dan de symlink toelaten:

Logbestandvermelding:

```
# grep "ERROR\|publish" /var/log/lb/health_check.log
E, [2022-11-23T20:13:54.984379 #26794] ERROR -- : AUTHOR -- Exception caught: Connection refused - connect(2)
I, [2022-11-23T20:13:54.984403 #26794]  INFO -- : [checkpublish]-author:0-publish:1-[checkpublish]
```

Snijden bij het ophalen van de fout en reageren:

```
# grep symlink /var/log/lb/health_check_reload.log
I, [2022-11-23T20:34:19.213179 #2275]  INFO -- : ADDING VHOST symlink /etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost => /etc/httpd/conf.d/enabled_vhosts/000_unhealthy_author.vhost
```

U kunt bepalen of de auteur of de gepubliceerde plaatsen deze foutenpagina kunnen hebben door het plaatsen van de herladingswijze te vormen binnen `/var/www/cgi-bin/health_check.conf`

```
# grep RELOAD_MODE /var/www/cgi-bin/health_check.conf
RELOAD_MODE='author'
```

Geldige opties:
- author
   - Dit is de standaardoptie.
   - Hiermee wordt een onderhoudspagina voor de auteur geplaatst als deze ongezond is
- publish
   - Met deze optie wordt een onderhoudspagina voor de uitgever geplaatst als deze ongezond is
- all
   - Met deze optie wordt een onderhoudspagina voor auteur of uitgever of beide weergegeven als deze ongezond worden
- none
   - Met deze optie wordt dit onderdeel van de health check overgeslagen

Wanneer u naar de `VirtualHost` het plaatsen voor deze zult u zult zien zij het zelfde document zoals een foutenpagina voor elk verzoek laden dat komt wanneer het wordt toegelaten:

```
<VirtualHost *:80>
	ServerName	unhealthyauthor
	ServerAlias	${AUTHOR_DEFAULT_HOSTNAME}
	ErrorDocument	503 /error.html
	DocumentRoot	/mnt/var/www/default
	<Directory />
		Options FollowSymLinks
		AllowOverride None
	</Directory>
	<Directory "/mnt/var/www/default">
		AllowOverride None
		Require all granted
	</Directory>
	<IfModule mod_headers.c>
		Header always add X-Dispatcher ${DISP_ID}
		Header always add X-Vhost "unhealthy-author"
	</IfModule>
	<IfModule mod_rewrite.c>
		ReWriteEngine   on
		RewriteCond %{REQUEST_URI} !^/error.html$
		RewriteRule ^/* /error.html [R=503,L,NC]
	</IfModule>
</VirtualHost>
```

De antwoordcode is nog steeds een `HTTP 503`

```
# curl -I https://we-retail.com/
HTTP/1.1 503 Service Unavailable
X-Dispatcher: dispatcher1useast1
X-Vhost: unhealthy-author
```

In plaats van een lege pagina krijgen ze deze pagina.

![De afbeelding toont de standaardonderhoudspagina](assets/load-balancer-healthcheck/unhealthy-page.png "ongezonde pagina")

### CGI-Bin-scripts

Er zijn vijf verschillende scripts die door uw CSE in de taakverdelingsinstellingen kunnen worden geconfigureerd. Deze kunnen het gedrag of de criteria wijzigen wanneer een Dispatcher uit het taakverdelingsmechanisme wordt gehaald.

#### /bin/checkauteur

Dit script wanneer het wordt gebruikt, controleert en registreert alle instanties die worden voorafgegaan, maar retourneert alleen een fout als de `author` AEM instantie is ongezond

> `Note:` Houd er rekening mee dat als de publicatie AEM-instantie ongezond was, de verzender in service zou blijven zodat het verkeer naar de AEM-instantie van de auteur kan gaan.

#### /bin/checkpublish (standaardwaarde)

Dit script wanneer het wordt gebruikt, controleert en registreert alle instanties die worden voorafgegaan, maar retourneert alleen een fout als de `publish` AEM instantie is ongezond

> `Note:` Houd in mening dat als de auteur AEM instantie ongezond was de verzender in dienst zou blijven om verkeer toe te staan om aan de publicatie AEM instantie te stromen

#### /bin/checkeeither

Dit script wanneer het wordt gebruikt, controleert en registreert alle instanties die worden voorafgegaan, maar retourneert alleen een fout als de `author` of de `publisher` AEM instantie is ongezond

> `Note:` Houd er rekening mee dat als de publicatie AEM instantie of de auteur AEM instantie ongezond was, de verzender de service zou verlaten.  Betekenis dat als een van hen gezond was, het ook geen verkeer zou krijgen

#### /bin/checkboth

Dit script wanneer het wordt gebruikt, controleert en registreert alle instanties die worden voorafgegaan, maar retourneert alleen een fout als de `author` en de `publisher` AEM-instantie is ongezond

> `Note:` Houd er rekening mee dat als de AEM instantie of de auteur AEM instantie ongezond was, de verzender de service niet zou hebben verlaten.  Betekenis dat als één van hen ongezond was, het verkeer zou blijven ontvangen en fouten zou geven aan mensen die om middelen vragen.

#### /bin/normal

Dit manuscript wanneer gebruikt zal om het even welke instanties controleren en registreren het vooraf gaat maar zal enkel gezond terugkeren ongeacht of AEM een fout terugkeert of niet.

> `Note:` Dit script wordt gebruikt wanneer de health check niet naar wens functioneert en een overschrijving toestaat om AEM instanties in het taakverdelingsmechanisme te houden.