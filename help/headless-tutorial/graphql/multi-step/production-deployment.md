---
title: Implementatie van de productie met behulp van een AEM-publicatieservice - Aan de slag met AEM headless - GraphQL
description: Meer informatie over AEM-auteur- en publicatieservices en het aanbevolen implementatiepatroon voor toepassingen zonder kop. In deze zelfstudie leert u omgevingsvariabelen te gebruiken om een eindpunt GraphQL dynamisch te wijzigen op basis van de doelomgeving. Leer om AEM voor het delen van bronnen tussen verschillende oorsprong (CORS) behoorlijk te vormen.
sub-product: elementen
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 7131
thumbnail: KT-7131.jpg
translation-type: tm+mt
source-git-commit: 81626b8d853f3f43d9c51130acf02561f91536ac
workflow-type: tm+mt
source-wordcount: '2361'
ht-degree: 0%

---


# Implementatie van productie met een AEM-publicatieservice

In deze zelfstudie stelt u een lokale omgeving in om inhoud te simuleren die wordt gedistribueerd van een instantie Auteur naar een instantie Publish. U zult ook productiebouwstijl van React App produceren die wordt gevormd om inhoud van het Publish milieu te verbruiken AEM gebruikend GraphQL APIs. Langs de manier, zult u leren hoe te om milieuvariabelen effectief te gebruiken en hoe te om de configuraties van AEM CORS bij te werken.

## Vereisten

Deze zelfstudie maakt deel uit van een meerdelige zelfstudie. Aangenomen wordt dat de in de vorige delen beschreven stappen zijn voltooid.

## Doelstellingen

Leer hoe u:

* Begrijp de AEM Auteur en publiceer architectuur.
* Leer beste praktijken voor het beheren van omgevingsvariabelen.
* Leer hoe u AEM voor het delen van bronnen tussen verschillende oorsprong (CORS) correct configureert.

## Implementatiepatroon voor publiceren auteur {#deployment-pattern}

Een volledige AEM omgeving bestaat uit een Auteur, Publish en Dispatcher. In de service Auteur kunnen interne gebruikers inhoud maken, beheren en voorvertonen. De publicatieservice wordt beschouwd als de &quot;live&quot;-omgeving en is doorgaans de omgeving waarmee eindgebruikers werken. Inhoud wordt na bewerking en goedkeuring in de service Auteur gedistribueerd naar de service Publiceren.

Het meest gebruikelijke implementatiepatroon met toepassingen zonder kop is dat de productieversie van de toepassing verbinding maakt met een AEM-publicatieservice.

![Implementatiepatroon op hoog niveau](assets/publish-deployment/high-level-deployment.png)

Het diagram hierboven toont dit gemeenschappelijke plaatsingspatroon.

1. Een **Inhoudsauteur** gebruikt de AEM auteurservice om inhoud te maken, te bewerken en te beheren.
2. De auteur van **Inhoud** en andere interne gebruikers kunnen de inhoud direct voorvertonen op de service Auteur. Er kan een voorvertoningsversie van de toepassing worden ingesteld die verbinding maakt met de service Auteur.
3. Nadat de inhoud is goedgekeurd, kan deze **gepubliceerd** zijn naar de AEM-publicatieservice.
4. **Eindgebruikers** communiceren met de productieversie van de toepassing. De productietoepassing maakt verbinding met de publicatieservice en gebruikt de GraphQL API&#39;s om inhoud aan te vragen en te verbruiken.

De zelfstudie simuleert de bovenstaande implementatie door een AEM-publicatieexemplaar toe te voegen aan de huidige installatie. In vorige hoofdstukken werkte de React App als voorproef door rechtstreeks met de instantie van de Auteur te verbinden. Een productie bouwt van React App zal aan een statische server worden opgesteld Node.js die met de nieuwe Publish instantie verbindt.

Uiteindelijk worden drie lokale servers uitgevoerd:

* http://localhost:4502 - Auteur-instantie
* http://localhost:4503 - Instantie publiceren
* http://localhost:5000 - React App in productiemodus, verbindend met de Publish instantie.

## AEM SDK installeren - Publicatiemodus {#aem-sdk-publish}

Momenteel hebben we een actieve instantie van de SDK in de modus **Auteur**. De SDK kan ook worden gestart in de modus **Publiceren** om een AEM-publicatie-omgeving te simuleren.

