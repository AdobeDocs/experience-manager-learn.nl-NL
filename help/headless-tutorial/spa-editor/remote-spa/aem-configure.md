---
title: AEM configureren voor SPA Editor en externe SPA
description: Een AEM project wordt vereist aan opstelling-ondersteunende configuratie en inhoudsvereisten om AEM SPA Redacteur toe te staan om een Verre SPA te schrijven.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7631
thumbnail: kt-7631.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 0bdb93c9-5070-483c-a34c-f2b348bfe5ae
duration: 432
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1230'
ht-degree: 0%

---

# AEM configureren voor SPA Editor

Terwijl de SPA codebase buiten AEM wordt beheerd, wordt een AEM project vereist aan opstelling ondersteunend configuratie en inhoudsvereisten. Dit hoofdstuk doorloopt de verwezenlijking van een AEM project dat noodzakelijke configuraties bevat:

+ Proxy&#39;s van WCM Core-componenten AEM
+ Proxy voor externe SPA AEM
+ Sjablonen voor externe SPA AEM
+ Basislijnpagina&#39;s voor externe SPA AEM
+ Subproject voor het definiëren van SPA naar AEM URL-toewijzingen
+ OSGi-configuratiemappen

## Download het basisproject van GitHub

Download de `aem-guides-wknd-graphql` project van Github.com. Dit zal sommige basislijndossiers bevatten die in dit project worden gebruikt.

```
$ mkdir -p ~/Code
$ git clone https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd remote-spa-tutorial
```

## Een AEM project maken

Creeer een AEM project waarin configuraties en basislijninhoud worden beheerd. Dit project wordt gegenereerd binnen de gekloonde `aem-guides-wknd-graphql` project `remote-spa-tutorial` map.

