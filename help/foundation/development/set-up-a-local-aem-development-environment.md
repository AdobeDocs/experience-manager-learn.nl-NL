---
title: Een ontwikkelomgeving voor lokale AEM instellen
description: 'Leer hoe u een lokale ontwikkelomgeving instelt voor Experience Manager. Ga vertrouwd met lokale installatie, Apache Maven, geïntegreerde ontwikkelomgevingen en foutopsporing en probleemoplossing. Eclipse IDE van het gebruik, CRXDE-Lite, de Code van Visual Studio, en IntelliJ. '
version: 6.4, 6.5
feature: Developer Tools
topics: development
activity: develop
audience: developer
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
source-git-commit: fb4a39a7b057ca39bc4cd4a7bce02216c3eb634c
workflow-type: tm+mt
source-wordcount: '2550'
ht-degree: 0%

---

# Een lokale AEM ontwikkelomgeving instellen

Handleiding voor het opzetten van een lokale ontwikkeling voor Adobe Experience Manager, AEM. Omvat belangrijke onderwerpen van lokale installatie, Apache Maven, geïntegreerde ontwikkelomgevingen en het zuiveren/het oplossen van problemen. Ontwikkeling met **[!DNL Eclipse IDE], [!DNL CRXDE Lite], [!DNL Visual Studio Code] en[!DNL IntelliJ]** worden besproken.

## Overzicht

Het opzetten van een lokale ontwikkelomgeving is de eerste stap bij het ontwikkelen voor Adobe Experience Manager of AEM. Neem de tijd om een ontwikkelomgeving voor kwaliteit in te stellen om uw productiviteit te verhogen en betere code te schrijven, sneller. We kunnen een AEM lokale ontwikkelingsomgeving onderverdelen in vier gebieden:

* Lokale AEM
* [!DNL Apache Maven] project
* Geïntegreerde ontwikkelomgevingen (IDE)
* Problemen oplossen

## Lokale AEM installeren

Wanneer we verwijzen naar een lokale AEM, hebben we het over een kopie van Adobe Experience Manager die wordt uitgevoerd op de persoonlijke machine van een ontwikkelaar. ***Alles*** AEM ontwikkeling zou moeten beginnen door code tegen een lokale AEM instantie te schrijven en in werking te stellen.

Als u nog geen ervaring hebt met AEM, kunt u twee standaardmodi installeren: ***Auteur*** en ***Publiceren***. De ***Auteur*** [runmode](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html)  Dit is de omgeving die digitale marketers zullen gebruiken om inhoud te maken en te beheren. Bij het ontwikkelen **meest** van de tijd u code aan een instantie van de Auteur zult opstellen. Hierdoor kunt u nieuwe pagina&#39;s maken en componenten toevoegen en configureren. AEM Sites is een WYSIWYG-ontwerpend CMS en daarom kunnen de meeste CSS en JavaScript worden getest op basis van een ontwerpinstantie.

Het is ook *kritisch* testcode tegen een lokale ***Publiceren*** -instantie. De ***Publiceren*** instantie is de AEM omgeving waarmee bezoekers van uw website zullen communiceren. Terwijl de ***Publiceren*** instantie is dezelfde technologiestapel als de ***Auteur*** instantie, zijn er één of ander belangrijk onderscheid met configuraties en toestemmingen. Code *altijd* worden getest op een lokale ***Publiceren*** -instantie voordat deze wordt bevorderd tot omgevingen op een hoger niveau.

### Stappen

1. Zorgen [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) is geïnstalleerd.
   * Voorkeur [Java JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list p.offset=0&amp;p.limit=14) voor AEM 6.5+
   * [Java JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8) voor AEM versies vóór AEM 6.5
