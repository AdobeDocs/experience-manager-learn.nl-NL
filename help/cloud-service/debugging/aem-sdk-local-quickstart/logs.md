---
title: Foutopsporing AEM SDK met behulp van logbestanden
description: De logboeken handelen als frontline voor het zuiveren AEM toepassingen, maar zijn afhankelijk van het adequate registreren in de opgestelde AEM toepassing.
feature: Gereedschappen voor ontwikkelaars
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: Ontwikkeling
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 0%

---


# Foutopsporing AEM SDK met behulp van logbestanden

Toegang tot de logboeken van de AEM SDK, of de lokale QuickStart Jar of Dispatcher Tools van AEM SDK kunnen zeer belangrijke inzichten in het zuiveren AEM toepassingen verstrekken.

## Logbestanden AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

De logboeken handelen als frontline voor het zuiveren AEM toepassingen, maar zijn afhankelijk van het adequate registreren in de opgestelde AEM toepassing. Adobe adviseert het houden van lokale ontwikkeling en AEM als Cloud Service Dev registrerende configuraties zoals mogelijk, aangezien het logboekzicht op lokale QuickStart van AEM SDK en AEM als milieu&#39;s van de Ontwikkelaar van een Cloud Service normaliseert, die configuratie het tweelingt en re-plaatsing verminderen.

Met [AEM Projectarchetype](https://github.com/adobe/aem-project-archetype) wordt registratie op DEBUG-niveau geconfigureerd voor Java-pakketten van uw AEM toepassing voor lokale ontwikkeling via de OSGi-configuratie van Sling Logger gevonden op

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

die aan `error.log` registreert.

Als het standaard registreren voor lokale ontwikkeling ontoereikend is, kan het ad hoc registreren via de lokale het Webconsole van de Steun van het Logboek van SDK van AEM lokale QuickStart, bij ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)) worden gevormd, nochtans wordt het niet geadviseerd ad hoc veranderingen aan Git worden voortgeduurd tenzij deze zelfde logboekconfiguraties op AEM ook als milieu&#39;s van de Cloud Service Dev worden vereist. Wijzigingen via de Log Support-console blijven rechtstreeks doorgevoerd in de lokale opslagruimte van de AEM SDK.

Java-loginstructies kunnen worden weergegeven in het bestand `error.log`:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Vaak is het handig om de `error.log` die de uitvoer naar de terminal stroomt, te &quot;staart&quot;.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Voor Windows is [toepassingen van externe leveranciers](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) of het gebruik van [Ophalen-Inhoud van PowerShell-opdracht](https://stackoverflow.com/a/46444596/133936) vereist.

## Logboeken voor verzending

De logboeken van de afzender zijn output aan stdout wanneer `bin/docker_run` wordt aangehaald, nochtans kunnen de logboeken rechtstreeks tot in Docker toegang hebben bevatten.

### De toegang tot van logboeken in de container van de Dokker{#dispatcher-tools-access-logs}

De logboekregistraties van de verzender kunnen direct tot in de container van de Dokker bij `/etc/httpd/logs` toegang hebben.

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

_De  `<CONTAINER ID>` insteekmodule  `docker exec -it <CONTAINER ID> /bin/sh` moet worden vervangen met de doelDocker CONTAINER-id die wordt vermeld via de  `docker ps` opdracht._


### De Docker-logbestanden worden naar het lokale bestandssysteem gekopieerd{#dispatcher-tools-copy-logs}

De logboekregistraties van de verzender kunnen uit de container van de Dokker bij `/etc/httpd/logs` aan het lokale dossiersysteem voor inspectie worden gekopieerd gebruikend uw favoriete hulpmiddel van de logboekanalyse. Merk op dat dit een punt-in-tijd exemplaar is, en geen updates in real time aan de logboeken verstrekt.

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

_De  `<CONTAINER_ID>` insteekmodule  `docker cp <CONTAINER_ID>:/var/log/apache2 ./` moet worden vervangen met de doelDocker CONTAINER-id die wordt vermeld via de  `docker ps` opdracht._
