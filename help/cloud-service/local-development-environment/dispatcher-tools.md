---
title: Dispatcher Tools instellen voor AEM as a Cloud Service ontwikkeling
description: AEM de Dispatcher Tools van SDK vergemakkelijkt de lokale ontwikkeling van Adobe Experience Manager (AEM) projecten door het gemakkelijk te maken om, Dispatcher plaatselijk te installeren in werking te stellen en problemen op te lossen.
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
jira: KT-4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
duration: 624
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1621'
ht-degree: 0%

---

# Lokale gereedschappen voor Dispatcher instellen {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Lokale verzendprogramma&#39;s"
>abstract="Dispatcher is een integraal onderdeel van de algehele architectuur van de Experience Manager en moet onderdeel zijn van de lokale ontwikkelinstellingen. De AEM as a Cloud Service SDK bevat de aanbevolen versie van Dispatcher Tools waarmee u het valideren kunt configureren en de Dispatcher lokaal kunt simuleren."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html" text="Dispatcher in de cloud"
>additional-url="https://experienceleague.adobe.com/docs/experience-cloud/software-distribution/home.html" text="AEM as a Cloud Service SDK downloaden"

De Verzender van Adobe Experience Manager (AEM) is een Apache HTTP- de servermodule die een veiligheid en prestatieslaag tussen CDN en AEM publicatielaag verstrekt. Dispatcher is een integraal onderdeel van de algehele architectuur van de Experience Manager en moet onderdeel zijn van de lokale ontwikkelinstellingen.

De AEM as a Cloud Service SDK bevat de aanbevolen versie van Dispatcher Tools waarmee u het valideren kunt configureren en de Dispatcher lokaal kunt simuleren. Dispatcher Tools bestaat uit:

