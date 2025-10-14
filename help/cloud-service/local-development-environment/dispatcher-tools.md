---
title: Dispatcher Tools instellen voor AEM as a Cloud Service Development
description: AEM SDK Dispatcher Tools maakt de lokale ontwikkeling van Adobe Experience Manager (AEM)-projecten mogelijk door het gemakkelijk te maken om Dispatcher lokaal te installeren, uit te voeren en problemen op te lossen.
version: Experience Manager as a Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
jira: KT-4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
duration: 624
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1620'
ht-degree: 0%

---

# Lokale Dispatcher-gereedschappen instellen {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Lokale Dispatcher-gereedschappen"
>abstract="Dispatcher is een integraal onderdeel van de algemene Experience Manager-architectuur en moet deel uitmaken van de plaatselijke ontwikkelingsinstellingen. De AEM as a Cloud Service SDK bevat de aanbevolen versie van Dispatcher Tools waarmee u het valideren en het lokaal simuleren van Dispatcher kunt configureren."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html?lang=nl-NL" text="Dispatcher in de cloud"
>additional-url="https://experienceleague.adobe.com/docs/experience-cloud/software-distribution/home.html?lang=nl-NL" text="AEM as a Cloud Service SDK downloaden"

Adobe Experience Manager (AEM)&#39;s Dispatcher is een Apache HTTP Web server module die een veiligheid en prestatieslaag tussen CDN en AEM Publish rij verstrekt. Dispatcher is een integraal onderdeel van de algemene Experience Manager-architectuur en moet deel uitmaken van de plaatselijke ontwikkelingsinstellingen.

De AEM as a Cloud Service SDK bevat de aanbevolen versie van Dispatcher Tools waarmee u het valideren en het lokaal simuleren van Dispatcher kunt configureren. Dispatcher Tools bestaat uit:

