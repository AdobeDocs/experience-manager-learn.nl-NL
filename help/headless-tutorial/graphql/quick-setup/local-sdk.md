---
title: AEM Headless, snelle installatie met de lokale AEM SDK
description: Ga aan de slag met Adobe Experience Manager (AEM) en GraphQL. Installeer de AEM SDK, voeg voorbeeldinhoud toe en implementeer een toepassing die inhoud uit AEM gebruikt met de GraphQL API's. Zie hoe AEM omni-channel ervaart.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d2da6efa-1f77-4391-adda-e3180c42addc
duration: 242
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1151'
ht-degree: 0%

---

# AEM Headless, snelle installatie met de lokale AEM SDK {#setup}

Met de snelle installatie van AEM Headless kunt u in de praktijk werken met AEM Headless via inhoud van het WKND-sitevoorbeeldproject en een voorbeeldtoepassing React (een SPA) die de inhoud gebruikt via AEM Headless GraphQL API&#39;s. Deze gids gebruikt [&#x200B; AEM as a Cloud Service SDK &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=nl-NL).

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

* [&#x200B; JDK 11 &#x200B;](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=software-type%3Atooling&fulltext=Oracle%7E+JDK%7E+11%7E&orderby=%40jcr%3Acontent%2fx jcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [&#x200B; Node.js v18 &#x200B;](https://nodejs.org/en/)
* [&#x200B; Git &#x200B;](https://git-scm.com/)

## 1. Installeer de AEM SDK {#aem-sdk}

Deze opstelling gebruikt [&#x200B; AEM as a Cloud Service SDK &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=nl-NL&#aem-as-a-cloud-service-sdk) om AEM te onderzoeken GraphQL APIs. In deze sectie vindt u een snelle handleiding voor het installeren van de AEM SDK en het uitvoeren ervan in de modus Auteur. Een meer gedetailleerde gids voor vestiging een lokale ontwikkelomgeving [&#x200B; kan hier &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=nl-NL#local-development-environment-set-up) worden gevonden.

>[!NOTE]
>
> Het is ook mogelijk om het leerprogramma met een [&#x200B; milieu van AEM as a Cloud Service &#x200B;](./cloud-service.md) te volgen. De zelfstudie bevat aanvullende notities voor het gebruik van een Cloud-omgeving.

1. Navigeer aan het **[Portaal van de Distributie van de Software &#x200B;](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)** > **AEM as a Cloud Service** en download de recentste versie van **AEM SDK**.

   ![&#x200B; Portaal van de Distributie van de Software &#x200B;](assets/quick-setup/aem-sdk/downloads__aem-sdk.png)

1. Pak de download uit en kopieer de QuickStart-jar (`aem-sdk-quickstart-XXX.jar`) naar een toegewezen map, d.w.z. `~/aem-sdk/author` .
1. Wijzig de naam van het jar-bestand in `aem-author-p4502.jar` .

   De naam `author` geeft aan dat de Quickstart-jar begint in de modus Auteur. `p4502` geeft de QuickStart-uitvoering op poort 4502 aan.

1. Als u de AEM-instantie wilt installeren en starten, opent u een opdrachtprompt in de map met het jar-bestand en voert u de volgende opdracht uit:

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. Geef een beheerderswachtwoord op als `admin` . Om het even welk admin wachtwoord is aanvaardbaar, nochtans adviseert het om `admin` voor lokale ontwikkeling te gebruiken om de behoefte te verminderen om te vormen.
1. Wanneer de dienst van AEM het installeren beëindigt, zou een nieuw browser venster in [&#x200B; http://localhost:4502 &#x200B;](http://localhost:4502) moeten openen.
1. Meld u aan met de gebruikersnaam `admin` en het wachtwoord die u hebt geselecteerd tijdens het opstarten van AEM (meestal `admin` ).

## 2. Voorbeeldinhoud installeren {#install-sample-content}

De inhoud van de steekproef van de **plaats van de Verwijzing WKND** wordt gebruikt om het leerprogramma te versnellen. De WKND is een fictief levensstijl, vaak gebruikt bij AEM training.

De plaats WKND omvat configuraties die worden vereist om a [&#x200B; eindpunt van GraphQL &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html?lang=nl-NL) bloot te stellen. In een implementatie in de praktijk, volg de gedocumenteerde stappen om [&#x200B; de eindpunten van GraphQL &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html?lang=nl-NL) in uw klantenproject te omvatten. A [&#x200B; CORS &#x200B;](#cors-config) is ook verpakt als deel van de Plaats WKND. Een configuratie CORS wordt vereist om toegang tot een externe toepassing te verlenen, kan meer informatie over [&#x200B; CORS &#x200B;](#cors-config) hieronder worden gevonden.

1. Download het recentste gecompileerde Pakket van AEM voor Plaats WKND: [&#x200B; aem-guides-wknd.all-x.x.x.zip &#x200B;](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Zorg ervoor om de standaardversie compatibel met AEM as a Cloud Service te downloaden en **niet** de `classic` versie.

1. Van het **Begin van AEM** menu, navigeer aan **Hulpmiddelen** > **Plaatsing** > **Pakketten**.

   ![&#x200B; ga aan Pakketten &#x200B;](assets/quick-setup/aem-sdk/aem-sdk__packages.png)

1. Klik **Upload Pakket** en kies het pakket WKND dat in de vroegere stap wordt gedownload. Klik **installeren** om het pakket te installeren.

1. Van het **Begin van AEM** menu, navigeer aan **Assets** > **Dossiers** > **Gedeelde WKND** > **Engels** > **Vluchtelingen**.

   ![&#x200B; mening van de Omslag van avonturen &#x200B;](assets/quick-setup/aem-sdk/aem-sdk__assets-folder.png)

   Dit is een map met alle middelen die bestaan uit de verschillende avonturen die door het WKND-merk worden bevorderd. Dit omvat traditionele media types zoals beelden en video, en media specifiek voor AEM als **de Fragmenten van de Inhoud**.

1. Klik in **het Afslanken van Wyoming** omslag en klik **het Afslappen van het Filgment van de Inhoud Wyoming** kaart:

   ![&#x200B; kaart van het Fragment van de Inhoud &#x200B;](assets/quick-setup/aem-sdk/aem-sdk__content-fragment.png)

1. De redacteur van het Fragment van de Inhoud opent voor het Downhill Scheidend Wyoming adventure.

   ![&#x200B; redacteur van het Fragment van de Inhoud &#x200B;](assets/quick-setup/aem-sdk/aem-sdk__content-fragment-editor.png)

   Merk op dat de diverse gebieden zoals **Titel**, **Beschrijving**, en **Activiteit** het fragment bepalen.

   **de Fragmenten van de Inhoud** zijn één van de manieren inhoud in AEM kan worden beheerd. Inhoudsfragment is herbruikbaar, presentatie-agnostische inhoud die bestaat uit gestructureerde gegevenselementen zoals tekst, tekst met opmaak, datums of verwijzingen naar andere inhoudsfragmenten. Inhoudsfragmenten worden later in de snelle installatie gedetailleerder onderzocht.

1. Klik **annuleren** om het fragment te sluiten. U kunt vrij navigeren in sommige andere mappen en de andere Adventure-inhoud verkennen.

>[!NOTE]
>
> Als het gebruiken van een milieu van Cloud Service de documentatie voor ziet hoe te [&#x200B; een codebasis zoals de plaats van de Verwijzing WKND aan een milieu van Cloud Service opstellen &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=nl-NL#coding-against-the-right-aem-version).

## 3. Download en voer de WKND React-app uit {#sample-app}

Een van de doelstellingen van deze zelfstudie is om te tonen hoe u AEM-inhoud van een externe toepassing kunt gebruiken met de GraphQL API&#39;s. In deze zelfstudie wordt een voorbeeld van React App gebruikt. De React-app is opzettelijk eenvoudig en richt zich op de integratie met AEM GraphQL API&#39;s.

1. Open een nieuwe bevelherinnering en kloon de steekproef React app van GitHub:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Open de React-app in `aem-guides-wknd-graphql/react-app` in de IDE van uw keuze.
1. Open het bestand `.env.development` at `/.env.development` in de IDE. Controleer of de regel `REACT_APP_AUTHORIZATION` geen opmerkingen bevat en of het bestand de volgende variabelen declareert:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   Zorg ervoor dat `REACT_APP_HOST_URI` punten naar uw lokale AEM SDK wijst. Voor gemak, verbindt dit snelle begin app Reageren met **de Auteur van AEM**. **de diensten van de auteur** vereisen authentificatie, zodat gebruikt app de `admin` gebruiker om zijn verbinding te vestigen. Het is gebruikelijk om tijdens de ontwikkeling een app te verbinden met AEM Author, omdat dit het snel doorlopen van inhoud zonder dat wijzigingen hoeven te worden gepubliceerd vergemakkelijkt.

   >[!NOTE]
   >
   > In een productiescenario zal App met AEM **verbinden publiceren** milieu. Dit wordt behandeld meer in detail in de _sectie van de Plaatsing van de 0&rbrace; Productie._


1. Installeer en start de React-app:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Een nieuw browser venster opent automatisch app op [&#x200B; http://localhost:3000 &#x200B;](http://localhost:3000).

   ![&#x200B; Reageer starter app &#x200B;](assets/quick-setup/aem-sdk/react-app__home-view.png)

   Er wordt een lijst met de avontuurlijke inhoud van AEM weergegeven.

1. Klik op een van de avontuurafbeeldingen om de details van het avontuur weer te geven. AEM wordt verzocht de details voor een avontuur terug te geven.

   ![&#x200B; mening van de Details van het avontuur &#x200B;](assets/quick-setup/aem-sdk/react-app__adventure-view.png)

1. Gebruik de ontwikkelaarshulpmiddelen van browser om de **verzoeken van het Netwerk** te inspecteren. Bekijk de **XHR** verzoeken en bekijk veelvoudige verzoeken van GET aan `/graphql/execute.json/...`. Met dit padvoorvoegsel wordt het voortgeduurde queryeindpunt van AEM aangeroepen, waarbij de voortgezette query wordt geselecteerd voor uitvoering met de naam en de gecodeerde parameters na het voorvoegsel.

   ![&#x200B; GraphQL Endpoint XHR- verzoek &#x200B;](assets/quick-setup/aem-sdk/react-app__graphql-request.png)

## 4. Inhoud bewerken in AEM

Zorg dat de React-app actief is en werk de inhoud in AEM bij en controleer of de wijziging doorwerkt in de app.

1. Navigeer aan AEM [&#x200B; http://localhost:4502 &#x200B;](http://localhost:4502).
1. Navigeer aan **Assets** > **Dossiers** > **Gedeelde WKND** > **Engels** > **Vluchtelingen** > **[Bali Surf Camp &#x200B;](http://localhost:4502/assets.html/content/dam/wknd-shared/en/adventures/bali-surf-camp)**.

   ![&#x200B; Bali Surf de omslag van de Camp &#x200B;](assets/setup/bali-surf-camp-folder.png)

1. Klik in het **Bali Surf de inhoudsfragment van de Camp** inhoud om de redacteur van het Fragment van de Inhoud te openen.
1. Wijzig de **Titel** en de **Beschrijving** van het avontuur.

   ![&#x200B; wijzig inhoudsfragment &#x200B;](assets/setup/modify-content-fragment-bali.png)

1. Klik **sparen** om de veranderingen te bewaren.
1. Vernieuw React app in [&#x200B; http://localhost:3000 &#x200B;](http://localhost:3000) om uw veranderingen te zien:

   ![&#x200B; Bijgewerkt Bali Surf het Adventure van de Camp van de Camp &#x200B;](assets/setup/overnight-bali-surf-camp-changes.png)

## 5. Ontdek GraphiQL {#graphiql}

1. Open [&#x200B; GraphiQL &#x200B;](http://localhost:4502/aem/graphiql.html) door aan **Hulpmiddelen** te navigeren > **Algemeen** > **de redacteur van de Vraag van GraphQL**
1. Selecteer bestaande voortgeduurde vragen op de linkerzijde, en stel hen in werking om de resultaten te zien.

   >[!NOTE]
   >
   > Het hulpmiddel GraphiQL en GraphQL API wordt [&#x200B; onderzocht meer in detail later in het leerprogramma &#x200B;](../multi-step/explore-graphql-api.md).

## Gefeliciteerd!{#congratulations}

U hebt nu een externe toepassing die AEM-inhoud gebruikt met GraphQL. U kunt de code in de app React bekijken en blijven experimenteren met het wijzigen van bestaande inhoudsfragmenten.

### Volgende stappen

* [Zelfstudie voor AEM Headless starten](../multi-step/overview.md)