+ een basislijnset van Apache HTTP Web server and Dispatcher configuration files, gevestigd op `.../dispatcher-sdk-x.x.x/src`
+ een CLI-hulpprogramma voor configuratievalidatie, dat zich bevindt op `.../dispatcher-sdk-x.x.x/bin/validate`
+ een hulpmiddel van de configuratiegeneratie CLI, dat bij wordt gevestigd `.../dispatcher-sdk-x.x.x/bin/validator`
+ een hulpmiddel van de configuratieplaatsing CLI, dat bij wordt gevestigd `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ een onveranderlijke configuratiedossiers die CLI hulpmiddel beschrijven, dat bij wordt gevestigd `.../dispatcher-sdk-x.x.x/bin/update_maven`
+ een Docker-afbeelding die Apache HTTP Web-server uitvoert met de module Dispatcher

Let op: `~` wordt gebruikt als steno voor de Folder van de Gebruiker. In Windows is dit het equivalent van `%HOMEPATH%`.

>[!NOTE]
>
> De video&#39;s op deze pagina zijn opgenomen op macOS. Windows-gebruikers kunnen de stappen volgen, maar gebruiken de equivalente Windows-opdrachten van Dispatcher Tools, die bij elke video worden geleverd.

## Vereisten

1. Windows-gebruikers moeten Windows 10 Professional gebruiken (of een versie die Docker ondersteunt)
1. Installeren [Experience Manager publiceert QuickStart Jar](./aem-runtime.md) op de lokale ontwikkelingsmachine.

+ Installeer indien nodig de nieuwste [AEM website](https://github.com/adobe/aem-guides-wknd/releases) op de lokale AEM-publicatieservice. Deze website wordt in deze zelfstudie gebruikt om een werkende Dispatcher te visualiseren.

1. De nieuwste versie van [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) op de lokale ontwikkelcomputer.

## Download de Dispatcher Tools (als onderdeel van de AEM SDK)

De AEM as a Cloud Service SDK, of AEM SDK, bevat de Dispatcher Tools die worden gebruikt om de Apache HTTP Web-server met de module Dispatcher lokaal voor ontwikkeling uit te voeren, en compatibele QuickStart Jar.

Als de AEM as a Cloud Service SDK al is gedownload naar [de lokale AEM-runtime instellen](./aem-runtime.md)hoeft u de toepassing niet opnieuw te downloaden.

1. Aanmelden bij [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list p.offset=0&amp;p.limit=1) met uw Adobe ID
   + Uw Adobe-organisatie __moet__ zijn ingericht voor AEM as a Cloud Service om de AEM as a Cloud Service SDK te downloaden
1. Klik op de nieuwste __AEM SDK__ te downloaden resultatenrij

## De Dispatcher Tools extraheren uit de AEM SDK zip

>[!TIP]
>
> Windows-gebruikers kunnen geen spaties of speciale tekens bevatten in het pad naar de map met de gereedschappen voor lokale verzending. Als er spaties in het pad voorkomen, `docker_run.cmd` mislukt.

De versie van Dispatcher Tools verschilt van die van de AEM SDK. Zorg ervoor dat de versie van Dispatcher Tools beschikbaar is via de AEM SDK-versie die overeenkomt met de as a Cloud Service versie van AEM.

1. De gedownloade gegevens decomprimeren `aem-sdk-xxx.zip` file
1. Verpak de Dispatcher Tools uit in `~/aem-sdk/dispatcher`

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!TAB Windows]

Unzip `aem-sdk-dispatcher-tools-x.x.x-windows.zip` in `C:\Users\<My User>\aem-sdk\dispatcher` (ontbrekende mappen maken, indien nodig).

>[!TAB Linux®]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!ENDTABS]

Alle hieronder uitgegeven bevelen veronderstellen dat de huidige het werk folder de het uitvouwen inhoud van de Hulpmiddelen van de Verzender bevat.

>[!VIDEO](https://video.tv.adobe.com/v/30601?quality=12&learn=on)

*Deze video gebruikt macOS ter illustratie. Met de equivalente Windows/Linux-opdrachten kunt u vergelijkbare resultaten bereiken.*

## De Dispatcher-configuratiebestanden begrijpen

>[!TIP]
> Experience Manager projecten die zijn gemaakt op basis van de [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype) Deze set Dispatcher-configuratiebestanden zijn vooraf ingevuld. U hoeft daarom niet meer te kopiëren uit de bronmap Dispatcher Tools.

De Dispatcher Tools biedt een set Apache HTTP Web-server- en Dispatcher-configuratiebestanden die gedrag voor alle omgevingen, inclusief lokale ontwikkeling, definiëren.

Deze bestanden zijn bedoeld om naar een Experience Manager Maven-project te worden gekopieerd `dispatcher/src` als deze niet bestaan in het Experience Manager Maven-project.

Een volledige beschrijving van de configuratiebestanden is beschikbaar in de onverpakte Dispatcher Tools als `dispatcher-sdk-x.x.x/docs/Config.html`.

## Configuraties valideren

Optioneel, de Dispatcher en Apache de serverconfiguraties van het Web (via `httpd -t`) kan worden gevalideerd met de `validate` script (niet te verwarren met het `validator` uitvoerbaar). De `validate` is een handige manier om het script uit te voeren [drie fasen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=en) van de `validator`.


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/validate.sh ./src
```

>[!TAB Windows]

```shell
$ bin\validate src
```

>[!TAB Linux®]

```shell
$ ./bin/validate.sh ./src
```

>[!ENDTABS]

## Verzending lokaal uitvoeren

AEM Dispatcher lokaal wordt uitgevoerd met Docker op de `src` Configuratiebestanden van de server Dispatcher en Apache Web.


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

De `docker_run_hot_reload` uitvoerbaar bestand heeft de voorkeur boven `docker_run` aangezien het configuratiedossiers opnieuw laadt aangezien zij worden veranderd, zonder het moeten manueel eindigen en opnieuw beginnen `docker_run`. Alternatief, `docker_run` kan worden gebruikt, maar moet handmatig worden beëindigd en opnieuw worden gestart `docker_run` wanneer configuratiebestanden worden gewijzigd.

>[!TAB Windows]

```shell
$ bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

De `docker_run_hot_reload` uitvoerbaar bestand heeft de voorkeur boven `docker_run` aangezien het configuratiedossiers opnieuw laadt aangezien zij worden veranderd, zonder het moeten manueel eindigen en opnieuw beginnen `docker_run`. Alternatief, `docker_run` kan worden gebruikt, maar moet handmatig worden beëindigd en opnieuw worden gestart `docker_run` wanneer configuratiebestanden worden gewijzigd.

>[!ENDTABS]

De `<aem-publish-host>` kan worden ingesteld op `host.docker.internal`, verstrekt een speciale DNS naamDocker in de container die aan IP van de gastheermachine oplost. Als de `host.docker.internal` niet wordt opgelost, zie de [problemen oplossen](#troubleshooting-host-docker-internal) hieronder.

U kunt bijvoorbeeld de Dispatcher Docker-container starten met de standaardconfiguratiebestanden van de Dispatcher Tools:

Start Dispatcher Docker-container die het pad naar de Dispatcher-configuratiemap bevat:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!ENDTABS]

De AEM as a Cloud Service publicatieservice van SDK, die plaatselijk op haven 4503 loopt is beschikbaar door Dispatcher bij `http://localhost:8080`.

