---
title: Fouten opsporen in AEM SDK met behulp van logbestanden
description: Logs fungeert als frontline voor foutopsporing in AEM-toepassingen, maar is afhankelijk van een adequate aanmelding in de geïmplementeerde AEM-toepassing.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
duration: 411
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---

# Fouten opsporen in AEM SDK met behulp van logbestanden

Als u toegang krijgt tot de AEM SDK-logboeken, kunt u met de lokale QuickStart Jar- of Dispatcher Tools van AEM SDK belangrijke inzichten krijgen in het opsporen van fouten in AEM-toepassingen.

## AEM Logs

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

Logs fungeert als frontline voor foutopsporing in AEM-toepassingen, maar is afhankelijk van een adequate aanmelding in de geïmplementeerde AEM-toepassing. Adobe raadt aan om de configuraties voor lokale ontwikkeling en AEM as a Cloud Service Dev-logboekregistratie zo vergelijkbaar mogelijk te houden, aangezien deze de zichtbaarheid van logbestanden in de lokale QuickStart- en AEM as a Cloud Service Dev-omgevingen van AEM SDK normaliseert, waardoor configuratie-tweeling en -herimplementatie worden beperkt.

Het [&#x200B; Archetype van het Project van AEM &#x200B;](https://github.com/adobe/aem-project-archetype) vormt registreren op het niveau DEBUG voor de pakketten van Java van uw toepassing van AEM voor lokale ontwikkeling via de Sling Logger OSGi configuratie die bij wordt gevonden

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

die zich aanmeldt bij de `error.log` .

Als het standaard registreren voor lokale ontwikkeling ontoereikend is, kan het ad hoc registreren via de lokale het Webconsole van de Steun van het Logboek van AEM SDK, bij ([/system/console/slinglog &#x200B;](http://localhost:4502/system/console/slinglog)) worden gevormd, nochtans wordt het niet geadviseerd ad hoc veranderingen aan Git worden voortgeduurd tenzij deze zelfde logboekconfiguraties ook op de milieu&#39;s van AEM as a Cloud Service Dev nodig zijn. Wijzigingen via de Log Support-console blijven rechtstreeks doorgevoerd in de lokale QuickStart-opslagruimte van AEM SDK.

Java-loginstructies kunnen worden weergegeven in het bestand `error.log` :

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Vaak is het handig om de `error.log` die de uitvoer naar de terminal stroomt, te &#39;staart&#39;.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ De vensters vereist [&#x200B; toepassingen van de 3de partijstaart &#x200B;](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) of het gebruik van [&#x200B; Powershell bevel krijgen-Inhoud &#x200B;](https://stackoverflow.com/a/46444596/133936).

## Dispatcher-logboeken

Dispatcher-logboeken worden uitgevoerd om te worden stopgezet wanneer `bin/docker_run` wordt aangeroepen, maar logbestanden kunnen rechtstreeks worden geopend in de Docker-inhoud.

### De toegang tot van logboeken in de container van de Dokker{#dispatcher-tools-access-logs}

Dispatcher-logbestanden kunnen rechtstreeks worden geopend in de Docker-container op `/etc/httpd/logs` .

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /etc/httpd/logs
/ # ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log

# When finished viewing the logs files, exit the Docker container's shell
/# exit
```

_`<CONTAINER ID>` in `docker exec -it <CONTAINER ID> /bin/sh` moet met doelDocker worden vervangen CONTAINER identiteitskaart die van het `docker ps` bevel wordt vermeld._


### De Docker-logbestanden worden naar het lokale bestandssysteem gekopieerd{#dispatcher-tools-copy-logs}

Dispatcher-logbestanden kunnen vanuit de Docker-container in `/etc/httpd/logs` naar het lokale bestandssysteem worden gekopieerd voor inspectie met uw favoriete programma voor logbestandsanalyse. Merk op dat dit een punt-in-tijd exemplaar is, en geen updates in real time aan de logboeken verstrekt.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/etc/httpd/logs logs 
$ cd logs
$ ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log
```

_`<CONTAINER_ID>` in `docker cp <CONTAINER_ID>:/var/log/apache2 ./` moet met doelDocker worden vervangen CONTAINER identiteitskaart die van het `docker ps` bevel wordt vermeld._
