---
title: AEM API's instellen die zijn gebaseerd op OpenAPI
description: Leer hoe u uw AEM as a Cloud Service-omgeving instelt om toegang tot de op OpenAPI gebaseerde AEM API's mogelijk te maken.
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-17426
thumbnail: KT-17426.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 1df4c816-b354-4803-bb6c-49aa7d7404c6
source-git-commit: b17e228c33ff2e3f2ee2d7e13da65a648c5df79d
workflow-type: tm+mt
source-wordcount: '1291'
ht-degree: 0%

---

# AEM API&#39;s instellen die zijn gebaseerd op OpenAPI

Leer hoe u uw AEM as a Cloud Service-omgeving instelt om toegang tot de op OpenAPI gebaseerde AEM API&#39;s mogelijk te maken.

>[!AVAILABILITY]
>
>AEM API&#39;s die zijn gebaseerd op OpenAPI zijn beschikbaar als onderdeel van een vroegtijdig toegangsprogramma. Als u in de toegang tot van hen geinteresseerd bent, moedigen wij u aan om [ aem-apis@adobe.com ](mailto:aem-apis@adobe.com) met een beschrijving van uw gebruiksgeval te e-mailen.

Het installatieproces op hoog niveau omvat de volgende stappen:

1. Modernisering van de AEM as a Cloud Service-omgeving.
1. Toegang tot AEM API&#39;s inschakelen.
1. Maak een Adobe Developer Console-project (ADC).
1. ADC-project configureren
1. Vorm de instantie van AEM om de mededeling van het Project van ADC toe te laten.

## Modernisering van de AEM as a Cloud Service-omgeving

De modernisering van het milieu van AEM as a Cloud Service is een eenmalige milieuactiviteit die de volgende stappen omvat:

- Update aan de Versie van AEM **2024.10.18459.20241031T210302Z** of later.
- Voeg er nieuwe productprofielen aan toe als de omgeving is gemaakt vóór de release 2024.10.18459.20241031T210302Z.

### AEM-instantie bijwerken

Om de instantie van AEM bij te werken, in de Adobe [ Cloud Manager _van de Milieu_ sectie van 1} {, selecteer het _ellips_ pictogram naast de milieunaam en selecteer **optie van de Update**.](https://my.cloudmanager.adobe.com/)

![ Update AEM instantie ](./assets/setup/update-aem-instance.png)

Dan klik **voorleggen** knoop en stel _gesuggereerde_ FullstackPipeline in werking.

![ Uitgezochte recentste versie van de versie van AEM ](./assets/setup/select-latest-aem-release.png)

In mijn geval, wordt de FullstackPipeline genoemd **Dev:: Fullstack-Deploy**, en het milieu van AEM wordt genoemd **wknd-programma-dev**. Uw namen kunnen verschillen.

### Nieuwe productprofielen toevoegen

Om nieuwe Profielen van het Product aan de instantie van AEM toe te voegen, in de 2} sectie van de Milieu&#39;s ](https://my.cloudmanager.adobe.com/) van Adobe __, selecteer het _ellips_ pictogram naast de milieunaam en selecteer **toevoegen de optie van de Profielen van het Product**.[

![ voeg nieuwe Profielen van het Product ](./assets/setup/add-new-product-profiles.png) toe

U kunt de onlangs toegevoegde Profielen van het Product herzien door op het _ellips_ pictogram naast de milieunaam te klikken en **te selecteren beheert Toegang** > **Profielen van de Auteur**.

Het _Admin Console_ venster toont de onlangs toegevoegde Profielen van het Product.

![ Herzie nieuwe Profielen van het Product ](./assets/setup/review-new-product-profiles.png)

Met de bovenstaande stappen wordt de modernisering van de AEM as a Cloud Service-omgeving voltooid.

## Toegang tot AEM API&#39;s inschakelen{#enable-aem-apis-access}

De aanwezigheid van de _nieuwe Profielen van het Product_ laat op OpenAPI-Gebaseerde toegang van AEM API in Adobe Developer Console (ADC) toe. Rappel dat [ Adobe Developer Console (ADC) ](./overview.md#accessing-adobe-apis-and-related-concepts) de ontwikkelaarshub voor de toegang tot van Adobe APIs, SDKs, gebeurtenissen in real time, serverless functies, en meer is.

De onlangs toegevoegde Profielen van het Product worden geassocieerd met de _Diensten_ die _gebruikersgroepen van AEM met vooraf bepaalde Lijsten van het Toegangsbeheer (ACLs)_ vertegenwoordigen. De _Diensten_ worden gebruikt om het niveau van toegang tot AEM APIs te controleren.

U kunt de _Diensten_ ook selecteren of schrappen verbonden aan het Profiel van het Product om het niveau van toegang te verminderen of te verhogen.

Herzie de vereniging door op het _pictogram van de Details van de Mening_ naast de naam van het Profiel van het Product te klikken.

{de diensten van het 0} Overzicht verbonden aan het Profiel van het Product ](./assets/setup/review-services-associated-with-product-profile.png)![

Door gebrek, wordt de **AEM Assets API Gebruikers** Dienst niet geassocieerd met om het even welk Profiel van het Product. Laat ons het met de onlangs toegevoegde **Gebruikers van de Medewerker van AEM Assets associëren - auteur - Programma XXX - Milieu XXX** Profiel van het Product. Na deze vereniging, kan de 20} ActivaAuteur API van het Project ADC _opstelling de gewenste Server-aan-Server authentificatie en de authentificatierekening van het project associëren ADC (die in volgende stap) met het Profiel van het Product wordt gecreeerd._