Om de Hulpmiddelen van de Verzender tegen de configuratie van de Verzender van een project van de Experience Manager in werking te stellen, richt aan uw project `dispatcher/src` map.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]


## Logboeken voor Dispatcher Tools

Logboeken van de verzender zijn nuttig tijdens lokale ontwikkeling om te begrijpen als en waarom de Verzoeken van HTTP worden geblokkeerd. U kunt het logniveau instellen door vooraf de uitvoering van `docker_run` met omgevingsparameters.

Logboeken van Dispatcher Tools worden uitgestraald naar de standaard wanneer `docker_run` wordt uitgevoerd.

Nuttige parameters voor het zuiveren Dispatcher omvatten:

+ `DISP_LOG_LEVEL=Debug` Hiermee stelt u de logboekregistratie van de module Dispatcher in op het niveau Foutopsporing
   + Standaardwaarde is: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` stelt Apache HTTP Web server herwrite module logging to Debug level in
   + Standaardwaarde is: `Warn`
+ `DISP_RUN_MODE` Hiermee stelt u de &quot;uitvoeringsmodus&quot; van de Dispatcher-omgeving in en laadt u de bijbehorende uitvoermodi Dispatcher-configuratiebestanden.
   + Standaardwaarden: `dev`
+ Geldige waarden: `dev`, `stage`, of `prod`

Een of meer parameters, die kunnen worden doorgegeven aan `docker_run`

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]

### Toegang tot logbestanden

Logbestanden van Apache-webservers en AEM Dispatcher kunnen rechtstreeks worden geopend in de Docker-container:

+ [De toegang tot van logboeken in de container van de Dokker](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [De Docker-logbestanden worden naar het lokale bestandssysteem gekopieerd](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Wanneer werkt u de Dispatcher Tools bij{#dispatcher-tools-version}

Versies van Dispatcher Tools nemen minder vaak toe dan de Experience Manager en dus vereisen Dispatcher Tools minder updates in de lokale ontwikkelomgeving.

De aanbevolen versie van Dispatcher Tools is die welke is meegeleverd bij de AEM as a Cloud Service SDK die overeenkomt met de as a Cloud Service versie van de Experience Manager. De versie van AEM as a Cloud Service is te vinden via [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Omgevingen__, per door de __AEM__ label

![Versie Experience Manager](./assets/dispatcher-tools/aem-version.png)

*De versie Dispatcher Tools komt niet overeen met de versie van Experience Manager.*

## De basislijnset van Apache- en Dispatcher-configuraties bijwerken

De basislijnset van Apache- en Dispatcher-configuratie wordt regelmatig verbeterd en vrijgegeven met de AEM as a Cloud Service SDK-versie. Het is beste praktijken om de verhogingen van de basislijnconfiguratie in uw AEM project op te nemen en te vermijden [lokale validatie](#validate-configurations) en fouten met de pijpleiding in Cloud Manager. Werk ze bij met de functie `update_maven.sh` script van het `.../dispatcher-sdk-x.x.x/bin` map.

>[!VIDEO](https://video.tv.adobe.com/v/3416744?quality=12&learn=on)

*Deze video gebruikt macOS ter illustratie. Met de equivalente Windows/Linux-opdrachten kunt u vergelijkbare resultaten bereiken.*


Laten we aannemen dat u in het verleden een AEM project hebt gemaakt met [Projectarchetype AEM](https://github.com/adobe/aem-project-archetype)De basislijnconfiguraties Apache en Dispatcher waren actueel. Gebruikend deze basislijnconfiguraties werden uw project-specifieke configuraties gecreeerd door, de dossiers als opnieuw te gebruiken en te kopiëren `*.vhost`, `*.conf`, `*.farm` en `*.any` van de `dispatcher/src/conf.d` en `dispatcher/src/conf.dispatcher.d` mappen. Uw lokale Dispatcher-validatie en Cloud Manager-pijplijnen waren prima.

Ondertussen werden de basislijnconfiguraties Apache en Dispatcher verbeterd om verschillende redenen, zoals nieuwe functies, beveiligingsoplossingen en optimalisatie. Ze worden vrijgegeven via een nieuwere versie van Dispatcher Tools als onderdeel van de AEM as a Cloud Service release.

Nu, wanneer het bevestigen van uw project-specifieke configuraties van de Verzender tegen de recentste versie van de Hulpmiddelen van de Verzender beginnen zij te ontbreken. Om dit te verhelpen, moeten de basislijnconfiguraties door onder stappen worden bijgewerkt te gebruiken:

+ Controleren of de validatie mislukt met de nieuwste versie van Dispatcher Tools

  ```shell
  $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Phase 3: Immutability check
  empty mode param, assuming mode = 'check'
  ...
  ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
  ```

+ Werk de onveranderlijke dossiers bij gebruikend `update_maven.sh` script

  ```shell
  $ ./bin/update_maven.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Updating dispatcher configuration at folder 
  running in 'extract' mode
  running in 'extract' mode
  reading immutable file list from /etc/httpd/immutable.files.txt
  preparing 'conf.d/available_vhosts/default.vhost' immutable file extraction
  ...
  immutable files extraction COMPLETE
  fd72f4521fa838daaaf006bb8c9c96ed33a142a2d63cc963ba4cc3dd228948fe
  Cloud manager validator 2.0.53
  ```

+ De bijgewerkte onveranderbare bestanden controleren zoals `dispatcher_vhost.conf`, `default.vhost`, en `default.farm` en breng indien nodig relevante wijzigingen aan in uw aangepaste bestanden die zijn afgeleid van deze bestanden.

+ Wijzig de configuratie en geef deze door

```shell
$ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src

