---
title: Dispatcher Tools instellen voor AEM als ontwikkeling van Cloud Servicen
description: AEM de Dispatcher Tools van SDK vergemakkelijkt de lokale ontwikkeling van Adobe Experience Manager (AEM) projecten door het gemakkelijk te maken om Dispatcher plaatselijk te installeren, in werking te stellen en problemen op te lossen.
sub-product: stichting
feature: dispatcher
topics: development, caching, security
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4679
thumbnail: 30603.jpg
translation-type: tm+mt
source-git-commit: 1b4a927a68d24eeb08d0ee244e85519323482910
workflow-type: tm+mt
source-wordcount: '1534'
ht-degree: 1%

---


# Lokale gereedschappen voor Dispatcher instellen

De Verzender van Adobe Experience Manager (AEM) is een Apache HTTP-webservermodule die een beveiligings- en prestatielaag biedt tussen de CDN- en AEM-publicatielaag. Dispatcher is een integraal onderdeel van de algemene architectuur van de Experience Manager en moet deel uitmaken van de plaatselijke ontwikkeling.

De AEM als Cloud Service SDK bevat de aanbevolen versie van Dispatcher Tools, waarmee u Dispatcher lokaal kunt configureren, valideren en simuleren. Dispatcher Tools bestaat uit:

