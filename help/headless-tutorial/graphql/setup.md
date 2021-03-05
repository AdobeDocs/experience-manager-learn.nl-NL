---
title: Snelle installatie - Aan de slag met AEM headless - GraphQL
description: Ga aan de slag met Adobe Experience Manager (AEM) en GraphQL. Installeer de AEM SDK, voeg voorbeeldinhoud toe en implementeer een toepassing die inhoud uit AEM gebruikt met de GraphQL-API's. Zie hoe AEM machten omni-channel ervaart.
sub-product: sites
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6386
thumbnail: KT-6386.jpg
feature: Inhoudsfragmenten, GraphQL API's
topic: Koploos, inhoudsbeheer
role: Developer
level: Begin
translation-type: tm+mt
source-git-commit: db9f4d09dcc83f85c8d02d94c383fa456af88c24
workflow-type: tm+mt
source-wordcount: '1832'
ht-degree: 0%

---


# Quick Setup {#setup}

Dit hoofdstuk biedt een snelle opstelling van een lokale milieu om een externe toepassing te zien die inhoud van AEM gebruikend AEM GraphQL APIs verbruikt. De recentere hoofdstukken in het leerprogramma zullen van deze opstelling bouwen.

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

* [11 JDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2 Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## Doelstellingen {#objectives}

1. Download en installeer de AEM SDK.
1. Download en installeer voorbeeldinhoud van de WKND-referentiesite.
1. Download en installeer een voorbeeld-app om inhoud te verbruiken met de GraphQL-API&#39;s.

## De AEM SDK {#aem-sdk} installeren

Deze zelfstudie gebruikt de [AEM als Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en#aem-as-a-cloud-service-sdk) om AEM GraphQL APIs te onderzoeken. In deze sectie vindt u een snelle handleiding voor het installeren van de AEM SDK en het uitvoeren ervan in de modus Auteur. Een meer gedetailleerde gids voor vestiging een lokale ontwikkelomgeving [kan hier worden gevonden](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up).

>[!NOTE]
>
> Het is ook mogelijk om de zelfstudie te volgen met een AEM als een Cloud Service-omgeving. De zelfstudie bevat aanvullende notities voor het gebruik van een Cloud-omgeving.

1. Navigeer naar **[Software Distribution Portal](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM als Cloud Service** en download de nieuwste versie van de **AEM SDK**.

   ![Software Distribution Portal](assets/setup/software-distribution-portal-download.png)

   >[!CAUTION]
   >
   > De eigenschap GraphQL wordt toegelaten door gebrek slechts op AEM SDK van 2021-02-04 of nieuwer.

1. Pak de download uit en kopieer de QuickStart-jar (`aem-sdk-quickstart-XXX.jar`) naar een toegewezen map, d.w.z. `~/aem-sdk/author`.
1. Wijzig de naam van het jar-bestand in `aem-author-p4502.jar`.

   De naam `author` geeft aan dat de QuickStart-jar zal starten in de modus Auteur. `p4502` specificeert dat de server van Quickstart op haven 4502 zal lopen.

1. Open een nieuw terminalvenster en navigeer naar de map die het jar-bestand bevat. Voer de volgende opdracht uit om de AEM te installeren en te starten:

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. Geef een beheerderswachtwoord op als `admin`. Om het even welk admin wachtwoord is aanvaardbaar, nochtans adviseert zijn om het gebrek voor lokale ontwikkeling te gebruiken om de behoefte te verminderen om te vormen.
1. Na een paar minuten wordt de AEM geïnstalleerd en wordt een nieuw browservenster geopend op [http://localhost:4502](http://localhost:4502).
1. Meld u aan met de gebruikersnaam `admin` en het wachtwoord `admin`.

## Voorbeeldinhoud en GraphQL-eindpunten {#wknd-site-content-endpoints} installeren

Voorbeeldinhoud van de **WKND-referentiesite** wordt geïnstalleerd om de zelfstudie te versnellen. De WKND is een fictief levensstijl merk, vaak gebruikt in combinatie met AEM training.

De WKND-verwijzingssite bevat configuraties die nodig zijn om een [GraphQL-eindpunt](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=en#graphql-aem-endpoint) weer te geven. Volg de gedocumenteerde stappen om [de GraphQL-eindpunten](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=en#graphql-aem-endpoint) in uw klantproject op te nemen in een real-world implementatie. Een [CORS](#cors-config) is ook verpakt als onderdeel van de WKND-site. Een configuratie CORS is nodig om toegang tot een externe toepassing te verlenen. Hieronder vindt u meer informatie over [CORS](#cors-config).

1. Download het nieuwste gecompileerde AEM Pakket voor WKND-site: [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Download de standaardversie die compatibel is met AEM als Cloud Service en **niet** de `classic`-versie.

1. Navigeer in het menu **AEM Start** naar **Tools** > **Implementatie** > **Pakketten**.

   ![Naar pakketten navigeren](assets/setup/navigate-to-packages.png)

1. Klik op **Pakket uploaden** en kies het WKND-pakket dat u in de vorige stap hebt gedownload. Klik op **Installeren** om het pakket te installeren.

1. Navigeer in het menu **AEM Start** naar **Elementen** > **Bestanden**.
1. Klik door de mappen om naar **WKND Site** > **Engels** > **Adventures** te navigeren.

   ![Mapweergave van avonturen](assets/setup/folder-view-adventures.png)

   Dit is een map van alle assets die bestaan uit de verschillende avonturen die worden bevorderd door het WKND-merk. Dit zijn onder andere traditionele mediatypen zoals afbeeldingen en video, en media die specifiek zijn voor AEM zoals **Contentfragmenten**.

1. Klik in de map **Downhill Skiing Wyoming** en klik op de kaart **Wyoming Content Fragment** downloaden:

   ![Skiende inhoudfragmentkaart downloaden](assets/setup/downhill-skiing-cntent-fragment.png)

1. De interface van de Content Fragment Editor opent voor het Wyoming-avontuur Downhill Skiing.

   ![Inhoudsfragment afnemen](assets/setup/down-hillskiing-fragment.png)

   Houd er rekening mee dat het fragment wordt gedefinieerd in verschillende velden, zoals **Title**, **Description** en **Activity**.

   **Inhoudsfragmentaties** zijn een van de manieren waarop inhoud in AEM kan worden beheerd. Inhoudsfragment is herbruikbare, presentatieonafhankelijke inhoud die bestaat uit gestructureerde gegevenselementen zoals tekst, RTF-tekst, datums of verwijzingen naar andere Inhoudsfragmenten. Inhoudsfragmenten worden later in de zelfstudie gedetailleerder besproken.

1. Klik op **Annuleren** om het fragment te sluiten. Bekijk de andere mappen en ontdek de andere Adventure-content.

>[!NOTE]
>
> Als het gebruiken van een milieu van de Cloud Service zie de documentatie voor hoe te om [een codebasis zoals de plaats van de Verwijzing WKND aan een milieu van de Cloud Service ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=en#deploying) op te stellen.

## Voorbeeldapp installeren{#sample-app}

Een van de doelstellingen van deze zelfstudie is om te tonen hoe u AEM inhoud uit een externe toepassing kunt gebruiken met de GraphQL API&#39;s. Deze zelfstudie gebruikt een voorbeeld van React App dat gedeeltelijk is voltooid om de zelfstudie te versnellen. Dezelfde lessen en concepten gelden voor apps die zijn gemaakt met iOS, Android of een ander platform. De React-app is opzettelijk eenvoudig om onnodige complexiteit te voorkomen. het is niet bedoeld als referentie - uitvoering .

1. Open een nieuw terminalvenster en kloon de startertak van de zelfstudie met Git:

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Open het bestand `.env.development` op `aem-guides-wknd-graphql/react-app/.env.development` in de IDE van uw keuze. Controleer of de regel `REACT_APP_AUTHORIZATION` geen opmerkingen bevat en of het bestand er als volgt uitziet:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   Zorg ervoor dat `React_APP_HOST_URI` overeenkomt met uw lokale AEM. In dit hoofdstuk wordt de React-app rechtstreeks verbonden met de AEM **Author**-omgeving. **Voor** authoromgevingen is standaard verificatie vereist, zodat onze app verbinding maakt als de  `admin` gebruiker. Dit komt vaak voor tijdens de ontwikkeling, omdat we hiermee snel wijzigingen kunnen aanbrengen in de AEM en deze direct kunnen zien in de app.

   >[!NOTE]
   >
   > In een productiescenario zal de App met een AEM **Publish** milieu verbinden. Dit wordt meer gedetailleerd behandeld in het hoofdstuk [Productie-implementatie](production-deployment.md).

1. Navigeer naar de map `aem-guides-wknd-graphql/react-app`. Installeer en start de app:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. De app wordt automatisch gestart in [http://localhost:3000](http://localhost:3000) in een nieuw browservenster.

   ![Starter-app reageren](assets/setup/react-starter-app.png)

   Een lijst van de huidige inhoud van het Avontuur van AEM zou moeten worden getoond.

1. Klik op een van de avontuurafbeeldingen om de details van het avontuur weer te geven. Er wordt een verzoek AEM om de details voor een avontuur te retourneren.

   ![Adventure Details-weergave](assets/setup/adventure-details-view.png)

1. Gebruik de ontwikkelaarsgereedschappen van de browser om de **Network**-verzoeken te inspecteren. Bekijk de **XHR** verzoeken en bekijk meerdere verzoeken van POSTEN aan `/content/graphql/global/endpoint.json`, het eindpunt GraphQL dat voor AEM wordt gevormd.

   ![GraphQL Endpoint XHR-verzoek](assets/setup/endpoint-gql.png)

1. U kunt de parameters en de JSON-reactie ook bekijken door de netwerkaanvraag te inspecteren. Het kan handig zijn om een browserextensie zoals [GraphQL Network](https://chrome.google.com/webstore/detail/graphql-network/igbmhmnkobkjalekgiehijefpkdemocm) voor Chrome te installeren voor een beter begrip van de query en de reactie.

   ![GraphQL-netwerkextensie](assets/setup/GraphQL-extension.png)

   *Het gebruiken van de uitbreiding van het Chroom GraphQL Netwerk*

## Een inhoudsfragment wijzigen

Nu de React-app actief is, kunt u de inhoud in AEM bijwerken en de wijziging weerspiegelen in de app.

1. Navigeer naar AEM [http://localhost:4502](http://localhost:4502).
1. Navigeer naar **Middelen** > **Bestanden** > **WKND-site** > **Engels** > **Adventures** > **[Bali Surf-kamp](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**

   ![Bali Surf Camp-map](assets/setup/bali-surf-camp-folder.png)

1. Klik in het inhoudsfragment **Bali Surf Camp** om de Inhoudsfragmenteditor te openen.
1. Wijzig de **Title** en de **Description** van het avontuur

   ![Inhoudsfragment wijzigen](assets/setup/modify-content-fragment-bali.png)

1. Klik **Opslaan** om de wijzigingen op te slaan.
1. Navigeer terug naar de React-app op [http://localhost:3000](http://localhost:3000) en vernieuw om uw wijzigingen te zien:

   ![Bijgewerkt Bali Surf Camp Adventure](assets/setup/overnight-bali-surf-camp-changes.png)

## Het gereedschap GraphiQL installeren {#install-graphiql}

[](https://github.com/graphql/graphiql) GraphiQLis een ontwikkeltool die alleen nodig is in omgevingen op een lager niveau, zoals een ontwikkelomgeving of een lokale instantie. Met GraphiQL IDE kunt u snel de geretourneerde query&#39;s en gegevens testen en verfijnen. GraphiQL biedt ook eenvoudige toegang tot de documentatie, waardoor u gemakkelijk kunt leren welke methoden beschikbaar zijn.

1. Navigeer naar de **[Softwaredistributieportal](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM als een Cloud Service**.
1. Zoek naar &quot;GraphiQL&quot; (vergeet niet **i** op te nemen in **GraphiQL**.
1. Download de nieuwste **GraphiQL Content Package v.x.x.x**

   ![GraphiQL-pakket downloaden](assets/explore-graphql-api/software-distribution.png)

   Het ZIP-bestand is een AEM pakket dat rechtstreeks kan worden geïnstalleerd.

1. Navigeer in het menu **AEM Start** naar **Tools** > **Implementatie** > **Pakketten**.
1. Klik op **Pakket uploaden** en kies het pakket dat u in de vorige stap hebt gedownload. Klik op **Installeren** om het pakket te installeren.

   ![GraphiQL-pakket installeren](assets/explore-graphql-api/install-graphiql-package.png)
1. Navigeer naar de GraphiQL IDE op [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html) en ontdek de GraphQL API&#39;s.

   >[!NOTE]
   >
   > De GraphiQL-tool en GraphQL-API worden later in de zelfstudie [gedetailleerder besproken.](./explore-graphql-api.md)

## Gefeliciteerd! {#congratulations}

U hebt nu een externe toepassing die AEM inhoud gebruikt met GraphQL. Bekijk de code in de React-app en experimenteer met het wijzigen van bestaande contentfragmenten.

## Volgende stappen {#next-steps}

In het volgende hoofdstuk, [Defining Content Fragment Models](content-fragment-models.md), leer hoe u inhoud modelleert en een schema bouwt met **Content Fragment Models**. U gaat bestaande modellen evalueren en een nieuw model maken. U leert ook over de verschillende gegevenstypen die kunnen worden gebruikt om een schema te definiëren als onderdeel van het model.

## (Bonus) CORS-configuratie {#cors-config}

AEM, die standaard beveiligd zijn, blokkeert aanvragen van verschillende oorsprong, zodat onbevoegde toepassingen geen verbinding kunnen maken met de inhoud en de inhoud ervan niet kunnen opzoeken.

Om de React-app van deze zelfstudie geschikt te maken voor interactie met AEM GraphQL API-eindpunten, is een configuratie voor het delen van bronnen tussen verschillende bronnen gedefinieerd in het WKND Site-verwijzingsproject.

![Configuratie voor het delen van bronnen tussen verschillende bronnen](assets/setup/cross-origin-resource-sharing-configuration.png)

De geïmplementeerde configuratie weergeven:

1. Navigeer naar de webconsole van AEM SDK op [http://localhost:4502/system/console](http://localhost:4502/system/console).

   >[!NOTE]
   >
   > De webconsole is alleen beschikbaar op de SDK. In een AEM als Cloud Service kan deze informatie worden bekeken via [de Developer Console](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html).

1. Klik in het bovenste menu op **OSGI** > **Configuration** om alle [OSGi Configurations](http://localhost:4502/system/console/configMgr) weer te geven.
1. Schuif omlaag op de pagina **Bron van graniet met kruisoorsprong delen**.
1. Klik op de configuratie voor `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`.
1. De volgende velden zijn bijgewerkt:
   * Toegestane oorsprong (Regex): `http://localhost:.*`
      * Hiermee worden alle lokale hostverbindingen toegestaan.
   * Toegestane paden: `/content/graphql/global/endpoint.json`
      * Dit is het enige momenteel gevormde eindpunt GraphQL. Als beste praktijk zouden de CvdR-configuraties zo restrictief mogelijk moeten zijn.
   * Toegestane methoden: `GET`, `HEAD`, `POST`
      * Alleen `POST` is vereist voor GraphQL, maar de andere methoden kunnen handig zijn bij het werken met AEM zonder kop.
   * Ondersteunde kopteksten: **Autorisatie** is toegevoegd om basisverificatie door te geven aan de auteuromgeving.
   * Ondersteunt referenties: `Yes`
      * Dit is nodig omdat onze React-app zal communiceren met de beveiligde GraphQL-eindpunten op de AEM Author-service.

Deze configuratie en de eindpunten GraphQL zijn een deel van het AEM WKND project. U kunt alle [OSGi-configuraties hier](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig) bekijken.
