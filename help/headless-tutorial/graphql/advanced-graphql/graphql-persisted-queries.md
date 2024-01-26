---
title: Blijvende GraphQL-query's - Geavanceerde concepten van AEM headless - GraphQL
description: In dit hoofdstuk van Geavanceerde concepten van Adobe Experience Manager (AEM) Headless, leer hoe te om voortgezette vragen van GraphQL met parameters tot stand te brengen en bij te werken. Leer hoe te om cache-controle parameters in persisted query's over te gaan.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 6a8e90ae-0765-4066-9df4-a3e4d2cda285
duration: 221
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 0%

---

# Blijvende GraphQL-query&#39;s

Blijvende query&#39;s zijn query&#39;s die zijn opgeslagen op de Adobe Experience Manager-server (AEM). Clients kunnen een HTTP-GET-aanvraag met de naam van de query verzenden om deze uit te voeren. Het voordeel van deze aanpak is cacheability. Terwijl client-side GraphQL query&#39;s ook kunnen worden uitgevoerd met HTTP POST request, dat niet in cache kan worden geplaatst, kunnen persisted query&#39;s in cache worden geplaatst door HTTP caches of een CDN, waardoor de prestaties verbeteren. De gepersisteerde vragen staan u toe om uw verzoeken te vereenvoudigen en veiligheid te verbeteren omdat uw vragen op de server worden ingekapseld en de AEM beheerder volledige controle over hen heeft. Het is **beste praktijken en hoogst aanbevolen** als u doorlopende query&#39;s wilt gebruiken wanneer u met de AEM GraphQL API werkt.

In het vorige hoofdstuk hebt u enkele geavanceerde GraphQL-query&#39;s onderzocht om gegevens voor de WKND-app te verzamelen. In dit hoofdstuk, blijft u de vragen aan AEM en leert hoe te om geheim voorgeheugencontrole op persisted query te gebruiken.

## Vereisten {#prerequisites}

Dit document is onderdeel van een zelfstudie met meerdere onderdelen. Zorg ervoor dat de [vorige hoofdstuk](explore-graphql-api.md) is voltooid voordat u verdergaat met dit hoofdstuk.

## Doelstellingen {#objectives}

Leer in dit hoofdstuk hoe te:

* GraphQL-query&#39;s met parameters blijven gebruiken
* De cache-control parameters van het gebruik met persistente vragen

## Controleren _Aangehouden GraphQL-query&#39;s_ configuratie-instelling

Laten we eens kijken naar: _Aangehouden GraphQL-query&#39;s_ worden toegelaten voor het project van de Plaats WKND in uw AEM instantie.

1. Navigeren naar **Gereedschappen** > **Algemeen** > **Configuratiebrowser**.

1. Selecteren **WKND gedeeld** selecteert u vervolgens **Eigenschappen** in de bovenste navigatiebalk om configuratie-eigenschappen te openen. Op de pagina van de Eigenschappen van de Configuratie, zou u moeten zien dat **Blijvende GraphQL-query&#39;s** machtiging is ingeschakeld.

   ![Configuratieeigenschappen](assets/graphql-persisted-queries/configuration-properties.png)

## Handhaaf GraphQL-query&#39;s met het gereedschap GraphiQL Explorer

In deze sectie, laten wij de vraag van GraphQL voortzetten die later in de cliënttoepassing wordt gebruikt om de gegevens van het Fragment van de Inhoud van de Avontuur te halen en terug te geven.

1. Ga de volgende vraag in de Ontdekkingsreiziger GraphiQL in:

   ```graphql
   query getAdventureDetailsBySlug($slug: String!) {
   adventureList(filter: {slug: {_expressions: [{value: $slug}]}}) {
       items {
       _path
       title
       activity
       adventureType
       price
       tripLength
       groupSize
       difficulty
       primaryImage {
           ... on ImageRef {
           _path
           mimeType
           width
           height
           }
       }
       description {
           html
           json
       }
       itinerary {
           html
           json
       }
       location {
           _path
           name
           description {
           html
           json
           }
           contactInfo {
           phone
           email
           }
           locationImage {
           ... on ImageRef {
               _path
           }
           }
           weatherBySeason
           address {
           streetAddress
           city
           state
           zipCode
           country
           }
       }
       instructorTeam {
           _metadata {
           stringMetadata {
               name
               value
           }
           }
           teamFoundingDate
           description {
           json
           }
           teamMembers {
           fullName
           contactInfo {
               phone
               email
           }
           profilePicture {
               ... on ImageRef {
               _path
               }
           }
           instructorExperienceLevel
           skills
           biography {
               html
           }
           }
       }
       administrator {
           fullName
           contactInfo {
           phone
           email
           }
           biography {
           html
           }
       }
       }
       _references {
       ... on ImageRef {
           _path
           mimeType
       }
       ... on LocationModel {
           _path
           __typename
       }
       }
   }
   }
   ```

   Controleer of de query werkt voordat u deze opslaat.

