---
title: De SDK AEM Headless gebruiken
description: Leer hoe u GraphQL-query's maakt met de AEM Headless SDK.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
duration: 200
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# AEM headless SDK

De AEM Headless SDK is een verzameling bibliotheken die door clients kunnen worden gebruikt om via HTTP snel en eenvoudig te communiceren met AEM Headless API&#39;s.

De AEM Headless SDK is beschikbaar voor verschillende platforms:

+ [ AEM Koploze SDK voor cliënt-zijbrowsers (JavaScript) ](https://github.com/adobe/aem-headless-client-js)
+ [ AEM Headless SDK voor server-side/Node.js (JavaScript) ](https://github.com/adobe/aem-headless-client-nodejs)
+ [ AEM Headless SDK voor Java™ ](https://github.com/adobe/aem-headless-client-java)

## Blijvende GraphQL-query&#39;s

Het vragen van AEM het gebruiken van GraphQL die voortgeduurde vragen (in tegenstelling tot [ cliënt-bepaalde vragen van GraphQL ](#graphl-queries)) gebruiken staat ontwikkelaars toe om een vraag (maar niet zijn resultaten) in AEM voort te zetten, en dan de vraag te verzoeken om door naam worden uitgevoerd. De aangehouden vragen zijn gelijkaardig aan het concept opgeslagen procedures in SQL gegevensbestanden.

Persisted query&#39;s zijn krachtiger dan door de client gedefinieerde GraphQL query&#39;s, aangezien persisted query&#39;s worden uitgevoerd met HTTP-GET, die in cache kunnen worden opgeslagen op de CDN- en AEM Dispatcher-lagen. Blijvende query&#39;s zijn ook van kracht, definiëren een API en ontkoppelen de noodzaak voor de ontwikkelaar om de details van elk Content Fragment Model te begrijpen.

### Codevoorbeelden{#persisted-graphql-queries-code-examples}

Hieronder volgen voorbeelden van code voor het uitvoeren van een GraphQL-query die wordt uitgevoerd tegen AEM.

+++ JavaScript-voorbeeld

Installeer [@adobe/aem-headless-client-js ](https://github.com/adobe/aem-headless-client-js) door het `npm install` bevel van de wortel van uw project van Node.js in werking te stellen.

```
$ npm i @adobe/aem-headless-client-js
```

Dit codevoorbeeld toont hoe te om AEM te vragen gebruikend [@adobe/aem-headless-cliënt-js ](https://github.com/adobe/aem-headless-client-js) npm module gebruikend `async/await` syntaxis. De AEM Zwaartepunt SDK voor JavaScript steunt ook [ Promise syntaxis ](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

Deze code gaat ervan uit dat er een voortgezette query met de naam `wknd/adventureNames` is gemaakt op AEM auteur en gepubliceerd naar AEM Publish.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json',  // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Uses the AEM Headless SDK to execute a persisted query with optional query variables.

 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
export async function executePersistedQuery(persistedQueryName, queryParameters) {
    let data;
    let errors;

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName, queryParameters);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
        errors = e;
    }

    return { data, errors };
};

// Execute the persisted query using its name 'wknd-shared/adventures-by-slug' and optional query variables
let { data, errors } = executePersistedQuery('wknd-shared/adventures-by-slug', { "slug": "bali-surf-camp" });
```

+++

+++ UseEffect(..) Reageren voorbeeld

Installeer [@adobe/aem-headless-cliënt-js ](https://github.com/adobe/aem-headless-client-js) door het `npm install` bevel van de wortel van uw React project in werking te stellen.

```
$ npm i @adobe/aem-headless-client-js
```

Dit codevoorbeeld toont hoe te om [ React useEffect (..) te gebruiken haak ](https://reactjs.org/docs/hooks-effect.html) om een asynchrone vraag aan AEM GraphQL uit te voeren.

Het gebruik van `useEffect` om de asynchrone GraphQL-aanroep in React te maken is nuttig omdat:

1. Het verstrekt synchrone omslag voor de asynchrone vraag aan AEM.
1. Het vermindert onnodig AEM.

Deze code gaat ervan uit dat er een voortgezette query met de naam `wknd-shared/adventure-by-slug` is gemaakt op AEM auteur en gepubliceerd naar AEM Publish met behulp van GraphiQL.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';
import { useEffect, useState } from "react";

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json'         // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Private, shared function that invokes the AEM Headless client. 
 * React components/views will invoke GraphQL via the custom React useEffect hooks defined below.
 * 
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  return { data, err };
}

/**
 * Calls the 'wknd-shared/adventure-by-slug' and provided the {slug} as the persisted query's `slug` parameter.
 *
 * @param {String!} slug the unique slug used to specify the adventure to return
 * @returns a JSON object representing the adventure
 */
export function useAdventureBySlug(slug) {
  const [adventure, setAdventure] = useState(null);
  const [errors, setErrors] = useState(null);

  useEffect(() => {
    async function fetchData() {
      // The key is the variable name as defined in the persisted query, and may not match the model's field name
      const queryParameters = { slug: slug };
      
      // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
      const { data, err } = await fetchPersistedQuery(
        "wknd-shared/adventure-by-slug",
        queryParameters
      );

      if (err) {
        // Capture errors from the HTTP request
        setErrors(err);
      } else if (data?.adventureList?.items?.length === 1) {
        // Set the adventure data after data validation (there should only be 1 matching adventure)
        setAdventure(data.adventureList.items[0]);
      } else {
        // Set an error if no adventure could be found
        setErrors(`Cannot find adventure with slug: ${slug}`);
      }
    }
    fetchData();
  }, [slug]);

  return { adventure, errors };
}
```

Roep de aangepaste React `useEffect` -haak aan vanaf een andere locatie in een React-component.

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

U kunt nieuwe `useEffect` haken maken voor elke doorlopende query die de React-app gebruikt.

+++

<p> </p>

## GraphQL-query&#39;s

AEM steunt cliënt-bepaalde vragen van GraphQL, nochtans is het AEM beste praktijken om [ voortgeduurde vragen van GraphQL ](#persisted-graphql-queries) te gebruiken.

## Webpack 5+

De AEM Headless JS SDK heeft afhankelijkheden van `util` die standaard niet in Webpack 5+ is opgenomen. Als u Webpack 5+ gebruikt, en de volgende fout ontvangt:

```
Compiled with problems:
× ERROR in ./node_modules/@adobe/aio-lib-core-errors/src/AioCoreSDKErrorWrapper.js 12:13-28
Module not found: Error: Can't resolve 'util' in '/Users/me/Code/wknd-headless-examples/node_modules/@adobe/aio-lib-core-errors/src'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
    - add a fallback 'resolve.fallback: { "util": require.resolve("util/") }'
    - install 'util'
If you don't want to include a polyfill, you can use an empty module like this:
    resolve.fallback: { "util": false }
```

Voeg het volgende `devDependencies` toe aan het `package.json` -bestand:

```json
  "devDependencies": {
    "buffer": "npm:buffer@^6.0.3",
    "crypto": "npm:crypto-browserify@^3.12.0",
    "http": "npm:stream-http@^3.2.0",
    "https": "npm:https-browserify@^1.0.0",
    "stream": "npm:stream-browserify@^3.0.0",
    "util": "npm:util@^0.12.5",
    "zlib": "npm:browserify-zlib@^0.2.0"
  },
```

Voer vervolgens `npm install` uit om de afhankelijkheden te installeren.
