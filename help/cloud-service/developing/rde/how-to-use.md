---
title: Hoe wordt de Rapid Development Environment gebruikt
description: Leer hoe u de functie Rapid Development Environment kunt gebruiken om code en inhoud van uw lokale computer te implementeren.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: 1d1bcb18-06cd-46fc-be2a-7a3627c1e2b2
duration: 792
source-git-commit: 2f7e10680c7211da836e33fdd241cd7f5d633d5f
workflow-type: tm+mt
source-wordcount: '788'
ht-degree: 0%

---

# Hoe wordt de Rapid Development Environment gebruikt

Leer **hoe te om** het Snelle Milieu van de Ontwikkeling (RDE) in AEM as a Cloud Service te gebruiken. Stel code en inhoud voor snellere ontwikkelingscycli van uw bijna-definitieve code aan RDE, van uw favoriete Geïntegreerde Milieu van de Ontwikkeling (winde) op.

Gebruikend [ AEM WKND Project van Plaatsen ](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) leert u hoe te om diverse artefacten van AEM aan RDE op te stellen door AEM-RDE `install` bevel van uw favoriete winde in werking te stellen.

- Implementatie van AEM-code en -inhoudspakket (alle, ui.apps)
- Implementatie van OSGi-bundel- en configuratiebestanden
- Implementatie van Apache- en Dispatcher-configuraties als ZIP-bestand
- Afzonderlijke bestanden, zoals HTML- en `.content.xml` (dialoogvenster XML)-implementatie
- Andere RDE-opdrachten controleren, zoals `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491?quality=12&learn=on)

## Vereiste

Kloon het [ WKND 1&rbrace; project van Plaatsen &lbrace;en open het in uw favoriete winde om de artefacten van AEM op RDE op te stellen.](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

Vervolgens bouwt u de toepassing en implementeert u deze in de lokale AEM-SDK door de volgende gemodelleerde opdracht uit te voeren.

```
$ cd aem-guides-wknd/
$ mvn clean package
```

## AEM-artefacten implementeren met de AEM-RDE-plug-in

Eerst, zorg ervoor u de [ recentste geïnstalleerde `aio` CLI module ](https://experienceleague.adobe.com/nl/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools#aio-cli) hebt.

Gebruik vervolgens de opdracht `aio aem:rde:install` om verschillende AEM-artefacten te implementeren.

### Pakketten `all` en `dispatcher` implementeren

Een gangbaar uitgangspunt is om eerst de pakketten `all` en `dispatcher` in te voeren door de volgende opdrachten uit te voeren.

```shell
# Install the 'all' content package (zip file)
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' deployment artifact (zip file)
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

Verifieer de WKND-site op zowel de auteur- als de publicatieservices bij een geslaagde implementatie. U moet de inhoud op de WKND-sitepagina&#39;s kunnen toevoegen, bewerken en publiceren.

### Een component verbeteren en implementeren

Verbeter `Hello World Component` en stel het aan RDE op.

1. Dialoogvenster-XML (`.content.xml`) openen vanuit `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` -map
1. Het tekstveld `Description` toevoegen na het bestaande dialoogvenster `Text`

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. Open het bestand `helloworld.html` vanuit de map `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld` .
1. Render de eigenschap `Description` na het bestaande `<div>` -element van de eigenschap `Text` .

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. Controleer de wijzigingen op de lokale AEM SDK door de Maven-build uit te voeren of afzonderlijke bestanden te synchroniseren.

1. Implementeer de wijzigingen in de RDE via het `ui.apps` -pakket of door de afzonderlijke Dialog- en HTML-bestanden te implementeren:

   ```shell
   # Using 'ui.apps' package
   
   $ cd ui.apps
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.ui.apps-2.1.3-SNAPSHOT.zip
   
   # Or by deploying the individual HTL and Dialog XML
   
   # HTL file
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html -t content-file -p /apps/wknd/components/helloworld/helloworld.html
   
   # Dialog XML
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml -t content-xml -p /apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Controleer de wijzigingen op de RDE door de `Hello World Component` toe te voegen of te bewerken op een WKND-sitepagina.

### Controleer de opdrachtopties voor `install`

In het bovenstaande voorbeeld met afzonderlijke opdrachten voor bestandsimplementatie worden de markeringen `-t` en `-p` gebruikt om respectievelijk het type en de bestemming van het JCR-pad aan te geven. Bekijk de beschikbare `install` bevelopties door het volgende bevel in werking te stellen.

```shell
$ aio aem:rde:install --help
```

De vlaggen zijn duidelijk, is de `-s` vlag nuttig om de plaatsing enkel aan de auteur of de publicatieservices te richten. Gebruik de `-t` vlag wanneer het opstellen van **inhoud-dossier of inhoud-xml** dossiers samen met de `-p` vlag om de weg van bestemmingsJCR in het milieu van AEM te specificeren RDE.

### OSGi-bundel implementeren

Om te leren hoe te om de bundel OSGi op te stellen, verbeteren wij de `HelloWorldModel` klasse Java™ en stellen het aan RDE op.

1. Open het bestand `HelloWorldModel.java` vanuit de map `core/src/main/java/com/adobe/aem/guides/wknd/core/models` .
1. Werk de methode `init()` als volgt bij:

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. Verifieer de wijzigingen op lokaal AEM-SDK door de `core` -bundel te implementeren via de maven-opdracht
1. Stel de veranderingen in RDE op door het volgende bevel in werking te stellen

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. Controleer de wijzigingen op de RDE door de `Hello World Component` toe te voegen of te bewerken op een WKND-sitepagina.

### OSGi-configuratie implementeren

U kunt de individuele config- dossiers of het volledige config- pakket, bijvoorbeeld opstellen:

```shell
# Deploy individual config file
$ aio aem:rde:install ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config/org.apache.sling.commons.log.LogManager.factory.config~wknd.cfg.json

