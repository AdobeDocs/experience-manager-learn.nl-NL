---
title: Dispatcher Tools instellen voor AEM as a Cloud Service ontwikkeling
description: AEM de Dispatcher Tools van SDK vergemakkelijkt de lokale ontwikkeling van Adobe Experience Manager (AEM) projecten door het gemakkelijk te maken om Dispatcher plaatselijk te installeren, in werking te stellen en problemen op te lossen.
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
kt: 4679
thumbnail: 30603.jpg
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
source-git-commit: bca51ece7a9b249727b8746cc9654503059116fb
workflow-type: tm+mt
source-wordcount: '1380'
ht-degree: 1%

---

# Lokale gereedschappen voor Dispatcher instellen {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Lokale verzendprogramma&#39;s"
>abstract="Dispatcher is een integraal onderdeel van de algemene architectuur van de Experience Manager en moet deel uitmaken van de plaatselijke ontwikkeling. De AEM as a Cloud Service SDK bevat de aanbevolen versie van Dispatcher Tools, waarmee u Dispatcher lokaal kunt configureren, valideren en simuleren."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/disp-overview.html" text="Dispatcher in de cloud"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="AEM as a Cloud Service SDK downloaden"

De Verzender van Adobe Experience Manager (AEM) is een Apache HTTP-webservermodule die een beveiligings- en prestatielaag biedt tussen de CDN- en AEM-publicatielaag. Dispatcher is een integraal onderdeel van de algemene architectuur van de Experience Manager en moet deel uitmaken van de plaatselijke ontwikkeling.

De AEM as a Cloud Service SDK bevat de aanbevolen versie van Dispatcher Tools, waarmee u Dispatcher lokaal kunt configureren, valideren en simuleren. Dispatcher Tools bestaat uit:

