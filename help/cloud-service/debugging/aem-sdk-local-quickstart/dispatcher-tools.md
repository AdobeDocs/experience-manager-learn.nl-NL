---
title: Fouten opsporen in Dispatcher-gereedschappen
description: De Dispatcher Tools verstrekt een inperkt milieu van de Server van het Web van Apache dat kan worden gebruikt om AEM als Verzender van de Diensten van de Publicatie van AEM plaatselijk te simuleren. Fouten opsporen in de logbestanden van Dispatcher Tools en de inhoud van het cachegeheugen kunnen van essentieel belang zijn om ervoor te zorgen dat de end-to-end AEM toepassing correct is en dat ondersteuning wordt geboden voor cache- en beveiligingsconfiguraties.
feature: Dispatcher
kt: 5918
topic: Development
role: Developer
level: Beginner, Intermediate
source-git-commit: 0737cd2410b48dbaa9b6dfaaa27b854d44536f15
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---


# Fouten opsporen in Dispatcher-gereedschappen

De Dispatcher Tools verstrekt een inperkt milieu van de Server van het Web van Apache dat kan worden gebruikt om AEM als Verzender van de Diensten van de Publicatie van AEM plaatselijk te simuleren.

Fouten opsporen in de logbestanden van Dispatcher Tools en de inhoud van het cachegeheugen kunnen van essentieel belang zijn om ervoor te zorgen dat de end-to-end AEM toepassing correct is en dat ondersteuning wordt geboden voor cache- en beveiligingsconfiguraties.

>[!NOTE]
>
>Aangezien de Hulpmiddelen van de Ontvanger op container-gebaseerd zijn, telkens als het opnieuw is begonnen, worden de vroegere logboeken en de geheim voorgeheugeninhoud vernietigd.

## Logboeken voor Dispatcher Tools

Logboeken voor Dispatcher Tools zijn beschikbaar via de opdracht `stdout` of `bin/docker_run`, of met meer details, beschikbaar in de Docker-container op `/etc/https/logs`.

Zie [Dispatcher logs](./logs.md#dispatcher-logs) voor instructies op hoe te om tot de logboeken van de Docker van de container van de Hulpmiddelen van de Verzender rechtstreeks toegang te hebben.

## Cache van Dispatcher Tools

### De toegang tot van logboeken in de container van de Dokker

Het cachegeheugen van de Dispatcher kan rechtstreeks toegang krijgen tot de Docker-container op ` /mnt/var/www/html`.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /mnt/var/www/html

# When finished viewing the cache, exit the Docker container's shell
/# exit
```

### De Docker-logbestanden worden naar het lokale bestandssysteem gekopieerd

De logboekregistraties van de verzender kunnen uit de container van de Dokker bij `/mnt/var/www/html` aan het lokale dossiersysteem voor inspectie worden gekopieerd gebruikend uw favoriete hulpmiddelen. Merk op dat dit een punt-in-tijd exemplaar is, en geen updates in real time aan het geheime voorgeheugen verstrekt.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

