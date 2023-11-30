---
title: Foutopsporing AEM SDK met behulp van logbestanden
description: De logboeken handelen als frontline voor het zuiveren AEM toepassingen, maar zijn afhankelijk van het adequate registreren in de opgestelde AEM toepassing.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
jira: KT-5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 0%

---

# Foutopsporing AEM SDK met behulp van logbestanden

Toegang tot de logboeken van de AEM SDK, of de lokale QuickStart Jar of Dispatcher Tools van AEM SDK kunnen zeer belangrijke inzichten in het zuiveren AEM toepassingen verstrekken.

## Logbestanden AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

De logboeken handelen als frontline voor het zuiveren AEM toepassingen, maar zijn afhankelijk van het adequate registreren in de opgestelde AEM toepassing. De Adobe adviseert het houden van lokale ontwikkeling en AEM as a Cloud Service Dev registrerende configuraties als gelijkaardig mogelijk, aangezien het logboekzicht op de lokale QuickStart van AEM SDK en AEM de milieu&#39;s van de Ontwikkelaar van de SDK normaliseert, die configuratietweeling en herplaatsing verminderen.

De [Projectarchetype AEM](https://github.com/adobe/aem-project-archetype) vormt registreren op het niveau van DEBUG voor de pakketten van Java van uw AEM toepassing voor lokale ontwikkeling via de Sling Logger OSGi configuratie die bij wordt gevonden

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

die zich tot de `error.log`.

Als de standaardlogboekregistratie onvoldoende is voor lokale ontwikkeling, kan ad-hoclogboekregistratie worden geconfigureerd via de lokale QuickStart-webconsole van AEM SDK voor logondersteuning op ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)), echter, wordt het niet geadviseerd ad hoc veranderingen aan Git worden voortgeduurd tenzij deze zelfde logboekconfiguraties ook op AEM as a Cloud Service Dev milieu&#39;s nodig zijn. Wijzigingen via de Log Support-console blijven rechtstreeks doorgevoerd in de lokale opslagruimte van de AEM SDK.

Java-loginstructies kunnen worden weergegeven in het dialoogvenster `error.log` bestand:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Het is vaak handig om de `error.log` dat zijn output aan de terminal stroomt.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows vereist [toepassingen van derden voor staart](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) of het gebruik van [Ophalen-Inhoud van PowerShell, opdracht](https://stackoverflow.com/a/46444596/133936).

## Logboeken voor verzending

Logbestanden van de verzender worden uitgevoerd naar stdout wanneer `bin/docker_run` wordt aangehaald, nochtans kunnen de logboeken rechtstreeks tot in Docker toegang hebben bevatten.

### De toegang tot van logboeken in de container van de Dokker{#dispatcher-tools-access-logs}

De logboekbestanden van de verzender kunnen rechtstreeks toegang krijgen tot de Docker-container op `/etc/httpd/logs`.

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

_De `<CONTAINER ID>` in `docker exec -it <CONTAINER ID> /bin/sh` moet worden vervangen door de doel Docker CONTAINER-id die wordt vermeld in het dialoogvenster `docker ps` gebruiken._


### De Docker-logbestanden worden naar het lokale bestandssysteem gekopieerd{#dispatcher-tools-copy-logs}

De logboekbestanden van de Dispatcher kunnen uit de Docker-container worden gekopieerd op `/etc/httpd/logs` naar het lokale bestandssysteem voor inspectie met uw favoriete programma voor loganalyse. Merk op dat dit een punt-in-tijd exemplaar is, en geen updates in real time aan de logboeken verstrekt.

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

_De `<CONTAINER_ID>` in `docker cp <CONTAINER_ID>:/var/log/apache2 ./` moet worden vervangen door de doel Docker CONTAINER-id die wordt vermeld in het dialoogvenster `docker ps` gebruiken._
