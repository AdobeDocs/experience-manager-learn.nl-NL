---
title: SPA Editor-project | Aan de slag met de AEM SPA Editor en Angular
description: Leer hoe u een Adobe Experience Manager (AEM) Maven-project gebruikt als beginpunt voor een Angular-toepassing die is geïntegreerd met de AEM SPA Editor.
sub-product: sites
feature: SPA-editor AEM projectarchetype
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5309
thumbnail: 5309-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 375df47a13b1820911a7ceb73af0dad15c68740e
workflow-type: tm+mt
source-wordcount: '1101'
ht-degree: 1%

---


# Project {#create-project} SPA

Leer hoe u een Adobe Experience Manager (AEM) Maven-project gebruikt als beginpunt voor een Angular-toepassing die is geïntegreerd met de AEM SPA Editor.

## Doelstelling

1. Begrijp de structuur van een nieuw AEM SPA van de Redacteur project dat van een Maven archetype wordt gebouwd.
2. Implementeer het startproject naar een lokale instantie van AEM.

## Wat u gaat maken

In dit hoofdstuk, zal een nieuw AEM project worden opgesteld, gebaseerd op [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). Het AEM project zal met een zeer eenvoudig uitgangspunt voor de SPA van de Angular worden opgevoerd. Het in dit hoofdstuk gebruikte project zal als basis dienen voor de tenuitvoerlegging van de WKND-SPA en zal in toekomstige hoofdstukken worden opgenomen.

![WKND SPA Angular Starter Project](./assets/create-project/what-you-will-build.png)

*Een klassiek Hello World-bericht.*

## Vereisten

