---
title: Project maken | Aan de slag met de AEM SPA Editor en reageren
description: Leer hoe u een Adobe Experience Manager (AEM) Maven-project genereert als beginpunt voor een React-toepassing die is geïntegreerd met de AEM SPA Editor.
sub-product: sites
feature: SPA Editor, AEM Project Archetype
version: Cloud Service
kt: 413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 57c8fc16-fed5-4af4-b98b-5c3f0350b240
source-git-commit: 09f6c4b0bec10edd306270a7416fcaff8a584e76
workflow-type: tm+mt
source-wordcount: '1081'
ht-degree: 0%

---

# Project maken {#spa-editor-project}

Leer hoe u een Adobe Experience Manager (AEM) Maven-project genereert als beginpunt voor een React-toepassing die is geïntegreerd met de AEM SPA Editor.

## Doelstelling

1. Genereer een SPA Editor ingeschakeld project met behulp van het AEM Project Archetype.
2. Implementeer het startproject naar een lokale instantie van AEM.

## Wat u gaat maken {#what-build}

In dit hoofdstuk wordt een nieuw AEM-project gegenereerd op basis van de [Projectarchetype AEM](https://github.com/adobe/aem-project-archetype). Het AEM project wordt opgestart met een heel eenvoudig startpunt voor de SPA React.

**Wat is een Maven-project?** - [Apache Maven](https://maven.apache.org/) is een hulpmiddel van het softwarebeheer om projecten te bouwen. *Alle Adobe Experience Manager* implementaties gebruiken Maven-projecten om aangepaste code bovenop AEM te bouwen, te beheren en te implementeren.

**Wat is een Maven archetype?** - A [Maven archetype](https://maven.apache.org/archetype/index.html) is een malplaatje of een patroon voor het produceren van nieuwe projecten. Het AEM archetype van het Project staat ons toe om een nieuw project met een douane te produceren namespace en een projectstructuur te omvatten die beste praktijken volgt, zeer versnellend ons project.

## Vereisten

Controleer de vereiste gereedschappen en instructies voor het instellen van een [plaatselijke ontwikkelomgeving](overview.md#local-dev-environment). Zorg ervoor dat er een nieuw exemplaar van Adobe Experience Manager is gestart in **auteur** wordt lokaal uitgevoerd.

## Het project maken {#create}

>[!NOTE]
>
>Deze zelfstudie gebruikt versie **39** van het archetype. Het is altijd verstandig om de **nieuwste** versie van archetype om een nieuw project te produceren.

1. Open een opdrachtregelterminal en voer de volgende Maven-opdracht in:

   ```shell
   mvn -B archetype:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=39 \
    -D appTitle="WKND SPA React" \
    -D appId="wknd-spa-react" \
    -D artifactId="aem-guides-wknd-spa.react" \
    -D groupId="com.adobe.aem.guides.wkndspa.react" \
    -D frontendModule="react" \
    -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Vervang AEM 6.5.5+ `aemVersion="cloud"` with `aemVersion="6.5.5"`. Gebruik, indien gericht op 6.4.8+, `aemVersion="6.4.8"`.

   Let op: `frontendModule=react` eigenschap. Dit vertelt de Archetype van het Project van de AEM om het project met een aanzet te lassen [Reageren op basis van code](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) te gebruiken met de AEM SPA Editor. Eigenschappen als `appTitle`, `appId`, `artifactId`, en `groupId` worden gebruikt om het project en het doel te identificeren.

   Een volledige lijst van beschikbare eigenschappen voor het vormen van een project [hier te vinden](https://github.com/adobe/aem-project-archetype#available-properties).

1. De volgende map en bestandsstructuur worden gegenereerd door het Maven archetype op uw lokale bestandssysteem:

   ```plain
   |--- aem-guides-wknd-spa.react/
       |--- LICENSE
       |--- README.md
       |--- all/
       |--- archetype.properties
       |--- core/
       |--- dispatcher/
       |--- it.tests/
       |--- pom.xml
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.config/
       |--- ui.content/
       |--- ui.frontend/
       |--- ui.tests /
       |--- .gitignore
   ```

   Elke map vertegenwoordigt een afzonderlijke module Maven. In deze zelfstudie werken we in de eerste plaats samen met de `ui.frontend` -module, de React-app. Meer informatie over de afzonderlijke modules vindt u in de [AEM documentatie van het type Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

## Het project implementeren en bouwen

Daarna, compileert, bouwt, en stelt de projectcode aan een lokale instantie van AEM op gebruikend Maven.

1. Verzeker een geval van AEM plaatselijk op haven loopt **4502**.
1. Navigeer vanaf de opdrachtregel naar de `aem-guides-wknd-spa.react` projectmap.

   ```shell
   $ cd aem-guides-wknd-spa.react
   ```

1. Stel het volgende bevel in werking om het volledige project te bouwen en op te stellen aan AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   De build zal ongeveer een minuut in beslag nemen en zal eindigen met het volgende bericht:

   ```shell
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd-spa.react 1.0.0-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd-spa.react .......................... SUCCESS [  0.257 s]
   [INFO] WKND SPA React - Core .............................. SUCCESS [ 12.553 s]
   [INFO] WKND SPA React - UI Frontend ....................... SUCCESS [01:46 min]
   [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  1.082 s]
   [INFO] WKND SPA React - UI apps ........................... SUCCESS [  8.237 s]
   [INFO] WKND SPA React - UI content ........................ SUCCESS [  5.633 s]
   [INFO] WKND SPA React - UI config ......................... SUCCESS [  0.234 s]
   [INFO] WKND SPA React - All ............................... SUCCESS [  0.643 s]
   [INFO] WKND SPA React - Integration Tests ................. SUCCESS [ 12.377 s]
   [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.066 s]
   [INFO] WKND SPA React - UI Tests .......................... SUCCESS [  0.074 s]
   [INFO] WKND SPA React - Project Analyser .................. SUCCESS [ 31.287 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Het profiel Maven `autoInstallSinglePackage` compileert de individuele modules van het project en stelt één enkel pakket aan de AEM instantie op. Dit pakket wordt standaard geïmplementeerd op een AEM-instantie die lokaal op de poort wordt uitgevoerd **4502** en met de geloofsbrieven van `admin:admin`.

1. Navigeren naar **Pakketbeheer** op uw lokale AEM: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

1. U moet meerdere pakketten zien die vooraf zijn voorzien van `aem-guides-wknd-spa.react`.

   ![WKND-SPA](assets/create-project/package-manager.png)

   *AEM Package Manager*

   Alle aangepaste code die nodig is voor het project, wordt in deze pakketten gebundeld en in de AEM-omgeving geïnstalleerd.

## Inhoud auteur

Open vervolgens de SPA die is gegenereerd door het archetype en werk een deel van de inhoud bij.

1. Ga naar de **Sites** console: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   De WKND-SPA bevat een basissitestructuur met een land, taal en homepage. Deze hiërarchie is gebaseerd op de standaardwaarden van het archetype voor `language_country` en `isSingleCountryWebsite`. Deze waarden kunnen worden overschreven door het bijwerken van de [beschikbare eigenschappen](https://github.com/adobe/aem-project-archetype#available-properties) wanneer u een project genereert.

2. Open de **ons** > **en** > **WKND SPA React Home Page** pagina door de pagina te selecteren en op de **Bewerken** in de menubalk:

   ![siteconsole](./assets/create-project/open-home-page.png)

3. A **Tekst** is al toegevoegd aan de pagina. U kunt deze component op dezelfde manier bewerken als elke andere component in AEM.

   ![Tekstcomponent bijwerken](./assets/create-project/update-text-component.gif)

4. Voeg een extra **Tekst** naar de pagina.

   U ziet dat de ontwerpervaring vergelijkbaar is met die van een traditionele AEM Sites-pagina. Momenteel is een beperkt aantal componenten beschikbaar die kunnen worden gebruikt. Tijdens de zelfstudie wordt meer toegevoegd.

## Inspect de toepassing Eén pagina

Controleer vervolgens of dit een toepassing voor één pagina is met gebruik van de ontwikkelaars van uw browser.

1. In de **Pagina-editor** klikt u op de knop **Pagina-informatie** knop > **Weergeven als gepubliceerd**:

   ![Weergeven als gepubliceerde knop](./assets/create-project/view-as-published.png)

   Hiermee wordt een nieuw tabblad met de queryparameter geopend `?wcmmode=disabled` waarmee de AEM-editor feitelijk wordt uitgeschakeld: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. De bron van de pagina weergeven. U ziet dat de tekstinhoud **[!DNL Hello World]** of een van de andere inhoud niet is gevonden. In plaats daarvan ziet u HTML als volgt:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` Dit is de SPA React die op de pagina wordt geladen en die verantwoordelijk is voor het renderen van de inhoud.

   Maar *waar komt de inhoud vandaan ?*

3. Terug naar de tab: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. Open de de ontwikkelaarshulpmiddelen van browser en inspecteer het netwerkverkeer van de pagina tijdens verfrissen zich. De weergave van **XHR** verzoeken:

   ![XHR-verzoeken](./assets/create-project/xhr-requests.png)

   Er moet een verzoek worden ingediend om [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Dit bevat alle inhoud, geformatteerd in JSON, die de SPA zal drijven.

5. Open een nieuw tabblad [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   Het verzoek `en.model.json` vertegenwoordigt het inhoudsmodel dat de toepassing zal drijven. Inspect de JSON-uitvoer en u moet het fragment kunnen vinden dat de **[!UICONTROL Text]** component(en).

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

   In het volgende hoofdstuk zullen wij inspecteren hoe deze inhoud JSON van AEM Componenten aan SPA Componenten wordt in kaart gebracht om de basis van de AEM SPA Ervaring van de Redacteur te vormen.

   >[!NOTE]
   >
   > Het kan handig zijn een browserextensie te installeren om de JSON-uitvoer automatisch op te maken.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, u hebt zojuist uw eerste AEM SPA Editor Project gemaakt!

Het SPA is heel eenvoudig. In de volgende hoofdstukken wordt meer functionaliteit toegevoegd.

### Volgende stappen {#next-steps}

[Een SPA integreren](integrate-spa.md) - Leer hoe de SPA broncode is geïntegreerd met het AEM Project en begrijp de beschikbare tools om de SPA snel te ontwikkelen.
