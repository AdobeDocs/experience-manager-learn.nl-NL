---
title: Een ontwikkelomgeving voor lokale AEM instellen
description: Leer hoe u een lokale ontwikkelomgeving instelt voor Experience Manager. Ga vertrouwd met lokale installatie, Apache Maven, geïntegreerde ontwikkelomgevingen en foutopsporing en probleemoplossing. Eclipse IDE van het gebruik, CRXDE-Lite, de Code van Visual Studio, en IntelliJ.
version: 6.5
feature: Developer Tools
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
last-substantial-update: 2022-07-20T00:00:00Z
doc-type: Tutorial
thumbnail: aem-local-dev-env.jpg
duration: 4537
source-git-commit: d731a7131b997fa272013e8d62aa2251e25c08e4
workflow-type: tm+mt
source-wordcount: '2394'
ht-degree: 0%

---

# Een lokale AEM ontwikkelomgeving instellen

Handleiding voor het opzetten van een lokale ontwikkeling voor Adobe Experience Manager, AEM. Omvat belangrijke onderwerpen van lokale installatie, Apache Maven, geïntegreerde ontwikkelomgevingen en het zuiveren/het oplossen van problemen. De ontwikkeling met **winde van de Verduistering, CRXDE Lite, de Code van Visual Studio, en IntelliJ** wordt besproken.

## Overzicht

Het opzetten van een lokale ontwikkelomgeving is de eerste stap bij het ontwikkelen voor Adobe Experience Manager of AEM. Neem de tijd om een ontwikkelomgeving voor kwaliteit in te stellen om uw productiviteit te verhogen en betere code te schrijven, sneller. We kunnen een AEM lokale ontwikkelingsomgeving in vier gebieden verdelen:

* Lokale AEM
* [!DNL Apache Maven] project
* Geïntegreerde ontwikkelomgevingen (IDE)
* Problemen oplossen

## Lokale AEM installeren

Wanneer we naar een lokale AEM verwijzen, hebben we het over een kopie van Adobe Experience Manager die op de persoonlijke machine van een ontwikkelaar draait. ***allen*** AEM ontwikkeling zou moeten beginnen door code tegen een lokale AEM instantie te schrijven en in werking te stellen.

Als u nieuw aan AEM bent, zijn er twee basis in werking gestelde wijzen kan worden geïnstalleerd: ***Auteur*** en ***Publish***. De ***Auteur*** [ runmode ](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/configure-runmodes.html?lang=en) is het milieu dat de digitale marketers gebruiken om inhoud tot stand te brengen en te beheren. Wanneer het ontwikkelen van het grootste deel van de tijd, stelt u code aan een instantie van de Auteur op. Zo kunt u pagina&#39;s maken en componenten toevoegen en configureren. AEM Sites is een WYSIWYG-ontwerpversie van CMS en daarom kunnen de meeste CSS en JavaScript worden getest op basis van een ontwerpinstantie.

