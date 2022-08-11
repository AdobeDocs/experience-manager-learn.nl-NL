---
title: Aan de slag met AEM Sites - Projectinstellingen
description: Creeer een Maven Multimoduleproject om de code en de configuraties voor een Plaats van de Experience Manager te beheren.
sub-product: sites
version: 6.5, Cloud Service
type: Tutorial
feature: AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
exl-id: bb0cae58-79bd-427f-9116-d46afabdca59
source-git-commit: 819188cabb784a2bbfa097dc4c47f75e38231b53
workflow-type: tm+mt
source-wordcount: '1848'
ht-degree: 0%

---

# Projectinstelling {#project-setup}

Deze zelfstudie behandelt het maken van een Maven Multi Module Project om de code en configuraties voor een Adobe Experience Manager Site te beheren.

## Vereisten {#prerequisites}

Controleer de vereiste gereedschappen en instructies voor het instellen van een [plaatselijke ontwikkelomgeving](./overview.md#local-dev-environment). Zorg ervoor dat u een nieuw exemplaar van Adobe Experience Manager lokaal beschikbaar hebt en dat er geen extra sample/demo-pakketten zijn geïnstalleerd (behalve de vereiste Servicepacks).

## Doelstelling {#objective}

1. Leer hoe u een nieuw AEM project kunt genereren met een Maven-archetype.
1. Begrijp de verschillende modules die door het AEM Archetype van het Project worden geproduceerd en hoe zij samenwerken.
1. Begrijp hoe AEM de Componenten van de Kern in een AEM Project inbegrepen zijn.

## Wat u gaat maken {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

In dit hoofdstuk genereert u een nieuw Adobe Experience Manager-project met de opdracht [Projectarchetype AEM](https://github.com/adobe/aem-project-archetype). Uw AEM project bevat alle code, inhoud, en configuraties die voor een implementatie van Plaatsen worden gebruikt. Het in dit hoofdstuk gegenereerde project zal als basis dienen voor een implementatie van de WKND-site en zal in toekomstige hoofdstukken worden opgenomen.

**Wat is een Maven-project?** - [Apache Maven](https://maven.apache.org/) is een hulpmiddel van het softwarebeheer om projecten te bouwen. *Alle Adobe Experience Manager* implementaties gebruiken Maven-projecten om aangepaste code bovenop AEM te bouwen, te beheren en te implementeren.

**Wat is een Maven archetype?** - A [Maven archetype](https://maven.apache.org/archetype/index.html) is een malplaatje of een patroon voor het produceren van nieuwe projecten. Het AEM archetype van het Project staat ons toe om een nieuw project met een douane te produceren namespace en een projectstructuur te omvatten die beste praktijken volgt, zeer versnellend ons project.

## Het project maken {#create}

Er zijn een paar opties voor het creëren van een Maven Multi-module project voor AEM. Deze zelfstudie maakt gebruik van de [Maven AEM Project Archetype **35**](https://github.com/adobe/aem-project-archetype). Cloud Manager ook [biedt een wizard UI](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/getting-started/create-application-project/using-the-wizard.html) het initiëren van de creatie van een AEM toepassingsproject. Het onderliggende project dat door de UI van de Manager van de Wolk wordt geproduceerd resulteert in de zelfde structuur zoals direct het gebruiken van archetype.

>[!NOTE]
>
>Deze zelfstudie gebruikt versie **35** van het archetype. Het is altijd verstandig om de **nieuwste** versie van archetype om een nieuw project te produceren.

De volgende reeks stappen zal plaatsvinden gebruikend een op UNIX gebaseerde terminal van de bevellijn, maar zou gelijkaardig moeten zijn als het gebruiken van een terminal van Vensters.

1. Open een opdrachtregelterminal. Controleer of Maven is geïnstalleerd:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. Navigeer naar een map waarin u het AEM project wilt genereren. Dit kan om het even welke folder zijn waarin u de broncode van uw project wilt handhaven. Bijvoorbeeld een map met de naam `code` onder de homemap van de gebruiker:

   ```shell
   $ cd ~/code
   ```

1. Plak het volgende in de opdrachtregel naar [produceer het project op partijwijze](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=35 \
       -D appTitle="WKND Sites Project" \
       -D appId="wknd" \
       -D groupId="com.adobe.aem.guides" \
       -D artifactId="aem-guides-wknd" \
       -D package="com.adobe.aem.guides.wknd" \
       -D version="0.0.1-SNAPSHOT" \
       -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Vervang bij AEM 6.5.10+ `aemVersion="cloud"` with `aemVersion="6.5.10"`.

   Een volledige lijst van beschikbare eigenschappen voor het vormen van een project [hier te vinden](https://github.com/adobe/aem-project-archetype#available-properties).

1. De volgende map en bestandsstructuur worden gegenereerd door het Maven archetype op uw lokale bestandssysteem:

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.apps.structure/
           |--- ui.config/
           |--- ui.content/
           |--- ui.frontend/
           |--- ui.tests /
           |--- it.tests/
           |--- dispatcher/
           |--- analyse/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## Het project implementeren en bouwen {#build}

Bouw en stel de projectcode aan een lokaal geval van AEM op.

1. Zorg ervoor dat u een auteur-instantie hebt van AEM die lokaal op de poort wordt uitgevoerd **4502**.
1. Navigeer vanaf de opdrachtregel naar de `aem-guides-wknd` projectmap.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Stel het volgende bevel in werking om het volledige project te bouwen en op te stellen aan AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   De build zal ongeveer een minuut in beslag nemen en zal eindigen met het volgende bericht:

   ```
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.269 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  8.047 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [01:02 min]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  1.985 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  8.037 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  4.672 s]
   [INFO] WKND Sites Project - UI config ..................... SUCCESS [  0.313 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.270 s]
   [INFO] WKND Sites Project - Integration Tests ............. SUCCESS [ 15.571 s]
   [INFO] WKND Sites Project - Dispatcher .................... SUCCESS [  0.232 s]
   [INFO] WKND Sites Project - UI Tests ...................... SUCCESS [  0.728 s]
   [INFO] WKND Sites Project - Project Analyser .............. SUCCESS [ 33.398 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  02:18 min
   [INFO] Finished at: 2021-01-31T12:33:56-08:00
   [INFO] ------------------------------------------------------------------------
   ```

   Het profiel Maven `autoInstallSinglePackage` compileert de individuele modules van het project en stelt één enkel pakket aan de AEM instantie op. Dit pakket wordt standaard geïmplementeerd op een AEM die lokaal op de poort wordt uitgevoerd **4502** en met de geloofsbrieven van `admin:admin`.

1. Navigeer naar Package Manager op uw lokale AEM-instantie: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). U moet pakketten zien voor `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content`, en `aem-guides-wknd.all`.

1. Navigeer naar de Sites-console: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). De WKND-site wordt een van de sites. Dit omvat een sitestructuur met een hiërarchie voor Amerikaanse en taalmeesters. Deze sitehiërarchie is gebaseerd op de waarden voor `language_country` en `isSingleCountryWebsite` wanneer het produceren van het project gebruikend archetype.

1. Open de **VS** `>` **Engels** pagina door de pagina te selecteren en op de **Bewerken** in de menubalk:

   ![siteconsole](assets/project-setup/aem-sites-console.png)

1. Starter-inhoud is al gemaakt en er zijn verschillende componenten beschikbaar die aan een pagina moeten worden toegevoegd. Experimenteer met deze componenten om een idee te krijgen van de functionaliteit. In het volgende hoofdstuk leert u de grondbeginselen van een component.

   ![Inhoud startpagina](assets/project-setup/start-home-page.png)

   *Voorbeeldinhoud gegenereerd door Archetype*

## Inspect het project {#project-structure}

Het geproduceerde AEM project bestaat uit individuele Geproduceerde modules, elk met een verschillende rol. Deze zelfstudie en een meerderheid van de ontwikkelingsprogramma&#39;s zijn gericht op de volgende modules:

* [kern](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html) - Java Code, voornamelijk back-end ontwikkelaars.
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) - Bevat broncode voor CSS, JavaScript, Sass, Type Script, voornamelijk voor front-end ontwikkelaars.
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html) - Bevat definities van componenten en dialoogvensters, sluit gecompileerde CSS en JavaScript in als clientbibliotheken.
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html) - bevat structurele inhoud en configuraties zoals bewerkbare sjablonen, metagegevensschema&#39;s (/content, /conf).

* **alles** - dit is een lege Maven module die de bovengenoemde modules in één enkel pakket combineert dat aan een AEM milieu kan worden opgesteld.

![Projectdiagram](assets/project-setup/project-pom-structure.png)

Zie de [AEM documentatie van het type Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) voor meer informatie over **alles** de Maven-modules.

### Opname van kerncomponenten {#core-components}

[AEM kerncomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) zijn een reeks gestandaardiseerde WCM-componenten (Web Content Management) voor AEM. Deze componenten bieden een basislijnset van een functionaliteit en zijn ontworpen om te worden vormgegeven, aangepast en uitgebreid voor afzonderlijke projecten.

AEM as a Cloud Service omgevingen zijn de nieuwste versie van [AEM kerncomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html). Daarom worden projecten die voor AEM as a Cloud Service worden gegenereerd **niet** omvat een insluiting van AEM Core Components.

Voor AEM 6.5/6.4 geproduceerde projecten sluit archetype automatisch in [AEM kerncomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) in het project. Het is aan te raden AEM 6.5/6.4 AEM Core Components in te sluiten om ervoor te zorgen dat de nieuwste versie wordt geïmplementeerd met uw project. Meer informatie over kerncomponenten [in het project te vinden](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html#core-components).

## Beheer van bronbeheer {#source-control}

Het is altijd een goed idee om een of andere vorm van broncontrole te gebruiken om de code in uw toepassing te beheren. Dit leerprogramma gebruikt git en GitHub. Er zijn verscheidene dossiers die door Geweven en/of winde van keus worden geproduceerd die door SCM zouden moeten worden genegeerd.

Maven maakt een doelmap wanneer u het codepakket maakt en installeert. De doelmap en -inhoud moeten worden uitgesloten van SCM.

Beneath `ui.apps` constateren dat velen `.content.xml` worden gemaakt. Deze XML-bestanden geven de knooppunttypen en eigenschappen weer van inhoud die in de JCR is geïnstalleerd. Deze bestanden zijn van essentieel belang en moeten **niet** worden genegeerd.

Het AEM project archetype zal een steekproef produceren `.gitignore` bestand dat kan worden gebruikt als beginpunt voor het veilig negeren van bestanden. Het bestand wordt gegenereerd op `<src>/aem-guides-wknd/.gitignore`.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, u hebt zojuist uw eerste AEM Project gemaakt!

### Volgende stappen {#next-steps}

De onderliggende technologie van een Adobe Experience Manager (AEM) Sites Component begrijpen via een eenvoudige `HelloWorld` met de [Basisbeginselen van componenten](component-basics.md) zelfstudie.

## Geavanceerde Maven-opdrachten (Bonus) {#advanced-maven-commands}

Tijdens ontwikkeling kunt u met enkel één van de modules werken en wilt vermijden bouw het volledige project om tijd te besparen. U kunt ook rechtstreeks aan een AEM publiceren instantie of misschien aan een geval van AEM willen opstellen die niet op haven 4502 loopt.

Vervolgens bekijken we enkele extra Maven-profielen en -opdrachten die u tijdens de ontwikkeling kunt gebruiken voor meer flexibiliteit.

### Kernmodule {#core-module}

De **[kern](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html)** bevat alle code van Java verbonden aan het project. Wanneer gebouwd stelt het een bundel OSGi aan AEM op. Alleen deze module samenstellen:

1. Navigeer in de `core` map (onder `aem-guides-wknd`):

   ```shell
   $ cd core/
   ```

1. Voer de volgende opdracht uit:

   ```shell
   $ mvn clean install -PautoInstallBundle
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   ```

1. Navigeren naar [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). Dit is de console OSGi van het Web en bevat informatie over alle bundels die op de AEM instantie worden geïnstalleerd.

1. Schakelen tussen **Id** sorteer de kolom en u moet zien welke WKND-bundel is geïnstalleerd en actief is.

   ![Basisbundel](assets/project-setup/wknd-osgi-console.png)

1. U kunt de &#39;fysieke&#39; locatie van de pot zien in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar):

   ![CRXDE Locatie van de ar](assets/project-setup/jcr-bundle-location.png)

### Ui.apps en Ui.content-modules {#apps-content-module}

De **[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)** de gemaskeerde module bevat alle renderingcode die nodig is voor de onderliggende site `/apps`. Dit omvat CSS/JS die in een AEM genoemd formaat zal worden opgeslagen [clientlibs](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html). Dit omvat ook [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/overview.html) scripts voor het renderen van dynamische HTML. Je kunt denken aan de **ui.apps** als een kaart aan de structuur in JCR maar in een formaat dat op een dossiersysteem kan worden opgeslagen en aan broncontrole wordt geëngageerd. De **ui.apps** bevat alleen code.

Om enkel deze module te bouwen:

1. Via de opdrachtregel. Navigeer in de `ui.apps` map (onder `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. Voer de volgende opdracht uit:

   ```shell
   $ mvn clean install -PautoInstallPackage
   ...
   Package installed in 122ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.972 s
   [INFO] Finished at: 2019-12-06T14:44:12-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Navigeren naar [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). U moet de `ui.apps` het pakket als het eerste geïnstalleerde pakket bevat en het moet een recentere tijdstempel hebben dan een van de andere pakketten.

   ![Ui.apps-pakket geïnstalleerd](assets/project-setup/ui-apps-package.png)

1. Ga terug naar de opdrachtregel en voer de volgende opdracht uit (binnen de `ui.apps` map):

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

   Het profiel `autoInstallPackagePublish` is bedoeld om het pakket te implementeren in een publicatieomgeving die op de poort wordt uitgevoerd **4503**. De bovenstaande fout wordt verwacht als er geen AEM op http://localhost:4503 wordt uitgevoerd.

1. Tot slot stel het volgende bevel in werking om op te stellen `ui.apps` pakket op poort **4504**:

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

   Opnieuw wordt een bouwstijlmislukking verwacht als geen AEM instantie die op haven loopt **4504** is beschikbaar. De parameter `aem.port` is gedefinieerd in het POM-bestand op `aem-guides-wknd/pom.xml`.

De **[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)** module is op dezelfde manier gestructureerd als de **ui.apps** module. Het enige verschil is dat **ui.content** module bevat wat bekend staat als **veranderlijk** inhoud. **Mutable** de inhoud verwijst hoofdzakelijk naar niet codeconfiguraties zoals Malplaatjes, Beleid, of omslagstructuren die in bron-controle worden opgeslagen **maar** kan rechtstreeks op een AEM worden gewijzigd. Dit zal in het hoofdstuk over Pagina&#39;s en Malplaatjes veel gedetailleerder worden onderzocht.

Dezelfde Maven-opdrachten die worden gebruikt om de **ui.apps** kan worden gebruikt om de **ui.content** module. U kunt de bovenstaande stappen vanuit de **ui.content** map.

## Problemen oplossen

Als u kwesties hebt die het project produceren gebruikend het AEM Archetype van het Project zie de lijst van [bekende problemen](https://github.com/adobe/aem-project-archetype#known-issues) en lijst van [kwesties](https://github.com/adobe/aem-project-archetype/issues).

## Nogmaals gefeliciteerd! {#congratulations-bonus}

Gefeliciteerd met het feit dat u de bonusinformatie hebt doorlopen.

### Volgende stappen {#next-steps-bonus}

De onderliggende technologie van een Adobe Experience Manager (AEM) Sites Component begrijpen via een eenvoudige `HelloWorld` met de [Basisbeginselen van componenten](component-basics.md) zelfstudie.
