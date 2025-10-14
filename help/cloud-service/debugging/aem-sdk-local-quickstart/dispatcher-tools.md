---
title: Fouten opsporen in Dispatcher-gereedschappen
description: De hulpmiddelen van Dispatcher verstrekt een containerized milieu van de Server van het Web Apache die kan worden gebruikt om AEM als Dispatcher van de dienst van Publish van de Cloud Service plaatselijk te simuleren AEM. Fouten opsporen in de logbestanden van Dispatcher Tools en de inhoud van het cachegeheugen kunnen van essentieel belang zijn om ervoor te zorgen dat de end-to-end AEM toepassing correct is en dat ondersteuning wordt geboden voor cache- en beveiligingsconfiguraties.
feature: Dispatcher
jira: KT-5918
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: f0adf7a6-c7c2-449a-9fa5-402c54b812e5
duration: 56
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# Fouten opsporen in Dispatcher-gereedschappen

De hulpmiddelen van Dispatcher verstrekt een containerized milieu van de Server van het Web Apache die kan worden gebruikt om AEM als Dispatcher van de dienst van Publish van de Cloud Service plaatselijk te simuleren AEM.

Fouten opsporen in de logbestanden van Dispatcher Tools en de inhoud van het cachegeheugen kunnen van essentieel belang zijn om ervoor te zorgen dat de end-to-end AEM toepassing correct is en dat ondersteuning wordt geboden voor cache- en beveiligingsconfiguraties.

>[!NOTE]
>
>Aangezien de Hulpmiddelen van Dispatcher op container-gebaseerd zijn, telkens als het opnieuw wordt begonnen, worden de vroegere logboeken en de geheim voorgeheugeninhoud vernietigd.

## Dispatcher Tools-logboeken

Logbestanden met Dispatcher-gereedschappen zijn beschikbaar via de opdracht `stdout` of `bin/docker_run` , of met meer details, beschikbaar in de Docker-container op `/etc/https/logs` .

Zie [&#x200B; de logboeken van Dispatcher &#x200B;](./logs.md#dispatcher-logs) voor instructies op hoe te om tot de logboeken van de container van de Docker van Dispatcher van Hulpmiddelen direct toegang te hebben.

## Dispatcher Tools-cache

### De toegang tot van logboeken in de container van de Dokker

De Dispatcher-cache kan rechtstreeks worden geopend in de Docker-container op ` /mnt/var/www/html` .

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

Dispatcher-logbestanden kunnen met uw favoriete programma&#39;s uit de Docker-container in `/mnt/var/www/html` worden gekopieerd naar het lokale bestandssysteem. Merk op dat dit een punt-in-tijd exemplaar is, en geen updates in real time aan het geheime voorgeheugen verstrekt.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```
