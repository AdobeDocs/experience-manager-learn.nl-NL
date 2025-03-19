---
title: Ontwikkelingshulpmiddelen instellen voor AEM as a Cloud Service-ontwikkeling
description: Stel een lokale ontwikkelcomputer in met alle basisgereedschappen die nodig zijn om zich te ontwikkelen in vergelijking met AEM.
feature: Developer Tools
version: Cloud Service
jira: KT-4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
duration: 3508
source-git-commit: b865156776865b1155af7c7f3bd234bd337be796
workflow-type: tm+mt
source-wordcount: '1308'
ht-degree: 0%

---

# Ontwikkelingshulpmiddelen instellen {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="Gereedschappen voor ontwikkelingstools instellen"
>abstract="Voor de ontwikkeling van Adobe Experience Manager (AEM) is een minimale set ontwikkelingstools vereist die op de ontwikkelcomputer moet worden geïnstalleerd en ingesteld. Tot deze gereedschappen behoren Java, Maven, Adobe I/O CLI, Development IDE en meer."
>additional-url="https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines" text="Richtlijnen voor ontwikkeling"
>additional-url="https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk" text="Grondbeginselen van ontwikkeling"

Voor de ontwikkeling van Adobe Experience Manager (AEM) is een minimale set ontwikkelingstools vereist die op de ontwikkelcomputer moet worden geïnstalleerd en ingesteld. Deze instrumenten ondersteunen de ontwikkeling en de bouw van AEM-projecten.

`~` wordt gebruikt als steno voor de gebruikerslijst. In Windows is dit het equivalent van `%HOMEPATH%` .

## Java installeren

Experience Manager is een Java-toepassing en vereist daarom dat de Java SDK de ontwikkeling en de AEM as a Cloud Service SDK ondersteunt.