+ een basislijnset van Apache HTTP Web server- en Dispatcher-configuratiebestanden op `.../dispatcher-sdk-x.x.x/src`
+ een CLI-hulpprogramma voor configuratievalidatie, dat zich bevindt op `.../dispatcher-sdk-x.x.x/bin/validate`
+ een CLI-hulpprogramma voor configuratiegeneratie, dat zich bevindt op `.../dispatcher-sdk-x.x.x/bin/validator`
+ een CLI-hulpprogramma voor configuratieimplementatie, dat zich bevindt op `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ een onveranderlijke configuratiedossiers die CLI hulpmiddel beschrijven, die bij `.../dispatcher-sdk-x.x.x/bin/update_maven` wordt gevestigd
+ een Docker-afbeelding die Apache HTTP Web-server uitvoert met de Dispatcher-module

`~` wordt gebruikt als steno voor de gebruikerslijst. In Windows is dit het equivalent van `%HOMEPATH%` .

>[!NOTE]
>
> De video&#39;s op deze pagina zijn opgenomen op macOS. Windows-gebruikers kunnen de stappen volgen, maar gebruiken de equivalente opdrachten van Dispatcher Tools Windows, die bij elke video worden geleverd.

## Vereisten

1. Windows-gebruikers moeten Windows 10 Professional gebruiken (of een versie die Docker ondersteunt)
1. Installeer [&#x200B; Experience Manager publiceert QuickStart Jar &#x200B;](./aem-runtime.md) op de lokale ontwikkelingsmachine.

+ Naar keuze, installeer de recentste [&#x200B; de verwijzingsWebsite van AEM &#x200B;](https://github.com/adobe/aem-guides-wknd/releases) op de lokale publicatiedienst van AEM. Deze website wordt in deze zelfstudie gebruikt om een werkende Dispatcher te visualiseren.

1. Installeer en begin de recentste versie van [&#x200B; Docker &#x200B;](https://www.docker.com/) (de Desktop van de Docker 2.2.0.5 +/de Motor van de Docker v19.03.9+) op de lokale ontwikkelingsmachine.

## Download de Dispatcher Tools (als onderdeel van de AEM SDK)

AEM as a Cloud Service SDK, of AEM SDK, bevat de Dispatcher Tools die worden gebruikt om Apache HTTP Web-server met de module van Dispatcher plaatselijk voor ontwikkeling in werking te stellen, en compatibele Jar QuickStart.

Als AEM as a Cloud Service SDK reeds aan [&#x200B; opstelling lokale runtime van AEM &#x200B;](./aem-runtime.md) is gedownload, te hoeven het niet opnieuw worden gedownload.

1. Meld u aan bij [&#x200B; experience.adobe.com/#/downloads &#x200B;](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list p.offset=0&amp;p.limit=1) met uw Adobe ID
   + Uw Adobe-organisatie __moet__ zijn ingericht voor AEM as a Cloud Service om de AEM as a Cloud Service SDK te downloaden
1. Klik op de recentste __AEM SDK__ resultaatrij om te downloaden

## De Dispatcher-gereedschappen extraheren uit het zip van AEM SDK

>[!TIP]
>
> Windows-gebruikers kunnen geen spaties of speciale tekens gebruiken in het pad naar de map met de lokale Dispatcher-gereedschappen. Als het pad spaties bevat, mislukt `docker_run.cmd` .

De versie van Dispatcher Tools verschilt van die van AEM SDK. Zorg ervoor dat de versie van Dispatcher Tools beschikbaar is via de AEM SDK-versie die overeenkomt met de AEM as a Cloud Service-versie.

1. Het gedownloade `aem-sdk-xxx.zip` bestand uitpakken
1. Pak de Dispatcher-gereedschappen uit in `~/aem-sdk/dispatcher`

>[!BEGINTABS]

>[!TAB  macOS ]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!TAB  Vensters ]

Pak `aem-sdk-dispatcher-tools-x.x.x-windows.zip` uit in `C:\Users\<My User>\aem-sdk\dispatcher` (maak indien nodig ontbrekende mappen).

>[!TAB  Linux® ]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!ENDTABS]

Alle hieronder uitgegeven bevelen veronderstellen dat de huidige het werk folder de het uitbreiden inhoud van de Hulpmiddelen van Dispatcher bevat.

>[!VIDEO](https://video.tv.adobe.com/v/30601?quality=12&learn=on)

*Deze video gebruikt macOS voor illustratieve doeleinden. De gelijkwaardige bevelen Windows/Linux kunnen worden gebruikt om gelijkaardige resultaten te bereiken.*

## De Dispatcher-configuratiebestanden begrijpen

>[!TIP]
> De projecten van Experience Manager die van het [&#x200B; Project van AEM worden gecreeerd Maven Archetype &#x200B;](https://github.com/adobe/aem-project-archetype) zijn pre-bevolkte deze reeks van de configuratiedossiers van Dispatcher, zodat is er geen behoefte om over van de omslag te kopiëren van Dispatcher Tools src.

De Dispatcher Tools biedt een set Apache HTTP Web server- en Dispatcher-configuratiebestanden die gedrag voor alle omgevingen, inclusief lokale ontwikkeling, definiëren.

Deze bestanden zijn bedoeld om naar een Experience Manager Maven-project naar de map `dispatcher/src` te worden gekopieerd als ze niet bestaan in het Experience Manager Maven-project.

Een volledige beschrijving van de configuratiebestanden is beschikbaar in de onverpakte Dispatcher Tools als `dispatcher-sdk-x.x.x/docs/Config.html` .

## Configuraties valideren

Optioneel kunnen de Dispatcher- en Apache Web-serverconfiguraties (via `httpd -t`) worden gevalideerd met behulp van het `validate` -script (niet te verwarren met het `validator` -uitvoerbare bestand). Het `validate` manuscript verstrekt een geschikte manier om de [&#x200B; drie fasen &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=nl-NL) van `validator` in werking te stellen.


>[!BEGINTABS]

>[!TAB  macOS ]

```shell
$ ./bin/validate.sh ./src
```

>[!TAB  Vensters ]

```shell
$ bin\validate src
```

>[!TAB  Linux® ]

```shell
$ ./bin/validate.sh ./src
```

>[!ENDTABS]

## Dispatcher lokaal uitvoeren

AEM Dispatcher wordt lokaal uitgevoerd met Docker op basis van de configuratiebestanden van de Dispatcher- en Apache-webserver. `src`


>[!BEGINTABS]

>[!TAB  macOS ]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

Het uitvoerbare bestand van `docker_run_hot_reload` heeft de voorkeur boven `docker_run` als het configuratiebestanden opnieuw laadt terwijl deze worden gewijzigd, zonder dat het bestand handmatig moet worden afgesloten en opnieuw moet worden gestart `docker_run` . U kunt `docker_run` ook gebruiken, maar hiervoor moet `docker_run` handmatig worden beëindigd en opnieuw worden gestart wanneer configuratiebestanden worden gewijzigd.

>[!TAB  Vensters ]

```shell
$ bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

>[!TAB  Linux® ]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

Het uitvoerbare bestand van `docker_run_hot_reload` heeft de voorkeur boven `docker_run` als het configuratiebestanden opnieuw laadt terwijl deze worden gewijzigd, zonder dat het bestand handmatig moet worden afgesloten en opnieuw moet worden gestart `docker_run` . U kunt `docker_run` ook gebruiken, maar hiervoor moet `docker_run` handmatig worden beëindigd en opnieuw worden gestart wanneer configuratiebestanden worden gewijzigd.

