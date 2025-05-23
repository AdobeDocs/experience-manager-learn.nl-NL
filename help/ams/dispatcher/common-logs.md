---
title: AEM Dispatcher Common Logs
description: Bekijk de gemeenschappelijke logboekingangen van Dispatcher en leer wat zij betekenen en hoe te om hen te richten.
version: Experience Manager 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 7fe1b4a5-6813-4ece-b3da-40af575ea0ed
duration: 229
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '796'
ht-degree: 0%

---

# Algemene logbestanden

[Inhoudsopgave](./overview.md)

[&lt;- Vorige: URL&#39;s met Vanity](./disp-vanity-url.md)

## Overzicht

Het document zal gemeenschappelijke logboekingangen beschrijven u zult zien en wat zij betekenen en hoe te om met hen te behandelen.

## GLOB-waarschuwing

Voorbeeldlogbestandvermelding:

```
Fri Jul 20 03:35:09 2018 W pid 8300 (tid 139937910880384) /etc/httpd/conf/publish-filters.any:5: Allowing requests with globs is considered unsafe.
Please consult the documentation at 'https://www.adobe.com/go/docs_dispatcher_config_en' on how to use attributes 
method/url/query/protocol/path/selectors/extension/suffix instead.
```

Sinds Dispatcher module 4.2.x begonnen ze mensen te ontmoedigen om het volgende type overeenkomsten in hun filterbestand te gebruiken:

```
/0041 { /type "allow" /glob "* *.css *"   }
```

In plaats daarvan is het beter om de nieuwe syntaxis als volgt te gebruiken:

```
/0041 { /type "allow" /url "*.css" }
```

Of zelfs beter om op een vervangingsmatcher helemaal niet aan te passen:

```
/0041 { /type "allow" /extension "css" }
```

Als u een van de voorgestelde methoden gebruikt, wordt dat foutbericht uit de logbestanden verwijderd.

## Filterafwijzingen

>[!NOTE]
>
>Deze ingangen tonen niet altijd omhoog zelfs als de verwerpingen gebeuren als het logboekniveau te laag wordt geplaatst. Stel deze in op Info of Foutopsporing om te controleren of de filters de aanvragen afwijzen.

Voorbeeld van logbestandvermelding:

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

of:

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

>[!CAUTION]
>
>Begrijp dat de regels van Dispatcher werden geplaatst om dat verzoek uit te filteren. In dit geval werd de pagina die probeerde te worden bezocht opzettelijk verworpen en zouden we hier niets mee willen doen.

Als uw logbestand er als volgt uitziet:

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

Dat laat ons weten dat ons ontwerpbestand `.eot` wordt geblokkeerd en dat willen we verhelpen.
We moeten dus naar ons filterbestand kijken en de volgende regel toevoegen om `.eot` -bestanden door te laten

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

Hierdoor kan het bestand worden doorlopen en wordt het niet meer geregistreerd.
Als u wilt zien wat wordt gefilterd kunt u dit bevel op uw logboekdossier in werking stellen:

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## Tijdslimieten van renderen

Logbestandvermelding van Socket-timeout:

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

Dit komt voor wanneer u het verkeerde IP adres hebt dat in de renderensectie van uw landbouwbedrijf wordt gevormd. Dat of de AEM-instantie reageerde of luisterde niet meer en de Dispatcher kan het niet bereiken.

Controleer uw firewallregels en of de AEM-instantie actief en gezond is.

Logboekvermeldingen voor time-outvoorbeelden van gateway:

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

Dit betekent dat de AEM-instantie een open socket had die het met de reactie kon bereiken en er een time-out voor kon maken. Dit betekent dat uw AEM-instantie te langzaam of ongezond was en Dispatcher de geconfigureerde time-outinstellingen in de rendersectie van het farm heeft bereikt. Verhoog de time-outinstelling of zorg dat uw AEM-instantie gezond is.

## Cacheniveau

Voorbeeld van logbestandvermelding:

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

Dit betekent dat het ophalen van gegevens uit het renderniveau versus uit de cache wordt gemeten. U wilt 80+ percenten van geheim voorgeheugen raken, en u zou de hulp [ hier ](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html?lang=nl-NL) moeten volgen:

Dit getal zo hoog mogelijk ophalen.

>[!NOTE]
>
>Zelfs als u uw geheim voorgeheugenmontages in het landbouwbedrijfdossier hebt om alles in het voorgeheugen onder te brengen zou u te vaak of te agressief kunnen spoelen die een lager percentage van geheim voorgeheugenklapverhouding kan veroorzaken om voor te komen

## Ontbrekende map

Voorbeeldlogbestandvermelding:

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

Dit zal typisch verschijnen wanneer een punt wordt gehaald terwijl een geheim voorgeheugen wordt ontruimd tezelfdertijd voorkomt.

Dat of de documentwortelfolder heeft slechte toestemmingen op het of de verkeerde SELinux dossiercontext op de omslag die apache van het creëren van de nieuwe nodig subfolders ontkent.

Voor toestemmingskwesties bekijk de toestemmingen van de documentwortel en zorg ervoor zij gelijkaardig aan kijken:

```
dispatcher$ ls -Z /var/www/
drwxr-xr-x+ apache apache system_u:object_r:httpd_sys_content_t:s0 html
```

## Vanity URL niet gevonden

Voorbeeldlogbestandvermelding:

```
Thu Sep 27 17:35:11 2018 D pid 18936 Checking vanity URLs 
Thu Sep 27 17:35:11 2018 D pid 18936 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:35:11 2018 W pid 18936 Unable to fetch vanity URLs from 10.43.0.42:4503/libs/granite/dispatcher/content/vanityUrls.html: remote server returned: HTTP/1.1 404 Not Found
```

Deze fout treedt op wanneer u de Dispatcher zodanig hebt geconfigureerd dat deze gebruikmaakt van de dynamische URL&#39;s met automatische filters. De installatie is echter nog niet voltooid door het pakket op de AEM-renderer te installeren.

Als u dit wilt verhelpen, installeert u het URL-functiepakket voor ijdelheid op de AEM-instantie en laat u deze klaar zijn voor de anonieme gebruiker. Details [ hier ](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html?lang=nl-NL)

Een werkende vanity URL opstelling kijkt als dit:

```
Thu Sep 27 17:40:29 2018 D pid 21844 Checking vanity URLs 
Thu Sep 27 17:40:29 2018 D pid 21844 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:40:29 2018 D pid 21844 Loaded 18 vanity URLs from file /tmp/vanity_urls
```

## Ontbrekende boerderij

Voorbeeldlogbestandvermelding:

```
Wed Nov 13 17:17:26 2019 W pid 19173:tid 140542738364160 No farm matches host 'we-retail.com', selected last farm 'publishfarm'
```

Deze fout geeft aan dat uit alle bestanden op de boerderij die beschikbaar zijn in `/etc/httpd/conf.dispatcher.d/enabled_farms/` geen overeenkomende vermelding is gevonden in de sectie `/virtualhost` .

De landbouwbedrijfdossiers passen verkeer aan dat op de domeinnaam of de weg wordt gebaseerd waarin het verzoek binnen kwam met. Het gebruikt glob aanpassing en als het niet dan aanpast hebt u of niet uw landbouwbedrijf behoorlijk gevormd, typt de ingang in het landbouwbedrijf, of heeft de ingang volledig missen. Wanneer het landbouwbedrijf om het even welke ingangen niet aanpast blijft het definitief aan het laatste landbouwbedrijf inbegrepen in de stapel landbouwbedrijfdossiers inbegrepen in gebreke. In dit voorbeeld is dit `999_ams_publish_farm.any` , dat de algemene naam van een uitgeverij heet.

Hier volgt een voorbeeld van een boerderijbestand `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any` dat is verkleind om de relevante onderdelen te benadrukken.

## Object verzonden van

Voorbeeldlogbestandvermelding:

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

De pagina is opgehaald via de GET http-methode voor de inhoud `/content/we-retail/us/en.html` en het duurde 24034 milliseconden. Het deel waar we aandacht aan wilden besteden is helemaal aan het eind `publishfarm/0` . U zult zien dat het doelt en `publishfarm` aanpast. Het verzoek werd opgehaald van render 0. Dit betekende dat deze pagina moest worden opgevraagd bij AEM en vervolgens in cache moest worden geplaatst. Nu vragen wij deze pagina opnieuw en zien wat met het logboek gebeurt.

[Volgende -> Alleen-lezen bestanden](./immutable-files.md)
