---
title: Overzicht van AEM API's
description: Leer meer over de verschillende typen API's in Adobe Experience Manager (AEM) en begrijp welke API u moet kiezen voor uw integratie.
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-17425
thumbnail: KT-17425.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 23b2be0d-a8d4-4521-96ba-78b70f4e9cba
source-git-commit: bb4f9982263a15f18b9f39b1577b61310dfbe643
workflow-type: tm+mt
source-wordcount: '998'
ht-degree: 0%

---

# Overzicht van AEM API&#39;s{#aem-apis-overview}

Leer meer over de verschillende typen API&#39;s in Adobe Experience Manager (AEM) en begrijp welke API u moet kiezen voor uw integratie.

Ontwikkelaars kunnen een groot aantal API&#39;s gebruiken om inhoud, elementen en formulieren in AEM te maken, lezen, bij te werken en te verwijderen. Met deze API&#39;s kunnen ontwikkelaars aangepaste toepassingen maken die communiceren met AEM.

Laten we de verschillende typen API&#39;s in AEM bekijken en begrijpen welke API u voor uw integratie moet kiezen.

## Typen AEM API&#39;s{#types-of-aem-apis}

AEM biedt de volgende API&#39;s aan voor interactie met de auteur en voor het publiceren van servicetypen.

| AEM API-type | Beschrijving | Beschikbaarheid | Hoofdletters gebruiken | API-voorbeelden |
| --- | --- | --- | --- | --- |
| AEM API&#39;s die zijn gebaseerd op OpenAPI | Gestandaardiseerde, machineleesbare API&#39;s voor Assets, Sites en Forms. | **slechts AEM as a Cloud Service** | API-eerste ontwikkeling, moderne toepassingen | [ de Auteur API van Assets ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/), [ Omslagen API ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/), [ AEM Sites API ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/), [ Forms Acrobat Services ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) en anderen |
| RESTful-API&#39;s | Traditionele REST-eindpunten voor interactie met AEM-bronnen. | AEM 6.X, AEM as a Cloud Service | CRUD-bewerkingen, moderne toepassingen | [ HTTP API van Assets ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), [ het REST API van het Werkschema ](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api), [ Exporter JSON voor de Diensten van de Inhoud ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter) en anderen |
| GraphQL API&#39;s | Geoptimaliseerd voor het efficiënt ophalen van gestructureerde inhoud met flexibele query&#39;s. | AEM 6.X, AEM as a Cloud Service | CMS, SPA&#39;s, mobiele apps zonder koppen | [ GraphQL API ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| Traditionele (niet-RESTful) API&#39;s | Oudere API&#39;s zoals JCR, Sling Models, Query Builder en andere. | AEM 6.X, AEM as a Cloud Service | Verouderde integratie, achterwaartse compatibiliteit | [ de Bouwer API van de Vraag ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api) en anderen |

