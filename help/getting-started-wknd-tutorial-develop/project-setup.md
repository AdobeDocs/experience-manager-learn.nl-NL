---
title: Aan de slag met AEM Sites - Projectinstellingen
seo-title: Aan de slag met AEM Sites - Projectinstellingen
description: Omvat de verwezenlijking van een Maven Multimoduleproject om de code en de configuraties voor een AEM Plaats te beheren.
sub-product: sites
feature: maven-archetype
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
translation-type: tm+mt
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '2468'
ht-degree: 1%

---


# Projectinstelling {#project-setup}

Deze zelfstudie behandelt het maken van een Maven Multi Module Project om de code en configuraties voor een Adobe Experience Manager Site te beheren.

## Vereisten {#prerequisites}

Controleer de vereiste gereedschappen en instructies voor het instellen van een [lokale ontwikkelomgeving](overview.md#local-dev-environment). Zorg ervoor dat u een nieuw exemplaar van Adobe Experience Manager lokaal beschikbaar hebt en dat er geen extra sample/demo-pakketten zijn geïnstalleerd (behalve de vereiste Servicepacks).

## Doelstelling {#objective}

1. Leer hoe u een nieuw AEM project kunt genereren met een Maven-archetype.
1. Begrijp de verschillende modules die door het AEM Archetype van het Project worden geproduceerd en hoe zij samenwerken.
1. Begrijp hoe AEM de Componenten van de Kern in een AEM Project inbegrepen zijn.

## Wat u gaat maken {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

In dit hoofdstuk, zult u een nieuw project van Adobe Experience Manager produceren gebruikend het [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). Uw AEM project bevat alle code, inhoud, en configuraties die voor een implementatie van Plaatsen worden gebruikt. Het in dit hoofdstuk gegenereerde project zal als basis dienen voor een implementatie van de WKND-site en zal in toekomstige hoofdstukken worden opgenomen.

## Achtergrond {#background}

**Wat is een Maven-project?** - [Apache Maven](https://maven.apache.org/) is een hulpprogramma voor softwarebeheer voor het opzetten van projecten. *Alle Adobe Experience Manager* -implementaties gebruiken Maven-projecten om aangepaste code bovenop AEM te bouwen, te beheren en te implementeren.

**Wat is een Maven archetype?** - Een [Maven archetype](https://maven.apache.org/archetype/index.html) is een sjabloon of patroon voor het genereren van nieuwe projecten. Het AEM archetype van het Project staat ons toe om een nieuw project met een douane te produceren namespace en een projectstructuur te omvatten die beste praktijken volgt, zeer versnellend ons project.

## Het project maken {#create}

Er zijn een paar opties voor het creëren van een Maven Multi-module project voor AEM. Deze zelfstudie maakt gebruik van het [Maven AEM Project Archetype **22**](https://github.com/adobe/aem-project-archetype). Cloud Manager [biedt ook een UI-wizard](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/create-an-application-project.html) waarmee u het maken van een AEM toepassingsproject kunt starten. Het onderliggende project dat door de UI van de Manager van de Wolk wordt geproduceerd resulteert in de zelfde structuur zoals direct het gebruiken van archetype.

>[!NOTE]
>
>Gebruik versie **22** van het archetype voor de volgende zelfstudie. Nochtans, is het altijd een beste praktijk om de **recentste** versie van archetype te gebruiken om een nieuw project te produceren.

De volgende reeks stappen zal plaatsvinden gebruikend een op UNIX gebaseerde terminal van de bevellijn, maar zou gelijkaardig moeten zijn als het gebruiken van een terminal van Vensters.

1. Open een opdrachtregelterminal en controleer of Maven is geïnstalleerd en toegevoegd aan het opdrachtregelpad:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. Controleer of het profiel **adobe-public** actief is door de volgende opdracht uit te voeren:

   ```shell
   $ mvn help:effective-settings
       ...
   <activeProfiles>
       <activeProfile>adobe-public</activeProfile>
   </activeProfiles>
   <pluginGroups>
       <pluginGroup>org.apache.maven.plugins</pluginGroup>
       <pluginGroup>org.codehaus.mojo</pluginGroup>
   </pluginGroups>
   </settings>
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  0.856 s
   ```

   Als u **niet** de **adobe-public** ziet, geeft dit aan dat in het `~/.m2/settings.xml` bestand niet naar de Adobe-repo wordt verwezen. Ga terug naar de stappen voor het installeren en configureren van Apache Maven in [een lokale ontwikkelomgeving](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven).

1. Navigeer naar een map waarin u het AEM project wilt genereren. Dit kan om het even welke folder zijn waarin u de broncode van uw project wilt handhaven. Bijvoorbeeld een map met de naam `code` onder de thuismap van de gebruiker:

   ```shell
   $ cd ~/code
   ```

1. Plak het volgende in de opdrachtregel om het project in de batchmodus [te](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html)genereren:

   ```shell
   $ mvn archetype:generate -B \
       -DarchetypeGroupId=com.adobe.granite.archetypes \
       -DarchetypeArtifactId=aem-project-archetype \
       -DarchetypeVersion=22 \
       -DgroupId=com.adobe.aem.guides \
       -Dversion=0.0.1-SNAPSHOT \
       -DappsFolderName=wknd \
       -DartifactId=aem-guides-wknd \
       -Dpackage=com.adobe.aem.guides.wknd \
       -DartifactName="WKND Sites Project" \
       -DcomponentGroupName=WKND \
       -DconfFolderName=wknd \
       -DcontentFolderName=wknd \
       -DcssId=wknd \
       -DisSingleCountryWebsite=n \
       -Dlanguage_country=en_us \
       -DoptionAemVersion=6.5.0 \
       -DoptionDispatcherConfig=none \
       -DoptionIncludeErrorHandler=n \
       -DoptionIncludeExamples=y \
       -DoptionIncludeFrontendModule=y \
       -DpackageGroup=wknd \
       -DsiteName="WKND Site"
   ```

   >[!NOTE]
   >
   >Standaard gebruikt het genereren van een project van het Maven archetype de interactieve modus. Als u wilt voorkomen dat vetvingerafdrukken worden gemaakt van waarden die u in de batchmodus hebt gegenereerd. Het is ook mogelijk om het Maven AEM-project te maken met de plug-in [AEM Developer Tools voor Eclipse](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/aem-eclipse.html).

   >[!CAUTION]
   >
   >Als u een dergelijke fout ontvangt: *Kan doel org.apache.maven.plugins:maven-archetype-plugin:3.1.1:generate (default-cli) niet uitvoeren op standalone-pom van project: Het gewenste archetype bestaat* niet. Er wordt aangegeven dat in het `~/.m2/settings.xml` bestand niet naar de Adobe-repo wordt verwezen. Ga terug naar de vorige stappen en controleer of het bestand settings.xml verwijst naar het Adobe-repo.

   In de volgende tabel worden de waarden weergegeven die voor deze zelfstudie worden gebruikt:

   | Naam | Waarden | Beschrijving |
   |-----------------------------|---------|--------------------|
   | groupId | com.adobe.aem.guides | Base Maven groupId |
   | artifactId | aem-hulplijnen | Basis Maven ArtifactId |
   | version | 0.0.1-MOMENTOPNAME | Versie |
   | package | com.adobe.aem.guides.wknd | Java-bronpakket |
   | appsFolderName | wkni | /apps, mapnaam |
   | artifactName | WKND-siteproject | Maven Project Name |
   | componentGroupName | WKND | AEM naam componentgroep |
   | contentFolderName | wkni | /content folder name |
   | confFolderName | wkni | /conf mapnaam |
   | cssId | wkni | voorvoegsel gebruikt in gegenereerde CSS |
   | packageGroup | wkni | Naam inhoudspakketgroep |
   | siteName | WKND-site | AEM sitenaam |
   | optionAemVersion | 6.5.0 | Doelversie AEM |
   | taal_land | nl_nl | taal/landcode om de inhoudsstructuur te maken op basis van (bijvoorbeeld nl_nl) |
   | optionIncludeExamples | y | Inclusief een voorbeeldsite van de componentbibliotheek |
   | optionIncludeErrorHandler | n | Een aangepaste 404-responspagina opnemen |
   | optionIncludeFrontendModule | y | Inclusief een speciale frontendmodule |
   | isSingleCountryWebsite | n | Taal-master structuur maken in voorbeeldinhoud |
   | optionDispatcherConfig | none | Een verzendersconfiguratiemodule genereren |

1. De volgende map en bestandsstructuur worden gegenereerd door het Maven archetype op uw lokale bestandssysteem:

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.content/
           |--- ui.frontend /
           |--- it.launcher/
           |--- it.tests/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## Het project bouwen {#build}

Nu wij een nieuw project hebben geproduceerd, kunnen wij de projectcode aan een lokaal geval van AEM opstellen.

1. Zorg ervoor dat u een instantie van AEM hebt die lokaal wordt uitgevoerd op poort **4502**.
1. Van de bevellijn navigeert in de `aem-guides-wknd` projectfolder.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Stel het volgende bevel in werking om het volledige project te bouwen en op te stellen aan AEM:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Het profiel Maven `autoInstallSinglePackage` compileert de individuele modules van het project en stelt één enkel pakket aan de AEM instantie op. Dit pakket wordt standaard geïmplementeerd op een AEM die lokaal wordt uitgevoerd op poort **4502** en met de referenties van `admin:admin`.

1. Navigeer naar Package Manager op uw lokale AEM-instantie: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). U moet drie pakketten zien voor `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.content`en `aem-guides-wknd.all`.

   U zou veelvoudige pakketten voor [AEM Componenten](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html) van de Kern ook moeten zien die in het project door archetype inbegrepen zijn. Dit wordt later in de zelfstudie besproken.

1. Navigeer naar de Sites-console: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). De WKND-site wordt een van de sites. Dit omvat een sitestructuur met een hiërarchie voor Amerikaanse en taalmeesters. Deze plaatshiërarchie is gebaseerd op de waarden voor `language_country` en `isSingleCountryWebsite` wanneer het produceren van het project gebruikend archetype.

1. Open de pagina **US** `>` English **door de pagina te selecteren en op de knop** Edit **** op de menubalk te klikken:

   ![siteconsole](assets/project-setup/aem-sites-console.png)

1. Sommige inhoud is al gemaakt en er zijn verschillende componenten beschikbaar die aan een pagina moeten worden toegevoegd. Experimenteer met deze componenten om een idee te krijgen van de functionaliteit. Hoe deze pagina en componenten worden gevormd zal in detail later in het leerprogramma worden onderzocht.

## Inspect het project {#project-structure}

Het AEM archetype bestaat uit individuele Geweven modules:

* [core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html) - Java-bundel die alle kernfunctionaliteit bevat, zoals OSGi-services, listeners of planners, en aan componenten gerelateerde Java-code, zoals servlets of aanvraagfilters.
* [ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html) : bevat de /apps-onderdelen van het project, dat wil zeggen JS&amp;CSS-clientlibs, -componenten en OSGi-configuraties
* [ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.html) - bevat structurele inhoud en configuraties zoals bewerkbare sjablonen, metagegevensschema&#39;s (/content, /conf)
* ui.tests - Java-bundel met JUnit-tests die op de server worden uitgevoerd. Deze bundel moet niet op productie worden opgesteld.
* ui.launcher - bevat lijm-code waarmee de ui.tests-bundel (en afhankelijke bundels) naar de server wordt geïmplementeerd en waarmee de externe JUnit-uitvoering wordt geactiveerd
* [ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html) - (facultatief) bevat de artefacten die worden vereist om de Web-pack-gebaseerde front-end bouwstijlmodule te gebruiken.
* alles - dit is een lege Gemaakt module die de bovengenoemde modules in één enkel pakket combineert dat aan een AEM milieu kan worden opgesteld.

![Projectdiagram](assets/project-setup/project-pom-structure.png)

Zie de documentatie [van de Archetype van het](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html) AEM Project om meer details van de Geweven modules te leren.

## Geavanceerde Maven-opdrachten {#advanced-maven-commands}

Tijdens ontwikkeling kunt u met enkel één van de modules werken en wilt vermijden bouw het volledige project om tijd te besparen. U kunt ook rechtstreeks aan een AEM publiceren instantie of misschien aan een geval van AEM willen opstellen die niet op haven 4502 loopt.

Vervolgens bekijken we enkele extra Maven-profielen en -opdrachten die u tijdens de ontwikkeling kunt gebruiken voor meer flexibiliteit.

### Kernmodule {#core-module}

De **[kernmodule](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)** bevat alle Java-code die aan het project is gekoppeld. Wanneer gebouwd stelt het een bundel OSGi aan AEM op. Alleen deze module samenstellen:

1. Navigeer in de `core` map (onder `aem-guides-wknd`):

   ```shell
   $ cd core/
   ```

1. Voer de volgende opdracht uit:

   ```shell
   $ mvn -PautoInstallBundle clean install
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   [INFO] Finished at: 2019-12-06T13:40:21-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Ga naar [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). Dit is de console OSGi van het Web en bevat informatie over alle bundels die op de AEM instantie worden geïnstalleerd.

1. Schakel de sorteerkolom **Id** in en u ziet dat de WKND-bundel is geïnstalleerd en actief is.

   ![Basisbundel](assets/project-setup/wknd-osgi-console.png)

1. U kunt de &quot;fysieke&quot;plaats van de pot in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd/install/wknd-sites-guide.core-0.0.1-SNAPSHOT.jar)zien:

   ![CRXDE Locatie van de ar](assets/project-setup/jcr-bundle-location.png)

### Ui.apps en Ui.content-modules {#apps-content-module}

De module **[ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)** die is gemaakt, bevat alle renderingcode die nodig is voor de onderliggende site `/apps`. Dit omvat CSS/JS die in een AEM formaat genoemd [clientlibs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)zal worden opgeslagen. Dit omvat ook [HTML](https://docs.adobe.com/docs/en/htl/overview.html) -scripts voor het renderen van dynamische HTML. U kunt de module **ui.apps** zien als een kaart aan de structuur in JCR maar in een formaat dat op een dossiersysteem kan worden opgeslagen en aan broncontrole wordt geëngageerd. De module **ui.apps** bevat alleen code.

Om enkel deze module te bouwen:

1. Via de opdrachtregel. Navigeer in de `ui.apps` map (onder `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. Voer de volgende opdracht uit:

   ```shell
   $ mvn -PautoInstallPackage clean install
   ...
   Package installed in 122ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.972 s
   [INFO] Finished at: 2019-12-06T14:44:12-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Ga naar [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). U moet het `ui.apps` pakket zien als het eerste geïnstalleerde pakket en het moet een recentere tijdstempel hebben dan een van de andere pakketten.

   ![Ui.apps-pakket geïnstalleerd](assets/project-setup/ui-apps-package.png)

1. Ga terug naar de opdrachtregel en voer de volgende opdracht uit (in de `ui.apps` map):

   ```shell
   $ mvn -PautoInstallPackagePublish clean install
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package-publish) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4503/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.717 s
   [INFO] Finished at: 2019-12-06T14:51:45-08:00
   [INFO] ------------------------------------------------------------------------
   ```

   Het profiel `autoInstallPackagePublish` is bedoeld om het pakket te implementeren in een publicatieomgeving die wordt uitgevoerd op poort **4503**. De bovenstaande fout wordt verwacht als er geen AEM op http://localhost:4503 wordt uitgevoerd.

1. Stel ten slotte het volgende bevel in werking om het `ui.apps` pakket op haven **4504** op te stellen:

   ```shell
   $ mvn -PautoInstallPackage clean install -Daem.port=4504
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4504/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] --------------------------------------------------------------------
   ```

   Opnieuw wordt een bouwstijlmislukking verwacht om voor te komen als geen AEM instantie die op haven **4504** loopt beschikbaar is. De parameter `aem.port` wordt gedefinieerd in het POM-bestand op `aem-guides-wknd/pom.xml`.

De module **[ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.htm)** is op dezelfde manier gestructureerd als de module **ui.apps** . Het enige verschil is dat de module **ui.content** de zogenaamde **muteerbare** inhoud bevat. **De inhoud van het Mutable** verwijst hoofdzakelijk naar niet-codeconfiguraties zoals Malplaatjes, Beleid, of omslagstructuren die in bron-controle wordt opgeslagen **maar** op een AEM instantie direct zou kunnen worden gewijzigd. Dit zal in het hoofdstuk over Pagina&#39;s en Malplaatjes veel gedetailleerder worden onderzocht. Momenteel is het belangrijk dat dezelfde Maven-opdrachten die worden gebruikt om de module **ui.apps** te maken, kunnen worden gebruikt om de module **ui.content** te bouwen. U kunt de bovenstaande stappen vrij weergeven vanuit de map **ui.content** .

### Ui.frontend, module {#ui-frontend-module}

De module **[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)** is een module Maven die eigenlijk een [webpack](https://webpack.js.org/) project is. Deze module is ingesteld op een speciaal front-end constructiesysteem dat JavaScript- en CSS-bestanden uitvoert, die op hun beurt worden geïmplementeerd op AEM. Met de module **ui.frontend** kunnen ontwikkelaars code schrijven met talen zoals [Sass](https://sass-lang.com/), [TypeScript](https://www.typescriptlang.org/), [npm](https://www.npmjs.com/) -modules gebruiken en de uitvoer rechtstreeks in AEM integreren.

De module **ui.frontend** zal meer in detail in het hoofdstuk over cliënt-zijbibliotheken en front-end ontwikkeling worden behandeld. Laten we nu bekijken hoe het in het project is geïntegreerd.

1. Via de opdrachtregel. Navigeer in de `ui.frontend` map (onder `aem-guides-wknd`):

   ```shell
   $ cd ../ui.frontend
   ```

1. Voer de volgende opdracht uit:

   ```shell
   $ mvn clean install
   ...
   [INFO] write clientlib asset txt file (type: js): ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js.txt
   [INFO] copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   [INFO]
   [INFO] write clientlib asset txt file (type: css): ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt
   [INFO] copy: dist/clientlib-site/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   [INFO]
   [INFO] --- maven-assembly-plugin:3.1.1:single (default) @ aem-guides-wknd.ui.frontend ---
   [INFO] Reading assembly descriptor: assembly.xml
   [INFO] Building zip: /Users/dgordon/code/aem-guides-wknd/ui.frontend/target/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip
   [INFO]
   [INFO] --- maven-install-plugin:2.5.2:install (default-install) @ aem-guides-wknd.ui.frontend ---
   [INFO] Installing /Users/dgordon/code/aem-guides-wknd/ui.frontend/pom.xml to /Users/dgordon/.m2/repository/com/adobe/aem/guides/aem-guides-wknd.ui.frontend/0.0.1-SNAPSHOT/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.pom
   [INFO] Installing /Users/dgordon/code/aem-guides-wknd/ui.frontend/target/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip to /Users/dgordon/.m2/repository/com/adobe/aem/guides/aem-guides-wknd.ui.frontend/0.0.1-SNAPSHOT/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  13.520 s
   [INFO] Finished at: 2019-12-06T15:26:16-08:00
   ```

   Let op de lijnen als `copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js`. Dit geeft aan dat de gecompileerde CSS en JS naar de `ui.apps` map worden gekopieerd.

1. De gewijzigde tijdstempel voor het bestand weergeven `aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt`. Het zou recentere dan de andere dossiers in de `ui.apps` module moeten worden bijgewerkt.

   In tegenstelling tot de andere modules die we hebben bekeken, wordt de module **ui.frontend** niet rechtstreeks naar AEM geïmplementeerd. In plaats daarvan worden de CSS en JS gekopieerd naar de module **ui.apps** en wordt de module **ui.apps** geïmplementeerd op AEM. Als u de bouwstijlorde van zeer eerste Maven bevel bekijkt, zult u zien dat **ui.frontend** altijd *vóór* **ui.apps** wordt gebouwd.

   Later in de zelfstudie zullen we de geavanceerde functies van de module **ui.frontend** en de geïntegreerde webpack ontwikkelingsserver bekijken voor snelle ontwikkeling.

## Opname van kerncomponenten {#core-components}

Het archetype sluit automatisch [AEM Componenten](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html) van de Kern in het project in. Eerder, toen het inspecteren van de opgestelde pakketten aan AEM, werden de veelvoudige pakketten met betrekking tot de Componenten van de Kern inbegrepen. De Componenten van de kern zijn een reeks basiscomponenten die worden ontworpen om de ontwikkeling van een project van AEM Sites te versnellen. De Componenten van de kern zijn open bron en beschikbaar op [GitHub](https://github.com/adobe/aem-core-wcm-components). Meer informatie over hoe de Componenten van de Kern in het project worden [omvat kan hier](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html#core-components)worden gevonden.

1. Uw favoriete teksteditor openen `aem-guides-wknd/pom.xml`.

1. Search for `core.wcm.components.version`. Hier ziet u welke versie van Core Components is opgenomen:

   ```xml
       <core.wcm.components.version>2.x.x</core.wcm.components.version>
   ```

   >[!NOTE]
   >
   > Het AEM Project Archetype zal een versie van AEM Core Componenten omvatten, nochtans hebben deze projecten verschillende versiecycli, en bijgevolg kan de inbegrepen versie van de Componenten van de Kern niet de recentste zijn. Als beste praktijken, zou u altijd aan hefboomwerking de recentste versie van de Componenten van de Kern moeten kijken. Nieuwe functies en opgeloste problemen worden regelmatig bijgewerkt. De recentste [versieinformatie kan op GitHub](https://github.com/adobe/aem-core-wcm-components/releases)worden gevonden.

1. Als u naar de `dependencies` sectie scrolt, zou u de individuele gebiedsdelen van de Component van de Kern moeten zien:

   ```xml
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.core</artifactId>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.content</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.config</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.examples</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   ```

## Beheer van bronbeheer {#source-control}

Het is altijd een goed idee om een of andere vorm van broncontrole te gebruiken om de code in uw toepassing te beheren. Dit leerprogramma gebruikt git en GitHub. Er zijn verscheidene dossiers die door Geweven en/of winde van keus worden geproduceerd die door SCM zouden moeten worden genegeerd.

Maven maakt een doelmap wanneer u het codepakket maakt en installeert. De doelmap en -inhoud moeten worden uitgesloten van SCM.

Onder ui.apps zult u ook vele .content.xml- dossiers opmerken die worden gecreeerd. Deze XML-bestanden geven de knooppunttypen en eigenschappen weer van inhoud die in de JCR is geïnstalleerd. Deze bestanden zijn van essentieel belang en moeten **niet** worden genegeerd.

Het AEM projectarchetype zal een steekproefdossier produceren `.gitignore` dat als uitgangspunt kan worden gebruikt waarvoor de dossiers veilig kunnen worden genegeerd. Het bestand wordt gegenereerd bij `<src>/aem-guides-wknd/.gitignore`.

## Controleren {#chapter-review}

>[!VIDEO](https://video.tv.adobe.com/v/30153/?quality=12&learn=on)

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, u hebt zojuist uw eerste AEM Project gemaakt!

### Volgende stappen {#next-steps}

Begrijp de onderliggende technologie van een Component van de Plaatsen van Adobe Experience Manager (AEM) door een eenvoudig `HelloWorld` voorbeeld met de [Zelfstudie van de Grondbeginselen](component-basics.md) van de Component.
