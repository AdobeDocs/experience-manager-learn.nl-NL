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
duration: 815
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '671'
ht-degree: 0%

---

# Hoe wordt de Rapid Development Environment gebruikt

Meer informatie **gebruiken** Rapid Development Environment (RDE) in AEM as a Cloud Service. Stel code en inhoud voor snellere ontwikkelingscycli van uw bijna-definitieve code aan RDE, van uw favoriete Geïntegreerde Milieu van de Ontwikkeling (winde) op.

Gebruiken [AEM WKND-siteproject](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) u leert hoe te om diverse AEM artefacten aan RDE op te stellen door AEM-RDE in werking te stellen `install` van uw favoriete winde.

- Implementatie van code en inhoud AEM (alle toepassingen, ui.apps)
- Implementatie van OSGi-bundel- en configuratiebestanden
- Apache en Dispatcher configureert de implementatie als een ZIP-bestand
- Afzonderlijke bestanden, zoals HTML `.content.xml` (dialoogvenster XML)-implementatie
- Andere RDE-opdrachten, zoals `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491?quality=12&learn=on)

## Vereiste

Klonen met [WKND-sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) en open het in uw favoriete winde om de AEM artefacten op RDE op te stellen.

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

Dan, bouwt en stelt het aan lokale AEM-SDK door het volgende gemaven bevel in werking te stellen op.

```
$ cd aem-guides-wknd/
$ mvn clean package
```

## Implementeer AEM artefacten met de AEM-RDE plug-in

Met de `aem:rde:install` bevel, stellen verschillende AEM artefacten op.

### Implementeren `all` en `dispatcher` pakketten

Een gemeenschappelijk uitgangspunt moet eerst opstellen `all` en `dispatcher` pakketten door de volgende bevelen in werking te stellen.

```shell
# Install the 'all' package
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' zip
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

Verifieer de WKND-site op zowel de auteur- als de publicatieservices bij een geslaagde implementatie. U moet de inhoud op de WKND-sitepagina&#39;s kunnen toevoegen, bewerken en publiceren.

### Een component verbeteren en implementeren

Laten we de `Hello World Component` en zet het aan RDE op.

1. Het dialoogvenster XML openen (`.content.xml`) bestand van `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` map
1. Voeg de `Description` tekstveld na het bestaande `Text` dialoogveld

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. Open de `helloworld.html` bestand van `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld` map
1. Render de `Description` eigenschap na de bestaande `<div>` element van het `Text` eigenschap.

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. Controleer de wijzigingen in de lokale AEM-SDK door de gemaakte build uit te voeren of afzonderlijke bestanden te synchroniseren.

1. De wijzigingen in de RDE implementeren via `ui.apps` of door de afzonderlijke dialoogvensters en HTML-bestanden te implementeren.

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

1. Verifieer veranderingen op RDE door toe te voegen of uit te geven `Hello World Component` op een WKND-sitepagina.

### Controleer de `install` opdrachtopties

In het bovenstaande voorbeeld met de afzonderlijke opdracht voor het implementeren van bestanden, `-t` en `-p` markeringen worden gebruikt om respectievelijk het type en de bestemming van het JCR-pad aan te geven. Laten we de beschikbare `install` de opties van het bevel door het volgende bevel in werking te stellen.

```shell
$ aio aem:rde:install --help
```

De vlaggen zijn vanzelfsprekend, de `-s` De markering is nuttig om de plaatsing enkel aan de auteur te richten of de publicatieservices te publiceren. Gebruik de `-t` vlag wanneer het opstellen van **content-file of content-xml** samen met de `-p` markering om de weg van bestemmingsJCR in het milieu van AEM RDE te specificeren.

### OSGi-bundel implementeren

Om te leren hoe te om de bundel op te stellen OSGi, verbeteren wij `HelloWorldModel` Java™-klasse en implementeren in de RDE.

1. Open de `HelloWorldModel.java` bestand van `core/src/main/java/com/adobe/aem/guides/wknd/core/models` map
1. Werk de `init()` methode als hieronder:

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. Verifieer de veranderingen op lokaal AEM-SDK door op te stellen `core` bundelen via maven, opdracht
1. Stel de veranderingen in RDE op door het volgende bevel in werking te stellen

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. Verifieer veranderingen op RDE door toe te voegen of uit te geven `Hello World Component` op een WKND-sitepagina.

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
>Om een configuratie OSGi slechts op een auteur te installeren of te publiceren instantie, gebruik `-s` markering.


### Apache- of Dispatcher-configuratie implementeren

De Apache- of Dispatcher-configuratiebestanden **kan niet individueel worden ingezet**, maar de volledige mapstructuur van Dispatcher moet worden geïmplementeerd in de vorm van een ZIP-bestand.

1. Breng een gewenste wijziging aan in het configuratiebestand van het `dispatcher` voor demo-doeleinden de `dispatcher/src/conf.d/available_vhosts/wknd.vhost` om de `html` alleen gedurende 60 seconden.

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

1. Controleer de wijzigingen lokaal. Zie [Verzending lokaal uitvoeren](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally) voor meer informatie .
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

Meer informatie over de [ontwikkeling/implementatie van levenscyclus met RDE](./development-life-cycle.md) om snel functies te leveren.


## Aanvullende bronnen

[Documentatie RDE-opdrachten](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands)

[Adobe I/O Runtime CLI-insteekmodule voor interactie met AEM Rapid Development Environment](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Projectinstelling AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html)
