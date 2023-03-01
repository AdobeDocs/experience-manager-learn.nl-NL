---
title: De ontwikkelingsinstrumenten voor AEM as a Cloud Service ontwikkeling instellen
description: Stel een lokale ontwikkelcomputer in met alle basislijngereedschappen die nodig zijn om zich te ontwikkelen in AEM lokale omgeving.
feature: Developer Tools
version: Cloud Service
kt: 4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
source-git-commit: e82c30e7f1a1fe04fd43ee639d74788f9bf100f6
workflow-type: tm+mt
source-wordcount: '1502'
ht-degree: 0%

---

# Ontwikkelingshulpmiddelen instellen {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="Gereedschappen voor ontwikkelingstools instellen"
>abstract="Voor de ontwikkeling van Adobe Experience Manager (AEM) is een minimale set ontwikkelingstools vereist die op de ontwikkelcomputer moet worden geïnstalleerd en ingesteld. Deze hulpmiddelen omvatten Java, Maven, Adobe I/O CLI, Ontwikkeling IDE en meer."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="Richtlijnen voor ontwikkeling"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="Grondbeginselen van ontwikkeling"

Voor de ontwikkeling van Adobe Experience Manager (AEM) is een minimale set ontwikkelingstools vereist die op de ontwikkelcomputer moet worden geïnstalleerd en ingesteld. Deze instrumenten ondersteunen de ontwikkeling en de bouw van AEM projecten.

Let op: `~` wordt gebruikt als steno voor de Folder van de Gebruiker. In Windows is dit het equivalent van `%HOMEPATH%`.

## Java installeren

Experience Manager is een Java-toepassing en vereist daarom dat de Java SDK de ontwikkeling en de AEM as a Cloud Service SDK ondersteunt.