+ een basislijnset van Apache HTTP Web-server en Dispatcher-configuratiebestanden in `.../dispatcher-sdk-x.x.x/src`
+ een CLI-hulpprogramma voor configuratievalidatie, dat zich bevindt op `.../dispatcher-sdk-x.x.x/bin/validate` (Dispatcher SDK 2.0.29+)
+ een hulpmiddel van de configuratiegeneratie CLI, dat bij wordt gevestigd `.../dispatcher-sdk-x.x.x/bin/validator`
+ een hulpmiddel van de configuratieplaatsing CLI, dat bij wordt gevestigd `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ een Docker-afbeelding die Apache HTTP Web-server uitvoert met de module Dispatcher

Merk op dat `~` als steno voor de Folder van de Gebruiker wordt gebruikt. In Windows is dit het equivalent van `%HOMEPATH%`.

>[!NOTE]
>
> De video&#39;s op deze pagina zijn opgenomen op macOS. Windows-gebruikers kunnen de stappen volgen, maar gebruiken de equivalente Windows-opdrachten van Dispatcher Tools, die bij elke video worden geleverd.

## Vereisten

1. Windows-gebruikers moeten Windows 10 Professional gebruiken
1. Installeer [Experience Manager Publish QuickStart Jar](./aem-runtime.md) op de lokale ontwikkelcomputer.
   + U kunt desgewenst de nieuwste [AEM referentiewebsite](https://github.com/adobe/aem-guides-wknd/releases) installeren op de lokale AEM-publicatieservice. Deze website wordt in deze zelfstudie gebruikt om een werkende Dispatcher te visualiseren.
1. Installeer en start de nieuwste versie van [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) op de lokale ontwikkelcomputer.

## Download de Dispatcher Tools (als onderdeel van de AEM SDK)

De AEM als Cloud Service SDK, of AEM SDK, bevat de Dispatcher Tools die worden gebruikt om de server van het Web van Apache HTTP met de module van de Verzender plaatselijk voor ontwikkeling in werking te stellen, evenals compatibele Jar QuickStart.

Als de AEM als Cloud Service SDK al is gedownload om de lokale AEM-runtime [in te](./aem-runtime.md)stellen, hoeft deze niet opnieuw te worden gedownload.

1. Meld u aan bij [Experience.adobe.com/#/download](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list p.offset=0&amp;p.limit=1) met uw Adobe ID
   + Uw organisatie van de Adobe __moet__ voor AEM als Cloud Service worden provisioned om de AEM als Cloud Service SDK te downloaden
1. Klik op de laatste __AEM SDK__ -resultaatrij die u wilt downloaden
   + Zorg ervoor dat AEM Dispatcher Tools v2.0.29+ van SDK is opgenomen in de downloadbeschrijving

## De Dispatcher Tools extraheren uit het ZIP-bestand van de AEM SDK

>[!TIP]
>
> Windows-gebruikers kunnen geen spaties of speciale tekens bevatten in het pad naar de map met de gereedschappen voor lokale verzending. Als het pad spaties bevat, `docker_run.cmd` mislukt het.

De versie van Dispatcher Tools verschilt van die van de AEM SDK. Zorg ervoor dat de versie van Dispatcher Tools beschikbaar is via de AEM SDK-versie die overeenkomt met de AEM als Cloud Service-versie.

1. Het gedownloade `aem-sdk-xxx.zip` bestand uitpakken
1. Verpak de Dispatcher Tools uit in `~/aem-sdk/dispatcher`
   + Windows: Uitpakken `aem-sdk-dispatcher-tools-x.x.x-windows.zip` in `C:\Users\<My User>\aem-sdk\dispatcher` (ontbrekende mappen maken indien nodig)
   + macOS / Linux: Voer het begeleidende shell manuscript uit `aem-sdk-dispatcher-tools-x.x.x-unix.sh` om de Hulpmiddelen van de Ontvanger uit te pakken
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Merk op dat alle hieronder uitgegeven bevelen veronderstellen de huidige het werk folder de het uitbreiden inhoud van de Hulpmiddelen van de Verzender bevat.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*Deze video gebruikt macOS voor illustratieve doeleinden. Met de equivalente Windows/Linux-opdrachten kunt u vergelijkbare resultaten bereiken*

## De Dispatcher-configuratiebestanden begrijpen

>[!TIP]
> De projecten van de Experience Manager die van het [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype) worden gecreeerd zijn pre-bevolkte deze reeks de configuratiedossiers van de Dispatcher, zodat is er geen behoefte om over van de omslag van de Hulpmiddelen van de Verzender te kopiëren src.

De Dispatcher Tools biedt een set Apache HTTP Web-server- en Dispatcher-configuratiebestanden die gedrag voor alle omgevingen, inclusief lokale ontwikkeling, definiëren.

Deze dossiers zijn bedoeld om in een Experience Manager Maven project aan de `dispatcher/src` omslag worden gekopieerd, als zij niet reeds in de Experience Manager Maven project bestaan.

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)

*Deze video gebruikt macOS voor illustratieve doeleinden. Met de equivalente Windows/Linux-opdrachten kunt u vergelijkbare resultaten bereiken*

Een volledige beschrijving van de configuratiebestanden is beschikbaar in de onverpakte Dispatcher Tools als `dispatcher-sdk-x.x.x/docs/Config.html`.

## Configuraties valideren

Naar keuze, kunnen de Dispatcher en Apache de serverconfiguraties van het Web (via `httpd -t`) worden bevestigd gebruikend het `validate` manuscript (niet om met uitvoerbaar `validator` te worden verward).

+ Gebruik:
   + Windows: `bin\validate src`
   + macOS / Linux: `./bin/validate ./src`

## Verzending lokaal uitvoeren

Om de Verzender plaatselijk in werking te stellen, moeten de de configuratiedossiers van de Verzender worden geproduceerd gebruikend het hulpmiddel `validator` CLI van de Hulpmiddelen van de Verzender.

+ Gebruik:
   + Windows: `bin\validator full -d out src`
   + macOS / Linux: `./bin/validator full -d ./out ./src`

Dit bevel transpileert de configuraties in een dossier-reeks compatibel met de Server van het Web van Apache HTTP van de container van de Docker.

Zodra geproduceerd, worden de getranspileerde configuraties gebruikt in werking gestelde Verzender plaatselijk in de container van het Dok. Het is belangrijk dat u ervoor zorgt dat de nieuwste configuraties zijn gevalideerd met behulp van de `validate` optie van de validator __en zijn uitgevoerd__ `-d` .

+ Gebruik:
   + Windows: `bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS / Linux: `./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

De `aem-publish-host` `host.docker.internal`kan aan worden geplaatst, verstrekt een speciale DNS naamDocker in de container die aan IP van de gastheermachine oplost. Raadpleeg de onderstaande sectie `host.docker.internal` Problemen oplossen [als hij dit](#troubleshooting-host-docker-internal) niet verhelpt.

U kunt bijvoorbeeld de Dispatcher Docker-container starten met de standaardconfiguratiebestanden van de Dispatcher Tools:

1. Genereer de `deployment-folder`naamruimte `out` bij conventie elke keer dat een configuratie verandert:

   + Windows: `del /Q out && bin\validator full -d out src`
   + macOS / Linux: `rm -rf ./out && ./bin/validator full -d ./out ./src`

2. (Opnieuw) start Dispatcher Docker-container die het pad naar de implementatiemap bevat:

   + Windows: `bin\docker_run out host.docker.internal:4503 8080`
   + macOS / Linux: `./bin/docker_run.sh ./out host.docker.internal:4503 8080`

De AEM als de Publish Service van Cloud Service SDK, die plaatselijk op haven 4503 loopt zal door Dispatcher bij `http://localhost:8080`. beschikbaar zijn.

Om de Hulpmiddelen van de Verzender tegen de configuratie van de Verzender van een project van de Experience Manager in werking te stellen, produceert eenvoudig het `deployment-folder` gebruiken van de `dispatcher/src` omslag van het project.

+ Windows:

   ```shell
   $ del -/Q out && bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ rm -rf ./out && ./bin/validator full -d ./out ~/code/my-project/dispatcher/src
   $ ./bin/docker_run.sh ./out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30603/?quality=12&learn=on)

*Deze video gebruikt macOS voor illustratieve doeleinden. Met de equivalente Windows/Linux-opdrachten kunt u vergelijkbare resultaten bereiken*

## Logboeken voor Dispatcher Tools

Logboeken van de verzender zijn nuttig tijdens lokale ontwikkeling om te begrijpen als en waarom de Verzoeken van HTTP worden geblokkeerd. U kunt het logniveau instellen door de uitvoering vooraf te bepalen `docker_run` met omgevingsparameters.

Logboeken voor Dispatcher Tools worden uitgestraald naar de standaard wanneer `docker_run` wordt uitgevoerd.

Nuttige parameters voor het zuiveren Dispatcher omvatten:

+ `DISP_LOG_LEVEL=Debug` Hiermee stelt u de logboekregistratie van de module Dispatcher in op het niveau Foutopsporing
   + De standaardwaarde is: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` stelt Apache HTTP Web server herwrite module logging to Debug level in
   + De standaardwaarde is: `Warn`
