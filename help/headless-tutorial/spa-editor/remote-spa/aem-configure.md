---
title: AEM configureren voor SPA Editor en externe SPA
description: Een AEM project wordt vereist aan opstelling ondersteunende configuratie en inhoudsvereisten om AEM SPA Redacteur toe te staan om een Verre SPA te schrijven.
topic: Zwaardeloze, SPA, ontwikkeling
feature: SPA Editor, kerncomponenten, API's, ontwikkelen
role: Developer, Architect
level: Beginner
kt: 7631
thumbnail: kt-7631.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '1220'
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

## Een AEM project maken

Creeer een AEM project waarin configuraties en basislijninhoud worden beheerd.

_Altijd de nieuwste versie van het  [AEM Archetype](https://github.com/adobe/aem-project-archetype) gebruiken._


```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ mvn -B archetype:generate \
 -D archetypeGroupId=com.adobe.aem \
 -D archetypeArtifactId=aem-project-archetype \
 -D archetypeVersion=27 \
 -D aemVersion=cloud \
 -D appTitle="WKND App" \
 -D appId="wknd-app" \
 -D groupId="com.adobe.aem.guides.wkndapp" \
 -D frontendModule="react"
$ mv ~/Code/wknd-app/wknd-app ~/Code/wknd-app/com.adobe.aem.guides.wknd-app
```

_Het laatste bevel wijzigt eenvoudig de naam van de AEM projectomslag zodat is het duidelijk het AEM project, en niet om met Verre SPA__ te worden verward

Terwijl `frontendModule="react"` wordt gespecificeerd, wordt het `ui.frontend` project niet gebruikt voor het Verre SPA gebruiksgeval. De SPA wordt extern ontwikkeld en beheerd voor AEM en gebruikt alleen AEM als inhoud-API. De `frontendModule="react"` vlag wordt vereist voor het project omvat `spa-project` AEM Java™ gebiedsdelen en opstelling de Verre Malplaatjes van de SPA van de Pagina.

Het AEM Archetype van het Project produceert de volgende elementen die gebruikte om AEM voor integratie met de SPA te vormen.

+ __AEM WCM Core Components__ proxiesat  `ui.content/src/.../apps/wknd-app/components`
+ __AEM__ Proxyat voor externe pagina SPA  `ui.content/src/.../apps/wknd-app/components/remotepage`
+ __AEM__ paginasjablonen  `ui.content/src/.../conf/wknd-app/settings/wcm/templates`
+ __Subproject voor het definiëren van__ inhoudstoewijzingen bij  `ui.content/src/...`
+ __Basislijn externe SPA AEM__ pagina&#39;s  `ui.content/src/.../content/wknd-app`
+ __OSGi-__ configuratiemap  `ui.config/src/.../apps/wknd-app/osgiconfig`

Met het basis AEM project wordt geproduceerd, verzekeren een paar aanpassingen SPA de verenigbaarheid van de Redacteur met Verre SPA.

## Ui.frontend-project verwijderen

Aangezien de SPA een Verre SPA is, veronderstel het buiten het AEM project wordt ontwikkeld en geleid. Om conflicten te vermijden, verwijder het `ui.frontend` project van het opstellen. Als het `ui.frontend` project niet wordt verwijderd, zullen twee SPA, het gebrek SPA in het `ui.frontend` project en de Verre SPA wordt verstrekt, tezelfdertijd in de AEM Redacteur SPA worden geladen.

1. Open het AEM project (`~/Code/wknd-app/com.adobe.aem.guides.wknd-app`) in uw winde
1. De hoofdmap `pom.xml` openen
1. Plaats de `<module>ui.frontend</module` uit de lijst `<modules>`

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

   Het `pom.xml`-bestand moet er als volgt uitzien:

   ![Ui.frontend-module verwijderen uit reactorpom](./assets/aem-project/uifrontend-reactor-pom.png)

1. `ui.apps/pom.xml` openen
1. Plaats de `<dependency>` op `<artifactId>wknd-app.ui.frontend</artifactId>`

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

   Het `ui.apps/pom.xml`-bestand moet er als volgt uitzien:

   ![Ui.frontend-afhankelijkheid verwijderen uit ui.apps](./assets/aem-project/uifrontend-uiapps-pom.png)

Als het AEM project vóór deze veranderingen werd gebouwd, schrap manueel `ui.frontend` geproduceerde de Bibliotheek van de Cliënt van `ui.apps` bij `ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react`.

## Inhoud toewijzen AEM

Voor AEM om de Verre SPA in de SPARedacteur te laden, moeten de afbeeldingen tussen de SPA routes en de AEM Pagina&#39;s worden gebruikt om te openen en auteursinhoud worden gevestigd.

Het belang van deze configuratie wordt later onderzocht.

De afbeelding kan worden toegewezen met [Sling Mapping](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#root-level-mappings-1) gedefinieerd in `/etc/map`.

1. In winde, open `ui.content` subproject
1. Ga naar  `src/main/content/jcr_root/etc`
1. Een map `map` maken
1. Maak in `map` een map `http`
1. Maak in `http` een bestand `.content.xml` met de inhoud:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. Maak in `http` een map `localhost_any`
1. Maak in `localhost_any` een bestand `.content.xml` met de inhoud:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. Maak in `localhost_any` een map `wknd-app-routes-adventure`
1. Maak in `wknd-app-routes-adventure` een bestand `.content.xml` met de inhoud:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   
   <!--
   The 'wknd-app-routes-adventure' mapping, maps requests to the SPA's adventure route 
   to it's corresponding page in AEM at /content/wknd-app/us/en/home/adventure/xxx.
   
   Note the adventure AEM pages will be created directly in AEM.
   -->
   
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="adventure:.*/([^/]+)/?$"
       sling:internalRedirect="/content/wknd-app/us/en/home/adventure/$1"/>
   ```

1. Voeg de toewijzingsknooppunten toe aan `ui.content/src/main/content/META-INF/vault/filter.xml` tot zij inbegrepen in het AEM pakket.

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

Het `filter.xml`-bestand moet er als volgt uitzien:

![Sling Mapping](./assets/aem-project/sling-mapping-filter.png)

Nu, wanneer het AEM project wordt opgesteld, zijn deze configuraties automatisch inbegrepen.

De effecten voor het toewijzen van objecten aan AEM die worden uitgevoerd op `http` en `localhost`, ondersteunen dus alleen lokale ontwikkeling. Wanneer het opstellen aan AEM als Cloud Service, moeten de gelijkaardige Verschuivende Toewijzingen worden toegevoegd die doel `https` en aangewezen AEM als Cloud Service domeinen. Zie de [documentatie voor het toewijzen van schaling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html) voor meer informatie.

## Beveiligingsbeleid voor het delen van bronnen tussen verschillende bronnen

Configureer vervolgens AEM om de inhoud te beschermen, zodat alleen deze SPA toegang heeft tot de AEM inhoud. C Vorm [Middel dat van de dwars-Oorsprong in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html) deelt.

1. In uw winde, open `ui.config` Gemaakt subproject
1. Navigeren `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config`
1. Een bestand met de naam `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json` maken
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
           "Authorization"
       ]
   }
   ```

Het `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`-bestand moet er als volgt uitzien:

![SPA Editor CORS-configuratie](./assets/aem-project/cors-configuration.png)

De belangrijkste configuratieelementen zijn:

+ `alloworigin` Hiermee geeft u op welke hosts inhoud mogen ophalen van AEM.
   + `localhost:3000` wordt toegevoegd ter ondersteuning van de SPA die lokaal wordt uitgevoerd
   + `https://external-hosted-app` fungeert als plaatsaanduiding die moet worden vervangen door het domein waarop de externe SPA wordt gehost.
+ `allowedpaths` specificeren welke wegen in AEM door deze configuratie CORS worden behandeld. Standaard is toegang tot alle inhoud in AEM toegestaan, maar dit kan alleen worden toegepast op de specifieke paden waartoe de SPA toegang heeft, bijvoorbeeld: `/content/wknd-app`.

## AEM pagina instellen als sjabloon voor externe SPA

Het AEM Project Archetype produceert een project dat voor AEM integratie met een Verre SPA wordt voorbereid, maar vereist een kleine, maar belangrijke aanpassing aan auto-geproduceerde AEM paginastructuur. Het type van de automatisch gegenereerde AEM moet worden gewijzigd in __Externe SPA pagina__ in plaats van een __SPA pagina__.

1. In uw winde, open `ui.content` subproject
1. Openen naar `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`
1. Dit `.content.xml`-bestand bijwerken met:

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

De belangrijkste veranderingen zijn updates aan de `jcr:content` knoop:

+ `cq:template` tot  `/conf/wknd-app/settings/wcm/templates/spa-remote-page`
+ `sling:resourceType` tot  `wknd-app/components/remotepage`

Het `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`-bestand moet er als volgt uitzien:

![Updates van de startpagina.content.xml](./assets/aem-project/home-content-xml.png)

Deze wijzigingen maken het mogelijk dat deze pagina, die de SPA hoofdmap in AEM vormt, de externe SPA in SPA Editor laadt.

>[!NOTE]
>
>Als dit project voorheen moest AEM, moet u de AEM pagina verwijderen als __Sites > WKND App > us > en > WKND App Home Page__, omdat het `ui.content`-project is ingesteld op __merge__-knooppunten in plaats van __update__.

Deze pagina kan ook worden verwijderd en opnieuw worden gemaakt als een externe SPA pagina in AEM zelf, maar aangezien deze pagina automatisch wordt gemaakt in het `ui.content`-project, is het beter om deze pagina in de codebasis bij te werken.

## Implementeer het AEM Project naar AEM SDK

1. Zorg ervoor dat de AEM-auteurservice wordt uitgevoerd op poort 4502
1. Navigeer vanaf de opdrachtregel naar de hoofdmap van het AEM Maven-project
1. Gebruik Maven om het project op te stellen aan uw lokale AEM SDK Auteur-service

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn clean install -PautoInstallSinglePackage](./assets/aem-project/mvn-install.png)

## De AEM van de hoofdmap configureren

Met AEM opgesteld Project, is er één laatste stap om SPA Redacteur voor te bereiden om onze Verre SPA te laden. In AEM, merk de AEM pagina die aan de SPA wortel, `/content/wknd-app/us/en/home` beantwoordt, die door het Archetype van het Project van de AEM wordt geproduceerd.

1. Aanmelden bij AEM-auteur
1. Navigeer naar __Sites > WKND App > us > en__
1. Selecteer __WKND App Home Page__ en tik __Eigenschappen__

   ![WKND App Home Page - Eigenschappen](./assets/aem-content/edit-home-properties.png)

1. Navigeer naar het tabblad __SPA__
1. __Configuratie van externe SPA__ invullen
   + __URL__ SPA host:  `http://localhost:3000`
      + De URL naar de hoofdmap van de externe SPA

   ![WKND App Home Page - Configuratie van externe SPA](./assets/aem-content/remote-spa-configuration.png)

1. Tik __Opslaan en sluiten__

Onthoud dat het type van deze pagina is gewijzigd in dat van een __Externe SPA Pagina__, waardoor we het tabblad __SPA__ in __Pagina-eigenschappen__ kunnen zien.

Deze configuratie moet alleen worden ingesteld op de AEM pagina die overeenkomt met de hoofdmap van de SPA. Alle AEM pagina&#39;s onder deze pagina nemen de waarde over.

## Gefeliciteerd

U hebt nu AEM configuraties voorbereid en deze geïmplementeerd op uw lokale AEM. Nu weet u hoe:

+ Verwijder de AEM Project Archetype-Geproduceerde SPA, door de gebiedsdelen in `ui.frontend` te becommentariëren
+ Voeg het Verschuiven Toewijzingen toe aan AEM die de SPA routes aan middelen in AEM in kaart brengen
+ Stel AEM het beveiligingsbeleid voor het delen van bronnen van verschillende oorsprong in waarmee de externe SPA inhoud van AEM kunnen verbruiken
+ Stel het AEM project aan uw lokale AEMdienst van de Auteur van SDK op
+ Een AEM Pagina markeren als de externe SPA met de eigenschap URL van SPA host

## Volgende stappen

Met AEM gevormd, kunnen wij op [bootstrapping de Verre SPA](./spa-bootstrap.md) met steun voor editable gebieden gebruikend AEM SPA Redacteur concentreren!