Een meer gedetailleerde gids voor vestiging een lokale ontwikkelomgeving [kan hier worden gevonden](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up).

1. Maak in uw lokale bestandssysteem een specifieke map voor de installatie van de instantie Publiceren, met de naam `~/aem-sdk/publish`.
1. Kopieer het QuickStart-jar-bestand dat voor de Auteur-instantie in vorige hoofdstukken is gebruikt en plak het bestand in de map `publish`. U kunt ook naar [Software Distribution Portal](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) navigeren en de nieuwste SDK downloaden en het QuickStart-jar-bestand uitpakken.
1. Wijzig de naam van het jar-bestand in `aem-publish-p4503.jar`.

   De `publish`-tekenreeks geeft aan dat de QuickStart-jar begint in de publicatiemodus. `p4503` specificeert dat de server van Quickstart op haven 4503 loopt.

1. Open een nieuw terminalvenster en navigeer naar de map die het jar-bestand bevat. Installeer en start de AEM-instantie:

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. Geef een beheerderswachtwoord op als `admin`. Om het even welk admin wachtwoord is aanvaardbaar, nochtans wordt het geadviseerd om het gebrek voor lokale ontwikkeling te gebruiken om extra configuraties te vermijden.
1. Wanneer het AEM-exemplaar is geïnstalleerd, wordt een nieuw browservenster geopend op [http://localhost:4503/content.html](http://localhost:4503/content.html)

   De verwachting is dat deze een pagina van 404 niet gevonden retourneert. Dit is een gloednieuw AEM-exemplaar en er is geen inhoud geïnstalleerd.

## Voorbeeldinhoud en GraphQL-eindpunten {#wknd-site-content-endpoints} installeren

Enkel zoals op de instantie van de Auteur, moet de Publish instantie toegelaten eindpunten hebben GraphQL en steekproefinhoud nodig. Installeer vervolgens de WKND Reference Site op de instantie Publish.

1. Download het nieuwste gecompileerde AEM Package voor WKND Site: [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Download de standaardversie die compatibel is met AEM als Cloud Service en **not** de `classic`-versie.

1. Meld u aan bij de instantie Publiceren door rechtstreeks naar: [http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html) met gebruikersnaam `admin` en wachtwoord `admin`.
1. Navigeer vervolgens naar Pakketbeheer op [http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp).
1. Klik **Pakket uploaden** en kies het pakket WKND dat in de vorige stap wordt gedownload. Klik **Installeren** om het pakket te installeren.
1. Na de installatie van het pakket is de WKND-referentiesite nu beschikbaar op [http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html).
1. Meld u af als de `admin`-gebruiker door op de knop Afmelden op de menubalk te klikken.

   ![WKND-aanmeldreferentiesite](assets/publish-deployment/sign-out-wknd-reference-site.png)

   In tegenstelling tot de instantie AEM-auteur worden in de AEM-publicatie-instanties anonieme alleen-lezen toegang standaard ingesteld. Wij willen de ervaring van een anonieme gebruiker wanneer het runnen van de React toepassing simuleren.

## Update Environment variables to point the Publish instance {#react-app-publish}

Werk vervolgens de omgevingsvariabelen bij die door de toepassing React worden gebruikt om naar de instantie Publish te verwijzen. React App zou **only** met de Publish instantie op productiemodus moeten verbinden.

Voeg vervolgens een nieuw bestand `.env.production.local` toe om de productieervaring te simuleren.

1. Open de WKND GraphQL React-app in uw IDE.

1. Voeg onder `aem-guides-wknd-graphql/react-app` een bestand met de naam `.env.production.local` toe.
1. Vul `.env.production.local` met het volgende:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![Nieuw omgevingsvariabele-bestand toevoegen](assets/publish-deployment/env-production-local-file.png)

   Het gebruiken van omgevingsvariabelen maakt het gemakkelijk om het eindpunt GraphQL tussen een Auteur of Publish milieu van een knevel te voorzien zonder extra logica binnen de toepassingscode toe te voegen. Meer informatie over [aangepaste omgevingsvariabelen voor React vindt u hier](https://create-react-app.dev/docs/adding-custom-environment-variables).

   >[!NOTE]
   >
   > Merk op dat geen authentificatieinformatie inbegrepen is aangezien de Publish milieu&#39;s anonieme toegang tot inhoud door gebrek verlenen.

## Een statische Node-server {#static-server} implementeren

De React-app kan worden gestart met behulp van de webpack-server, maar dit is alleen voor ontwikkeling. Vervolgens simuleert u een productieimplementatie met [serve](https://github.com/vercel/serve) om een productiebuild van de React-app te hosten met Node.js.

1. Open een nieuw terminalvenster en navigeer naar de map `aem-guides-wknd-graphql/react-app`

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Installeer [serve](https://github.com/vercel/serve) met de volgende opdracht:

   ```shell
   $ npm install serve --save-dev
   ```

1. Open het bestand `package.json` om `react-app/package.json`. Voeg een manuscript genoemd `serve` toe:

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   Het `serve` manuscript voert twee acties uit. Ten eerste wordt een productiebuild van de React App gegenereerd. Ten tweede, begint de server Node.js en gebruikt de productie bouwt.

1. Terugkeer aan de terminal en ga het bevel in om de statische server te beginnen:

   ```shell
   $ npm run serve
   
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.111:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

1. Open een nieuwe browser en navigeer naar [http://localhost:5000/](http://localhost:5000/). De React App moet worden weergegeven.

   ![React App Served](assets/publish-deployment/react-app-served-port5000.png)

   Bericht dat de vraag GraphQL aan de homepage werkt. Inspect het **XHR** verzoek gebruikend uw ontwikkelaarshulpmiddelen. Merk op dat de POST GraphQL aan de Publish instantie op `http://localhost:4503/content/graphql/global/endpoint.json` is.

   Alle afbeeldingen worden echter wel afgebroken op de startpagina!

1. Klik in één van de pagina&#39;s van het Detail van het avontuur.

   ![Adventure-detailfout](assets/publish-deployment/adventure-detail-error.png)

   Merk op dat een fout GraphQL voor `adventureContributor` wordt geworpen. In de volgende oefeningen zijn de verbroken afbeeldingen en de `adventureContributor`-problemen opgelost.

## Absolute afbeeldingsreferenties {#absolute-image-references}

De afbeeldingen worden verbroken weergegeven omdat het `<img src`-kenmerk is ingesteld op een relatief pad en uiteindelijk verwijst naar de statische Node-server op `http://localhost:5000/`. In plaats daarvan moeten deze afbeeldingen verwijzen naar de AEM-publicatie-instantie. Hiervoor bestaan verschillende mogelijke oplossingen. Wanneer het gebruiken van de webpack dev server `react-app/src/setupProxy.js` opstelling een volmacht tussen de webpack server en de AEM auteursinstantie voor om het even welke verzoeken aan `/content`. Een volmachtsconfiguratie kan in een productiemilieu worden gebruikt maar moet op het niveau van de Webserver worden gevormd. Bijvoorbeeld [Apache&#39;s proxymodule](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html).

De toepassing kan worden bijgewerkt om een absolute URL op te nemen met behulp van de omgevingsvariabele `REACT_APP_HOST_URI`. Laten we in plaats daarvan een functie van AEM GraphQL API gebruiken om een absolute URL naar de afbeelding aan te vragen.

1. Stop de Node.js-server.
1. Ga terug naar winde en open het dossier `Adventures.js` om `react-app/src/components/Adventures.js`.
1. Voeg de eigenschap `_publishUrl` toe aan de `ImageRef` binnen `allAdventuresQuery`:

   ```diff
   const allAdventuresQuery = `
   {
       adventureList {
       items {
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
           ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
           }
           }
       }
       }
   }
   `;
   ```

   `_publishUrl` en  `_authorUrl` zijn waarden die in het  `ImageRef` object zijn ingebouwd om het gemakkelijker te maken absolute URL&#39;s op te nemen.

1. Herhaal de bovenstaande stappen om de query te wijzigen die in de functie `filterQuery(activity)` wordt gebruikt om de eigenschap `_publishUrl` op te nemen.
1. Wijzig de `AdventureItem` component bij `function AdventureItem(props)` om `_publishUrl` in plaats van het `_path` bezit van verwijzingen te voorzien wanneer het construeren van `<img src=''>` markering:

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. Open het bestand `AdventureDetail.js` om `react-app/src/components/AdventureDetail.js`.
1. Herhaal de zelfde stappen om de vraag te wijzigen GraphQL en het `_publishUrl` bezit voor het Avontuur toe te voegen

   ```diff
    adventureByPath (_path: "${_path}") {
       item {
           _path
           adventureTitle
           adventureActivity
           adventureType
           adventurePrice
           adventureTripLength
           adventureGroupSize
           adventureDifficulty
           adventurePrice
           adventurePrimaryImage {
               ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
               }
           }
           adventureDescription {
               html
           }
           adventureItinerary {
               html
           }
           adventureContributor {
               fullName
               occupation
               pictureReference {
                   ...on ImageRef {
                       _path
   +                   _publishUrl
                   }
               }
           }
       }
       }
   } 
   ```

1. Wijzig de twee `<img>` markeringen voor het Primaire Beeld van het Avontuur en de verwijzing van het Beeld van de Medewerker in `AdventureDetail.js`:

   ```diff
   /* AdventureDetail.js */
   ...
   <img className="adventure-detail-primaryimage"
   -       src={adventureData.adventurePrimaryImage._path} 
   +       src={adventureData.adventurePrimaryImage._publishUrl} 
           alt={adventureData.adventureTitle}/>
   ...
   pictureReference =  <img className="contributor-image" 
   -                        src={props.pictureReference._path}
   +                        src={props.pictureReference._publishUrl} 
                            alt={props.fullName} />
   ```

1. Terugkeer aan de terminal en begin de statische server:

   ```shell
   $ npm run serve
   ```

1. Navigeer naar [http://localhost:5000/](http://localhost:5000/) en zorg dat er afbeeldingen worden weergegeven en dat het `<img src''>`-kenmerk naar `http://localhost:4503` wijst.

   ![Gebroken afbeeldingen hersteld](assets/publish-deployment/broken-images-fixed.png)

## Publiceren van inhoud simuleren {#content-publish}

Rappel dat een fout GraphQL voor `adventureContributor` wordt geworpen wanneer een pagina van de Details van het Avontuur wordt gevraagd. Het model voor het inhoudsfragment **Medewerker** bestaat nog niet in de instantie Publiceren. De updates aan **Adventure** Model van het Fragment van de Inhoud worden gemaakt zijn ook niet beschikbaar op de Publish instantie die. Deze wijzigingen zijn rechtstreeks aangebracht in de instantie Auteur en moeten worden verspreid naar de instantie Publiceren.

Dit is iets waarmee u rekening moet houden wanneer u nieuwe updates wilt uitvoeren voor een toepassing die afhankelijk is van updates van een inhoudsfragment of een inhoudsfragmentmodel.

Vervolgens kunt u het publiceren van inhoud simuleren tussen de lokale auteur- en publicatie-instanties.

1. Start de instantie Auteur (indien nog niet gestart) en navigeer naar Package Manager op [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Download het pakket [EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip) en installeer het met Package Manager.

   Met dit pakket wordt een configuratie geïnstalleerd waarmee de instantie Auteur inhoud kan publiceren naar de instantie Publiceren. Handmatige stappen voor [deze configuratie vindt u hier](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution).

   >[!NOTE]
   >
   > In een AEM als Cloud Service wordt de laag Auteur automatisch ingesteld op het distribueren van inhoud naar de laag Publiceren.

1. Navigeer in het menu **AEM Start** naar **Tools** > **Assets** > **Content Fragment Models**.

1. Klik in **WKND Plaats** omslag.

1. Selecteer alle drie modellen en klik **Publiceren**:

   ![Modellen voor inhoudsfragmenten publiceren](assets/publish-deployment/publish-contentfragment-models.png)

   Er verschijnt een bevestigingsvenster. Klik op **Publiceren**.

1. Navigeer naar het Bali Surf Camp Content Fragment op [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp).

1. Klik op de knop **Publiceren** in de bovenste menubalk.

   ![Klik op de knop Publiceren in de Content Fragment Editor](assets/publish-deployment/publish-bali-content-fragment.png)

1. De wizard Publiceren toont alle afhankelijke elementen die moeten worden gepubliceerd. In dit geval wordt het desbetreffende fragment **stack-roswells** weergegeven en wordt ook verwezen naar verschillende afbeeldingen. De middelen waarnaar wordt verwezen, worden samen met het fragment gepubliceerd.

   ![Verwezen elementen om te publiceren](assets/publish-deployment/referenced-assets.png)

   Klik nogmaals op de knop **Publiceren** om het inhoudsfragment en afhankelijke elementen te publiceren.

1. Keer terug naar React App die bij [http://localhost:5000/](http://localhost:5000/) loopt. U kunt nu in de Camp van Bali Surf klikken om de avontuurdetails te zien.

1. Ga terug naar de AEM Auteur-instantie op [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) en werk de **Title** van het fragment bij. **Sla het fragment op en** sluit het. Vervolgens **publiceert** het fragment.
1. Ga terug naar [http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) en bekijk de gepubliceerde wijzigingen.

   ![Bali Surf Camp Publish Update](assets/publish-deployment/bali-surf-camp-update.png)

## COR-configuratie bijwerken

AEM is standaard beveiligd en staat niet toe dat niet-AEM wegeigenschappen aanroepen aan de clientzijde uitvoeren. AEM configuratie het Delen van het Middel van de Cross-Origin (CORS) kan specifieke domeinen toestaan om vraag aan AEM te maken.

Experimenteer vervolgens met de CORS-configuratie van de AEM Publish-instantie.

1. Keer aan het eindvenster terug waar React App met het bevel `npm run serve` loopt:

   ```shell
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.205:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

   Controleer of er twee URL&#39;s zijn opgegeven. Eén met `localhost` en een ander met het lokale IP-adres van het netwerk.

1. Navigeer aan het adres dat met [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000) begint. Het adres zal lichtjes verschillend zijn voor elke lokale computer. Merk op dat er een fout CORS wanneer het halen van de gegevens is. Dit is omdat de huidige configuratie CORS slechts verzoeken van `localhost` toestaat.

   ![CORS-fout](assets/publish-deployment/cors-error-not-fetched.png)

   Werk vervolgens de configuratie van AEM voor het publiceren van CORS bij om aanvragen van het IP-adres van het netwerk toe te staan.

1. Navigeer naar [http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html) en meld u aan met de gebruikersnaam `admin` en het wachtwoord `admin`.

1. Navigeer naar [http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr) en zoek de WKND GraphQL-configuratie op `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`.

1. Werk **Toegestane Origins** gebied bij om het netwerkIP adres te omvatten:

   ![CORS-configuratie bijwerken](assets/publish-deployment/cors-update.png)

   Het is ook mogelijk een reguliere expressie op te nemen om alle aanvragen van een specifiek subdomein toe te staan. Sla de wijzigingen op.

1. Zoek naar **Apache Sling Referrer Filter** en herzie de configuratie. De **Allow Lege** configuratie is ook nodig om verzoeken GraphQL van een extern domein toe te laten.

   ![Filter Verschuivingsverwijzing](assets/publish-deployment/sling-referrer-filter.png)

   Deze zijn gevormd als deel van de WKND verwijzingsplaats. U kunt de volledige reeks configuraties OSGi via [de bewaarplaats GitHub](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig) bekijken.

   >[!NOTE]
   >
   > De configuraties OSGi worden beheerd in een AEM project dat aan broncontrole wordt geëngageerd. Met Cloud Manager kunt u een AEM-project implementeren om AEM te maken als een Cloud Service. Met de [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) kunt u een project voor een specifieke implementatie genereren.

1. Keer terug naar React App die met [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000) begint en merk op dat de toepassing niet meer een fout CORS veroorzaakt.

   ![CORS-fout gecorrigeerd](assets/publish-deployment/cors-error-corrected.png)

## Gefeliciteerd! {#congratulations}

Gefeliciteerd! U hebt nu een volledige productieplaatsing gesimuleerd gebruikend een milieu van de Publicatie AEM. U leerde ook hoe te om de configuratie CORS in AEM te gebruiken.

## Overige bronnen

Voor meer details over de Fragmenten van de Inhoud en GraphQL zie de volgende middelen:

* [Aflevering van inhoud zonder kop met gebruik van inhoudsfragmenten met GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html)
* [AEM GraphQL API voor gebruik met Content Fragments](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html)
* [Verificatie op basis van token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=en#authentication)
* [Code implementeren om als Cloud Service te AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)