Voor meer details, zie de [ Adobe Experience Manager as a Cloud Service APIs ](https://developer.adobe.com/experience-cloud/experience-manager-apis/) pagina.

## Welke API moet worden gekozen{#which-api-to-choose}

Houd rekening met de volgende factoren wanneer u een API voor uw integratie selecteert:

- **Geval van het Gebruik**: Bepaal of AEM API uw gebruiksgeval steunt. Waar mogelijk, _gebruik op openAPI-Gebaseerde AEM APIs_, aangezien zij een gestandaardiseerde, moderne benadering verstrekken om met AEM in wisselwerking te staan. Als API&#39;s op basis van OpenAPI&#39;s niet beschikbaar zijn, kunt u overwegen RESTful API&#39;s of GraphQL API&#39;s te gebruiken en als laatste redmiddel traditionele API&#39;s.

- **Verenigbaarheid**: Zorg ervoor dat geselecteerde API met uw versie van AEM compatibel is. Bijvoorbeeld, _op openAPI-Gebaseerde AEM APIs zijn exclusief aan AEM as a Cloud Service_ en zijn niet beschikbaar in AEM 6.X.

- **het Type van Dienst van AEM: Auteur vs. publiceer**: De keus van API hangt ook van af of het op de Auteur of de Publish dienst loopt, aangezien hun toegangsmodellen verschillend zijn. De AEM Author-service wordt gebruikt voor het maken van inhoud en vereist altijd verificatie. De AEM-publicatieservice wordt gebruikt voor de levering van inhoud en vereist mogelijk geen verificatie, afhankelijk van het gebruiksgeval.

- **Authentificatie**: Verifieer dat API de authentificatiemethode steunt u van plan bent te gebruiken. Bijvoorbeeld:
   - **op openAPI-Gebaseerde AEM APIs**: steun OAuth 2.0 authentificatie, met inbegrip van de Ontvankelijkheden van de Cliënt (server-aan-server), de Code van de Vergunning (de Toepassing van het Web), en Sleutel van het Bewijs voor de Uitwisseling van de Code (Enige App van de Pagina) subsidietypes. Andere AEM API&#39;s bieden geen ondersteuning voor OAuth 2.0-verificatie.
   - **RESTful APIs**: steun de Symbolische authentificatie van het Web JSON (JWT), weet ook als symbolisch-gebaseerde authentificatie.

## Verschil tussen JSON Web Token (JWT) en OAuth 2.0{#difference-between-jwt-and-oauth}

Vergelijk JSON Web Token (JWT) en OAuth 2.0, twee gemeenschappelijke authentificatiemechanismen die in AEM APIs worden gebruikt:

| Functie | JSON Web Token (JWT) | OAuth 2.0 |
| --- | --- | --- |
| Gebruikt in | RESTful-API&#39;s | AEM API&#39;s die zijn gebaseerd op OpenAPI&#39;s (niet ondersteund in RESTful of andere API&#39;s) |
| Doel | Serviceverificatie | Gebruiker- of serviceverificatie |
| Gebruikersinteractie | Geen gebruikersinteractie vereist | Gebruikersinteractie vereist voor machtigingscode en toepassingstypen voor één pagina |
| Meest geschikt voor | Server-aan-server API vraag | Beveiligde, geoorloofde toegang voor apps en gebruikers |
| Vereiste informatie | Persoonlijke sleutel voor ondertekening van JWT | Client ID and Client Secret for OAuth 2.0 |
| Tokenvervaldatum | Kortstondig en vaak vernieuwbaar | Het toegangstoken is van korte duur. Vernieuwt-teken is lang en gebruikt om een nieuw toegang-token te krijgen |
| Credentials Management | [ AEM Developer Console ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console) | [ Adobe Developer Console ](https://developer.adobe.com/developer-console/) |

## AEM API&#39;s die zijn gebaseerd op OpenAPI

Leer meer over OpenAPI-Gebaseerde AEM APIs en de belangrijke concepten om tot Adobe APIs in de [ op OpenAPI-Gebaseerde gids van AEM toegang te hebben APIs ](./openapis/overview.md).

### Gevallen gebruiken

<!-- CARDS
{target = _self}

* ./openapis/use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./openapis/assets/s2s/OAuth-S2S.png}
* ./openapis/use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./openapis/assets/web-app/OAuth-WebApp.png} 
* ./openapis/use-cases/invoke-api-using-oauth-single-page-app.md
  {title = Invoke API using OAuth Single Page App}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom Single Page App (SPA) using OAuth 2.0 PKCE flow.}
  {image = ./openapis/assets/spa/OAuth-SPA.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" title="API aanroepen met Server-naar-server verificatie" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./openapis/assets/s2s/OAuth-S2S.png" alt="API aanroepen met Server-naar-server verificatie"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="API aanroepen met Server-naar-server verificatie"> Oproep API gebruikend Server-aan-Server authentificatie </a>
                    </p>
                    <p class="is-size-6">Leer hoe u op OpenAPI gebaseerde AEM API's aanroept vanuit een aangepaste NodeJS-toepassing met OAuth Server-to-Server-verificatie.</p>
                </div>
                <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" title="API aanroepen met webtoepassingsverificatie" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./openapis/assets/web-app/OAuth-WebApp.png" alt="API aanroepen met webtoepassingsverificatie"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="API aanroepen met webtoepassingsverificatie"> Oproep API gebruikend de authentificatie van de App van het Web </a>
                    </p>
                    <p class="is-size-6">Leer hoe u op OpenAPI gebaseerde AEM API's aanroept vanuit een aangepaste webtoepassing met OAuth Web App-verificatie.</p>
                </div>
                <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using OAuth Single Page App">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./openapis/use-cases/invoke-api-using-oauth-single-page-app.md" title="API aanroepen met OAuth-app voor één pagina" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./openapis/assets/spa/OAuth-SPA.png" alt="API aanroepen met OAuth-app voor één pagina"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./openapis/use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" title="API aanroepen met OAuth-app voor één pagina"> OAuth roepen API </a>
                    </p>
                    <p class="is-size-6">Leer hoe u op OpenAPI gebaseerde AEM API's aanroept vanuit een aangepaste Single Page App (SPA) met OAuth 2.0 PKCE-stroom.</p>
                </div>
                <a href="./openapis/use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



## GraphQL API&#39;s - Voorbeelden

Leer meer over GraphQL APIs en hoe te om hen in [ te gebruiken die met de Zetel van AEM worden begonnen - GraphQL ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview)

### Gevallen gebruiken

<!-- CARDS
{target = _self}

* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app
  {title = Single Page Application (SPA)}
  {description = Learn how to build a Single Page Application (SPA) that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/react-app-card.png}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps
  {title = Mobile App}
  {description = Learn how to build a mobile app that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/ios-app-card.png}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component
  {title = Web Component}
  {description = Learn how to build a web component that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/web-component-card.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single Page Application (SPA)">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" title="Single Page Application (SPA)" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/react-app-card.png" alt="Single Page Application (SPA)"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" target="_self" rel="referrer" title="Single Page Application (SPA)"> Enige Toepassing van de Pagina (SPA) </a>
                    </p>
                    <p class="is-size-6">Leer hoe u een toepassing voor één pagina (SPA) maakt die inhoud van AEM ophaalt met behulp van GraphQL API's.</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Mobile App">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" title="Mobiele app" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/ios-app-card.png" alt="Mobiele app"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" target="_self" rel="referrer" title="Mobiele app"> Mobiele Toepassing </a>
                    </p>
                    <p class="is-size-6">Leer hoe u een mobiele app maakt die inhoud van AEM ophaalt met GraphQL API's.</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" title="Webcomponent" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-component-card.png" alt="Webcomponent"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" target="_self" rel="referrer" title="Webcomponent"> Component van het Web </a>
                    </p>
                    <p class="is-size-6">Leer hoe u een webcomponent maakt die inhoud van AEM ophaalt met GraphQL API's.</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## RESTful-API&#39;s - Voorbeelden

Leer meer over RESTful APIs, zoals [ HTTP API van Assets ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets) en [ JSON Exporter ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter).

### Gevallen gebruiken

<!-- CARDS
{target = _self}

* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview
  {title = Using Content Services for Headless App}
  {description = Learn how to build a native mobile app that fetches content from AEM using Content Services RESTful APIs.}
  {image = ./assets/RESTful-Content-Service.png}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview
  {title = Token-based Authentication for RESTful APIs}
  {description = Learn how to invoke RESTful APIs using JSON Web Token (JWT) authentication.}
  {image = ./assets/RESTful-TokenAuth.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Using Content Services for Headless App">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" title="Inhoudsservices gebruiken voor Headless App" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/RESTful-Content-Service.png" alt="Inhoudsservices gebruiken voor Headless App"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" target="_self" rel="referrer" title="Inhoudsservices gebruiken voor Headless App"> Gebruikend de Diensten van de Inhoud voor Hoofdloze App </a>
                    </p>
                    <p class="is-size-6">Leer hoe u een systeemeigen mobiele app maakt die inhoud van AEM ophaalt met RESTful-API's van Content Services.</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Token-based Authentication for RESTful APIs">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" title="Token-based Authentificatie voor RESTful APIs" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/RESTful-TokenAuth.png" alt="Token-based Authentificatie voor RESTful APIs"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" target="_self" rel="referrer" title="Token-based Authentificatie voor RESTful APIs"> Op token-gebaseerde Authentificatie voor RESTful APIs </a>
                    </p>
                    <p class="is-size-6">Leer hoe u RESTful API's aanroept met JSON Web Token (JWT)-verificatie.</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