>[!ENDTABS]

`<aem-publish-host>` kan aan `host.docker.internal` worden geplaatst, verstrekt een speciale DNS naamDocker in de container die aan IP van de gastheermachine oplost. Als `host.docker.internal` niet oplost, te zien gelieve de [&#x200B; het oplossen van problemen](#troubleshooting-host-docker-internal) hieronder sectie.

U kunt bijvoorbeeld de Dispatcher Docker-container starten met de standaardconfiguratiebestanden van de Dispatcher Tools:

Start de Dispatcher Docker-container die het pad naar de SDR-configuratiemap van Dispatcher bevat:

>[!BEGINTABS]

>[!TAB  macOS ]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!TAB  Vensters ]

```shell
$ bin\docker_run src host.docker.internal:4503 8080
```

>[!TAB  Linux® ]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!ENDTABS]

De publicatieservice van AEM as a Cloud Service SDK, die lokaal wordt uitgevoerd op poort 4503, is beschikbaar via Dispatcher op `http://localhost:8080` .

Als u Dispatcher Tools wilt uitvoeren in combinatie met de Dispatcher-configuratie van een Experience Manager-project, wijst u naar de map `dispatcher/src` van uw project.

>[!BEGINTABS]

>[!TAB  macOS ]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB  Vensters ]

```shell
$ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB  Linux® ]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]


## Dispatcher Tools-logboeken

Dispatcher-logboeken zijn handig tijdens lokale ontwikkeling om te begrijpen of en waarom HTTP-aanvragen worden geblokkeerd. U kunt het logniveau instellen door de uitvoering van `docker_run` vooraf te bepalen met omgevingsparameters.

Logboeken van Dispatcher Tools worden uitgestraald naar de standaard-out wanneer `docker_run` wordt uitgevoerd.

Handige parameters voor foutopsporing in Dispatcher zijn:

+ `DISP_LOG_LEVEL=Debug` stelt logboekregistratie voor de Dispatcher-module in op Foutopsporingsniveau
   + Standaardwaarde is: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` stelt Apache HTTP Web server rewrite module logging to Debug level in
   + Standaardwaarde is: `Warn`
+ `DISP_RUN_MODE` stelt de &quot;uitvoeringsmodus&quot; van de Dispatcher-omgeving in en laadt de bijbehorende uitvoermodi Dispatcher-configuratiebestanden.
   + Heeft als standaardwaarde `dev`
+ Geldige waarden: `dev`, `stage` of `prod`

Een of meerdere parameters, kan worden doorgegeven aan `docker_run`

>[!BEGINTABS]

>[!TAB  macOS ]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB  Vensters ]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB  Linux® ]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]

### Toegang tot logbestanden

Apache-webserver en AEM Dispatcher-logbestanden zijn rechtstreeks toegankelijk in de Docker-container:

+ [De toegang tot van logboeken in de container van de Dokker](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [De Docker-logbestanden worden naar het lokale bestandssysteem gekopieerd](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Wanneer moet u Dispatcher Tools bijwerken?{#dispatcher-tools-version}

De versies van Dispatcher Tools stijgen minder vaak dan de Experience Manager, en daarom vereisen de Hulpmiddelen van Dispatcher minder updates in de lokale ontwikkelomgeving.

De aanbevolen versie van Dispatcher Tools is de versie die wordt meegeleverd bij de AEM as a Cloud Service SDK die overeenkomt met de Experience Manager as a Cloud Service-versie. De versie van AEM as a Cloud Service kan via [&#x200B; Cloud Manager &#x200B;](https://my.cloudmanager.adobe.com/) worden gevonden.

+ __Cloud Manager > Milieu&#39;s__, per milieu dat door het __wordt gespecificeerd van de Versie van AEM__ etiket

![&#x200B; Versie van Experience Manager &#x200B;](./assets/dispatcher-tools/aem-version.png)

*Merk op dat de versie van de Hulpmiddelen van Dispatcher niet de versie van Experience Manager aanpast.*

## De basislijnset van Apache- en Dispatcher-configuraties bijwerken

De basisset van Apache- en Dispatcher-configuratie wordt regelmatig verbeterd en vrijgegeven met de AEM as a Cloud Service SDK-versie. Het is beste praktijken om de verhogingen van de basislijnconfiguratie in uw project van AEM op te nemen en [&#x200B; lokale bevestiging &#x200B;](#validate-configurations) en de pijpleidingsmislukkingen van Cloud Manager te vermijden. Werk deze bij met het script `update_maven.sh` in de map `.../dispatcher-sdk-x.x.x/bin` .

>[!VIDEO](https://video.tv.adobe.com/v/3416744?quality=12&learn=on)

*Deze video gebruikt macOS voor illustratieve doeleinden. De gelijkwaardige bevelen Windows/Linux kunnen worden gebruikt om gelijkaardige resultaten te bereiken.*


Laten we veronderstellen u een project van AEM in het verleden gebruikend [&#x200B; Archetype van het Project van AEM &#x200B;](https://github.com/adobe/aem-project-archetype) creeerde, waren de basislijn Apache en de configuraties van Dispatcher huidig. Met behulp van deze basislijnconfiguraties zijn uw projectspecifieke configuraties gemaakt door de bestanden als `*.vhost` , `*.conf` , `*.farm` en `*.any` uit de mappen `dispatcher/src/conf.d` en `dispatcher/src/conf.dispatcher.d` opnieuw te gebruiken en te kopiëren. Je lokale Dispatcher-validatie en Cloud Manager-pijpleidingen werkten prima.

Ondertussen werden de basislijnconfiguraties Apache en Dispatcher verbeterd om verschillende redenen, zoals nieuwe functies, beveiligingsoplossingen en optimalisatie. Ze worden vrijgegeven via een nieuwere versie van Dispatcher Tools als onderdeel van de AEM as a Cloud Service-release.

Wanneer u nu uw projectspecifieke Dispatcher-configuraties valideert op basis van de nieuwste versie van Dispatcher Tools, mislukt deze versie. Om dit te verhelpen, moeten de basislijnconfiguraties door onder stappen worden bijgewerkt te gebruiken:

+ Controleren of de validatie mislukt met de nieuwste versie van Dispatcher Tools

  ```shell
  $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Phase 3: Immutability check
  empty mode param, assuming mode = 'check'
  ...
  ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
  ```

+ Werk de onveranderlijke dossiers bij gebruikend het `update_maven.sh` manuscript

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

+ Verifieer de bijgewerkte onveranderlijke dossiers zoals `dispatcher_vhost.conf`, `default.vhost`, en `default.farm` en breng indien nodig relevante veranderingen in uw douanedossiers aan die uit deze dossiers worden afgeleid.

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

`host.docker.internal` is een hostnaam die aan Docker wordt verstrekt die aan de gastheer oplost. Per docs.docker.com ([&#x200B; macOS &#x200B;](https://docs.docker.com/desktop/networking/), [&#x200B; Vensters &#x200B;](https://docs.docker.com/desktop/networking/)):

> Vanaf Docker 18.03 moet de aanbeveling met de speciale DNS naam host.docker.internal verbinden, die aan het interne IP adres oplost dat door de gastheer wordt gebruikt

Wanneer `bin/docker_run src host.docker.internal:4503 8080` in het bericht __het Wachten tot host.docker.internal beschikbaar is__ resulteert, dan:

1. Zorg ervoor dat de geïnstalleerde versie van Docker 18.03 of hoger is
1. U hebt mogelijk een lokale computer ingesteld die de registratie/resolutie van de naam `host.docker.internal` verhindert. Gebruik in plaats daarvan uw lokale IP.

>[!BEGINTABS]

>[!TAB  macOS ]

+ Van Eind, voer `ifconfig` uit en registreer het Gastheer __inwoner__ IP adres, gewoonlijk het __en0__ apparaat.
+ Voer vervolgens `docker_run` uit met behulp van het IP-adres van de host: `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!TAB  Vensters ]

+ Van de Herinnering van het Bevel, voer `ipconfig` uit, en registreer het 1&rbrace; IPv4 Adres van de gastheer __van de gastheermachine.__
+ Voer vervolgens `docker_run` uit met behulp van dit IP-adres: `$ bin\docker_run src <HOST IP>:4503 8080`

>[!TAB  Linux® ]

+ Van Eind, voer `ifconfig` uit en registreer het Gastheer __inwoner__ IP adres, gewoonlijk het __en0__ apparaat.
+ Voer vervolgens `docker_run` uit met behulp van het IP-adres van de host: `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

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

+ [&#x200B; Download AEM SDK &#x200B;](https://experience.adobe.com/#/downloads)
+ [&#x200B; Adobe Cloud Manager &#x200B;](https://my.cloudmanager.adobe.com/)
+ [&#x200B; Docker van de Download &#x200B;](https://www.docker.com/)
+ [&#x200B; Download de Website van de Verwijzing van AEM (WKND) &#x200B;](https://github.com/adobe/aem-guides-wknd/releases)
+ [&#x200B; de Documentatie van Experience Manager Dispatcher &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=nl-NL)