1. [Download en installeer de nieuwste versie van Java 11 SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2 Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Controleer of de Java 11 SDK is geïnstalleerd met de opdracht:
   + Windows: `java -version`
   + macOS / Linux: `java --version`

![Java](./assets/development-tools/java.png)

## Homebrew installeren

_Het gebruik van Homebrew is optioneel, maar wordt wel aanbevolen._

Homebrew is een opensource-pakketbeheer voor macOS, Windows en Linux. Alle ondersteunende hulpmiddelen kunnen afzonderlijk worden geïnstalleerd, verstrekt Homebrew een geschikte manier om een verscheidenheid van ontwikkelingshulpmiddelen te installeren en bij te werken die voor de ontwikkeling van de Experience Manager worden vereist.

1. Open uw terminal
1. Controleer of Homebrew al is geïnstalleerd door de opdracht uit te voeren: `brew --version`.
1. Als Homebrew niet is geïnstalleerd, installeert u Homebrew
   + [Homebrew installeren op macOS](https://brew.sh/)
      + Voor Homebrew op macOS is vereist [Xcode](https://apps.apple.com/us/app/xcode/id497799835) of [Command Line Tools](https://developer.apple.com/download/more/), installeerbaar via het bevel:
         + `xcode-select --install`
   + [Homebrew installeren op Linux](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [Homebrew installeren in Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. Controleer of Homebrew is geïnstalleerd door de opdracht uit te voeren: `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

Als u Homebrew gebruikt, volgt u de instructies __Installeren met Homebrew__ instructies in de onderstaande secties. Als u __niet__ Gebruik Homebrew om de gereedschappen te installeren met behulp van de OS-specifieke koppelingen.

## Installatiegit

[Git](https://git-scm.com/) is het bronbeheersysteem dat wordt gebruikt door [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html), en dus noodzakelijk voor ontwikkeling.

+ Git installeren met Homebrew
   1. Open uw Eind/Herinnering van het Bevel
   1. Voer het bevel uit: `brew install git`
   1. Controleer of Git is geïnstalleerd met de opdracht: `git --version`
+ Of download en installeer Git (macOS, Linux of Windows)
   1. [Git downloaden en installeren](https://git-scm.com/downloads)
   1. Open uw Eind/Herinnering van het Bevel
   1. Controleer of Git is geïnstalleerd met de opdracht: `git --version`

![Git](./assets/development-tools/git.png)

## Node.js (en npm) installeren{#node-js}

[Node.js](https://nodejs.org) is een JavaScript-runtimeomgeving die wordt gebruikt voor het werken met de front-end elementen van de AEM __ui.frontend__ subproject. Node.js wordt gedistribueerd met [npm](https://www.npmjs.com/)is de defacto Node.js-pakketbeheerder die wordt gebruikt om JavaScript-afhankelijkheden te beheren.

+ Node.js installeren met Homebrew
   1. Open uw Eind/Herinnering van het Bevel
   1. Voer het bevel uit: `brew install node`
   1. Verifieer Node.js geïnstalleerd is, gebruikend het bevel: `node -v`
   1. Verifieer npm geïnstalleerd is, gebruikend het bevel: `npm -v`
+ Of download en installeer Node.js (macOS, Linux of Windows)
   1. [Node.js downloaden en installeren](https://nodejs.org/en/download/)
   1. Open uw Eind/Herinnering van het Bevel
   1. Verifieer Node.js geïnstalleerd is, gebruikend het bevel: `node -v`
   1. Verifieer npm geïnstalleerd is, gebruikend het bevel: `npm -v`

![Node.js en npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[Projectarchetype AEM](https://github.com/adobe/aem-project-archetype)Op -gebaseerde AEM Projecten installeren een geïsoleerde versie van Node.js in bouwstijltijd. Het is goed om de versie van het lokale ontwikkelingssysteem in synchronisatie (of dicht bij) Node.js en npm versies te houden die in AEM Maven de Reactor pom.xml van het Project van de Reactor wordt gespecificeerd.
>
>Zie dit voorbeeld [AEM Projectreactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) voor waar te om van Node.js de plaats te bepalen en npm bouwt versies.

## Gemaakt installeren

Apache Maven is het opensource Java-opdrachtregelprogramma dat wordt gebruikt om AEM Projecten te bouwen die zijn gegenereerd van het AEM Project Maven Archetype. Alle belangrijke IDE&#39;s ([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Visual Studio-code](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/), enz.) geïntegreerde ondersteuning voor Maven.

+ Maven installeren met Homebrew
   1. Open uw Eind/Herinnering van het Bevel
   1. Voer het bevel uit: `brew install maven`
   1. Verifieer Maven geïnstalleerd is, gebruikend het bevel: `mvn -v`
+ Of download en installeer Maven (macOS, Linux of Windows)
   1. [Gemaakt downloaden](https://maven.apache.org/download.cgi)
   1. [Gemaakt installeren](https://maven.apache.org/install.html)
   1. Open uw Eind/Herinnering van het Bevel
   1. Verifieer Maven geïnstalleerd is, gebruikend het bevel: `mvn -v`

![Maven](./assets/development-tools/maven.png)

## Adobe I/O CLI instellen{#aio-cli}

De [Adobe I/O CLI](https://github.com/adobe/aio-cli), of `aio`biedt opdrachtregeltoegang tot diverse Adobe-services, waaronder [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) en [asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). Adobe I/O CLI speelt een integrale rol in ontwikkeling op AEM as a Cloud Service aangezien het ontwikkelaars de capaciteit verleent om:

+ Logboeken van AEM als Cloud Services
+ Cloud Manager-pijpleidingen beheren vanuit de CLI
+ Distribueren naar [AEM snelle-ontwikkelomgevingen](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html)

### De Adobe I/O CLI installeren

1. Zorgen [Node.js is geïnstalleerd](#node-js) aangezien Adobe I/O CLI een npm module is
   + Uitvoeren `node --version` bevestigen
1. Uitvoeren `npm install -g @adobe/aio-cli` om de `aio` npm-module wereldwijd

### De plug-in Adobe I/O CLI Cloud Manager instellen{#aio-cloud-manager}

Met de insteekmodule Adobe I/O Cloud Manager kan de AIR CLI via de `aio cloudmanager` gebruiken.

1. Uitvoeren `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` om de [Insteekmodule AIR Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager).

#### De Adobe I/O CLI-verificatie instellen

Als u wilt dat de Adobe I/O CLI communiceert met Cloud Manager, kunt u [Integratie van Cloud Manager moet worden gemaakt in Adobe I/O Console](https://github.com/adobe/aio-cli-plugin-cloudmanager)en gegevens moeten zijn verkregen om te worden geverifieerd.

1. Aanmelden bij [console.adobe.io](https://console.adobe.io)
1. Zorg ervoor dat uw organisatie die het product Cloud Manager bevat waarmee verbinding moet worden gemaakt, actief is in de Adobe Org-switch
1. Een nieuwe of bestaande [Adobe I/O-programma](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + De programma&#39;s van de Console van Adobe I/O zijn eenvoudig organisatorische groeperingen van integratie, creeer of gebruik en bestaand programma gebaseerd op hoe u uw integratie wilt beheren
   + Als u een nieuw project maakt, selecteert u &quot;Leeg project&quot; indien hierom wordt gevraagd (vs. &quot;Maken van sjabloon&quot;)
   + Adobe I/O Console-programma&#39;s zijn verschillende concepten voor programma&#39;s van Cloud Manager
1. Maak een nieuwe API-integratie voor Cloud Manager met het profiel &quot;Developer - Cloud Service&quot;
1. Verkrijg de geloofsbrieven van de Rekening van de Dienst (JWT) moet Adobe I/O CLI bevolken [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)
1. Laad de `config.json` bestand in de Adobe I/O CLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager PATH_TO_CONFIG_JSON_FILE --file --json`
1. Laad de `private.key` bestand in de Adobe I/O CLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key PATH_TO_PRIVATE_KEY_FILE --file`

Beginnen [uitvoeren, opdrachten](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) voor Cloud Manager via de Adobe I/O CLI.

### De insteekmodule AEM Rapid Development Environment instellen{#rde}

Met de insteekmodule AEM Rapid Development Environment kan de AIR CLI communiceren met AEM as a Cloud Service [Snelle ontwikkelomgevingen](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html) via de `aio aem:rde` gebruiken.

1. Uitvoeren `aio plugins:install @adobe/aio-cli-plugin-aem-rde` om de [Insteekmodule AEM Rapid Development Environment](https://github.com/adobe/aio-cli-plugin-aem-rde).

### De Adobe I/O CLI-Asset compute insteekmodule instellen{#aio-asset-compute}

Met de insteekmodule Adobe I/O Cloud Manager kan de AIR CLI Asset compute-workers genereren en uitvoeren via de `aio asset-compute` gebruiken.

1. Uitvoeren `aio plugins:install @adobe/aio-cli-plugin-asset-compute` om de [Ao Asset compute plug-in](https://github.com/adobe/aio-cli-plugin-asset-compute).


## De ontwikkelings-IDE instellen

AEM ontwikkeling bestaat voornamelijk uit de ontwikkeling van Java en Front-end (JavaScript, CSS, enz.) en XML-beheer. Hieronder vindt u de populairste IDE&#39;s voor AEM ontwikkeling.

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ is een krachtige IDE voor de ontwikkeling van Java. IntelliJ IDEA bestaat uit twee smaken, een gratis editie van de Gemeenschap en een commerciële (betaalde) ultieme versie. De gratis versie van de Gemeenschap is voldoende voor AEM ontwikkeling, maar de Ultimate [breidt zijn vermogensreeks uit](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [IntelliJ IDEA downloaden](https://www.jetbrains.com/idea/download)
+ [Het gereedschap Repo downloaden](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio-code

__[Visual Studio-code](https://code.visualstudio.com/)__ (VS-code) is een gratis opensource-programma voor ontwikkelaars aan de voorkant. De Code van Visual Studio kan opstelling zijn om inhoudsync met AEM met behulp van een hulpmiddel van Adobe te integreren, __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

De Code van Visual Studio is de ideale keus voor front-end ontwikkelaars hoofdzakelijk die front-end code creëren; JavaScript, CSS en HTML. Terwijl VS-code Java-ondersteuning heeft via [extensions](https://code.visualstudio.com/docs/java/java-tutorial)bepaalde geavanceerde functies van meer Java-specifieke functies ontbreken.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Visual Studio-code downloaden](https://code.visualstudio.com/Download)
+ [Het gereedschap Repo downloaden](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Geëxporteerde VS-codeextensie downloaden](https://aemfed.io/)
+ [Download AEM Sync VS Code extension](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__ is een populaire IDEs voor de ontwikkeling van Java, en steunt  __[AEM Developer Tools](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html?lang=en)__ insteekmodule van Adobe, die een in-IDE GUI voor creatie verstrekt en JCR inhoud met een lokale AEM instantie synchroniseert.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Eclipse downloaden](https://www.eclipse.org/ide/)
+ [Eclipse Dev-gereedschappen downloaden](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html?lang=en)
