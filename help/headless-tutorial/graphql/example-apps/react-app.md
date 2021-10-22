---
title: Reageren toepassing - Voorbeeld AEM zonder kop
description: Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Er is een React-toepassing beschikbaar die aantoont hoe inhoud kan worden opgezocht met behulp van de GraphQL-API's van AEM. De AEM Headless-client voor JavaScript wordt gebruikt om de GraphQL-query's uit te voeren die de toepassing van stroom voorzien.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9b1e38c8d4a0301c124c6f1607a9e4362b0e9cd1
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 0%

---


# App Reageren

Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Er is een React-toepassing beschikbaar die aantoont hoe inhoud kan worden opgezocht met behulp van de GraphQL-API&#39;s van AEM. De AEM Headless-client voor JavaScript wordt gebruikt om de GraphQL-query&#39;s uit te voeren die de toepassing van stroom voorzien.

De weergave van [broncode op GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

![Toepassing Reageren](./assets/react-screenshot.png)

Een volledige zelfstudie stap voor stap is beschikbaar [hier](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html).

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

* [11 JDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2 Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## AEM

De toepassing is ontworpen om verbinding te maken met een AEM **Auteur** of **Publiceren** milieu met de meest recente introductie van de [WKND-referentiesite](https://github.com/adobe/aem-guides-wknd/releases/latest) geïnstalleerd.

* [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html)

We raden aan [het opstellen van de plaats van de Verwijzing WKND aan een milieu van de Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). Een lokale instelling die [de SDK van AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) of [AEM 6.5 QuickStart-jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) kan ook worden gebruikt.

## Hoe wordt het gebruikt

1. Klonen met `aem-guides-wknd-graphql` opslagplaats:

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Bewerk de `aem-guides-wknd/react-app/.env.development` en zorg ervoor dat `REACT_APP_HOST_URI` verwijst naar de AEM van uw doel. Werk de authentificatiemethode bij (als het verbinden met een auteursinstantie).

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   ...
   ```

1. Open een terminal en voer de opdrachten uit:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```
1. Een nieuw browservenster moet worden geladen op [http://localhost:3000](http://localhost:3000)
1. Op de toepassing moet een lijst met avonturen van de WKND-referentiesite worden weergegeven.

## De code

Hieronder volgt een korte samenvatting van de belangrijke bestanden en code die worden gebruikt om de toepassing te besturen. U vindt de volledige code op [GitHub](https://github.com/adobe/aem-guides-wknd-graphql).

### AEM headless-client voor JavaScript

De [AEM headless-client](https://github.com/adobe/aem-headless-client-js) wordt gebruikt om de vraag uit te voeren GraphQL. De AEM Zwaardeloze Cliënt verstrekt twee methodes om vragen uit te voeren, [`runQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunqueryquery-options--promiseany) en [`runPersistedQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany).

`runQuery` Voert een standaardvraag GraphQL voor AEM inhoud uit en is het gemeenschappelijkste type van vraaglooppas.

[Blijvende query&#39;s](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) zijn een eigenschap in AEM die de resultaten van een vraag GraphQL in het voorgeheugen onderbrengt en dan het resultaat beschikbaar over GET. De aanhoudende Vragen zouden voor gemeenschappelijke vragen moeten worden gebruikt die over en over zullen worden uitgevoerd. In deze toepassing is de lijst met avonturen de eerste query die wordt uitgevoerd op het beginscherm. Dit zal een zeer populaire vraag zijn en daarom zou een voortgezette vraag moeten worden gebruikt. `runPersistedQuery` Voert een verzoek tegen een voortgeduurd vraageindpunt uit.

`src/api/useGraphQL.js` is een [React-haak](https://reactjs.org/docs/hooks-overview.html#effect-hook) die luistert naar wijzigingen in de parameter `query` en `path`. Indien `query` is leeg dan wordt een voortgezette vraag gebruikt gebaseerd op `path`. Hier ziet u waar de AEM Headless-client wordt geconstrueerd en gebruikt om gegevens op te halen.

```js
function useGraphQL(query, path) {
    let [data, setData] = useState(null);
    let [errorMessage, setErrors] = useState(null);

    useEffect(() => {
      // construct a new AEMHeadless client based on the graphQL endpoint
      const sdk = new AEMHeadless({ endpoint: REACT_APP_GRAPHQL_ENDPOINT })

      // if query is not null runQuery otherwise fall back to runPersistedQuery
      const request = query ? sdk.runQuery.bind(sdk) : sdk.runPersistedQuery.bind(sdk);

      request(query || path)
        .then(({ data, errors }) => {
          //If there are errors in the response set the error message
          if(errors) {
            setErrors(mapErrors(errors));
          }
          //If data in the response set the data as the results
          if(data) {
            setData(data);
          }
        })
        .catch((error) => {
          setErrors(error);
        });
    }, [query, path]);

    return {data, errorMessage}
}
```

### Adventure-inhoud

De app geeft vooral een lijst met avonturen weer en geeft de gebruikers de mogelijkheid om in de details van een avontuur te klikken.

`Adventures.js` - Geeft een kaartlijst met avonturen weer.  In de oorspronkelijke staat wordt gebruikgemaakt van [Blijvende query&#39;s](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) die [voorverpakt](https://github.com/adobe/aem-guides-wknd/tree/master/ui.content/src/main/content/jcr_root/conf/wknd/settings/graphql/persistentQueries/adventures-all/_jcr_content) met de WKND-referentiesite. Het eindpunt is `/wknd/adventures-all`. Er zijn verscheidene knopen die een gebruiker toestaan om met het filtreren resultaten te experimenteren die op een activiteit worden gebaseerd:

```javascript
function filterQuery(activity) {
  return `
    {
      adventureList (filter: {
        adventureActivity: {
          _expressions: [
            {
              value: "${activity}"
            }
          ]
        }
      }){
        items {
          _path
        adventureTitle
        adventurePrice
        adventureTripLength
        adventurePrimaryImage {
          ... on ImageRef {
            _path
            mimeType
            width
            height
          }
        }
      }
    }
  }
  `;
}
```

`AdventureDetail.js` - Toont een detailmening van het avontuur. Maakt een graphQL vraag die op de weg aan het avontuur wordt gebaseerd die van url wordt ontleed:

```javascript
//parse the content fragment from the url
const contentFragmentPath = props.location.pathname.substring(props.match.url.length);
...
function adventureDetailQuery(_path) {
  return `{
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
      }
    }
  }
  `;
}
```

### Omgevingsvariabelen

Meerdere [omgevingsvariabelen](https://create-react-app.dev/docs/adding-custom-environment-variables) worden gebruikt door dit project om met een AEM milieu te verbinden. De standaardinstelling maakt verbinding met een AEM auteursomgeving die wordt uitgevoerd op http://localhost:4502. Als u dit gedrag wilt wijzigen, werkt u de `.env.development` bestand dienovereenkomstig:

* `REACT_APP_HOST_URI=http://localhost:4502` - Instellen op AEM doelhost
* `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json` - Het pad naar het eindpunt van GraphQL instellen
* `REACT_APP_AUTH_METHOD=` - De voorkeursverificatiemethode. Optioneel wordt standaard geen verificatie gebruikt.
   * `service-token` - Service token-uitwisseling gebruiken voor Cloud Env PROD
   * `dev-token` - Dev-token gebruiken voor lokale ontwikkeling met Cloud Env
   * `basic` - gebruiker/pas gebruiken voor lokale ontwikkeling met Local Author Env
   * leeg laten om geen verificatiemethode te gebruiken
* `REACT_APP_AUTHORIZATION=admin:admin` - Stel de basisverificatiegegevens in die u wilt gebruiken als u verbinding maakt met een AEM-auteuromgeving (alleen voor ontwikkeling). Als u verbinding maakt met een publicatieomgeving, is deze instelling niet nodig.
* `REACT_APP_DEV_TOKEN` - Dev token string. Naast Basic-auth (user:pass) kunt u naast de externe instantie een verbinding maken met behulp van Betover-auth met DEV-token vanuit de Cloud-console
* `REACT_APP_SERVICE_TOKEN` - Pad naar service-tokenbestand. Als u verbinding wilt maken met een externe instantie, kan verificatie ook worden uitgevoerd met het servicetoken (het bestand downloaden vanaf de Cloud-console)

### Proxy-API-verzoeken

Bij gebruik van de webpack-ontwikkelingsserver (`npm start`) het project berust op een [proxyinstelling](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) gebruiken `http-proxy-middleware`. Het bestand is geconfigureerd op [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) en is gebaseerd op verschillende aangepaste omgevingsvariabelen die zijn ingesteld op `.env` en `.env.development`.

Als het verbinden met een AEM auteursmilieu, moet de overeenkomstige authentificatiemethode worden gevormd.

### CORS - het delen van bronnen van kruisoorsprong

Dit project is gebaseerd op een CORS-configuratie die wordt uitgevoerd in de AEM en gaat ervan uit dat de toepassing wordt uitgevoerd op http://localhost:3000 in de ontwikkelingsmodus. De [Configuratie van het COR](https://github.com/adobe/aem-guides-wknd/blob/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) maakt deel uit van de [WKND-referentiesite](https://github.com/adobe/aem-guides-wknd).

![CORS-configuratie](assets/cross-origin-resource-sharing-configuration.png)

*Voorbeeld van CORS-configuratie voor auteuromgeving*