2. Hiermee wordt een kopie van het dialoogvenster [AEM QuickStart Jar en a [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware).
3. Maak als volgt een mapstructuur op uw computer:

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. De naam van de [!DNL QuickStart] JAR naar ***aem-auteur-p4502.jar*** en plaatst deze onder de `/author` directory. Voeg de ***[!DNL license.properties]*** bestand onder het `/author` directory.
5. Maak een kopie van het dialoogvenster [!DNL QuickStart] JAR, naam wijzigen in ***aem-publish-p4503.jar*** en plaatst deze onder de `/publish` directory. Een kopie van het dialoogvenster toevoegen ***[!DNL license.properties]*** bestand onder het `/publish` directory.

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. Dubbelklik op de knop ***aem-auteur-p4502.jar*** te installeren **Auteur** -instantie. Hiermee wordt de instantie van de auteur gestart en wordt deze op de poort uitgevoerd **4502** op de lokale computer.

   Dubbelklik op de knop ***aem-publish-p4503.jar*** te installeren **Publiceren** -instantie. Hiermee wordt de instantie Publiceren gestart, die op de poort wordt uitgevoerd **4503** op de lokale computer.

   >[!NOTE]
   >
   >Afhankelijk van de hardware van uw ontwikkelcomputer kan het moeilijk zijn om zowel een **Auteur en publicatie** -instantie die tegelijkertijd wordt uitgevoerd. Zelden moet u allebei gelijktijdig op een lokale opstelling in werking stellen.

   Zie voor meer informatie [Een AEM-instantie implementeren en onderhouden](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html).

## Apache Maven installeren

***[!DNL Apache Maven]*** is een hulpmiddel om de bouwstijl te beheren en procedure voor op Java-Gebaseerde projecten op te stellen. AEM is een Java-platform en [!DNL Maven] is de standaardmanier om code voor een AEM project te beheren. Wanneer we zeggen ***AEM Maven Project*** of alleen uw ***AEM project***, verwijzen wij naar een Maven-project dat alle *aangepast* code voor uw site.

Alle AEM projecten moeten worden opgebouwd uit de meest recente versie van de **[!DNL AEM Project Archetype]**: [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). De [!DNL AEM Project Archetype] zal tot een laarzentrekker van een AEM project met wat steekproefcode en inhoud leiden. De [!DNL AEM Project Archetype] omvat ook **[!DNL AEM WCM Core Components]** geconfigureerd voor gebruik op uw project.

>[!CAUTION]
>
>Wanneer het beginnen van een nieuw project is het beste praktijken om de recentste versie van archetype te gebruiken. Houd in mening dat er veelvoudige versies van archetype zijn en niet alle versies compatibel met vroegere versies van AEM.

### Stappen

1. Downloaden [Apache Maven](https://maven.apache.org/download.cgi)
2. Installeren [Apache Maven](https://maven.apache.org/install.html) en zorg ervoor dat de installatie aan uw bevellijn is toegevoegd `PATH`.
   * [!DNL macOS] gebruikers kunnen Maven installeren met [Homebrew](https://brew.sh/)
3. Controleren of **[!DNL Maven]** wordt geïnstalleerd door een nieuwe terminal van de bevellijn te openen en het volgende uit te voeren:

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
   > In het verleden is de toevoeging van `adobe-public` Gemaakt profiel is vereist voor punt `nexus.adobe.com` om AEM artefacten te downloaden. Alle AEM artefacten zijn nu beschikbaar via Maven Central en de `adobe-public` is niet nodig.

## Een geïntegreerde ontwikkelomgeving instellen

Een geïntegreerde ontwikkelomgeving of IDE is een toepassing waarin een teksteditor, syntaxisondersteuning en buildtools worden gecombineerd. Afhankelijk van het type van ontwikkeling u doet, zou één winde boven een andere kunnen verkiezen. Ongeacht IDE, zal het belangrijk zijn om periodiek te kunnen ***duwen*** code naar een lokale AEM-instantie om deze te testen. Het zal ook belangrijk zijn om af en toe ***trekken*** configuraties van een lokale AEM instantie in uw AEM project om aan een bron-controle beheerssysteem zoals Git voort te zetten.

Hieronder staan een paar populairdere IDEs die met AEM ontwikkeling met overeenkomstige video&#39;s worden gebruikt die de integratie met een lokale AEM instantie tonen.

>[!NOTE]
>
> Het WKND-project is standaard bijgewerkt om aan AEM as a Cloud Service te werken. Het is bijgewerkt om [achterwaarts compatibel met 6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). Indien u AEM 6.5 of 6.4 gebruikt, voegt u de `classic` aan om het even welke Gemaakt bevelen.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Wanneer het gebruiken van winde gelieve te controleren `classic` op het tabblad Geweven profiel.

![Tabblad Geweven profiel](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Maven Profile*

### [!DNL Eclipse] IDE

De **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** is een van de populairste IDE&#39;s voor Java-ontwikkeling, grotendeels omdat het een open-source is en ***vrij***! Adobe beschikt over een plug-in, **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html)**, for [!DNL Eclipse] om gemakkelijker ontwikkeling met een aardige GUI toe te staan om code met een lokale AEM instantie te synchroniseren. De [!DNL Eclipse] IDE wordt geadviseerd voor ontwikkelaars nieuw om grotendeels te AEM wegens GUI steun door [!DNL AEM Developer Tools].

#### Installatie en installatie

1. Download en installeer de [!DNL Eclipse] IDE voor [!DNL Java EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. Volg de instructies om de [!DNL AEM Developer Tools] insteekmodule: [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Geweven project importeren
* 01:24 - Bouw en stel broncode met Maven op
* 04:33 - Pushcodewijzigingen met AEM Developer Tool
* 10:55 - Draai codewijzigingen door AEM Developer Tool
* 13:12 - De geïntegreerde foutopsporingsgereedschappen van Eclipse gebruiken

### IntelliJ IDEA

De **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** is een krachtige IDE voor professionele ontwikkeling van Java. [!DNL IntelliJ IDEA] komt voor in twee kleveringen, a ***vrij*** [!DNL Community] editie en een handelsversie (betaald) [!DNL Ultimate] versie. De gratis [!DNL Community] versie van [!DNL IntellIJ IDEA] is voldoende voor meer AEM ontwikkeling, maar [!DNL Ultimate] [breidt zijn vermogensreeks uit](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. Download en installeer de [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. Installeren [!DNL Repo] (opdrachtregelgereedschap): [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 - Maven-project importeren
* 05:47 - Bouw en stel broncode met Maven op
* 08:17 - Push changes with Repo
* 14:39 - Pull changes with Repo
* 17:25 - Het gebruiken van de geïntegreerde het zuiveren hulpmiddelen van IntelliJ IDEA

### [!DNL Visual Studio Code]

**[Visual Studio-code](https://code.visualstudio.com/)** is al snel een favoriete tool geworden voor ***front-end ontwikkelaars*** met verbeterde JavaScript-ondersteuning, [!DNL Intellisense]en ondersteuning voor browserfoutopsporing. **[!DNL Visual Studio Code]** is opensource, gratis, met veel krachtige extensies. [!DNL Visual Studio Code] kan worden opgezet om met AEM te integreren met behulp van een Adobe-instrument, **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** Er zijn ook verschillende door de gemeenschap ondersteunde extensies die kunnen worden geïnstalleerd om met AEM te integreren.

[!DNL Visual Studio Code] is een goede keuze voor front-end ontwikkelaars die vooral CSS/LESS- en JavaScript-code schrijven om AEM clientbibliotheken te maken. Dit hulpmiddel kan niet de beste keus voor nieuwe AEM ontwikkelaars zijn aangezien de knoopdefinities (dialogen, componenten) allen in ruwe XML zullen moeten uitgeven. Er zijn verschillende Java-extensies beschikbaar voor [!DNL Visual Studio Code], echter in de eerste plaats bij het ontwikkelen van Java [!DNL Eclipse IDE] of [!DNL IntelliJ] heeft de voorkeur.

#### Belangrijke koppelingen

* [**Downloaden**](https://code.visualstudio.com/Download) **Visual Studio-code**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - FTP-achtig gereedschap voor JCR-inhoud
* **[geaëerd](https://aemfed.io/)** - Verhoog uw AEM front-end workflow
* **[Synchroniseren](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Door de Gemeenschap ondersteund&#42; uitbreiding voor de Code van Visual Studio

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Geweven project importeren
* 00:53 - bouw en stel broncode met Maven op
* 04:03 - Push code changes with Repo command line tool
* 08:29 - Pull code changes with Repo command line tool
* 10:40 - Push code changes with aemfed tool
* 14:24 - Problemen oplossen, Clientbibliotheken opnieuw samenstellen

### [!DNL CRXDE Lite]

[CRXDE Lite](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) is een browserweergave van de AEM repository. [!DNL CRXDE Lite] is ingesloten in AEM en stelt een ontwikkelaar in staat standaardontwikkelingstaken uit te voeren, zoals het bewerken van bestanden, het definiëren van componenten, dialoogvensters en sjablonen. [!DNL CRXDE Lite] is ***niet*** bedoeld om een volledige ontwikkelomgeving te zijn, maar zeer effectief als foutopsporingsprogramma. [!DNL CRXDE Lite] is nuttig wanneer het uitbreiden van of eenvoudig begrip van productcode buiten uw codebasis. [!DNL CRXDE Lite] biedt een krachtige weergave van de opslagplaats en een manier om machtigingen effectief te testen en te beheren.

[!DNL CRXDE Lite] zou altijd samen met andere IDEs moeten worden gebruikt om code te testen en te zuiveren maar nooit als primair ontwikkelingshulpmiddel. Er is beperkte syntaxisondersteuning, geen automatische functionaliteit en beperkte integratie met bronbeheersystemen.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Problemen oplossen

***Help!*** Mijn code werkt niet! Net als bij alle andere ontwikkelingen zullen er tijden zijn (waarschijnlijk veel) waarin uw code niet naar behoren werkt. AEM is een krachtig platform, maar met veel kracht... komt tot grote complexiteit. Hieronder volgen een paar basispunten op hoog niveau voor het oplossen van problemen en het volgen van problemen (maar ver van een volledige lijst van dingen die verkeerd kunnen gaan):

### Codeimplementatie verifiëren

Een goede eerste stap, wanneer het ontmoeten van een kwestie moet verifiëren dat de code met succes aan AEM is opgesteld en geïnstalleerd.

1. **Controleren[!UICONTROL Package Manager]** om ervoor te zorgen dat het codepakket is geüpload en geïnstalleerd: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Controleer de tijdstempel om te controleren of het pakket onlangs is geïnstalleerd.
1. Als u incrementele bestandsupdates uitvoert met een gereedschap als [!DNL Repo] of [!DNL AEM Developer Tools], **controleren[!DNL CRXDE Lite]** of het bestand naar de lokale AEM-instantie is geduwd en of de bestandsinhoud wordt bijgewerkt: [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **Controleren of de bundel is geüpload** als u problemen met Java-code ziet in een OSGi-bundel. Open de [!UICONTROL Adobe Experience Manager Web Console]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) en zoek naar uw bundel. Zorg ervoor dat de bundel een **[!UICONTROL Active]** status. Zie hieronder voor meer informatie over het oplossen van problemen met een bundel in een **[!UICONTROL Installed]** status.

#### Logbestanden controleren

AEM is een chatplatform en registreert veel nuttige informatie in de **error.log**. De **error.log** kan worden gevonden waar AEM is geïnstalleerd: &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

Een handige techniek voor het bijhouden van problemen is het toevoegen van loginstructies in uw Java-code:

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

Standaard worden de **error.log** is gevormd aan logboek *[!DNL INFO]* instructies. Als u het logboekniveau wilt veranderen kunt u dit doen door te gaan [!UICONTROL Log Support]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). U kunt ook vaststellen dat de **error.log** is te chagrijnig. U kunt de [!UICONTROL Log Support] om logboekverklaringen voor enkel een gespecificeerd pakket van Java te vormen. Dit is een beste praktijk voor projecten, om de kwesties van de douanecode van OTB AEM platformkwesties gemakkelijk te scheiden.

![Logconfiguratie in AEM](./assets/set-up-a-local-aem-development-environment/logging.png)

#### Bundel bevindt zich in de status Geïnstalleerd {#bundle-active}

Alle bundels (met uitzondering van fragmenten) moeten zich in een **[!UICONTROL Active]** status. Als u de codebundel in een [!UICONTROL Installed] er is een probleem dat moet worden opgelost . Meestal is dit een afhankelijkheidskwestie:

![Bundelfout in AEM](assets/set-up-a-local-aem-development-environment/bundle-error.png)

In het bovenstaande scherm neemt u de [!DNL WKND Core bundle] is een [!UICONTROL Installed] status. Dit komt omdat de bundel een andere versie van `com.adobe.cq.wcm.core.components.models` dan beschikbaar is op de AEM.

Een handig gereedschap dat u kunt gebruiken is de [!UICONTROL Dependency Finder]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). Voeg de naam van het Java-pakket toe om te controleren welke versie beschikbaar is op het AEM:

![Kernonderdelen](assets/set-up-a-local-aem-development-environment/core-components.png)

Als u doorgaat met het bovenstaande voorbeeld, kunt u zien dat de versie die op de AEM is geïnstalleerd, **12,2** vs **12,6** dat de bundel had verwacht. Daar kun je achterwaarts werken en zien of de [!DNL Maven] afhankelijkheden van AEM komen overeen met de [!DNL Maven] afhankelijkheden in het AEM project. In het bovenstaande voorbeeld [!DNL Core Components] **v2.2.0** is geïnstalleerd op de AEM instantie maar de codebundel is gebouwd met een afhankelijkheid van **v2.2.2**, vandaar de reden voor het probleem van de afhankelijkheid.

#### Registratie van verkoopmodellen verifiëren {#osgi-component-sling-models}

AEM componenten moeten altijd worden ondersteund door een [!DNL Sling Model] om bedrijfslogica in te kapselen en ervoor te zorgen dat het HTML teruggevende manuscript schoon blijft. Als er problemen optreden waarbij het verkoopmodel niet kan worden gevonden, is het handig om de [!DNL Sling Models] vanaf de console: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Dit zal u vertellen als uw het Verkopen Model is geregistreerd en welk middeltype (de componentenweg) het aan verbonden is.

![Status van verkoopmodel](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Hiermee wordt de registratie van een [!DNL Sling Model], `BylineImpl` dat is gekoppeld aan een type componentbron van `wknd/components/content/byline`.

#### CSS- of JavaScript-problemen

Voor de meeste problemen met CSS en JavaScript is het gebruik van de ontwikkelingstools van de browser de meest effectieve manier om problemen op te lossen. Om het probleem bij het ontwikkelen tegen een AEM instantie van de auteur te beperken, is het nuttig om de pagina &quot;zoals Gepubliceerd&quot;te bekijken.

![Problemen met CSS of JS](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Open de [!UICONTROL Page Properties] menu en klik op [!UICONTROL View as Published]. Hiermee wordt de pagina geopend zonder de AEM-editor en met een queryparameter ingesteld op **wcmmode=disabled**. Dit zal effectief de AEM auteursUI onbruikbaar maken en het oplossen van problemen/het zuiveren front-end kwesties veel gemakkelijker maken.

Een ander vaak ondervonden probleem bij het ontwikkelen van front-end code is oud of verouderd CSS/JS wordt geladen. Als eerste stap moet u ervoor zorgen dat de browsergeschiedenis is gewist en zo nodig een incognitobrowser of een nieuwe sessie starten.

#### Fouten opsporen in clientbibliotheken

Met verschillende methoden van categorieën en insluitingen om meerdere clientbibliotheken op te nemen, kan het lastig zijn problemen op te lossen. AEM stelt verschillende hulpmiddelen beschikbaar om hierbij te helpen. Een van de belangrijkste instrumenten is [!UICONTROL Rebuild Client Libraries] waardoor AEM alle LESS-bestanden opnieuw moet compileren en de CSS moet genereren.

* [Stompe lampen](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Hiermee worden alle clientbibliotheken weergegeven die in de AEM zijn geregistreerd. &lt;host>/libs/granite/ui/content/dumplibs.html
* [Uitvoer testen](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - stelt een gebruiker in staat de verwachte HTML-uitvoer van clientlib te zien op basis van categorie. &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [Validatie van bibliotheekafhankelijke onderdelen](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - Hiermee worden afhankelijkheden of ingesloten categorieën gemarkeerd die niet zijn gevonden. &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [Client-bibliotheken opnieuw samenstellen](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - kan een gebruiker de AEM dwingen alle clientbibliotheken opnieuw samen te stellen of de cache van clientbibliotheken ongeldig te maken. Dit hulpmiddel is vooral effectief wanneer het ontwikkelen met LESS aangezien dit AEM kan dwingen om geproduceerde CSS opnieuw te compileren. Over het algemeen is het effectiever om de Pagina&#39;s ongeldig te maken en dan een pagina uit te voeren verfrist zich tegenover het herbouwen van alle bibliotheken. &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![Fouten opsporen in Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Als u constant het geheime voorgeheugen moet invalideren gebruikend [!UICONTROL Rebuild Client Libraries] kan het de moeite waard zijn om een eenmalige heropbouw van alle clientbibliotheken uit te voeren. Dit kan ongeveer 15 minuten in beslag nemen, maar in de toekomst worden problemen met het in cache plaatsen meestal opgelost.