1. [ Download en installeer de recentste versie Java 11 SDK ](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2fx jcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Controleer of Oracle Java 11 SDK is geïnstalleerd met de opdracht:

>[!BEGINTABS]

>[!TAB  macOS ]

```shell
$ java --version
```

>[!TAB  Vensters ]

```shell
$ java -version
```

>[!TAB  Linux ]

```shell
$ java --version
```

>[!ENDTABS]

![ Java ](./assets/development-tools/java.png)

## Homebrew installeren

_het gebruik van Homebrew is facultatief, maar geadviseerd._

Homebrew is een opensource-pakketbeheer voor macOS, Windows en Linux. Alle ondersteunende hulpmiddelen kunnen afzonderlijk worden geïnstalleerd, verstrekt Homebrew een geschikte manier om een verscheidenheid van ontwikkelingshulpmiddelen te installeren en bij te werken die voor de ontwikkeling van Experience Manager worden vereist.

1. Open uw terminal
1. Controleer of Homebrew al is geïnstalleerd met de opdracht: `brew --version` .
1. Als Homebrew niet is geïnstalleerd, installeert u Homebrew

>[!BEGINTABS]

>[!TAB  macOS ]

[ Homebrew op macOS ](https://brew.sh/) vereist [ Xcode ](https://apps.apple.com/us/app/xcode/id497799835) of [ Hulpmiddelen van de Lijn van het Bevel ](https://developer.apple.com/download/more/), installeerbaar via het bevel:

```shell
$ xcode-select --install
```

>[!TAB  Vensters ]

[ installeer Homebrew op Vensters 10 ](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!TAB  Linux ]

[ installeer Homebrew op Linux ](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!ENDTABS]

1. Controleer of Homebrew is geïnstalleerd met de opdracht: `brew --version`

![ Homebrew ](./assets/development-tools/homebrew.png)

Als u Homebrew gebruikt, volg __installeer gebruikend 1} instructies Homebrew in de hieronder secties.__ Als u __niet__ gebruikend Homebrew bent, installeer de hulpmiddelen gebruikend OS-specifieke verbindingen.

## Installatiegit

[ Git ](https://git-scm.com/) is het bronbeheersysteem dat door [ Adobe Cloud Manager ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html) wordt gebruikt, en zo wordt vereist voor ontwikkeling.

>[!BEGINTABS]

>[!TAB  installeer Git gebruikend Homebrew ]

1. Open uw Eind/Herinnering van het Bevel
1. Voer de opdracht uit: `$ brew install git`
1. Controleer of Git is geïnstalleerd en gebruik de opdracht: `$ git --version`

>[!TAB  Git van de Download en installeert ]

1. [ Git van de Download en installeert ](https://git-scm.com/downloads)
1. Open uw Eind/Herinnering van het Bevel
1. Controleer of Git is geïnstalleerd en gebruik de opdracht: `$ git --version`

>[!ENDTABS]

![ Git ](./assets/development-tools/git.png)

## Node.js (en npm) installeren{#node-js}

[ Node.js ](https://nodejs.org) is een runtime van JavaScript milieu dat wordt gebruikt om met de front-end activa van het project van AEM te werken __ui.frontend__ subproject. Node.js wordt gedistribueerd met [ npm ](https://www.npmjs.com/), is de facto het pakketmanager Node.js, die wordt gebruikt om de gebiedsdelen van JavaScript te beheren.

>[!BEGINTABS]

>[!TAB  installeer Node.js gebruikend Homebrew ]

1. Open uw Eind/Herinnering van het Bevel
1. Voer de opdracht uit: `$ brew install node`
1. Verifieer Node.js geïnstalleerd is, gebruikend het bevel: `$ node -v`
1. Controleer of npm is geïnstalleerd en gebruik de opdracht: `$ npm -v`

>[!TAB  Download en installeer Node.js ]

1. [ Download en installeer Node.js ](https://nodejs.org/en/download/)
2. Open uw Eind/Herinnering van het Bevel
3. Verifieer Node.js geïnstalleerd is, gebruikend het bevel: `$ node -v`
4. Controleer of npm is geïnstalleerd en gebruik de opdracht: `$ npm -v`

>[!ENDTABS]

![ Node.js en npm ](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>](https://github.com/adobe/aem-project-archetype) Gebaseerde Projecten van AEM van het Project van AEM 1} installeren een geïsoleerde versie van Node.js in bouwstijltijd. [ Het is goed om de versie van het lokale ontwikkelingssysteem synchroon (of dicht bij) te houden de versies Node.js en npm die in de Reactor pom.xml van uw AEM Maven project worden gespecificeerd.
>
>Zie dit voorbeeld [ van de Reactor van het Project van AEM pom.xml ](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) waar te om van Node.js en npm versie te bepalen bouwen.

## Gemaakt installeren

Apache Maven is het opensource Java-opdrachtregelprogramma dat wordt gebruikt om AEM-projecten te bouwen die zijn gegenereerd via het AEM Project Maven Archetype. Al belangrijke winde ([ IntelliJ IDEA ](https://www.jetbrains.com/idea/), [ Code van Visual Studio ](https://code.visualstudio.com/), [ Verduistering ](https://www.eclipse.org/), enz.) heeft GeMaven steun geïntegreerd.


>[!BEGINTABS]

>[!TAB  installeer Gemaakt gebruikend Homebrew ]

1. Open uw Eind/Herinnering van het Bevel
1. Voer de opdracht uit: `$ brew install maven`
1. Verifieer Maven is geïnstalleerd, gebruikend het bevel: `$ mvn -v`

>[!TAB  Download en installeer Gemaakt ]

1. [ Gemaakte Download ](https://maven.apache.org/download.cgi)
1. [ installeer Gemaakt ](https://maven.apache.org/install.html)
1. Open uw Eind/Herinnering van het Bevel
1. Verifieer Maven is geïnstalleerd, gebruikend het bevel: `$ mvn -v`

>[!ENDTABS]

![ Gemaakt ](./assets/development-tools/maven.png)

## Adobe I/O CLI instellen{#aio-cli}

[ Adobe I/O CLI ](https://github.com/adobe/aio-cli), of `aio`, verleent de toegang van de bevellijn tot een verscheidenheid van de diensten van Adobe, met inbegrip van [ Cloud Manager ](https://github.com/adobe/aio-cli-plugin-cloudmanager) en [ Asset Compute ](https://github.com/adobe/aio-cli-plugin-asset-compute). De Adobe I/O CLI speelt een integrale rol in ontwikkeling op AEM as a Cloud Service aangezien het ontwikkelaars de mogelijkheid biedt om:

+ Logbestanden van AEM als Cloud Services-services tikken
+ Cloud Manager-pijpleidingen beheren vanuit de CLI
+ Stel aan [ de Snelle Milieu&#39;s van de Ontwikkeling van AEM ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html) op

### Adobe I/O CLI installeren

1. Verzeker [ Node.js wordt geïnstalleerd ](#node-js) aangezien Adobe I/O CLI een npm module is
   + Voer `node --version` uit om te bevestigen
1. `npm install -g @adobe/aio-cli` uitvoeren om de `aio` npm-module globaal te installeren

### De Adobe I/O CLI Cloud Manager-insteekmodule instellen{#aio-cloud-manager}

Met de Adobe I/O Cloud Manager-insteekmodule kan de AIR CLI communiceren met Adobe Cloud Manager via de opdracht `aio cloudmanager` .

1. Voer `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` uit om het [ insteekmodule van AIR ](https://github.com/adobe/aio-cli-plugin-cloudmanager) te installeren.

#### Adobe I/O CLI-verificatie instellen

Opdat Adobe I/O CLI met Cloud Manager communiceert, moet de integratie van a [ Cloud Manager in de Console van Adobe I/O ](https://github.com/adobe/aio-cli-plugin-cloudmanager) worden gecreeerd, en de geloofsbrieven moeten worden verkregen om met succes voor authentiek te verklaren.

1. Login aan [ console.adobe.io ](https://console.adobe.io)
1. Zorg ervoor dat uw organisatie die het Cloud Manager-product bevat waarmee verbinding moet worden gemaakt, actief is in de Adobe Organization-switch
1. Creeer nieuw of open een bestaand [ programma van Adobe I/O ](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Adobe I/O Console-projecten zijn eenvoudigweg organisatorische groepen van integratie, maken of gebruiken en bestaande projecten op basis van hoe u uw integratie wilt beheren.
   + Als u een nieuw project maakt, selecteert u &quot;Leeg project&quot; indien hierom wordt gevraagd (vs. &quot;Maken van sjabloon&quot;)
   + Adobe I/O Console-programma&#39;s zijn verschillende concepten voor Cloud Manager-programma&#39;s
1. Een nieuwe Cloud Manager API-integratie maken
   + Selecteer het type &#39;Oauth Server-to-server&#39; referentie.
   + Selecteer het productprofiel &quot;Deployment Manager - Cloud Service&quot;.
   + geconfigureerde API opslaan
1. Verkrijg de geloofsbrieven moeten Adobe I/O CLI [ config.json ](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication) bevolken door de pas gecreëerde &quot;Server-aan-server&quot;geloofsbrieven van OAuth te openen, en &quot;Download JSON&quot;van de hoogste juiste actiebar te selecteren.
1. Open het gedownloade JSON-bestand en geef alle toetsen een andere naam om te verkleinen. `CLIENT_ID` wordt bijvoorbeeld `client_id` .
1. Het `config.json` -bestand in de Adobe I/O CLI laden
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager /path/to/downloaded/json --file --json`

Begin [ uitvoerend bevelen ](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) voor Cloud Manager via Adobe I/O CLI.

### De insteekmodule AEM Rapid Development Environment instellen{#rde}

De insteekmodule van het Milieu van de Ontwikkelomgeving van AEM Rapid staat de interface CLI toe om met AEM as a Cloud Service [ in wisselwerking te staan Snelle Milieu&#39;s van de Ontwikkeling ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html) via het `aio aem:rde` bevel.

1. Voer `aio plugins:install @adobe/aio-cli-plugin-aem-rde` uit om de [ insteekmodule van de Milieu&#39;s van de Snelle Ontwikkeling van AEM ](https://github.com/adobe/aio-cli-plugin-aem-rde) te installeren.

### De Adobe I/O CLI Asset Compute-insteekmodule instellen{#aio-asset-compute}

Met de Adobe I/O Cloud Manager-insteekmodule kan de AIR CLI Asset Compute-workers genereren en uitvoeren via de opdracht `aio asset-compute` .

1. Voer `aio plugins:install @adobe/aio-cli-plugin-asset-compute` uit om het [ insteekmodule van AIR ](https://github.com/adobe/aio-cli-plugin-asset-compute) te installeren.

## De ontwikkelings-IDE instellen

AEM-ontwikkeling bestaat voornamelijk uit de ontwikkeling van Java en Front-end (JavaScript, CSS, enz.) en XML-beheer. Hieronder vindt u de meest gebruikte IDE&#39;s voor AEM-ontwikkeling.

### IntelliJ IDEA

__[IntelliJ IDEA ](https://www.jetbrains.com/idea/)__ is krachtige winde voor de ontwikkeling van Java. IntelliJ IDEA bestaat uit twee smaken, een gratis editie van de Gemeenschap en een commerciële (betaalde) Ultimate-versie. De vrije versie van de Gemeenschap is voldoende voor de ontwikkeling van AEM, nochtans breidt Ultimate [ zijn vermogensreeks ](https://www.jetbrains.com/idea/download) uit.

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

+ [ Download IntelliJ IDEA ](https://www.jetbrains.com/idea/download)
+ [ Download het hulpmiddel van de Repo ](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio-code

__[Code van Visual Studio ](https://code.visualstudio.com/)__ (de Code van VS) is een vrij, open-bronhulpmiddel voor front-end ontwikkelaars. De Code van Visual Studio kan opstelling zijn om inhoudssynchronisatie met AEM met hulp van een hulpmiddel van Adobe te integreren, __[repo ](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

De Code van Visual Studio is de ideale keus voor front-end ontwikkelaars hoofdzakelijk die front-end code creëren; JavaScript, CSS en HTML. Terwijl de Code van VS de steun van Java via [ uitbreidingen ](https://code.visualstudio.com/docs/java/java-tutorial) heeft, kan het sommige geavanceerde eigenschappen missen die door meer Java-specifiek worden verstrekt.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [ Code van Visual Studio van de Download {](https://code.visualstudio.com/Download)
+ [ Download het hulpmiddel van de Repo ](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [ Download geaemfed de uitbreiding van de Code van VS ](https://aemfed.io/)
+ [ Download AEM Sync VS de uitbreiding van de Code ](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[winde van de Verduistering ](https://www.eclipse.org/ide/)__ is populaire IDEs voor de ontwikkeling van Java, en steunt het __[AEM ontwikkelaarshulpmiddel ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)__ verstrekt door Adobe, verstrekkend een in-IDE GUI voor creatie en om inhoud JCR met een lokale instantie van AEM te synchroniseren.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [ Eclipse van de Download ](https://www.eclipse.org/ide/)
+ [ Eclipse Dev Tools van de Download {](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)
