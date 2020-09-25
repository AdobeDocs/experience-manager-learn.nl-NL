---
title: SPA-editor | Aan de slag met de AEM SPA Editor en Reageren
description: Leer hoe te om een Adobe Experience Manager (AEM) Gemaakt project als uitgangspunt voor een React toepassing te gebruiken die met de AEM Redacteur van het KUUROORD wordt geïntegreerd.
sub-product: sites
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 413
thumbnail: 413-spa-react.jpg
translation-type: tm+mt
source-git-commit: ab5b92dd9c901075347cc521bf0abe0dfc0e5319
workflow-type: tm+mt
source-wordcount: '1114'
ht-degree: 1%

---


# SPA-editor {#spa-editor-project}

Leer hoe te om een Adobe Experience Manager (AEM) Gemaakt project als uitgangspunt voor een React toepassing te gebruiken die met de AEM Redacteur van het KUUROORD wordt geïntegreerd.

## Doelstelling

1. Begrijp de structuur van een nieuw project van de Redacteur van het AEMKUUROORD dat van een Maven archetype wordt gebouwd.
2. Implementeer het startproject naar een lokale instantie van AEM.

## Wat u gaat maken

In dit hoofdstuk wordt een nieuw AEM-project geïmplementeerd op basis van het [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). Het AEM project zal met een zeer eenvoudig uitgangspunt voor React SPA worden opgepakt. Het project dat in dit hoofdstuk wordt gebruikt zal als basis voor een implementatie van de KND SPA dienen en zal op in toekomstige hoofdstukken worden voortgebouwd.

![WKND SPA React Starter Project](./assets/create-project/wknd-spa-react.png)

*De beginnende plaatshiërarchie voor het KND KUUROORD.*

## Vereisten