+ een basislijnset van Apache HTTP Web-server en Dispatcher-configuratiebestanden in `.../dispatcher-sdk-x.x.x/src`
+ een CLI-hulpprogramma voor configuratievalidatie, dat zich bevindt op `.../dispatcher-sdk-x.x.x/bin/validate`
+ een hulpmiddel van de configuratiegeneratie CLI, dat bij wordt gevestigd `.../dispatcher-sdk-x.x.x/bin/validator`
+ een hulpmiddel van de configuratieplaatsing CLI, dat bij wordt gevestigd `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ een Docker-afbeelding die Apache HTTP Web-server uitvoert met de module Dispatcher

Let op: `~` wordt gebruikt als steno voor de Folder van de Gebruiker. In Windows is dit het equivalent van `%HOMEPATH%`.

>[!NOTE]
>
> De video&#39;s op deze pagina zijn opgenomen op macOS. Windows-gebruikers kunnen de stappen volgen, maar gebruiken de equivalente Windows-opdrachten van Dispatcher Tools, die bij elke video worden geleverd.

## Vereisten

1. Windows-gebruikers moeten Windows 10 Professional gebruiken (of een versie die Docker ondersteunt)
1. Installeren [Experience Manager publiceert QuickStart Jar](./aem-runtime.md) op de lokale ontwikkelmachine.
   + Installeer indien nodig de nieuwste [AEM](https://github.com/adobe/aem-guides-wknd/releases) op de lokale AEM-publicatieservice. Deze website wordt in deze zelfstudie gebruikt om een werkende Dispatcher te visualiseren.
1. De nieuwste versie van [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) op de lokale ontwikkelcomputer.

## Download de Dispatcher Tools (als onderdeel van de AEM SDK)

De AEM as a Cloud Service SDK, of AEM SDK, bevat de Dispatcher Tools die worden gebruikt om de Apache HTTP Web-server met de Dispatcher-module lokaal uit te voeren voor ontwikkeling, evenals de compatibele QuickStart Jar.

Als de AEM as a Cloud Service SDK al is gedownload naar [de lokale AEM-runtime instellen](./aem-runtime.md)hoeft u de toepassing niet opnieuw te downloaden.

1. Aanmelden bij [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list p.offset=0&amp;p.limit=1) met uw Adobe ID
   + Uw Adobe-organisatie __moet__ zijn ingericht voor AEM as a Cloud Service om de AEM as a Cloud Service SDK te downloaden
1. Klik op de nieuwste __AEM SDK__ te downloaden resultatenrij

## De Dispatcher Tools extraheren uit het ZIP-bestand van de AEM SDK

>[!TIP]
>
> Windows-gebruikers kunnen geen spaties of speciale tekens bevatten in het pad naar de map met de gereedschappen voor lokale verzending. Als er spaties in het pad voorkomen, `docker_run.cmd` zal mislukken.

De versie van Dispatcher Tools verschilt van die van de AEM SDK. Zorg ervoor dat de versie van Dispatcher Tools beschikbaar is via de AEM SDK-versie die overeenkomt met de as a Cloud Service AEM versie.

1. De gedownloade `aem-sdk-xxx.zip` file
1. Verpak de Dispatcher Tools uit in `~/aem-sdk/dispatcher`
   + Windows: Unzip `aem-sdk-dispatcher-tools-x.x.x-windows.zip` in `C:\Users\<My User>\aem-sdk\dispatcher` (ontbrekende mappen maken, indien nodig)
   + macOS / Linux: Het bijbehorende shellscript uitvoeren `aem-sdk-dispatcher-tools-x.x.x-unix.sh` om de Dispatcher Tools uit te pakken
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Merk op dat alle hieronder uitgegeven bevelen veronderstellen de huidige het werk folder de het uitbreiden inhoud van de Hulpmiddelen van de Verzender bevat.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*Deze video gebruikt macOS ter illustratie. Met de equivalente Windows/Linux-opdrachten kunt u vergelijkbare resultaten bereiken*

## De Dispatcher-configuratiebestanden begrijpen

>[!TIP]
> Experience Manager projecten die zijn gemaakt op basis van de [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype) Deze set Dispatcher-configuratiebestanden zijn vooraf ingevuld. U hoeft daarom niet meer te kopiëren uit de bronmap Dispatcher Tools.

De Dispatcher Tools biedt een set Apache HTTP Web-server- en Dispatcher-configuratiebestanden die gedrag voor alle omgevingen, inclusief lokale ontwikkeling, definiëren.

Deze bestanden zijn bedoeld om naar een Experience Manager Maven-project te worden gekopieerd `dispatcher/src` als deze nog niet bestaan in het Experience Manager Maven-project.

Een volledige beschrijving van de configuratiebestanden is beschikbaar in de onverpakte Dispatcher Tools als `dispatcher-sdk-x.x.x/docs/Config.html`.

## Configuraties valideren

Optioneel, de Dispatcher en Apache de serverconfiguraties van het Web (via `httpd -t`) kan worden gevalideerd met de `validate` script (niet te verwarren met het `validator` uitvoerbaar). De `validate` is een handige manier om het script uit te voeren [3 fasen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/validation-debug.html?lang=en#local-validation-flexible-mode) van de `validator`.

+ Gebruik:
   + Windows: `bin\validate src`
   + macOS / Linux: `./bin/validate.sh ./src`

## Verzending lokaal uitvoeren

AEM Dispatcher lokaal wordt uitgevoerd met Docker op de `src` Configuratiebestanden van de server Dispatcher en Apache Web.

+ Gebruik:
   + Windows: `bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS / Linux: `./bin/docker_run.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

De `<aem-publish-host>` kan worden ingesteld op `host.docker.internal`, verstrekt een speciale DNS naamDocker in de container die aan IP van de gastheermachine oplost. Als hij `host.docker.internal` niet wordt opgelost, raadpleeg de [problemen oplossen](#troubleshooting-host-docker-internal) hieronder.

U kunt bijvoorbeeld de Dispatcher Docker-container starten met de standaardconfiguratiebestanden van de Dispatcher Tools:

Start Dispatcher Docker-container die het pad naar de Dispatcher-configuratiemap bevat:

+ Windows: `bin\docker_run src host.docker.internal:4503 8080`
+ macOS / Linux: `./bin/docker_run.sh ./src host.docker.internal:4503 8080`

De AEM as a Cloud Service publicatieservice van SDK, die plaatselijk op haven 4503 loopt zal door Dispatcher bij beschikbaar zijn `http://localhost:8080`.

Om de Hulpmiddelen van de Verzender tegen de configuratie van de Verzender van een project van de Experience Manager in werking te stellen, richt aan uw project `dispatcher/src` map.

+ Windows:

   ```shell
   $ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

## Logboeken voor Dispatcher Tools

Logboeken van de verzender zijn nuttig tijdens lokale ontwikkeling om te begrijpen als en waarom de Verzoeken van HTTP worden geblokkeerd. U kunt het logniveau instellen door vooraf de uitvoering van `docker_run` met omgevingsparameters.

Logboeken van Dispatcher Tools worden uitgestraald naar de standaard wanneer `docker_run` wordt uitgevoerd.

Nuttige parameters voor het zuiveren Dispatcher omvatten:

+ `DISP_LOG_LEVEL=Debug` Hiermee stelt u de logboekregistratie van de module Dispatcher in op het niveau Foutopsporing
   + De standaardwaarde is: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` stelt Apache HTTP Web server herwrite module logging to Debug level in
   + De standaardwaarde is: `Warn`
