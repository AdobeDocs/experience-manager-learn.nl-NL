---
title: De ontwikkelingstools voor AEM instellen als een Cloud Service-ontwikkeling
description: Stel een lokale ontwikkelcomputer in met alle basislijngereedschappen die nodig zijn om zich te ontwikkelen in AEM lokale omgeving.
feature: Gereedschappen voor ontwikkelaars
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4267
thumbnail: 25907.jpg
topic: Ontwikkeling
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1428'
ht-degree: 0%

---


# Ontwikkelingshulpmiddelen instellen

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="Gereedschappen voor ontwikkelingstools instellen"
>abstract="Voor de ontwikkeling van Adobe Experience Manager (AEM) is een minimale set ontwikkelingstools vereist die op de ontwikkelcomputer moet worden geïnstalleerd en ingesteld. Deze hulpmiddelen omvatten Java, Maven, Adobe I/O CLI, Ontwikkeling IDE en meer."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="Richtlijnen voor ontwikkeling"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="Grondbeginselen van ontwikkeling"

Voor de ontwikkeling van Adobe Experience Manager (AEM) is een minimale set ontwikkelingstools vereist die op de ontwikkelcomputer moet worden geïnstalleerd en ingesteld. Deze instrumenten ondersteunen de ontwikkeling en de bouw van AEM projecten.

Merk op dat `~` als steno voor de Folder van de Gebruiker wordt gebruikt. In Windows is dit het equivalent van `%HOMEPATH%`.

## Java installeren

Experience Manager is een Java-toepassing en vereist daarom dat de Java SDK de ontwikkeling en de AEM als Cloud Service SDK ondersteunt.