Controleer de vereiste gereedschappen en instructies voor het instellen van een [lokale ontwikkelomgeving](overview.md#local-dev-environment). Zorg ervoor dat een nieuw exemplaar van Adobe Experience Manager, dat in de modus **auteur** is gestart, lokaal wordt uitgevoerd.

## Het project ophalen

Er zijn verscheidene opties om een Maven Multi-module project voor AEM tot stand te brengen. Deze zelfstudie gebruikte het nieuwste [AEM Projectarchetype](https://github.com/adobe/aem-project-archetype) als basis voor de zelfstudiecode. De projectcode is gewijzigd om veelvoudige versies van AEM te steunen. Lees [de opmerking over achterwaartse compatibiliteit](overview.md#compatibility).

>[!CAUTION]
>
> Het is een beste praktijk om de **recentste** versie van [archetype](https://github.com/adobe/aem-project-archetype) te gebruiken om een nieuw project voor een echte implementatie te produceren. AEM projecten zouden één enkele versie van AEM moeten richten gebruikend het `aemVersion` bezit van archetype.

1. Download het beginpunt voor deze zelfstudie via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/create-project-start
   ```

2. De volgende map en bestandsstructuur vertegenwoordigen het AEM Project dat is gegenereerd door het Maven-archetype op het lokale bestandssysteem:

   ```plain
   |--- aem-guides-wknd-spa
       |--- all/
       |--- core/
       |--- dispatcher/
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.content/
       |--- ui.frontend /
       |--- it.tests/
       |--- pom.xml
       |--- README.md
       |--- .gitignore
       |--- archetype.properties
   ```

3. De volgende eigenschappen werden gebruikt toen het produceren van het AEM project van het archetype [van het](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14)AEM Project:

   | Eigenschap | Waarde |
   |-----------------|-------------------------------------|
   | aemVersion | wolk |
   | appTitle | WKND SPA React |
   | appId | wknd-spa-response |
   | groupId | com.adobe.aem.guides |
   | frontendModule | reageren |
   | package | com.adobe.aem.guides.wknd.spa.react |
   | includeExamples | n |

   >[!NOTE]
   >
   > Let op de `frontendModule=react` eigenschap. Dit vertelt het Archetype van het Project van het AEM om het project met een starter [React codebasis](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) te laarzen die met de Redacteur van het AEMKUUROORD moet worden gebruikt.

## Het project bouwen

Daarna, compileert, bouwt, en stelt de projectcode aan een lokale instantie van AEM op gebruikend Maven.

1. Zorg ervoor dat een instantie van AEM lokaal wordt uitgevoerd op poort **4502**.
2. Van de terminal van de bevellijn verifieert dat Maven geïnstalleerd is:

   ```shell
   $ mvn --version
    Apache Maven 3.6.2
    Maven home: /Library/apache-maven-3.6.2
    Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. Stel hieronder Geweven bevel van de `aem-guides-wknd-spa` folder in werking om het project te bouwen en op te stellen aan AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Bij gebruik van [AEM 6.x](overview.md#compatibility):

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   De veelvoudige modules van het project zouden moeten worden gecompileerd en aan AEM worden opgesteld.

   ```plain
    [INFO] ------------------------------------------------------------------------
    [INFO] Reactor Summary for wknd-spa-react 1.0.0-SNAPSHOT:
    [INFO] 
    [INFO] wknd-spa-react ..................................... SUCCESS [  0.523 s]
    [INFO] WKND SPA React - Core .............................. SUCCESS [  8.069 s]
    [INFO] wknd-spa-react.ui.frontend - UI Frontend ........... SUCCESS [01:23 min]
    [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  0.830 s]
    [INFO] WKND SPA React - UI apps ........................... SUCCESS [  4.654 s]
    [INFO] WKND SPA React - UI content ........................ SUCCESS [  1.607 s]
    [INFO] WKND SPA React - All ............................... SUCCESS [  0.384 s]
    [INFO] WKND SPA React - Integration Tests Bundles ......... SUCCESS [  0.770 s]
    [INFO] WKND SPA React - Integration Tests Launcher ........ SUCCESS [  1.407 s]
    [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.055 s]
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time:  01:44 min
   ```

   Met het profiel Maven ***autoInstallSinglePackage*** worden de afzonderlijke modules van het project gecompileerd en wordt één pakket naar de AEM-instantie geïmplementeerd. Dit pakket wordt standaard geïmplementeerd op een AEM die lokaal op poort **4502** wordt uitgevoerd en met de gegevens van **admin:admin**.

4. Navigeer naar **[!UICONTROL Package Manager]** de lokale AEM-instantie: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. U moet drie pakketten zien voor `wknd-spa-react.all`, `wknd-spa-react.ui.apps` en `wknd-spa-react.ui.content`.

   ![WKND SPA-pakketten](./assets/create-project/package-manager.png)

   *AEM Package Manager*

   Alle aangepaste code die nodig is voor het project, wordt in deze pakketten gebundeld en op de AEM-runtime geïnstalleerd.

6. U moet ook verschillende pakketten zien voor `spa.project.core` en `core.wcm.components`. Deze gebiedsdelen worden automatisch omvat door archetype. Meer informatie over [AEM Core Components vindt u hier](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html).

   `spa.project.core` is een gebiedsdeel nodig om JSON model API te produceren dat de Redacteur van het KUUROORD verwacht.

## Inhoud auteur

Vervolgens opent u de starter-SPA die is gegenereerd door het archetype en werkt u een gedeelte van de inhoud bij.

1. Navigeer naar de **[!UICONTROL Sites]** console: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   De WKND SPA omvat een basissitestructuur met een land, taal en homepage. Deze hiërarchie is gebaseerd op de standaardwaarden van archetype voor `language_country` en `isSingleCountryWebsite`. Deze waarden kunnen worden overschreven door de [beschikbare eigenschappen](https://github.com/adobe/aem-project-archetype#available-properties) bij te werken wanneer u een project genereert.

2. Open de pagina **[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA React Home Page]** door de pagina te selecteren en op de **[!UICONTROL Edit]** knop op de menubalk te klikken:

   ![siteconsole](./assets/create-project/open-home-page.png)

3. Er is al een **[!UICONTROL Text]** component aan de pagina toegevoegd. U kunt deze component op dezelfde manier bewerken als elke andere component in AEM.

   ![Tekstcomponent bijwerken](./assets/create-project/update-text-component.gif)

4. Voeg een extra **[!UICONTROL Text]** component aan de pagina toe.

   U ziet dat de ontwerpervaring vergelijkbaar is met die van een traditionele AEM Sites-pagina. Momenteel is een beperkt aantal componenten beschikbaar die kunnen worden gebruikt. Tijdens de zelfstudie wordt meer toegevoegd.

## Inspect de toepassing Eén pagina

Controleer vervolgens of dit een toepassing voor één pagina is met gebruik van de ontwikkelaars van uw browser.

1. Klik in de **[!UICONTROL Page Editor]** sectie op de **[!UICONTROL Page Information]** knop > **[!UICONTROL View as Published]**:

   ![Weergeven als gepubliceerde knop](./assets/create-project/view-as-published.png)

   Hiermee wordt een nieuw tabblad geopend met de queryparameter `?wcmmode=disabled` waarmee de AEM-editor wordt uitgeschakeld: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. Bekijk de bron van de pagina. U ziet dat de tekstinhoud **[!DNL Hello World]** of een van de andere inhoud niet is gevonden. In plaats daarvan ziet u HTML als volgt:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` is React SPA die op de pagina wordt geladen en voor het teruggeven van de inhoud verantwoordelijk.

   Maar *waar komt de inhoud vandaan?*

3. Terug naar de tab: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. Open de de ontwikkelaarshulpmiddelen van browser en inspecteer het netwerkverkeer van de pagina tijdens verfrissen zich. Bekijk de **XHR** verzoeken:

   ![XHR-verzoeken](./assets/create-project/xhr-requests.png)

   Er moet een verzoek worden ingediend naar [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Dit bevat alle inhoud, die in JSON wordt geformatteerd, die SPA zal drijven.

5. Ga naar een nieuw tabblad en open [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   Het verzoek `en.model.json` vertegenwoordigt het inhoudsmodel dat de toepassing zal drijven. Inspect de JSON-uitvoer en u moet het fragment kunnen vinden dat de **[!UICONTROL Text]** component(en) vertegenwoordigt.

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
   }
   ...
   ```

   In het volgende hoofdstuk zullen wij inspecteren hoe deze inhoud JSON van AEM Componenten aan de Componenten van het KUUROORD in kaart wordt gebracht om de basis van de ervaring van de Redacteur van AEMKUUROORD te vormen.

   >[!NOTE]
   >
   > Het kan handig zijn een browserextensie te installeren om de JSON-uitvoer automatisch op te maken.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, u hebt enkel uw eerste Project van de Redacteur van het AEMKUUROORD gecreeerd!

De SPA is vrij eenvoudig. In de volgende hoofdstukken wordt meer functionaliteit toegevoegd.

### Volgende stappen {#next-steps}

[Integreer het KUUROORD](integrate-spa.md) - leer hoe de broncode van het KUUROORD met het AEM Project wordt geïntegreerd en beschikbare hulpmiddelen begrijpt om het KUUROORD snel te ontwikkelen.