...
checking 'conf.dispatcher.d/renders/default_renders.any' immutability (if present)
checking existing 'conf.dispatcher.d/renders/default_renders.any' for changes
checking 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' immutability (if present)
checking existing 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' for changes
no immutable file has been changed - check is SUCCESSFUL
Phase 3 finished
```

+ Nadat de wijzigingen lokaal zijn geverifieerd, past u de bijgewerkte configuratiebestanden toe

## Problemen oplossen

### docker_run resulteert in het bericht &#39;Wachten tot host.docker.internal beschikbaar is&#39;{#troubleshooting-host-docker-internal}

De `host.docker.internal` is hostname die aan Docker wordt verstrekt die aan de gastheer oplost. Per docs.docker.com ([macOS](https://docs.docker.com/desktop/networking/), [Windows](https://docs.docker.com/desktop/networking/)):

> Vanaf Docker 18.03 moet de aanbeveling met de speciale DNS naam host.docker.internal verbinden, die aan het interne IP adres oplost dat door de gastheer wordt gebruikt

Wanneer `bin/docker_run src host.docker.internal:4503 8080` resulteert in het bericht __Wachten tot host.docker.internal beschikbaar is__, dan:

1. Zorg ervoor dat de geïnstalleerde versie van Docker 18.03 of hoger is
1. U hebt mogelijk een lokale machine ingesteld die de registratie/resolutie van de `host.docker.internal` naam. Gebruik in plaats daarvan uw lokale IP.

>[!BEGINTABS]

>[!TAB macOS]

+ Vanuit terminal uitvoeren `ifconfig` en registreer de gastheer __kabinet__ IP adres, gewoonlijk __nl0__ apparaat.
+ Dan uitvoeren `docker_run` het gebruiken van het gastheerIP adres: `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!TAB Windows]

+ Vanuit de opdrachtprompt uitvoeren `ipconfig`en registreert de host __IPv4-adres__ van de hostcomputer.
+ Dan, voer uit `docker_run` het gebruiken van dit IP adres: `$ bin\docker_run src <HOST IP>:4503 8080`

>[!TAB Linux®]

+ Vanuit terminal uitvoeren `ifconfig` en registreer de gastheer __kabinet__ IP adres, gewoonlijk __nl0__ apparaat.
+ Dan uitvoeren `docker_run` het gebruiken van het gastheerIP adres: `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!ENDTABS]

#### Voorbeeldfout

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

## Aanvullende bronnen

+ [SDK AEM downloaden](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Docker downloaden](https://www.docker.com/)
+ [Download de AEM Reference Website (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentatie Experience Manager Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html)