_Gebruik altijd de nieuwste versie van het dialoogvenster [AEM Archetype](https://github.com/adobe/aem-project-archetype)._

```
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial
$ mvn -B archetype:generate \
 -D archetypeGroupId=com.adobe.aem \
 -D archetypeArtifactId=aem-project-archetype \
 -D archetypeVersion=39 \
 -D aemVersion=cloud \
 -D appTitle="WKND App" \
 -D appId="wknd-app" \
 -D groupId="com.adobe.aem.guides.wkndapp" \
 -D frontendModule="react"
$ mv ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/wknd-app ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/com.adobe.aem.guides.wknd-app
```

_Het laatste bevel wijzigt eenvoudig de naam van de AEM projectomslag zodat is het duidelijk het AEM project, en niet om met Verre SPA worden verward__

while `frontendModule="react"` wordt opgegeven, wordt de `ui.frontend` Het project wordt niet gebruikt voor het Verre SPA gebruiksgeval. De SPA wordt extern ontwikkeld en beheerd om te AEM en gebruikt alleen AEM als inhoud-API. De `frontendModule="react"` markering is vereist voor het project met  `spa-project` AEM Java™-afhankelijkheden en stel de externe SPA paginasjablonen in.

Het AEM Archetype van het Project produceert de volgende elementen die gebruikte om AEM voor integratie met de SPA te vormen.

+ __Proxy&#39;s van WCM Core-componenten AEM__ om `ui.apps/src/.../apps/wknd-app/components`
+ __Proxy voor externe SPA AEM__ om `ui.apps/src/.../apps/wknd-app/components/remotepage`
+ __Paginasjablonen AEM__ om `ui.content/src/.../conf/wknd-app/settings/wcm/templates`
+ __Subproject voor het definiëren van inhoudstoewijzingen__ om `ui.content/src/...`
+ __Basislijnpagina&#39;s voor externe SPA AEM__ om `ui.content/src/.../content/wknd-app`
+ __OSGi-configuratiemappen__ om `ui.config/src/.../apps/wknd-app/osgiconfig`

Met het basis AEM project wordt geproduceerd, verzekeren een paar aanpassingen SPA de verenigbaarheid van de Redacteur met Verre SPA.

## Ui.frontend-project verwijderen

Aangezien de SPA een Verre SPA is, veronderstel het buiten het AEM project wordt ontwikkeld en geleid. Als u conflicten wilt voorkomen, verwijdert u de `ui.frontend` project van het opstellen. Als de `ui.frontend` project niet wordt verwijderd, twee SPA, de SPA die standaard in de `ui.frontend` project en de Verre SPA, wordt geladen tezelfdertijd in de AEM SPA Redacteur.

1. Het AEM-project openen (`~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/com.adobe.aem.guides.wknd-app`) in uw IDE
1. De hoofdmap openen `pom.xml`
1. Opmerkingen plaatsen bij `<module>ui.frontend</module` uit de `<modules>` list

   ```
   <modules>
       <module>all</module>
       <module>core</module>
   
       <!-- <module>ui.frontend</module> -->
   
       <module>ui.apps</module>
       <module>ui.apps.structure</module>
       <module>ui.config</module>
       <module>ui.content</module>
       <module>it.tests</module>
       <module>dispatcher</module>
       <module>ui.tests</module>
       <module>analyse</module>
   </modules>
   ```

   De `pom.xml` bestand moet er als volgt uitzien:

   ![Ui.frontend-module verwijderen uit reactorpom](./assets/aem-project/uifrontend-reactor-pom.png)

1. Open de `ui.apps/pom.xml`
1. Maak een opmerking in het dialoogvenster `<dependency>` op `<artifactId>wknd-app.ui.frontend</artifactId>`

   ```
   <dependencies>
   
       <!-- Remote SPA project will provide all frontend resources
       <dependency>
           <groupId>com.adobe.aem.guides.wkndapp</groupId>
           <artifactId>wknd-app.ui.frontend</artifactId>
           <version>${project.version}</version>
           <type>zip</type>
       </dependency>
       --> 
   </dependencies>
   ```

   De `ui.apps/pom.xml` bestand moet er als volgt uitzien:

   ![Ui.frontend-afhankelijkheid verwijderen uit ui.apps](./assets/aem-project/uifrontend-uiapps-pom.png)

Als het AEM project vóór deze veranderingen werd gebouwd, schrap manueel `ui.frontend` gegenereerde clientbibliotheek vanuit de `ui.apps` project bij `ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react`.

## Inhoud toewijzen AEM

Voor AEM om de Verre SPA in de SPARedacteur te laden, moeten de afbeeldingen tussen de SPA routes en de AEM Pagina&#39;s worden gebruikt om te openen en auteursinhoud worden gevestigd.

Het belang van deze configuratie wordt later onderzocht.

U kunt de toewijzing uitvoeren met [Sling Mapping](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#root-level-mappings-1) gedefinieerd in `/etc/map`.

1. In winde, open `ui.content` subproject
1. Navigeren naar  `src/main/content/jcr_root`
1. Een map maken `etc`
1. In `etc`, maakt u een map `map`
1. In `map`, maakt u een map `http`
1. In `http`, maakt u een bestand `.content.xml` met de inhoud:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. In `http` , maakt u een map `localhost_any`
1. In `localhost_any`, maakt u een bestand `.content.xml` met de inhoud:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. In `localhost_any` , maakt u een map `wknd-app-routes-adventure`
1. In `wknd-app-routes-adventure`, maakt u een bestand `.content.xml` met de inhoud:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   
   <!--
   The 'wknd-app-routes-adventure' mapping, maps requests to the SPA's adventure route 
   to it's corresponding page in AEM at /content/wknd-app/us/en/home/adventure/xxx.
   
   Note the adventure AEM pages are created directly in AEM.
   -->
   
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="adventure:.*/([^/]+)/?$"
       sling:internalRedirect="/content/wknd-app/us/en/home/adventure/$1"/>
   ```

1. Koppelingsknooppunten toevoegen aan `ui.content/src/main/content/META-INF/vault/filter.xml` aan hen die in het AEM zijn opgenomen.

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd-app" mode="merge"/>
       <filter root="/content/wknd-app" mode="merge"/>
       <filter root="/content/dam/wknd-app/asset.jpg" mode="merge"/>
       <filter root="/content/experience-fragments/wknd-app" mode="merge"/>
   
       <!-- Add the Sling Mapping rules for the WKND App -->
       <filter root="/etc/map" mode="merge"/>
   </workspaceFilter>
   ```

De mapstructuur en `.context.xml` bestanden moeten er als volgt uitzien:

![Sling Mapping](./assets/aem-project/sling-mapping.png)

De `filter.xml` bestand moet er als volgt uitzien:

![Sling Mapping](./assets/aem-project/sling-mapping-filter.png)

Nu, wanneer het AEM project wordt opgesteld, zijn deze configuraties automatisch inbegrepen.

De effecten voor AEM toewijzing van objecten die worden uitgevoerd `http` en `localhost`, dus alleen lokale ontwikkeling. Wanneer het opstellen aan AEM as a Cloud Service, gelijkaardige het Schipen Toewijzingen moet worden toegevoegd dat doel `https` en de toepasselijke AEM as a Cloud Service domein(en). Zie de klasse [Documentatie over het toewijzen van objecten](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html).

## Beveiligingsbeleid voor het delen van bronnen tussen verschillende bronnen

Configureer vervolgens AEM om de inhoud te beschermen, zodat alleen deze SPA toegang heeft tot de AEM inhoud. Configureren [Delen van bronnen tussen verschillende bronnen in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html).

1. In uw winde, open `ui.config` Maven-subproject
1. Navigeren `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config`
1. Een bestand met de naam `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`
1. Voeg het volgende toe aan het bestand:

   ```
   {
       "supportscredentials":true,
       "exposedheaders":[
           ""
       ],
       "supportedmethods":[
           "GET",
           "HEAD",
           "POST",
           "OPTIONS"
       ],
       "alloworigin":[
           "https://external-hosted-app", "localhost:3000"
       ],
       "maxage:Integer":1800,
       "alloworiginregexp":[
           ".*"
       ],
       "allowedpaths":[
           ".*"
       ],
       "supportedheaders":[
           "Origin",
           "Accept",
           "X-Requested-With",
           "Content-Type",
           "Access-Control-Request-Method",
           "Access-Control-Request-Headers",
           "authorization"
       ]
   }
   ```

De `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json` bestand moet er als volgt uitzien:

![SPA Editor CORS-configuratie](./assets/aem-project/cors-configuration.png)

De belangrijkste configuratieelementen zijn:

+ `alloworigin` Hiermee geeft u op welke hosts inhoud mogen ophalen van AEM.
   + `localhost:3000` wordt toegevoegd ter ondersteuning van de SPA die lokaal wordt uitgevoerd
   + `https://external-hosted-app` fungeert als plaatsaanduiding die moet worden vervangen door het domein waarop de externe SPA wordt gehost.
+ `allowedpaths` specificeren welke wegen in AEM door deze configuratie CORS worden behandeld. Standaard is toegang tot alle inhoud in AEM toegestaan, maar dit kan alleen worden toegepast op de specifieke paden waartoe de SPA toegang heeft, bijvoorbeeld: `/content/wknd-app`.

## AEM pagina instellen als sjabloon voor externe SPA

Het AEM Project Archetype produceert een project dat voor AEM integratie met een Verre SPA wordt voorbereid, maar vereist een kleine, maar belangrijke aanpassing aan auto-geproduceerde AEM paginastructuur. Het type van de automatisch gegenereerde AEM moet worden gewijzigd in __Externe SPA__, in plaats van __SPA pagina__.

1. In uw winde, open `ui.content` subproject
1. Openen naar `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`
1. Deze bijwerken `.content.xml` bestand met:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="cq:Page">
       <jcr:content
           cq:template="/conf/wknd-app/settings/wcm/templates/spa-remote-page"
           jcr:primaryType="cq:PageContent"
           jcr:title="WKND App Home Page"
           sling:resourceType="wknd-app/components/remotepage">
           <root
               jcr:primaryType="nt:unstructured"
               sling:resourceType="wcm/foundation/components/responsivegrid">
               <responsivegrid
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wcm/foundation/components/responsivegrid">
                   <text
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="wknd-app/components/text"
                       text="&lt;p>Hello World!&lt;/p>"
                       textIsRich="true">
                       <cq:responsive jcr:primaryType="nt:unstructured"/>
                   </text>
               </responsivegrid>
           </root>
       </jcr:content>
   </jcr:root>
   ```

De belangrijkste wijzigingen zijn updates voor de `jcr:content` knooppunten:

+ `cq:template` tot `/conf/wknd-app/settings/wcm/templates/spa-remote-page`
+ `sling:resourceType` tot `wknd-app/components/remotepage`

De `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml` bestand moet er als volgt uitzien:

![Updates van de startpagina.content.xml](./assets/aem-project/home-content-xml.png)

Deze wijzigingen maken het mogelijk dat deze pagina, die de SPA hoofdmap in AEM vormt, de externe SPA in SPA Editor laadt.

>[!NOTE]
>
>Als dit project eerder aan AEM werd opgesteld, zorg ervoor om de AEM pagina te schrappen zoals __Sites > WKND App > us > en > WKND App Home Page__ als de `ui.content`  project is ingesteld op __samenvoegen__ knooppunten, in plaats van __update__.

Deze pagina kan ook worden verwijderd en opnieuw worden gemaakt als een externe SPA pagina op AEM zichzelf, maar deze pagina wordt automatisch gemaakt in het dialoogvenster `ui.content` het beste om het in de codebasis bij te werken.

## Implementeer het AEM Project naar AEM SDK

1. Zorg ervoor dat AEM Auteur-service wordt uitgevoerd op poort 4502
1. Navigeer vanaf de opdrachtregel naar de hoofdmap van het AEM Maven-project
1. Gebruik Maven om het project op te stellen aan uw lokale AEM SDK Auteur-service

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn clean install -PautoInstallSinglePackage](./assets/aem-project/mvn-install.png)

## De AEM van de hoofdmap configureren

Met AEM opgesteld Project, is er één laatste stap om SPA Redacteur voor te bereiden om onze Verre SPA te laden. Markeer in AEM de AEM pagina die overeenkomt met het SPA.`/content/wknd-app/us/en/home`, gegenereerd door het AEM Project Archetype.

1. Aanmelden bij AEM auteur
1. Navigeren naar __Sites > WKND App > us > en__
1. Selecteer de __WKND App Home Page__ en tikken __Eigenschappen__

   ![WKND App Home Page - Eigenschappen](./assets/aem-content/edit-home-properties.png)

1. Ga naar de __SPA__ tab
1. Vul de __Configuratie van externe SPA__
   + __URL SPA__: `http://localhost:3000`
      + De URL naar de hoofdmap van de externe SPA

   ![WKND App Home Page - Configuratie van externe SPA](./assets/aem-content/remote-spa-configuration.png)

1. Tikken __Opslaan en sluiten__

Onthoud dat het paginatype van deze pagina is gewijzigd in dat van een __Externe SPA__, wat ons in staat stelt de __SPA__ in zijn __Pagina-eigenschappen__.

Deze configuratie moet alleen worden ingesteld op de AEM pagina die overeenkomt met de hoofdmap van de SPA. Alle AEM pagina&#39;s onder deze pagina nemen de waarde over.

## Gefeliciteerd

U hebt nu AEM configuraties voorbereid en deze geïmplementeerd op uw lokale AEM. Nu weet u hoe:

+ Verwijder het AEM Project Archetype-Gegenereerde SPA door de gebiedsdelen in te becommentariëren `ui.frontend`
+ Voeg het Verschuiven Toewijzingen toe aan AEM die de SPA routes aan middelen in AEM in kaart brengen
+ Stel AEM het beveiligingsbeleid voor het delen van bronnen van verschillende oorsprong in waarmee de externe SPA inhoud van AEM kunnen verbruiken
+ Implementeer het AEM project op uw lokale AEM SDK-auteurservice
+ Een AEM Pagina markeren als de externe SPA met de eigenschap URL van SPA host

## Volgende stappen

Met AEM geconfigureerd kunnen we ons richten op [bootstrapping de Verre SPA](./spa-bootstrap.md) met ondersteuning voor bewerkbare gebieden via AEM SPA Editor!