# Or deploy the complete config package
$ cd ui.config
$ mvn clean package
$ aio aem:rde:install target/aem-guides-wknd.ui.config-2.1.3-SNAPSHOT.zip
```

>[!TIP]
>
>Gebruik de markering `-s` om een OSGi-configuratie alleen op een auteur- of publicatieinstantie te installeren.


### Apache- of Dispatcher-configuratie implementeren

Apache of Dispatcher config- dossiers **kunnen niet individueel** worden opgesteld, maar de volledige de omslagstructuur van Dispatcher moet in de vorm van een dossier van het PIT worden opgesteld.

1. Breng een gewenste wijziging aan in het configuratiebestand van de module `dispatcher` en werk voor demodoeleinden de `dispatcher/src/conf.d/available_vhosts/wknd.vhost` bij om de bestanden van `html` slechts gedurende 60 seconden in cache te plaatsen.

   ```
   ...
   <LocationMatch "^/content/.*\.html$">
       Header unset Cache-Control
       Header always set Cache-Control "max-age=60,stale-while-revalidate=60" "expr=%{REQUEST_STATUS} < 400"
       Header always set Surrogate-Control "stale-while-revalidate=43200,stale-if-error=43200" "expr=%{REQUEST_STATUS} < 400"
       Header set Age 0
   </LocationMatch>
   ...
   ```

1. Verifieer de veranderingen plaatselijk, zie [ Looppas Dispatcher ](https://experienceleague.adobe.com/nl/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools) voor meer details.
1. Stel de veranderingen in RDE op door het volgende bevel in werking te stellen:

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. Verifieer veranderingen op RDE.

### Configuratiebestanden (YAML) implementeren

De CDN, onderhoudstaken, het logboek door:sturen en de configuratiedossiers van de AEM API authentificatie kunnen aan RDE worden opgesteld gebruikend het `install` bevel. Deze configuraties worden beheerd als dossiers YAML in de `config` omslag van het project van AEM, zie [ Ondersteunde Configuraties ](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/operations/config-pipeline#configurations) voor meer details.

Om te leren hoe te om de configuratiedossiers op te stellen, verbeteren wij het `cdn` configuratiedossier en stellen het aan RDE op.

1. Open het bestand `cdn.yaml` vanuit de map `config` .
1. Werk de gewenste configuratie bij, bijvoorbeeld, werk de tariefgrens aan 200 verzoeken per seconde bij

   ```yaml
   kind: "CDN"
   version: "1"
   metadata:
     envTypes: ["dev", "stage", "prod"]
   data:
     trafficFilters:
       rules:
       #  Block client for 5m when it exceeds an average of 100 req/sec to origin on a time window of 10sec
       - name: limit-origin-requests-client-ip
         when:
           reqProperty: tier
           equals: 'publish'
         rateLimit:
           limit: 200 # updated rate limit
           window: 10
           count: fetches
           penalty: 300
           groupBy:
             - reqProperty: clientIp
         action: log
   ...
   ```

1. Stel de veranderingen in RDE op door het volgende bevel in werking te stellen

   ```shell
   $ aio aem:rde:install -t env-config ./config
   ```

1. Wijzigingen op de RDE controleren


## Aanvullende opdrachten voor AEM RDE-insteekmodule

Laten we de extra opdrachten voor AEM RDE-insteekmodules bekijken die u kunt beheren en communiceren met de RDE van uw lokale computer.

```shell
$ aio aem:rde --help
Interact with RapidDev Environments.

USAGE
$ aio aem rde COMMAND

COMMANDS
aem rde delete   Delete bundles and configs from the current rde.
aem rde history  Get a list of the updates done to the current rde.
aem rde install  Install/update bundles, configs, and content-packages.
aem rde reset    Reset the RDE
aem rde restart  Restart the author and publish of an RDE
aem rde status   Get a list of the bundles and configs deployed to the current rde.
```

Met behulp van de bovenstaande opdrachten kan uw RDE worden beheerd vanaf uw favoriete IDE voor een snellere ontwikkeling/implementatie levenscyclus.

## Volgende stap

Leer over de [ ontwikkeling/de cyclus van het plaatsingsleven gebruikend RDE ](./development-life-cycle.md) om eigenschappen met snelheid te leveren.


## Aanvullende bronnen

[ RDE bevelen documentatie ](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments)

[ Adobe I/O Runtime CLI Insteekmodule voor interactie met de Milieu&#39;s van de Snelle Ontwikkeling van AEM ](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[ de opstelling van het Project van AEM ](https://experienceleague.adobe.com/nl/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup)