+ `DISP_RUN_MODE` Hiermee stelt u de &quot;uitvoeringsmodus&quot; van de Dispatcher-omgeving in en laadt u de bijbehorende uitvoermodi Dispatcher-configuratiebestanden.
   + Defaults to `dev`
+ Geldige waarden: `dev`, `stage`of `prod`

Een of meer parameters, die kunnen worden doorgegeven aan `docker_run`

+ Windows:

   ```shell
   $ bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ ./bin/validator full -d out ~/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30604/?quality=12&learn=on)

*Deze video gebruikt macOS voor illustratieve doeleinden. Met de equivalente Windows/Linux-opdrachten kunt u vergelijkbare resultaten bereiken*

## Wanneer werkt u de Dispatcher Tools bij{#dispatcher-tools-version}

Versies van Dispatcher Tools nemen minder vaak toe dan de Experience Manager en dus vereisen Dispatcher Tools minder updates in de lokale ontwikkelomgeving.

De geadviseerde versie van de Hulpmiddelen van de Verzender is die die met de AEM als Cloud Service SDK wordt gebundeld die de Experience Manager als versie van de Cloud Service aanpast. De versie van AEM als Cloud Service vindt u via [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Omgevingen__, per omgeving die is opgegeven met het label voor __AEM release__

![Versie Experience Manager](./assets/dispatcher-tools/aem-version.png)

_De versie Dispatcher Tools zelf komt niet overeen met de versie van Experience Manager._

## Problemen oplossen

### docker_run resulteert in het bericht &#39;Wachten tot host.docker.internal beschikbaar is&#39;{#troubleshooting-host-docker-internal}

`host.docker.internal` is hostname die aan Docker wordt verstrekt die aan de gastheer oplost. Per docs.docker.com ([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)):

> Vanaf Docker 18.03 is onze aanbeveling met de speciale DNS naam host.docker.internal te verbinden, die aan het interne IP adres oplost dat door de gastheer wordt gebruikt

Als, wanneer in het bericht het `bin/docker_run out host.docker.internal:4503 8080` Wachten tot host.docker.internal beschikbaar ____ is, dan resulteert:

1. Zorg ervoor dat de geïnstalleerde versie van Docker 18.03 of hoger is
2. U kunt een lokale machine hebben opstelling die de registratie/de resolutie van de `host.docker.internal` naam verhindert. Gebruik in plaats daarvan uw lokale IP.
   + Windows:
      + Van de Herinnering van het Bevel, voer uit `ipconfig`, en registreer het __IPv4 Adres__ van de gastheer van de gastheermachine.
      + Voer vervolgens `docker_run` het volgende IP-adres uit:
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS / Linux:
      + Van Eind, voer uit `ifconfig` en registreer het __inet__ IP van de Gastheer adres, gewoonlijk het __en0__ apparaat.
      + Voer vervolgens `docker_run` het IP-adres van de host uit:
         `bin/docker_run.sh out <HOST IP>:4503 8080`

#### Voorbeeldfout

```shell
$ docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

### docker_run resulteert in &#39;** fout: Implementatiemap niet gevonden&#39;

Tijdens het uitvoeren `docker_run.cmd`wordt een fout weergegeven met de volgende __** fout: Implementatiemap niet gevonden:__. Dit gebeurt gewoonlijk omdat het pad spaties bevat. Verwijder indien mogelijk de spaties in de map of verplaats de `aem-sdk` map naar een pad dat geen spaties bevat.

Windows-gebruikersmappen zijn bijvoorbeeld vaak `<First name> <Last name>`met een tussenruimte. In het onderstaande voorbeeld `...\My User\...` bevat de map een spatie waarmee de uitvoering van de lokale Dispatcher Tools wordt `docker_run` verbroken. Als de ruimten in een gebruikersomslag van Vensters zijn, probeer niet om deze omslag anders te noemen aangezien het Vensters zal breken, in plaats daarvan de `aem-sdk` omslag naar een nieuwe plaats te verplaatsen uw gebruiker toestemmingen heeft om volledig te wijzigen. Merk op dat de instructies die veronderstellen de `aem-sdk` omslag in de huisfolder van de gebruiker is zullen moeten worden aangepast aan de nieuwe plaats.

#### Voorbeeldfout

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### docker_run kan niet worden gestart in Windows{#troubleshooting-windows-compatible}

Als u `docker_run` Windows uitvoert, treedt mogelijk de volgende fout op, waardoor Dispatcher niet kan worden gestart. Dit probleem wordt gemeld bij Dispatcher in Windows en in een toekomstige versie opgelost.

#### Voorbeeldfout

```shell
$ \Users\MyUser\aem-sdk\dispatcher>bin\docker_run out host.docker.internal:4503 8080

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
+ [Documentatie Experience Manager Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/dispatcher.html)