![ associeerde de Dienst van de Gebruikers van AEM Assets API met het Profiel van het Product ](./assets/setup/associate-aem-assets-api-users-service-with-product-profile.png)

>[!IMPORTANT]
>
>De bovenstaande stap is van essentieel belang om de Server-to-Server-verificatie voor de AEM Assets API in te schakelen. Zonder deze koppeling kan de AEM Assets API niet worden gebruikt met de Server-to-Server verificatiemethode.

## Adobe Developer Console-project (ADC) maken

Het ADC-project wordt gebruikt om de gewenste API&#39;s toe te voegen, de verificatie ervan in te stellen en de verificatieaccount aan het productprofiel te koppelen.

Een ADC-project maken:

1. Login aan [ Adobe Developer Console ](https://developer.adobe.com/console) gebruikend uw Adobe ID.

   ![ Adobe Developer Console ](./assets/setup/adobe-developer-console.png)

1. Van de _Snelle sectie van het Begin_, klik op **creeer nieuwe project** knoop.

   ![ creeer nieuw project ](./assets/setup/create-new-project.png)

1. Het leidt tot een nieuw project met de standaardnaam.

   ![ Nieuw gecreeerd project ](./assets/setup/new-project-created.png)

1. Bewerk de projectnaam door **te klikken uitgeeft project** knoop in de hoogste juiste hoek. Verstrek een betekenisvolle naam en klik **sparen**.

   ![ geef projectnaam ](./assets/setup/edit-project-name.png) uit

## ADC-project configureren

Nadat u het ADC-project hebt gemaakt, moet u de gewenste AEM API&#39;s toevoegen, de verificatie ervan instellen en de verificatieaccount aan het productprofiel koppelen.

1. Om AEM APIs toe te voegen, klik op **voeg API** knoop toe.

   ![ voeg API ](./assets/s2s/add-api.png) toe

1. In _voeg API_ dialoog toe, filter door _Experience Cloud_ en selecteer gewenste AEM API. Bijvoorbeeld, in dit geval, wordt _Auteur API van Activa_ geselecteerd.

   ![ voeg AEM API ](./assets/s2s/add-aem-api.png) toe

1. Daarna, in _vorm API_ dialoog, selecteer de gewenste authentificatieoptie. Bijvoorbeeld, in dit geval, wordt de **server-aan-Server** authentificatieoptie geselecteerd.

   ![ Uitgezochte authentificatie ](./assets/s2s/select-authentication.png)

   De server-aan-server authentificatie is ideaal voor de backenddiensten die API toegang zonder gebruikersinteractie vereisen. De opties Web App en Single Page App voor verificatie zijn geschikt voor toepassingen die API-toegang nodig hebben namens gebruikers. Zie [ Verschil tussen Server-aan-Server van OAuth vs Web App vs Één enkele geloofsbrieven van de Toepassing van de Pagina ](./overview.md#difference-between-oauth-server-to-server-vs-web-app-vs-single-page-app-credentials) voor meer informatie.

1. Indien nodig kunt u de naam van de API wijzigen om deze gemakkelijker te kunnen identificeren. Voor demo-doeleinden wordt de standaardnaam gebruikt.

   ![ noem referentie ](./assets/s2s/rename-credential.png) anders

1. In dit geval, is de authentificatiemethode **OAuth Server-aan-Server** zodat moet u de authentificatierekening met het Profiel van het Product associëren. Selecteer de **Gebruikers van de Medewerker van AEM Assets - auteur - Programma XXX - het Profiel van het Product van Milieu XXX** en klik **sparen**.

   ![ Uitgezochte Profiel van het Product ](./assets/s2s/select-product-profile.png)

1. Controleer de AEM API- en verificatieconfiguratie.

   ![ de configuratie van AEM API ](./assets/s2s/aem-api-configuration.png)

   ![ configuratie van de Authentificatie ](./assets/s2s/authentication-configuration.png)

Als u **of** OAuth App **authentificatiemethode van de Toepassing van de Toepassing van de enig-Pagina** kiest, wordt de vereniging van het Profiel van het Product niet veroorzaakt maar de toepassing richt URI wordt vereist. De URI voor omleiding van de toepassing wordt gebruikt om de gebruiker na verificatie met een machtigingscode om te leiden naar de toepassing. In de relevante zelfstudies voor gebruiksgevallen worden dergelijke configuraties beschreven die specifiek zijn voor verificatie.

## De AEM-instantie configureren om ADC-projectcommunicatie in te schakelen

Om de ClientID van het Project van ADC aan communicatie met de instantie van AEM toe te laten, moet u de instantie van AEM vormen.

Hiervoor definieert u de API-configuratie in het `config.yaml` -bestand van
het AEM-project en de implementatie ervan met behulp van de Config Pipeline in de Cloud Manager.

1. Zoek in AEM Project het `config.yaml` -bestand in de map `config` of maak dit.

   ![ plaats config YAML ](./assets/setup/locate-config-yaml.png)

1. Voeg de volgende configuratie toe aan het `config.yaml` dossier.

   ```yaml
   kind: "API"
   version: "1.0"
   metadata: 
       envTypes: ["dev", "stage", "prod"]
   data:
       allowedClientIDs:
           author:
           - "<ADC Project's Credentials ClientID>"
   ```

   Vervang `<ADC Project's Credentials ClientID>` door de werkelijke ClientID van de Credentials-waarde van het ADC-project. Het API eindpunt dat in dit leerprogramma wordt gebruikt is beschikbaar slechts op de auteurslaag, maar voor andere APIs, kan yaml config ook a _hebben publiceren_ of _voorproef_ knoop.

   >[!CAUTION]
   >
   > Voor demo-doeleinden wordt dezelfde ClientID gebruikt voor alle omgevingen. Het wordt aanbevolen afzonderlijke ClientID per omgeving (dev, stage, prod) te gebruiken voor betere beveiliging en controle.

1. Leg de configuratiewijzigingen vast en duw de wijzigingen naar de externe Git-opslagplaats waar de Cloud Manager-pijplijn op is aangesloten.

1. Implementeer de bovenstaande wijzigingen met behulp van de configuratiegids in de Cloud Manager. Het `config.yaml` -bestand kan ook worden geïnstalleerd in een RDE met behulp van opdrachtregelprogramma&#39;s.

   ![ stel config.yaml ](./assets/setup/config-pipeline.png) op

## Volgende stappen

Zodra de AEM-instantie is geconfigureerd om ADC-projectcommunicatie in te schakelen, kunt u beginnen met het gebruik van de op OpenAPI gebaseerde AEM API&#39;s. Leer hoe u de op OpenAPI gebaseerde AEM API&#39;s gebruikt met verschillende OAuth-verificatiemethoden:

<!-- CARDS
{target = _self}

* ./use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./assets/s2s/OAuth-S2S.png}
* ./use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./assets/web-app/OAuth-WebApp.png}
* ./use-cases/invoke-api-using-oauth-single-page-app.md
  {title = Invoke API using Single Page App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom Single Page App (SPA) using OAuth 2.0 PKCE flow.}
  {image = ./assets/spa/OAuth-SPA.png}  
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-s2s.md" title="API aanroepen met Server-naar-server verificatie" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/s2s/OAuth-S2S.png" alt="API aanroepen met Server-naar-server verificatie"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="API aanroepen met Server-naar-server verificatie"> Oproep API gebruikend Server-aan-Server authentificatie </a>
                    </p>
                    <p class="is-size-6">Leer hoe u op OpenAPI gebaseerde AEM API's aanroept vanuit een aangepaste NodeJS-toepassing met OAuth Server-to-Server-verificatie.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-web-app.md" title="API aanroepen met webtoepassingsverificatie" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-app/OAuth-WebApp.png" alt="API aanroepen met webtoepassingsverificatie"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="API aanroepen met webtoepassingsverificatie"> Oproep API gebruikend de authentificatie van de App van het Web </a>
                    </p>
                    <p class="is-size-6">Leer hoe u op OpenAPI gebaseerde AEM API's aanroept vanuit een aangepaste webtoepassing met OAuth Web App-verificatie.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Single Page App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" title="API aanroepen met App-verificatie met één pagina" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/spa/OAuth-SPA.png" alt="API aanroepen met App-verificatie met één pagina"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" title="API aanroepen met App-verificatie met één pagina"> Oproep API gebruikend de Enige authentificatie van de Toepassing van de Pagina </a>
                    </p>
                    <p class="is-size-6">Leer hoe u op OpenAPI gebaseerde AEM API's aanroept vanuit een aangepaste Single Page App (SPA) met OAuth 2.0 PKCE-stroom.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
