---
title: Artefacten van derden installeren - niet beschikbaar in de openbare Maven-opslagplaats
description: Leer hoe u artefacten van derden installeert die *niet beschikbaar zijn in de openbare Maven-opslagruimte* bij het maken en implementeren van een AEM-project.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: OSGI
topic: Development
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-09-13T00:00:00Z
jira: KT-16207
thumbnail: KT-16207.jpeg
exl-id: 0cec14b3-4be5-4666-a36c-968ea2fc634f
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1569'
ht-degree: 0%

---

# Artefacten van derden installeren - niet beschikbaar in de openbare Maven-opslagplaats

Leer hoe te om derdeartefacten te installeren die *niet beschikbaar in de openbare Gemaakte bewaarplaats* wanneer het bouwen van en het opstellen van een project van AEM zijn.

De **derdeartefacten** kunnen zijn:

- [&#x200B; OSGi bundel &#x200B;](https://www.osgi.org/resources/architecture/): Een bundel OSGi is een Java™ archiefdossier dat de klassen, de middelen van Java bevat, en manifest die de bundel en zijn gebiedsdelen beschrijft.
- [&#x200B; Java jar &#x200B;](https://docs.oracle.com/javase/tutorial/deployment/jar/basicsindex.html): Een Java™ archiefdossier dat de klassen en de middelen van Java bevat.
- [&#x200B; Pakket &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/administering/contentmanagement/package-manager#what-are-packages): Een pakket is een zip dossier dat bewaarplaats inhoud in dossier-systeem rangschikkingsvorm bevat.

## Standaardscenario

Typisch zou u de derdebundel installeren, pakket dat ** in de openbare GeMaven bewaarplaats als gebiedsdeel in het 2&rbrace; dossier van uw AEM project &lbrace;beschikbaar is.`pom.xml`

Bijvoorbeeld:

- [&#x200B; &#x200B;](https://github.com/adobe/aem-core-wcm-components) de bundel van de Kern van AEM WCM **wordt toegevoegd als gebiedsdeel in het** WKND- project [&#x200B; &#x200B;](https://github.com/adobe/aem-guides-wknd/blob/main/pom.xml#L747-L753) dossier. `pom.xml` Hier wordt het bereik `provided` gebruikt omdat de AEM WCM Core Components-bundel wordt geleverd door de AEM-runtime. Als de bundel niet door de runtime van AEM wordt verstrekt, zou u het `compile` werkingsgebied gebruiken en het is het standaardwerkingsgebied.

- [&#x200B; Gedeelde WKND &#x200B;](https://github.com/adobe/aem-guides-wknd-shared) **pakket** wordt toegevoegd als gebiedsdeel in het [&#x200B; WKND- project &#x200B;](https://github.com/adobe/aem-guides-wknd/blob/main/pom.xml#L767-L773) `pom.xml` dossier.



## Zeldzame scenario

Af en toe, wanneer het bouwen van en het opstellen van een project van AEM, kunt u een derdebundel of jar of pakket **moeten installeren die niet** in [&#x200B; Gemaakt Centrale Bewaarplaats &#x200B;](https://mvnrepository.com/) of [&#x200B; Openbare Bewaarplaats van Adobe &#x200B;](https://repo.adobe.com/index.html) beschikbaar is.

De redenen hiervoor zouden kunnen zijn:

- De bundel of het pakket wordt verstrekt door een intern team of een derdeverkoper en _is niet beschikbaar in de openbare GeMaven bewaarplaats_.

- Het Java™ jar dossier _is geen bundel OSGi_ en kan of niet beschikbaar in de openbare GeMaven bewaarplaats zijn.

- U hebt een functie nodig die nog niet wordt vrijgegeven in de meest recente versie van het pakket van derden dat beschikbaar is in de openbare Maven-opslagruimte. U hebt ervoor gekozen de lokaal gebouwde versie van RELEASE of SNAPSHOT te installeren.

## Vereisten

Voor het volgen van deze zelfstudie hebt u het volgende nodig:

- De [&#x200B; lokale ontwikkelomgeving van AEM &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview) of [&#x200B; Snelle Milieu van de Ontwikkeling (RDE) &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/rde/overview) opstelling.

- Het [&#x200B; project van AEM WKND &#x200B;](https://github.com/adobe/aem-guides-wknd) _om de derdebundel of jar of pakket_ toe te voegen en de veranderingen te verifiëren.

## Instellen

- Stel de lokale ontwikkelomgeving van AEM 6.X of AEM as a Cloud Service (AEMCS) of de RDE-omgeving in.

- Het AEM WKND-project klonen en implementeren.

  ```
  $ git clone git@github.com:adobe/aem-guides-wknd.git
  $ cd aem-guides-wknd
  $ mvn clean install -PautoInstallPackage 
  ```

  Controleer of de WKND-sitepagina&#39;s correct worden weergegeven.

## Een bundel van derden installeren in een AEM-project{#install-third-party-bundle}

Laten wij een demo OSGi [&#x200B; mijn-voorbeeld-bundel &#x200B;](./assets/install-third-party-articafcts/my-example-bundle.zip) installeren en gebruiken dat _niet beschikbaar in de openbare GeMaven bewaarplaats_ aan het project van AEM WKND is.

De **my-example-bundle** voert `HelloWorldService` dienst OSGi uit, zijn `sayHello()` methode keert `Hello Earth!` bericht terug.

Voor meer details, verwijs naar het README.md- dossier in het {[&#x200B; dossier 0} my-example-bundle.zip.](./assets/install-third-party-articafcts/my-example-bundle.zip)

### Voeg de bundel toe aan de module `all`

De eerste stap bestaat uit het toevoegen van `my-example-bundle` aan de module `all` van het AEM WKND-project.

- Download en haal het {[&#x200B; dossier 0} my-example-bundle.zip.](./assets/install-third-party-articafcts/my-example-bundle.zip)

- Maak de mappenstructuur van `all` in de module `all/src/main/content/jcr_root/apps/wknd-vendor-packages/container/install` van het AEM WKND-project. De map `/all/src/main/content` bestaat, u hoeft alleen de mappen `jcr_root/apps/wknd-vendor-packages/container/install` te maken.

- Kopieer het `my-example-bundle-1.0-SNAPSHOT.jar` -bestand uit de uitgepakte `target` -map naar de bovenstaande `all/src/main/content/jcr_root/apps/wknd-vendor-packages/container/install` -map.

  ![&#x200B; derde-partij-bundel in alle module &#x200B;](./assets/install-third-party-articafcts/3rd-party-bundle-all-module.png)

### De service uit de bundel gebruiken

Laten we de `HelloWorldService` OSGi-service van `my-example-bundle` in het AEM WKND-project gebruiken.

- Maak in de module `core` van het AEM WKND-project de `SayHello.java` Sling servlet @ `core/src/main/java/com/adobe/aem/guides/wknd/core/servlet` .

  ```java
  package com.adobe.aem.guides.wknd.core.servlet;
  
  import java.io.IOException;
  
  import javax.servlet.Servlet;
  import javax.servlet.ServletException;
  
  import org.apache.sling.api.SlingHttpServletRequest;
  import org.apache.sling.api.SlingHttpServletResponse;
  import org.apache.sling.api.servlets.HttpConstants;
  import org.apache.sling.api.servlets.ServletResolverConstants;
  import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
  import org.osgi.service.component.annotations.Component;
  import org.osgi.service.component.annotations.Reference;
  import com.example.services.HelloWorldService;
  
  @Component(service = Servlet.class, property = {
      ServletResolverConstants.SLING_SERVLET_PATHS + "=/bin/sayhello",
      ServletResolverConstants.SLING_SERVLET_METHODS + "=" + HttpConstants.METHOD_GET
  })
  public class SayHello extends SlingSafeMethodsServlet {
  
          private static final long serialVersionUID = 1L;
  
          // Injecting the HelloWorldService from the `my-example-bundle` bundle
          @Reference
          private HelloWorldService helloWorldService;
  
          @Override
          protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws ServletException, IOException {
              // Invoking the HelloWorldService's `sayHello` method
              response.getWriter().write("My-Example-Bundle service says: " + helloWorldService.sayHello());
          }
  }
  ```

- Voeg de `pom.xml` toe als afhankelijkheid in het hoofdbestand `my-example-bundle` van het AEM WKND-project.

  ```xml
  ...
  <!-- My Example Bundle -->
  <dependency>
      <groupId>com.example</groupId>
      <artifactId>my-example-bundle</artifactId>
      <version>1.0-SNAPSHOT</version>
      <scope>system</scope>
      <systemPath>${maven.multiModuleProjectDirectory}/all/src/main/content/jcr_root/apps/wknd-vendor-packages/container/install/my-example-bundle-1.0-SNAPSHOT.jar</systemPath>
  </dependency>
  ...
  ```

  Hier:
   - Het bereik `system` geeft aan dat de afhankelijkheid niet moet worden opgezocht in de openbare Maven-opslagruimte.
   - `systemPath` is het pad naar het `my-example-bundle` -bestand in de module `all` van het AEM WKND-project.
   - `${maven.multiModuleProjectDirectory}` is een Maven bezit dat aan de wortelfolder van het multi-moduleproject richt.

- Voeg in het bestand `core` van de module `core/pom.xml` van de AEM WKND-project de `my-example-bundle` als een afhankelijkheid toe.

  ```xml
  ...
  <!-- My Example Bundle -->
  <dependency>
      <groupId>com.example</groupId>
      <artifactId>my-example-bundle</artifactId>
  </dependency>
  ...
  ```

- Bouw en stel het project van AEM WKND gebruikend het volgende bevel op:

  ```
  $ mvn clean install -PautoInstallPackage
  ```

- Controleer of de `SayHello` servlet naar behoren werkt door de URL `http://localhost:4502/bin/sayhello` in de browser te openen.

- Leg bovenstaande wijzigingen vast in de opslagplaats van het AEM WKND-project. Dan verifieer de veranderingen in het milieu RDE of van AEM door de pijpleiding van Cloud Manager in werking te stellen.

  ![&#x200B; verifieer de HaveHello servlet - de Dienst van de Bundel &#x200B;](./assets/install-third-party-articafcts/verify-sayhello-servlet-bundle-service.png)

De [&#x200B; leerprogramma/installatie-3de partij-bundel &#x200B;](https://github.com/adobe/aem-guides-wknd/compare/main...tutorial/install-3rd-party-bundle) tak van het project van AEM WKND heeft de bovengenoemde veranderingen voor uw verwijzing.

### Belangrijke lessen{#key-learnings-bundle}

De OSGi-bundels die niet beschikbaar zijn in de openbare Maven-opslagplaats kunnen in een AEM-project worden geïnstalleerd door de volgende stappen uit te voeren:

- Kopieer de OSGi-bundel naar de map `all` van de module `jcr_root/apps/<PROJECT-NAME>-vendor-packages/container/install` . Deze stap is nodig om de bundel in een pakket te plaatsen en in te zetten op de AEM-instantie.

- Werk de `pom.xml` -bestanden van de basis- en kernmodule bij om de OSGi-bundel toe te voegen als een afhankelijkheid met de `system` scope en `systemPath` die naar het bundelbestand wijzen. Deze stap is noodzakelijk om het project te compileren.

## Een externe pot installeren in een AEM-project

In dit voorbeeld is `my-example-jar` geen OSGi-bundel, maar een Java-jar-bestand.

Laten wij een demo [&#x200B; mijn-voorbeeld-jar &#x200B;](./assets/install-third-party-articafcts/my-example-jar.zip) installeren en gebruiken dat _niet beschikbaar in de openbare GeMaven bewaarplaats_ aan het project van AEM WKND is.

**my-example-jar** is een Jar dossier van Java dat een `MyHelloWorldService` klasse met een `sayHello()` methode bevat die `Hello World!` bericht terugkeert.

Voor meer details, verwijs naar het README.md- dossier in het [&#x200B; my-example-jar.zip &#x200B;](./assets/install-third-party-articafcts/my-example-jar.zip) dossier.

### De jar toevoegen aan de module `all`

De eerste stap bestaat uit het toevoegen van `my-example-jar` aan de module `all` van het AEM WKND-project.

- Download en haal het {[&#x200B; dossier 0} my-example-jar.zip.](./assets/install-third-party-articafcts/my-example-jar.zip)

- Maak de mappenstructuur van `all` in de module `all/resource/jar` van het AEM WKND-project.

- Kopieer het `my-example-jar-1.0-SNAPSHOT.jar` -bestand uit de uitgepakte `target` -map naar de bovenstaande `all/resource/jar` -map.

  ![&#x200B; derde-partij-jar in alle module &#x200B;](./assets/install-third-party-articafcts/3rd-party-JAR-all-module.png)

### De service van de pot gebruiken

Laten we de `MyHelloWorldService` uit de `my-example-jar` gebruiken in het AEM WKND-project.

- Maak in de module `core` van het AEM WKND-project de `SayHello.java` Sling servlet @ `core/src/main/java/com/adobe/aem/guides/wknd/core/servlet` .

  ```java
  package com.adobe.aem.guides.wknd.core.servlet;
  
  import java.io.IOException;
  
  import javax.servlet.Servlet;
  import javax.servlet.ServletException;
  
  import org.apache.sling.api.SlingHttpServletRequest;
  import org.apache.sling.api.SlingHttpServletResponse;
  import org.apache.sling.api.servlets.HttpConstants;
  import org.apache.sling.api.servlets.ServletResolverConstants;
  import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
  import org.osgi.service.component.annotations.Component;
  
  import com.my.example.MyHelloWorldService;
  
  @Component(service = Servlet.class, property = {
          ServletResolverConstants.SLING_SERVLET_PATHS + "=/bin/sayhello",
          ServletResolverConstants.SLING_SERVLET_METHODS + "=" + HttpConstants.METHOD_GET
  })
  public class SayHello extends SlingSafeMethodsServlet {
  
      private static final long serialVersionUID = 1L;
  
      @Override
      protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response)
              throws ServletException, IOException {
  
          // Creating an instance of MyHelloWorldService
          MyHelloWorldService myHelloWorldService = new MyHelloWorldService();
  
          // Invoking the MyHelloWorldService's `sayHello` method
          response.getWriter().write("My-Example-JAR service says: " + myHelloWorldService.sayHello());
      }
  }    
  ```

- Voeg de `pom.xml` toe als afhankelijkheid in het hoofdbestand `my-example-jar` van het AEM WKND-project.

  ```xml
  ...
  <!-- My Example JAR -->
  <dependency>
      <groupId>com.my.example</groupId>
      <artifactId>my-example-jar</artifactId>
      <version>1.0-SNAPSHOT</version>
      <scope>system</scope>
      <systemPath>${maven.multiModuleProjectDirectory}/all/resource/jar/my-example-jar-1.0-SNAPSHOT.jar</systemPath>
  </dependency>            
  ...
  ```

  Hier:
   - Het bereik `system` geeft aan dat de afhankelijkheid niet moet worden opgezocht in de openbare Maven-opslagruimte.
   - `systemPath` is het pad naar het `my-example-jar` -bestand in de module `all` van het AEM WKND-project.
   - `${maven.multiModuleProjectDirectory}` is een Maven bezit dat aan de wortelfolder van het multi-moduleproject richt.

- Breng twee wijzigingen aan in het bestand `core` van de module `core/pom.xml` van AEM WKND:

   - Voeg de `my-example-jar` toe als afhankelijkheid.

     ```xml
     ...
     <!-- My Example JAR -->
     <dependency>
         <groupId>com.my.example</groupId>
         <artifactId>my-example-jar</artifactId>
     </dependency>
     ...
     ```

   - Werk `bnd-maven-plugin` -configuratie bij om de `my-example-jar` op te nemen in de OSGi-bundel (aem-guides-wknd.core) die wordt gebouwd.

     ```xml
     ...
     <plugin>
         <groupId>biz.aQute.bnd</groupId>
         <artifactId>bnd-maven-plugin</artifactId>
         <executions>
             <execution>
                 <id>bnd-process</id>
                 <goals>
                     <goal>bnd-process</goal>
                 </goals>
                 <configuration>
                     <bnd><![CDATA[
                 Import-Package: javax.annotation;version=0.0.0,*
                 <!-- Include the 3rd party jar as inline resource-->
                 -includeresource: \
                 lib/my-example-jar.jar=my-example-jar-1.0-SNAPSHOT.jar;lib:=true
                         ]]></bnd>
                 </configuration>
             </execution>
         </executions>
     </plugin>        
     ...
     ```

- Bouw en stel het project van AEM WKND gebruikend het volgende bevel op:

  ```
  $ mvn clean install -PautoInstallPackage
  ```

- Controleer of de `SayHello` servlet naar behoren werkt door de URL `http://localhost:4502/bin/sayhello` in de browser te openen.

- Leg bovenstaande wijzigingen vast in de opslagplaats van het AEM WKND-project. Dan verifieer de veranderingen in het milieu RDE of van AEM door de pijpleiding van Cloud Manager in werking te stellen.

  ![&#x200B; verifieer de HaveHello servlet - de Dienst van JAR &#x200B;](./assets/install-third-party-articafcts/verify-sayhello-servlet-jar-service.png)

De [&#x200B; leerprogramma/installatie-3de-partij-jar &#x200B;](https://github.com/adobe/aem-guides-wknd/compare/main...tutorial/install-3rd-party-jar) tak van het project van AEM WKND heeft de bovengenoemde veranderingen voor uw verwijzing.

In scenario&#39;s waar het Jar dossier van Java _in de openbare Gemaakt bewaarplaats beschikbaar is maar GEEN bundel OSGi_ is, kunt u de bovengenoemde stappen volgen behalve `<dependency>` `system` werkingsgebied en `systemPath` elementen niet worden vereist.

### Belangrijke lessen{#key-learnings-jar}

De Java-potten die geen OSGi-bundels zijn en al dan niet beschikbaar zijn in de openbare Maven-opslagplaats, kunnen in een AEM-project worden geïnstalleerd door de volgende stappen uit te voeren:

- Werk de `bnd-maven-plugin` configuratie in het dossier van de kernmodule `pom.xml` bij om Java als gealigneerde middel in de bundel te omvatten OSGi die wordt gebouwd.

De volgende stappen zijn alleen vereist als de Java-jar niet beschikbaar is in de openbare Maven-opslagplaats:

- Kopieer de Java-jar naar de map `all` van de module `resource/jar` .

- Werk de `pom.xml` -bestanden van de basis- en kernmodule bij om de Java-jar toe te voegen als een afhankelijkheid met de `system` scope en `systemPath` die naar het jar-bestand wijzen.

## Een pakket van derden installeren in een AEM-project

Laat [&#x200B; ACS AEM Commons &#x200B;](https://adobe-consulting-services.github.io/acs-aem-commons/) _SNAPSHOT_ versie installeren plaatselijk van de belangrijkste tak wordt gebouwd.

Dit is louter bedoeld om de stappen aan te tonen voor de installatie van een AEM-pakket dat niet beschikbaar is in de openbare Maven-opslagplaats.

Het ACS AEM Commons-pakket is beschikbaar in de openbare Maven-opslagplaats. Verwijs [&#x200B; ACS AEM Commons aan uw AEM Gemaakt project &#x200B;](https://adobe-consulting-services.github.io/acs-aem-commons/pages/maven.html) toevoegen om het aan uw project van AEM toe te voegen.

### Het pakket toevoegen aan de module `all`

De eerste stap bestaat uit het toevoegen van het pakket aan de module `all` van het AEM WKND-project.

- Maak een opmerking over of verwijder de ACS AEM Commons-afhankelijkheid van het POM-bestand. Verwijs [&#x200B; ACS AEM Commons aan uw AEM Gemaakt project &#x200B;](https://adobe-consulting-services.github.io/acs-aem-commons/pages/maven.html) toevoegen om het gebiedsdeel te identificeren.

- Kloon de `master` tak van de [&#x200B; ACS AEM Commons bewaarplaats &#x200B;](https://github.com/Adobe-Consulting-Services/acs-aem-commons) aan uw lokale machine.

- Bouw de ACS AEM Commons SNAPSHOT versie gebruikend het volgende bevel:

  ```
  $mvn clean install
  ```

- Het lokaal gebouwde pakket bevindt zich @ `all/target` , er zijn twee .zip-bestanden, het ene dat eindigt met `-cloud` is bedoeld voor AEM as a Cloud Service en het andere voor AEM 6.X.

- Maak de mappenstructuur van `all` in de module `all/src/main/content/jcr_root/apps/wknd-vendor-packages/container/install` van het AEM WKND-project. De map `/all/src/main/content` bestaat, u hoeft alleen de mappen `jcr_root/apps/wknd-vendor-packages/container/install` te maken.

- Kopieer het lokaal gebouwde pakketbestand (.zip) naar de map `/all/src/main/content/jcr_root/apps/mysite-vendor-packages/container/install` .

- Bouw en stel het project van AEM WKND gebruikend het volgende bevel op:

  ```
  $ mvn clean install -PautoInstallPackage
  ```

- Verifieer het geïnstalleerde ACS AEM Commons pakket:

   - CRX Package Manager @ `http://localhost:4502/crx/packmgr/index.jsp`

     ![&#x200B; ACS AEM Commons SNAPSHOT versiepakket &#x200B;](./assets/install-third-party-articafcts/acs-aem-commons-snapshot-package.png)

   - De OSGi-console @ `http://localhost:4502/system/console/bundles`

     ![&#x200B; ACS AEM Commons SNAPSHOT versiebundel &#x200B;](./assets/install-third-party-articafcts/acs-aem-commons-snapshot-bundle.png)

- Leg bovenstaande wijzigingen vast in de opslagplaats van het AEM WKND-project. Dan verifieer de veranderingen in het milieu RDE of van AEM door de pijpleiding van Cloud Manager in werking te stellen.

### Belangrijke lessen{#key-learnings-package}

De AEM-pakketten die niet beschikbaar zijn in de openbare Maven-opslagplaats kunnen in een AEM-project worden geïnstalleerd door de volgende stappen uit te voeren:

- Kopieer het pakket naar de map `all` van de module `jcr_root/apps/<PROJECT-NAME>-vendor-packages/container/install` . Deze stap is nodig om het pakket te verpakken en te implementeren in de AEM-instantie.


## Samenvatting

In deze zelfstudie hebt u geleerd hoe u artefacten van derden (bundel, Java jar en pakket) kunt installeren die niet beschikbaar zijn in de openbare Maven-opslagruimte bij het bouwen en implementeren van een AEM-project.
