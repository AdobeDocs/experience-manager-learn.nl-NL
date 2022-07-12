---
title: De SDK AEM Headless gebruiken
description: Leer hoe te om vragen te maken GraphQL gebruikend de AEM Headless SDK.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
source-git-commit: 68970493802c7194bcb3ac3ac9ee10dbfb0fc55d
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# AEM headless SDK

De AEM Headless SDK is een verzameling bibliotheken die door clients kunnen worden gebruikt om via HTTP snel en eenvoudig te communiceren met AEM Headless API&#39;s.

De AEM Headless SDK is beschikbaar voor verschillende platforms:

+ [AEM Headless SDK voor clientbrowsers (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [AEM Headless SDK voor server-side/Node.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [AEM headless SDK voor Java™](https://github.com/adobe/aem-headless-client-java)

## GraphQL-query&#39;s

AEM steunt cliënt-bepaalde vragen GraphQL, nochtans is het AEM beste praktijken aan gebruik [voortgeduurde vragen GraphQL](#persisted-graphql-queries).

## Blijvende query&#39;s voor GraphQL

Het vragen AEM het gebruiken van GraphQL die gepersisteerde vragen (in tegenstelling tot [client-defined GraphQL-query&#39;s](#graphl-queries)) staat ontwikkelaars toe om een vraag (maar niet zijn resultaten) in AEM voort te zetten, en dan te verzoeken om de vraag die door naam moet worden uitgevoerd. De aangehouden vragen zijn gelijkaardig aan het concept opgeslagen procedures in SQL gegevensbestanden.

De gepersisteerde vragen zijn krachtiger dan cliënt-bepaalde vragen GraphQL, aangezien de voortgeduurde vragen gebruikend de GET van HTTP worden uitgevoerd, die cache-able bij CDN en AEM de rijen van de Verzender is. Blijvende query&#39;s zijn ook van kracht, definiëren een API en ontkoppelen de noodzaak voor de ontwikkelaar om de details van elk Content Fragment Model te begrijpen.

### Codevoorbeelden{#persisted-graphql-queries-code-examples}

Hieronder volgen codevoorbeelden van de manier waarop een GraphQL-voortgezette query op AEM moet worden uitgevoerd.

+++ JavaScript-voorbeeld

Installeer de [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) door de `npm install` bevel van de wortel van uw project Node.js.

```
$ npm i @adobe/aem-headless-client-js
```

In dit codevoorbeeld wordt getoond hoe u een query kunt uitvoeren AEM het [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) npm-module gebruiken `async/await` syntaxis. De AEM Headless SDK voor JavaScript ondersteunt ook [Promise syntaxis](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

In deze code wordt ervan uitgegaan dat de query met de naam doorloopt `wknd/adventureNames` is gemaakt op AEM-auteur en gepubliceerd naar AEM-publicatie.

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

Installeer de [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) door de `npm install` bevel van de wortel van uw project van de Reactie.

```
$ npm i @adobe/aem-headless-client-js
```

In dit codevoorbeeld wordt getoond hoe u de [UseEffect(..) Reageren haak](https://reactjs.org/docs/hooks-effect.html) om een asynchrone vraag aan AEM GraphQL uit te voeren.

Gebruiken `useEffect` om de asynchrone Vraag GraphQL in React te maken is nuttig omdat:

1. Het verstrekt synchrone omslag voor de asynchrone vraag aan AEM.
1. Het vermindert onnodig AEM.

In deze code wordt ervan uitgegaan dat de query met de naam doorloopt `wknd-shared/adventure-by-slug` is gemaakt op AEM-auteur en gepubliceerd naar AEM Publish met behulp van GraphiQL.

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

Aangepaste reactie aanroepen `useEffect` haak van elders in een component van de Reactie.

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

Nieuw `useEffect` U kunt haken maken voor elke doorlopende query die de React-app gebruikt.

+++

<p> </p>