Het is ook *kritieke* testcode tegen een lokale ***Publish*** instantie. De ***Publish*** instantie is het AEM milieu dat de bezoekers aan uw website met in wisselwerking staan. Terwijl ***Publish*** instantie de zelfde technologiestapel zoals de ***3} instantie van de Auteur {is, zijn er één of ander belangrijk onderscheid met configuraties en toestemmingen.*** De code moet tegen een lokale ***Publish*** instantie worden getest alvorens aan milieu&#39;s op hoger niveau wordt bevorderd.

### Stappen

1. Zorg ervoor dat Java™ is geïnstalleerd.
   * Voorkeur [ Java™ JDK 11 ](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list p.offset=0&amp;p.limit=14) voor AEM 6.5+
   * [ Java™ JDK 8 ](https://www.oracle.com/java/technologies/downloads/) voor AEM versies vóór AEM 6.5
1. Krijg een exemplaar van [ AEM Jar QuickStart en a  [!DNL license.properties] ](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html).
1. Maak als volgt een mapstructuur op uw computer:

```plain
~/aem-sdk
    /author
    /publish
```

1. Wijzig de naam van [!DNL QuickStart] JAR aan ***aem-auteur-p4502.jar*** en plaats het onder de `/author` folder. Voeg het bestand ***[!DNL license.properties]*** toe onder de map `/author` .

1. Maak een exemplaar van [!DNL QuickStart] JAR, noem het aan ***aem-publish-p4503.jar*** anders en plaats het onder de `/publish` folder. Voeg een kopie van het ***[!DNL license.properties]*** -bestand toe onder de map `/publish` .

```plain
~/aem-sdk
    /author
        + aem-author-p4502.jar
        + license.properties
    /publish
        + aem-publish-p4503.jar
        + license.properties
```

1. Dubbelklik het {***dossier 0} aem-auteur-p4502.jar om de **instantie van de Auteur**te installeren.*** Dit begint de auteursinstantie, die op haven **4502** op de lokale computer loopt.

Dubbelklik het {***dossier 0} aem-publish-p4503.jar om de **Publish**instantie te installeren.*** Dit begint de instantie van Publish, lopend op haven **4503** op de lokale computer.

>[!NOTE]
>
>Afhankelijk van de hardware van uw ontwikkelingsmachine, kan het moeilijk zijn om zowel een **Auteur als de 1} instantie te hebben die van Publish tezelfdertijd loopt.** Zelden moet u allebei gelijktijdig op een lokale opstelling in werking stellen.

### Opdrachtregel gebruiken

U kunt ook dubbelklikken op het JAR-bestand door AEM te starten vanaf de opdrachtregel of een script te maken (`.bat` of `.sh` ), afhankelijk van de lokale systeemsmaak. Hieronder ziet u een voorbeeld van de voorbeeldopdracht:

```shell
$ java -Xmx2048M -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=30303 -jar aem-author-p4502.jar -gui -r"author,localdev"
```

Hier, zijn `-X` opties JVM en `-D` zijn extra kadereigenschappen, voor meer informatie, zie [ het Opstellen en het Onderhouden van een AEM instantie ](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html) en [ Verdere opties beschikbaar van het dossier van QuickStart ](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/custom-standalone-install.html#further-options-available-from-the-quickstart-file).

## Apache Maven installeren

***[!DNL Apache Maven]*** is een hulpmiddel om de bouwstijl te beheren en procedure voor op Java-Gebaseerde projecten op te stellen. AEM is een op Java gebaseerd platform en [!DNL Maven] is de standaardmanier om code voor een AEM project te beheren. Wanneer wij ***AEM Gemaakt Project*** of enkel uw ***AEM Project*** zeggen, verwijzen wij naar een Geweven project dat alle *douane* code voor uw plaats omvat.

Alle AEM Projecten zouden van de recentste versie van **[!DNL AEM Project Archetype]** moeten worden gebouwd: [ https://github.com/adobe/aem-project-archetype ](https://github.com/adobe/aem-project-archetype). [!DNL AEM Project Archetype] verstrekt een laarzentrekker van een AEM project met wat steekproefcode en inhoud. [!DNL AEM Project Archetype] omvat ook **[!DNL AEM WCM Core Components]** dat wordt gevormd om op uw project te worden gebruikt.

>[!CAUTION]
>
>Wanneer het beginnen van een nieuw project, is het beste praktijken om de recentste versie van archetype te gebruiken. Houd in mening dat er veelvoudige versies van archetype zijn en niet alle versies compatibel met vroegere versies van AEM.

### Stappen

1. Download [ Apache Maven ](https://maven.apache.org/download.cgi)
2. Installeer [ Apache Maven ](https://maven.apache.org/install.html) en zorg ervoor dat de installatie aan uw bevel-lijn `PATH` is toegevoegd.
   * [!DNL macOS] de gebruikers kunnen Gemaakt installeren gebruikend [ Homebrew ](https://brew.sh/)
3. Controleer of **[!DNL Maven]** is geïnstalleerd door een nieuwe opdrachtregelterminal te openen en het volgende uit te voeren:

```shell
$ mvn --version
Apache Maven 3.3.9
Maven home: /Library/apache-maven-3.3.9
Java version: 1.8.0_111, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
```

>[!NOTE]
>
> In het verleden moest het profiel `adobe-public` Maven zijn toegevoegd om naar `nexus.adobe.com` te wijzen om AEM artefacten te downloaden. Alle AEM artefacten zijn nu beschikbaar via Maven Central en het profiel `adobe-public` is niet nodig.

## Een geïntegreerde ontwikkelomgeving instellen

Een geïntegreerde ontwikkelomgeving of IDE is een toepassing waarin een teksteditor, syntaxisondersteuning en build-tools worden gecombineerd. Afhankelijk van het type van ontwikkeling u doet, zou één winde boven een andere kunnen verkiezen. Ongeacht winde, is het belangrijk om ***druk*** code aan een lokale AEM instantie periodiek te kunnen {om het te testen. Het is belangrijk om af en toe ****** configuraties van een lokale AEM instantie in uw AEM project te trekken om aan een bron-controle beheerssysteem zoals Git voort te zetten.

Hieronder staan een paar populairdere IDEs die met AEM ontwikkeling met overeenkomstige video&#39;s worden gebruikt die de integratie met een lokale AEM instantie tonen.

>[!NOTE]
>
> Het WKND-project is standaard bijgewerkt om te werken op AEM as a Cloud Service. Het is bijgewerkt om [ achterwaarts compatibel met 6.5/6.4 ](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx) te zijn. Als u AEM 6.5 of 6.4 gebruikt, voegt u het `classic` -profiel toe aan Maven-opdrachten.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Wanneer u een IDE gebruikt, moet u `classic` controleren op het tabblad Geweven profiel.

![ Gemaakt Lusje van het Profiel ](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Geweven Profiel*

### [!DNL Eclipse] IDE

**[[!DNL Eclipse] winde ](https://www.eclipse.org/ide/)** is één van populairdere IDEs voor ontwikkeling Java™, in het grootste deel omdat het open bron en ***vrij*** is! Adobe verstrekt een stop, **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)**, voor [!DNL Eclipse] om gemakkelijkere ontwikkeling met een aardige GUI toe te staan om code met een lokale AEM instantie te synchroniseren. De [!DNL Eclipse] IDE wordt aanbevolen voor ontwikkelaars die niet voor een groot deel AEM vanwege de GUI-ondersteuning door [!DNL AEM Developer Tools] .

#### Installatie en installatie

1. Download en installeer [!DNL Eclipse] winde voor [!DNL Java™ EE Developers]: [ https://www.eclipse.org ](https://www.eclipse.org/)
1. Volg de instructies om de [!DNL AEM Developer Tools] stop te installeren: [ https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Geweven project importeren
* 01:24 - Bouw en stel broncode met Maven op
* 04:33 - Pushcodewijzigingen met AEM Developer Tool
* 10:55 - Draai codewijzigingen door AEM Developer Tool
* 13:12 - De geïntegreerde foutopsporingsgereedschappen van Eclipse gebruiken

### IntelliJ IDEA

**[IntelliJ IDEA ](https://www.jetbrains.com/idea/)** is krachtige winde voor professionele ontwikkeling Java™. [!DNL IntelliJ IDEA] komt in twee aroma&#39;s, a ***vrije*** [!DNL Community] uitgave en een commerciële (betaalde) [!DNL Ultimate] versie. De vrije [!DNL Community] versie van [!DNL IntellIJ IDEA] is voldoende voor meer AEM ontwikkeling, nochtans [!DNL Ultimate] [ breidt zijn vermogensreeks ](https://www.jetbrains.com/idea/download) uit.

#### [!DNL Installation and Setup]

1. Download en installeer [!DNL IntelliJ IDEA]: [ https://www.jetbrains.com/idea/download ](https://www.jetbrains.com/idea/download)
1. Installeer [!DNL Repo] (bevel-lijn hulpmiddel): [ https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo ](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

* 00:00 - Maven-project importeren
* 05:47 - Bouw en stel broncode met Maven op
* 08:17 - Push changes with Repo
* 14:39 - Pull changes with Repo
* 17:25 - Het gebruiken van de geïntegreerde het zuiveren hulpmiddelen van IntelliJ IDEA

### [!DNL Visual Studio Code]

**[Code van Visual Studio ](https://code.visualstudio.com/)** is snel een favoriet hulpmiddel voor ***front-end ontwikkelaars*** met de verbeterde steun van JavaScript, [!DNL Intellisense], en browser het zuiveren steun geworden. **[!DNL Visual Studio Code]** is opensource, gratis, met veel krachtige extensies. [!DNL Visual Studio Code] kan opstelling worden om met AEM met behulp van een hulpmiddel van de Adobe, **[repo ](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code) te integreren.** Er zijn ook verschillende door de gemeenschap ondersteunde extensies die kunnen worden geïnstalleerd om met AEM te integreren.

[!DNL Visual Studio Code] is een goede keuze voor front-end ontwikkelaars die vooral CSS/LESS en JavaScript-code schrijven om AEM clientbibliotheken te maken. Dit hulpmiddel kan niet de beste keus voor nieuwe AEM ontwikkelaars zijn aangezien de knoopdefinities (dialogen, componenten) in ruwe XML moeten worden uitgegeven. Er zijn verschillende Java™-extensies beschikbaar voor [!DNL Visual Studio Code] , maar als de voorkeur uitgaat naar het ontwikkelen van Java™ [!DNL Eclipse IDE] of [!DNL IntelliJ] .

#### Belangrijke koppelingen

* [**Download** ](https://code.visualstudio.com/Download) **Code van Visual Studio**
* **[repo ](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - FTP-als hulpmiddel voor inhoud JCR
* **[geaemfed ](https://aemfed.io/)** - versneller uw AEM front-end werkschema
* **[AEM Synchronisatie ](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Communautaire gesteunde &#42; uitbreiding voor de Code van Visual Studio
* **[WKND project ](https://github.com/adobe/aem-guides-wknd)** - Voorbeeld AEM project dat in deze video wordt getoond.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Geweven project importeren
* 00:53 - bouw en stel broncode met Maven op
* 04:03 - Push code changes with Repo command-line tool
* 08:29 - Pull code changes with Repo command-line tool
* 10:40 - Push code changes with aemfed tool
* 14:24 - Problemen oplossen, Clientbibliotheken opnieuw samenstellen

### [!DNL CRXDE Lite]

[ CRXDE Lite ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/developing-with-crxde-lite.html) is een op browser-gebaseerde mening van de AEM bewaarplaats. [!DNL CRXDE Lite] is ingesloten in AEM en biedt een ontwikkelaar de mogelijkheid om standaardontwikkelingstaken uit te voeren, zoals het bewerken van bestanden, het definiëren van componenten, dialoogvensters en sjablonen. [!DNL CRXDE Lite] is ***niet*** bedoeld om een volledig ontwikkelomgeving te zijn maar is efficiënt als het zuiveren hulpmiddel. [!DNL CRXDE Lite] is nuttig wanneer het uitbreiden van of eenvoudig begrip van productcode buiten uw codebasis. [!DNL CRXDE Lite] biedt een krachtige weergave van de opslagplaats en een manier om machtigingen effectief te testen en te beheren.

[!DNL CRXDE Lite] zou met andere IDEs moeten worden gebruikt om code te testen en te zuiveren maar nooit als primair ontwikkelingshulpmiddel. Er is beperkte syntaxisondersteuning, geen automatische functionaliteit en beperkte integratie met bronbeheersystemen.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Problemen oplossen

***Hulp!*** Mijn code werkt niet! Net als bij alle ontwikkelingen zijn er tijden (waarschijnlijk veel) waarin uw code niet naar behoren werkt. AEM is een krachtig platform, maar met grote kracht.. komt er grote complexiteit. Hieronder vindt u een aantal uitgangspunten op hoog niveau voor het oplossen van problemen en het opsporen van problemen (maar ver van een volledige lijst met dingen die fout kunnen gaan):

### Codeimplementatie verifiëren

Een goede eerste stap, wanneer het ontmoeten van een kwestie moet verifiëren dat de code met succes aan AEM is opgesteld en geïnstalleerd.

1. **Controle[!UICONTROL Package Manager]** om ervoor te zorgen dat het codepakket is geupload en geïnstalleerd: [ http://localhost:4502/crx/packmgr/index.jsp ](http://localhost:4502/crx/packmgr/index.jsp). Controleer de tijdstempel om te controleren of het pakket onlangs is geïnstalleerd.
1. Als het doen van stijgende dossierupdates gebruikend een hulpmiddel zoals [!DNL Repo] of [!DNL AEM Developer Tools], **controle[!DNL CRXDE Lite]** dat het dossier aan de lokale AEM instantie is geduwd en dat de dossierinhoud wordt bijgewerkt: [ http://localhost:4502/crx/de/index.jsp ](http://localhost:4502/crx/de/index.jsp)
1. **Controle dat de bundel** wordt geupload als het zien van kwesties met betrekking tot code Java™ in een bundel OSGi. Open [!UICONTROL Adobe Experience Manager Web Console]: [ http://localhost:4502/system/console/bundles ](http://localhost:4502/system/console/bundles) en onderzoek naar uw bundel. Zorg ervoor dat de bundel de status **[!UICONTROL Active]** heeft. Zie hieronder voor meer informatie over het oplossen van problemen met een bundel in een **[!UICONTROL Installed]** status.

#### Logbestanden controleren

AEM is een praatjesplatform en registreert nuttige informatie in **error.log**. **error.log** kan worden gevonden waar AEM is geïnstalleerd: &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

Een handige techniek voor het bijhouden van problemen is het toevoegen van loginstructies in uw Java™-code:

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
...

public class MyClass {
    private final Logger log = LoggerFactory.getLogger(getClass());

    ...

    String myVariable = "My Variable";

    log.debug("Debug statement of myVariable {}", myVariable);

    log.info("Info statement of myVariable {}", myVariable);
}
```

Door gebrek **error.log** wordt gevormd om *[!DNL INFO]* verklaringen te registreren. Als u het logboekniveau wilt veranderen, kunt u dit doen door [!UICONTROL Log Support] te gaan: [ http://localhost:4502/system/console/slinglog ](http://localhost:4502/system/console/slinglog). U kunt ook vinden dat **error.log** te praatje is. Met de [!UICONTROL Log Support] kunt u loginstructies configureren voor alleen een opgegeven Java™-pakket. Dit is een beste praktijk voor projecten, om de kwesties van de douanecode van OTB AEM platformkwesties gemakkelijk van elkaar te scheiden.

![ het Registreren configuratie in AEM ](./assets/set-up-a-local-aem-development-environment/logging.png)

#### Bundel bevindt zich in de status Geïnstalleerd {#bundle-active}

Alle bundels (met uitzondering van fragmenten) moeten de status **[!UICONTROL Active]** hebben. Als u de codebundel in een [!UICONTROL Installed] staat ziet, dan is er een kwestie die moet worden opgelost. Meestal is dit een afhankelijkheidskwestie:

![ de Fout van de Bundel in AEM ](assets/set-up-a-local-aem-development-environment/bundle-error.png)

In de bovenstaande schermafbeelding is [!DNL WKND Core bundle] een [!UICONTROL Installed] -status. Dit komt omdat de bundel een andere versie van `com.adobe.cq.wcm.core.components.models` verwacht dan beschikbaar is op de AEM instantie.

Een nuttig hulpmiddel dat kan worden gebruikt is [!UICONTROL Dependency Finder]: [ http://localhost:4502/system/console/depfinder ](http://localhost:4502/system/console/depfinder). Voeg de naam van het Java™-pakket toe om te controleren welke versie beschikbaar is op het AEM:

![ Componenten van de Kern ](assets/set-up-a-local-aem-development-environment/core-components.png)

Voortdurend met het bovenstaande voorbeeld, kunnen wij zien dat de versie die op de AEM instantie wordt geïnstalleerd **12.2** vs **12.6** is dat de bundel verwachtte. Daarna kunt u achterwaarts werken en zien of de [!DNL Maven] -afhankelijkheden van AEM overeenkomen met de [!DNL Maven] -afhankelijkheden in het AEM-project. In, is het bovenstaande voorbeeld [!DNL Core Components] **v2.2.0** geïnstalleerd op de AEM instantie maar de codebundel werd gebouwd met een gebiedsdeel op **v2.2.2**, vandaar de reden voor de gebiedsdeelkwestie.

#### Registratie van verkoopmodellen verifiëren {#osgi-component-sling-models}

AEM componenten moeten door een [!DNL Sling Model] worden gesteund om om het even welke bedrijfslogica in te kapselen en ervoor te zorgen dat het HTML teruggevende manuscript schoon blijft. Als het ervaren van kwesties waar het het Verdelen Model niet kan worden gevonden, kan het nuttig zijn om [!DNL Sling Models] van de console te controleren: [ http://localhost:4502/system/console/status-slingmodels ](http://localhost:4502/system/console/status-slingmodels). Dit vertelt u als uw het Verkopen Model is geregistreerd en welk middeltype (de componentenweg) het aan verbonden is.

![ Sling Model Status ](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Toont de registratie van een [!DNL Sling Model] `BylineImpl` die aan een componentenmiddeltype van `wknd/components/content/byline` gebonden is.

#### Problemen met CSS of JavaScript

Voor de meeste problemen met CSS en JavaScript is het gebruik van de ontwikkelingstools van de browser de meest effectieve manier om problemen op te lossen. Als u het probleem wilt beperken bij het ontwikkelen tegen een AEM instantie van de auteur, is het handig om de pagina &quot;zoals gepubliceerd&quot; weer te geven.

![ CSS of JS Kwesties ](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Open het menu [!UICONTROL Page Properties] en klik op [!UICONTROL View as Published] . Dit opent de pagina zonder de AEMRedacteur en met een vraagparameter die aan **wordt geplaatst wcmmode=disabled**. Dit maakt effectief de AEM auteursUI onbruikbaar en maakt het oplossen van problemen/het zuiveren front-end kwesties veel gemakkelijker.

Een ander vaak ondervonden probleem bij het ontwikkelen van front-end code is oud of verouderd CSS/JS wordt geladen. Als eerste stap moet u ervoor zorgen dat de browsergeschiedenis is gewist en zo nodig een incognitobrowser of een nieuwe sessie starten.

#### Fouten opsporen in clientbibliotheken

Met de verschillende methodes van categorieën en bedden om veelvoudige cliëntbibliotheken te omvatten kan het lastig zijn om problemen op te lossen. AEM stelt verschillende hulpmiddelen beschikbaar om hierbij te helpen. Een van de belangrijkste gereedschappen is [!UICONTROL Rebuild Client Libraries] , waarmee AEM alle LESS-bestanden opnieuw moet compileren en de CSS moet genereren.

* [ de Libben van de Reliëf ](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - maakt een lijst van alle cliëntbibliotheken die in de AEM instantie worden geregistreerd. &lt;host>/libs/granite/ui/content/dumplibs.html
* [ Output van de Test ](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - staat een gebruiker toe om de verwachte HTML output van clientlib te zien omvat gebaseerd op categorie. &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [ de bevestiging van de Afhankelijkheden van Bibliotheken ](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - benadrukt om het even welke gebiedsdelen of ingebedde categorieën die niet kunnen worden gevonden. &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [ herbouwt de Bibliotheken van de Cliënt ](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - staat een gebruiker toe om AEM te dwingen om alle cliëntbibliotheken te herbouwen of het geheime voorgeheugen van cliëntbibliotheken ongeldig te maken. Dit gereedschap is effectief bij het ontwikkelen met LESS, omdat dit AEM ertoe kan dwingen de gegenereerde CSS opnieuw te compileren. Over het algemeen is het effectiever om de cache ongeldig te maken en vervolgens een pagina te vernieuwen in plaats van alle bibliotheken opnieuw samen te stellen. &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![ het Zuiveren Clientlibs ](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Als u de cache voortdurend moet invalideren met het gereedschap [!UICONTROL Rebuild Client Libraries] , kan het nuttig zijn om alle clientbibliotheken opnieuw samen te stellen. Dit kan ongeveer 15 minuten in beslag nemen, maar in de toekomst worden problemen met het in cache plaatsen meestal opgelost.