1. [Download en installeer de nieuwste versie van Java 11 SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2 Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Controleer of de Java 11 SDK is geïnstalleerd met de opdracht:
   + Windows: `java -version`
   + macOS / Linux: `java --version`

![Java](./assets/development-tools/java.png)

## Homebrew installeren

_Het gebruik van Homebrew is optioneel, maar wordt wel aanbevolen._

Homebrew is een opensource-pakketbeheer voor MacOS, Windows en Linux. Alle ondersteunende hulpmiddelen kunnen afzonderlijk worden geïnstalleerd, verstrekt Homebrew een geschikte manier om een verscheidenheid van ontwikkelingshulpmiddelen te installeren en bij te werken die voor de ontwikkeling van de Experience Manager worden vereist.

1. Open uw terminal
1. Controleer of Homebrew al is geïnstalleerd door de opdracht uit te voeren: `brew --version`.
1. Als Homebrew niet is geïnstalleerd, installeert u Homebrew
   + [Homebrew installeren op macOS](https://brew.sh/)
      + Voor HomeBrew op macOS is [Xcode](https://apps.apple.com/us/app/xcode/id497799835) of [Opdrachtregelprogramma&#39;s](https://developer.apple.com/download/more/) vereist, die u kunt installeren via de opdracht:
         + `xcode-select --install`
   + [Homebrew installeren op Linux](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [Homebrew installeren in Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. Controleer of Homebrew is geïnstalleerd door de opdracht uit te voeren: `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

Als u Homebrew gebruikt, volgt u de __installatie met behulp van Homebrew__ instructies in de onderstaande secties. Als u __niet__ gebruikend Homebrew bent, installeer de hulpmiddelen gebruikend OS-specifieke verbindingen.

## Installatiegit

[](https://git-scm.com/) Bitis het bronbeheersysteem dat door  [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html) wordt gebruikt, en is dus vereist voor ontwikkeling.

+ Git installeren met Homebrew
   1. Open uw Eind/Herinnering van het Bevel
   1. Voer het bevel uit: `brew install git`
   1. Controleer of Git is geïnstalleerd met de opdracht: `git --version`
+ Of download en installeer Git (MacOS, Linux of Windows)
   1. [Git downloaden en installeren](https://git-scm.com/downloads)
   1. Open uw Eind/Herinnering van het Bevel
   1. Controleer of Git is geïnstalleerd met de opdracht: `git --version`

![Git](./assets/development-tools/git.png)

## Node.js (en npm) installeren{#node-js}

[Node.](https://nodejs.org) jsis een runtimeomgeving van JavaScript die wordt gebruikt om met de front-end activa van het  __ui.__ frontendsubproject van een AEM project te werken. Node.js wordt gedistribueerd met [npm](https://www.npmjs.com/), is de facto Node.js pakketmanager, die wordt gebruikt om JavaScript gebiedsdelen te beheren.

+ Node.js installeren met Homebrew
   1. Open uw Eind/Herinnering van het Bevel
   1. Voer het bevel uit: `brew install node`
   1. Verifieer Node.js geïnstalleerd is, gebruikend het bevel: `node -v`
   1. Verifieer npm geïnstalleerd is, gebruikend het bevel: `npm -v`
+ Of download en installeer Node.js (MacOS, Linux of Windows)
   1. [Node.js downloaden en installeren](https://nodejs.org/en/download/)
   1. Open uw Eind/Herinnering van het Bevel
   1. Verifieer Node.js geïnstalleerd is, gebruikend het bevel: `node -v`
   1. Verifieer npm geïnstalleerd is, gebruikend het bevel: `npm -v`

![Node.js en npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM Project archetype](https://github.com/adobe/aem-project-archetype)-Gebaseerde AEMProjecten installeren een geïsoleerde versie van Node.js bij bouwstijltijd. Het is goed om de versie van het lokale ontwikkelingssysteem in synchronisatie (of dicht bij) Node.js en npm versies te houden die in AEM Maven de Reactor pom.xml van het Project van de Reactor wordt gespecificeerd.
>
>Zie dit voorbeeld [AEM Projectreactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) voor waar te om van Node.js en npm bouwt versies de plaats te bepalen.

## Gemaakt installeren

Apache Maven is het opensource Java-opdrachtregelprogramma dat wordt gebruikt om AEM Projecten te bouwen die zijn gegenereerd van het AEM Project Maven Archetype. Alle belangrijke IDE&#39;s ([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Visual Studio Code](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/), enz.) geïntegreerde ondersteuning voor Maven.

+ Maven installeren met Homebrew
   1. Open uw Eind/Herinnering van het Bevel
   1. Voer het bevel uit: `brew install maven`
   1. Verifieer Maven geïnstalleerd is, gebruikend het bevel: `mvn -v`
+ Of download en installeer Maven (MacOS, Linux of Windows)
   1. [Gemaakt downloaden](https://maven.apache.org/download.cgi)
   1. [Gemaakt installeren](https://maven.apache.org/install.html)
   1. Open uw Eind/Herinnering van het Bevel
   1. Verifieer Maven geïnstalleerd is, gebruikend het bevel: `mvn -v`

![Maven](./assets/development-tools/maven.png)

## Adobe I/O CLI instellen{#aio-cli}

De [Adobe I/O CLI](https://github.com/adobe/aio-cli), of `aio`, verleent bevellijntoegang tot een verscheidenheid van de diensten van de Adobe, met inbegrip van [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) en [Asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). Adobe I/O CLI speelt een integrale rol in ontwikkeling op AEM als Cloud Service aangezien het ontwikkelaars de capaciteit verleent om:

+ Logboeken van AEM als Cloud Services
+ Cloud Manager-pijpleidingen beheren vanuit de CLI

### De Adobe I/O CLI installeren

1. Zorg ervoor dat [Node.js is geïnstalleerd](#node-js) omdat Adobe I/O CLI een npm-module is
   + `node --version` uitvoeren om te bevestigen
1. `npm install -g @adobe/aio-cli` uitvoeren om de `aio` npm module globaal te installeren

### De plug-in Adobe I/O CLI Cloud Manager instellen{#aio-cloud-manager}

Met de insteekmodule Adobe I/O Cloud Manager kan de AIR CLI communiceren met Adobe Cloud Manager via de opdracht `aio cloudmanager`.

1. `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` uitvoeren om de [insteekmodule voor AIR Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) te installeren.

### De Adobe I/O CLI-Asset compute insteekmodule instellen{#aio-asset-compute}

Met de insteekmodule Adobe I/O Cloud Manager kan de AIR CLI Asset compute-workers genereren en uitvoeren via de opdracht `aio asset-compute`.

1. `aio plugins:install @adobe/aio-cli-plugin-asset-compute` uitvoeren om de [Asset compute plug-in](https://github.com/adobe/aio-cli-plugin-asset-compute) te installeren.

### De Adobe I/O CLI-verificatie instellen

De Adobe I/O CLI kan alleen communiceren met Cloud Manager als er een [Cloud Manager-integratie is gemaakt in Adobe I/O Console](https://github.com/adobe/aio-cli-plugin-cloudmanager) en gegevens zijn verkregen voor verificatie.

1. Aanmelden bij [console.adobe.io](https://console.adobe.io)
1. Zorg ervoor dat uw organisatie die het product Cloud Manager bevat waarmee verbinding moet worden gemaakt, actief is in de Adobe Org-switch
1. Een nieuw programma maken of een bestaand [Adobe I/O-programma](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md) openen
   + De programma&#39;s van de Console van Adobe I/O zijn eenvoudig organisatorische groeperingen van integratie, creeer of gebruik en bestaand programma gebaseerd op hoe u uw integratie wilt beheren
   + Als u een nieuw project maakt, selecteert u &quot;Leeg project&quot; indien hierom wordt gevraagd (vs. &quot;Maken van sjabloon&quot;)
   + Adobe I/O Console-programma&#39;s zijn verschillende concepten voor programma&#39;s van Cloud Manager
1. Maak een nieuwe API-integratie voor Cloud Manager met het profiel &quot;Developer - Cloud Service&quot;
1. Verkrijg de geloofsbrieven van de Rekening van de Dienst (JWT) moet Adobe I/O CLI [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication) bevolken
1. Het `config.json`-bestand in de Adobe I/O CLI laden
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager PATH_TO_CONFIG_JSON_FILE --file --json`
1. Het `private.key`-bestand in de Adobe I/O CLI laden
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key PATH_TO_PRIVATE_KEY_FILE --file`

Start [het uitvoeren van opdrachten](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) voor Cloud Manager via de Adobe I/O CLI.

## De ontwikkelings-IDE instellen

AEM ontwikkeling bestaat voornamelijk uit de ontwikkeling van Java en Front-end (JavaScript, CSS, enz.) en XML-beheer. Hieronder vindt u de populairste IDE&#39;s voor AEM ontwikkeling.

### IntelliJ IDEA

__[IntelliJ ](https://www.jetbrains.com/idea/)__ IDEA is een krachtige IDE voor de ontwikkeling van Java. IntelliJ IDEA bestaat uit twee smaken, een gratis editie van de Gemeenschap en een commerciële (betaalde) ultieme versie. De gratis versie van de Gemeenschap is voldoende voor AEM ontwikkeling, maar de Ultimate [breidt zijn capaciteitenset](https://www.jetbrains.com/idea/download) uit.

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [IntelliJ IDEA downloaden](https://www.jetbrains.com/idea/download)
+ [Het gereedschap Repo downloaden](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio-code

__[Visual Studio Code](https://code.visualstudio.com/)__  (de Code van VS) is een vrij, open-bronhulpmiddel voor front-end ontwikkelaars. De Code van Visual Studio kan opstelling zijn om inhoudssynchronisatie met AEM met behulp van een hulpmiddel van de Adobe te integreren, __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

De Code van Visual Studio is de ideale keus voor front-end ontwikkelaars hoofdzakelijk die front-end code creëren; JavaScript, CSS en HTML. Terwijl de Code van VS steun van Java via [extensions](https://code.visualstudio.com/docs/java/java-tutorial) heeft, kan het sommige geavanceerde eigenschappen missen die door meer Java-specifiek worden verstrekt.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Visual Studio-code downloaden](https://code.visualstudio.com/Download)
+ [Het gereedschap Repo downloaden](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Geëxporteerde VS-codeextensie downloaden](https://aemfed.io/)
+ [Download AEM Sync VS Code extension](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse ](https://www.eclipse.org/ide/)__ IDE is een populaire IDEs voor ontwikkeling Java, en steunt   __[AEM ](https://eclipse.adobe.com/aem/dev-tools/)__ Toolsplug-in van de Ontwikkelaar die door Adobe wordt verstrekt, die een in-IDE GUI voor creatie verstrekken en JCR inhoud met een lokale AEM instantie synchroniseren.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Eclipse downloaden](https://www.eclipse.org/ide/)
+ [Eclipse Dev-gereedschappen downloaden](https://eclipse.adobe.com/aem/dev-tools/)