Controleer de vereiste gereedschappen en instructies voor het instellen van een [lokale ontwikkelomgeving](overview.md#local-dev-environment). Zorg ervoor dat een nieuw exemplaar van Adobe Experience Manager, dat is gestart in de modus **auteur**, lokaal wordt uitgevoerd.

## Het project ophalen

Er zijn verscheidene opties om een Maven Multi-module project voor AEM tot stand te brengen. In deze zelfstudie werd het nieuwste [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) gebruikt als basis voor de zelfstudiecode. De projectcode is gewijzigd om veelvoudige versies van AEM te steunen. Raadpleeg [de opmerking over achterwaartse compatibiliteit](overview.md#compatibility).

>[!CAUTION]
>
>Het is aan te raden de **nieuwste**-versie van [archetype](https://github.com/adobe/aem-project-archetype) te gebruiken om een nieuw project te genereren voor een implementatie in de praktijk. AEM projecten zouden één enkele versie van AEM moeten richten gebruikend het `aemVersion` bezit van archetype.

1. Download het beginpunt voor deze zelfstudie via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/create-project-start
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

3. De volgende eigenschappen werden gebruikt toen het produceren van het AEM project van [AEM Project archetype](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14):

   | Eigenschap | Waarde |
   |-----------------|---------------------------------------|
   | aemVersion | wolk |
   | appTitle | WKND SPA Angular |
   | appId | wknd-spa-angular |
   | groupId | com.adobe.aem.guides |
   | frontendModule | angular |
   | package | com.adobe.aem.guides.wknd.spa.angular |
   | includeExamples | n |

   >[!NOTE]
   >
   > Let op de eigenschap `frontendModule=angular`. Dit vertelt het Archetype van het Project van het AEM om het project met een starter [de codebasis van de Angular ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html) te laarzen die met de AEM SPA Redacteur moet worden gebruikt.

## Het project bouwen

Daarna, compileert, bouwt, en stelt de projectcode aan een lokaal geval van AEM op gebruikend Maven.

1. Zorg ervoor dat een instantie van AEM lokaal wordt uitgevoerd op poort **4502**.
2. Van de terminal van de bevellijn verifieert dat Maven geïnstalleerd is:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. Voer de onderstaande opdracht Geweven uit vanuit de map `aem-guides-wknd-spa` om het project te bouwen en te implementeren in AEM:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ```

   Bij gebruik van [AEM 6.x](overview.md#compatibility):

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   De veelvoudige modules van het project zouden moeten worden gecompileerd en aan AEM worden opgesteld.

   ```plain
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for wknd-spa-angular 1.0.0-SNAPSHOT:
   [INFO] 
   [INFO] wknd-spa-angular ................................... SUCCESS [  0.473 s]
   [INFO] WKND SPA Angular - Core ............................ SUCCESS [ 54.866 s]
   [INFO] wknd-spa-angular.ui.frontend - UI Frontend ......... SUCCESS [02:10 min]
   [INFO] WKND SPA Angular - Repository Structure Package .... SUCCESS [  0.694 s]
   [INFO] WKND SPA Angular - UI apps ......................... SUCCESS [  6.351 s]
   [INFO] WKND SPA Angular - UI content ...................... SUCCESS [  2.885 s]
   [INFO] WKND SPA Angular - All ............................. SUCCESS [  1.736 s]
   [INFO] WKND SPA Angular - Integration Tests Bundles ....... SUCCESS [  2.563 s]
   [INFO] WKND SPA Angular - Integration Tests Launcher ...... SUCCESS [  1.846 s]
   [INFO] WKND SPA Angular - Dispatcher ...................... SUCCESS [  0.270 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Met het profiel Maven ***autoInstallSinglePackage*** worden de afzonderlijke modules van het project gecompileerd en wordt één pakket naar de AEM-instantie geïmplementeerd. Dit pakket wordt standaard geïmplementeerd op een AEM-instantie die lokaal op poort **4502** wordt uitgevoerd en met de gegevens van **admin:admin**.

4. Navigeer naar **[!UICONTROL Package Manager]** op uw lokale AEM-instantie: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. Er moeten drie pakketten worden weergegeven voor `wknd-spa-angular.all`, `wknd-spa-angular.ui.apps` en `wknd-spa-angular.ui.content`.

   ![WKND-SPA](./assets/create-project/package-manager.png)

   Alle aangepaste code die nodig is voor het project wordt in deze pakketten gebundeld en op de AEM-runtime geïnstalleerd.

6. U zou verscheidene pakketten voor `spa.project.core` en `core.wcm.components` ook moeten zien. Dit zijn gebiedsdelen automatisch inbegrepen door archetype. Meer informatie over [AEM Core Components kunt u hier vinden](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html).

## Inhoud auteur

Open vervolgens de SPA die is gegenereerd door het archetype en werk een deel van de inhoud bij.

1. Navigeer naar de **[!UICONTROL Sites]**-console: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   De WKND-SPA bevat een basissitestructuur met een land, taal en homepage. Deze hiërarchie is gebaseerd op de standaardwaarden van archetype voor `language_country` en `isSingleCountryWebsite`. Deze waarden kunnen worden beschreven door [beschikbare eigenschappen](https://github.com/adobe/aem-project-archetype#available-properties) bij te werken wanneer het produceren van een project.

2. Open de pagina **[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA Angular Home Page]** door de pagina te selecteren en op de knop **[!UICONTROL Edit]** in de menubalk te klikken:

   ![siteconsole](./assets/create-project/open-home-page.png)

3. Er is al een **[!UICONTROL Text]**-component toegevoegd aan de pagina. U kunt deze component op dezelfde manier bewerken als elke andere component in AEM.

   ![Tekstcomponent bijwerken](./assets/create-project/update-text-component.gif)

4. Voeg een extra **[!UICONTROL Text]** component aan de pagina toe.

   U ziet dat de ontwerpervaring vergelijkbaar is met die van een traditionele AEM Sites-pagina. Momenteel is een beperkt aantal componenten beschikbaar die kunnen worden gebruikt. Tijdens de zelfstudie wordt meer toegevoegd.

## Inspect de toepassing Eén pagina

Controleer vervolgens of dit een toepassing voor één pagina is met gebruik van de ontwikkelaars van uw browser.

1. Klik in **[!UICONTROL Page Editor]** op het menu **[!UICONTROL Page Information]** > **[!UICONTROL View as Published]**:

   ![Weergeven als gepubliceerde knop](./assets/create-project/view-as-published.png)

   Hiermee wordt een nieuw tabblad geopend met de queryparameter `?wcmmode=disabled` waarmee de AEM-editor feitelijk wordt uitgeschakeld: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)

2. Bekijk de bron van de pagina. U ziet dat de tekstinhoud **[!DNL Hello World]** of een van de andere inhoud niet is gevonden. In plaats daarvan ziet u HTML als volgt:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-angular/clientlibs/clientlib-angular.min.js"></script>
       ...
   </body>
   ...
   ```

   `clientlib-angular.min.js` Dit is de Angular SPA die op de pagina wordt geladen en die verantwoordelijk is voor het weergeven van de inhoud.

   *Waar komt de inhoud vandaan?*

3. Terug naar de tab: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)
4. Open de de ontwikkelaarshulpmiddelen van browser en inspecteer het netwerkverkeer van de pagina tijdens verfrissen zich. Bekijk de **XHR** verzoeken:

   ![XHR-verzoeken](./assets/create-project/xhr-requests.png)

   Er zou een verzoek aan [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) moeten zijn. Dit bevat alle inhoud, geformatteerd in JSON, die de SPA zal drijven.

5. Open [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) op een nieuw tabblad

   Het verzoek `en.model.json` vertegenwoordigt het inhoudsmodel dat de toepassing zal drijven. Inspect de JSON-uitvoer en u moet het fragment kunnen vinden dat de **[!UICONTROL Text]**-component(en) vertegenwoordigt.

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
   },
   ...
   ```

   In het volgende hoofdstuk zullen wij inspecteren hoe de inhoud JSON van AEM Componenten aan SPA Componenten wordt in kaart gebracht om de basis van de AEM SPA Ervaring van de Redacteur te vormen.

   >[!NOTE]
   >
   > Het kan handig zijn een browserextensie te installeren om de JSON-uitvoer automatisch op te maken.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, u hebt zojuist uw eerste AEM SPA Editor Project gemaakt!

Het is nu heel eenvoudig, maar in de volgende hoofdstukken wordt meer functionaliteit toegevoegd.

### Volgende stappen {#next-steps}

[Integreer de SPA](integrate-spa.md)  - Leer hoe de SPA broncode met het AEM Project wordt geïntegreerd en begrijp hulpmiddelen beschikbaar om de SPA snel te ontwikkelen.