+ `DISP_RUN_MODE` Hiermee stelt u de &quot;uitvoeringsmodus&quot; van de Dispatcher-omgeving in en laadt u de bijbehorende uitvoermodi Dispatcher-configuratiebestanden.
   + Standaardwaarden: `dev`
+ Geldige waarden: `dev`, `stage`, of `prod`

Een of meer parameters, die kunnen worden doorgegeven aan `docker_run`

+ Windows:

   ```shell
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

### Toegang tot logbestanden

Logbestanden van Apache-webservers en AEM Dispatcher kunnen rechtstreeks worden geopend in de Docker-container:

+ [De toegang tot van logboeken in de container van de Dokker](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [De Docker-logbestanden worden naar het lokale bestandssysteem gekopieerd](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Wanneer werkt u de Dispatcher Tools bij{#dispatcher-tools-version}

Versies van Dispatcher Tools nemen minder vaak toe dan de Experience Manager en dus vereisen Dispatcher Tools minder updates in de lokale ontwikkelomgeving.

De aanbevolen versie van Dispatcher Tools is die welke is meegeleverd bij de AEM as a Cloud Service SDK die overeenkomt met de as a Cloud Service versie van de Experience Manager. De versie van AEM as a Cloud Service is te vinden via [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Omgevingen__, per door de __AEM__ label

![Versie Experience Manager](./assets/dispatcher-tools/aem-version.png)

_De versie Dispatcher Tools zelf komt niet overeen met de versie van Experience Manager._

## Problemen oplossen

### docker_run resulteert in het bericht &#39;Wachten tot host.docker.internal beschikbaar is&#39;{#troubleshooting-host-docker-internal}

`host.docker.internal` is hostname die aan Docker wordt verstrekt die aan de gastheer oplost. Per docs.docker.com ([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)):

> Vanaf Docker 18.03 is onze aanbeveling met de speciale DNS naam host.docker.internal te verbinden, die aan het interne IP adres oplost dat door de gastheer wordt gebruikt

Als, wanneer `bin/docker_run src host.docker.internal:4503 8080` resulteert in het bericht __Wachten tot host.docker.internal beschikbaar is__, dan:

1. Zorg ervoor dat de geïnstalleerde versie van Docker 18.03 of hoger is
2. U hebt mogelijk een lokale machine ingesteld die de registratie/resolutie van de `host.docker.internal` naam. Gebruik in plaats daarvan uw lokale IP.
   + Windows:
      + Vanuit de opdrachtprompt uitvoeren `ipconfig`en registreert de host __IPv4-adres__ van de hostcomputer.
      + Dan, voer uit `docker_run` het gebruiken van dit IP adres:
         `bin\docker_run src <HOST IP>:4503 8080`
   + macOS / Linux:
      + Vanuit terminal uitvoeren `ifconfig` en registreer de gastheer __kabinet__ IP adres, gewoonlijk __nl0__ apparaat.
      + Dan uitvoeren `docker_run` het gebruiken van het gastheerIP adres:
         `bin/docker_run.sh src <HOST IP>:4503 8080`

#### Voorbeeldfout

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

### docker_run kan niet worden gestart in Windows{#troubleshooting-windows-compatible}

Wordt uitgevoerd `docker_run` in Windows kan de volgende fout optreden, zodat Dispatcher niet kan worden gestart. Dit probleem wordt gemeld bij Dispatcher in Windows en in een toekomstige versie opgelost.

#### Voorbeeldfout

```shell
$ \Users\MyUser\aem-sdk\dispatcher>bin\docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
host.docker.internal resolves to 192.168.65.2
Running script /docker_entrypoint.d/40-generate-allowed-clients.sh
Running script /docker_entrypoint.d/50-check-expiration.sh
Running script /docker_entrypoint.d/60-check-loglevel.sh
Running script /docker_entrypoint.d/70-check-forwarded-host-secret.sh
Starting httpd server
[Sun Feb 09 17:32:22.256020 2020] [dispatcher:warn] [pid 1:tid 140080096570248] Unable to obtain parent directory of /etc/httpd/conf.dispatcher.d/enabled_farms/farms.any: No such file or directory
[Sun Feb 09 17:32:22.256069 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Unable to import config file: /etc/httpd/conf.dispatcher.d/dispatcher.any
[Sun Feb 09 17:32:22.256074 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Dispatcher initialization failed.
AH00016: Configuration Failed
```

## Aanvullende bronnen

+ [SDK AEM downloaden](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Docker downloaden](https://www.docker.com/)
+ [Download de AEM Reference Website (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentatie Experience Manager Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html)
