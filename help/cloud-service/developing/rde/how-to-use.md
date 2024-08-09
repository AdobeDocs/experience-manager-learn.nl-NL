---
title: Hoe wordt de Rapid Development Environment gebruikt
description: Leer hoe u de functie Rapid Development Environment kunt gebruiken om code en inhoud van uw lokale computer te implementeren.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: 1d1bcb18-06cd-46fc-be2a-7a3627c1e2b2
duration: 792
source-git-commit: 60139d8531d65225fa1aa957f6897a6688033040
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 0%

---

# Hoe wordt de Rapid Development Environment gebruikt

Leer **hoe te om** Snelle Ontwikkelomgeving (RDE) in AEM as a Cloud Service te gebruiken. Stel code en inhoud voor snellere ontwikkelingscycli van uw bijna-definitieve code aan RDE, van uw favoriete Geïntegreerde Milieu van de Ontwikkeling (winde) op.

Gebruikend [ AEM het Project van Plaatsen WKND ](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) leert u hoe te om diverse AEM artefacten aan RDE op te stellen door het bevel van AEM-RDE `install` van uw favoriete winde in werking te stellen.

- Implementatie van code en inhoud AEM (alle toepassingen, ui.apps)
- Implementatie van OSGi-bundel- en configuratiebestanden
- Apache en Dispatcher configureren de implementatie als een zip-bestand
- Afzonderlijke bestanden, zoals HTML- en `.content.xml` (dialoogvenster XML)-implementatie
- Andere RDE-opdrachten controleren, zoals `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491?quality=12&learn=on)

## Vereiste

Kloon het [ project van de Plaatsen van WKND ](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) en open het in uw favoriete winde om de AEM artefacten op RDE op te stellen.

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

Dan, bouwt en stelt het aan lokale AEM-SDK door het volgende gemaven bevel in werking te stellen op.

```
$ cd aem-guides-wknd/
$ mvn clean package
```

## Implementeer AEM artefacten met de AEM-RDE plug-in

Eerst, zorg ervoor u de [ recentste geïnstalleerde `aio` CLI module ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools#aio-cli) hebt.

Gebruik vervolgens de opdracht `aio aem:rde:install` om verschillende AEM artefacten te implementeren. Nu moet u

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

1. Controleer de wijzigingen in de lokale AEM SDK door de Maven-build uit te voeren of afzonderlijke bestanden te synchroniseren.

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

De vlaggen zijn duidelijk, is de `-s` vlag nuttig om de plaatsing enkel aan de auteur of de publicatieservices te richten. Gebruik de `-t` vlag wanneer het opstellen van **inhoud-dossier of inhoud-xml** dossiers samen met de `-p` vlag om de weg van bestemmingsJCR in het milieu van AEMRDE te specificeren.

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

1. Verifieer de veranderingen op lokale AEM-SDK door de {`core` bundel via maven bevel op te stellen
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

1. Verifieer de veranderingen plaatselijk, zie [ Looppas Dispatcher ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally) voor meer details.
1. Stel de veranderingen in RDE op door het volgende bevel in werking te stellen:

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. Wijzigingen op de RDE controleren

## Aanvullende opdrachten AEM RDE-insteekmodule

Laten we de extra opdrachten voor AEM RDE-plug-in die u wilt beheren, bekijken en communiceren met de RDE van uw lokale computer.

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

Met behulp van de bovenstaande opdrachten kan uw RDE worden beheerd vanaf uw favoriete IDE voor een snellere ontwikkelings-/implementatielevenscyclus.

## Volgende stap

Leer over de [ ontwikkeling/de cyclus van het plaatsingsleven gebruikend RDE ](./development-life-cycle.md) om eigenschappen met snelheid te leveren.


## Aanvullende bronnen

[ RDE bevelen documentatie ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands)

[ Adobe I/O Runtime CLI Insteekmodule voor interactie met AEM Snelle Milieu&#39;s van de Ontwikkeling ](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[ AEM de opstelling van het Project ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html)