1. Tik op Opslaan als en voer de volgende keer in `adventure-details-by-slug` als Naam query.

   ![GraphQL-query behouden](assets/graphql-persisted-queries/persist-graphql-query.png)

## Doorlopende query uitvoeren met variabelen door speciale tekens te coderen

Laten we begrijpen hoe voortgezette query&#39;s met variabelen worden uitgevoerd door de toepassing aan de clientzijde door de speciale tekens te coderen.

Om een voortgezette vraag uit te voeren, doet de cliënttoepassing een verzoek van de GET gebruikend de volgende syntaxis:

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>
```

Een voortgezette query uitvoeren _met een variabele_ De bovenstaande syntaxis verandert in:

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>;variable1=value1;variable2=value2
```

De speciale tekens zoals puntkomma&#39;s (;), gelijkteken (=), slashes (/) en spatie moeten worden omgezet om de overeenkomstige UTF-8-codering te gebruiken.

Door het `getAllAdventureDetailsBySlug` vraag van de bevel-lijn terminal, herzien wij deze concepten in actie.

1. Open de GrahiQL Explorer en klik op **ovalen** (...) naast de permanente query `getAllAdventureDetailsBySlug`en klik vervolgens op **URL kopiëren**. Plak gekopieerde URL in een tekstblok en ziet er als volgt uit:

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=
   ```

1. Toevoegen `yosemite-backpacking` als variabele waarde

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=yosemite-backpacking
   ```

1. De puntkomma&#39;s (;) en speciale tekens voor het gelijkteken (=) coderen

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

1. Een opdrachtregelterminal openen en gebruiken [Krol](https://curl.se/) de query uitvoeren

   ```shell
   $ curl -X GET http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

>[!TIP]
>
>    Als het runnen van de bovengenoemde vraag tegen het milieu van de AEM Auteur, moet u de geloofsbrieven verzenden. Zie [Toegangstoken lokale ontwikkeling](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token.html) ter demonstratie daarvan en [De AEM-API aanroepen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#calling-the-aem-api) voor meer informatie.

Ook, revisie [Hoe te om een Verlengde vraag uit te voeren](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#execute-persisted-query), [Query-variabelen gebruiken](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#query-variables), en [De URL van de query coderen voor gebruik door een app](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#encoding-query-url) om aanhoudende vraaguitvoering door cliënttoepassingen te leren.

## De cache-control parameters van de update in persistente query&#39;s {#cache-control-all-adventures}

Met de AEM GraphQL API kunt u de standaardparameters voor het beheren van cache bijwerken naar uw query&#39;s om de prestaties te verbeteren. De standaardwaarden voor cachebeheer zijn:

* 60 seconden is het gebrek (maxage=60) TTL voor de cliënt (bijvoorbeeld, browser)

* 7200 seconden is het gebrek (s-maxage=7200) TTL voor de Verzender en CDN; ook gekend als gedeelde geheime voorgeheugens

Gebruik de `adventures-all` vraag om de geheim voorgeheugen-controle parameters bij te werken. De vraagreactie is groot en het is nuttig om zijn te controleren `age` in de cache. Deze voortgezette query wordt later gebruikt om de [clienttoepassing](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).

1. Open de GrahiQL Explorer en klik op **ovalen** (...) naast de permanente query klikt u op **Kopteksten** openen **Cacheconfiguratie** modal.

   ![Optie GraphQL-koptekst behouden](assets/graphql-persisted-queries/persist-graphql-header-option.png)


1. In de **Cacheconfiguratie** modaal, update `max-age` koptekstwaarde naar `600 `seconden (10 minuten) en klik vervolgens op **Opslaan**

   ![Configuratie GraphQL-cache behouden](assets/graphql-persisted-queries/persist-graphql-cache-config.png)


Controleren [Door uw doorlopende query&#39;s in cache te plaatsen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#caching-persisted-queries) voor meer informatie over standaardcache-control parameters.


## Gefeliciteerd!

Gefeliciteerd! U hebt nu geleerd hoe te om de vragen van GraphQL met parameters voort te zetten, voortgeduurde vragen bij te werken, en cache-controle parameters met voortgezette vragen te gebruiken.

## Volgende stappen

In de [volgende hoofdstuk](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md), implementeert u de aanvragen voor doorlopende query&#39;s in de WKND-app.
