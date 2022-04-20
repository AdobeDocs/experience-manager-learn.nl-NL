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
source-git-commit: 4966a48c29ae1b5d0664cb43feeb4ad94f43b4e1
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---


# AEM headless SDK

De AEM Headless SDK is een verzameling bibliotheken die door clients kunnen worden gebruikt om via HTTP snel en eenvoudig te communiceren met AEM Headless API&#39;s.

De AEM Headless SDK is beschikbaar voor verschillende platforms:

+ [AEM Headless SDK voor clientbrowsers (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [AEM Headless SDK voor server-side/Node.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [AEM headless SDK voor Java™](https://github.com/adobe/aem-headless-client-java)

## GraphQL-query&#39;s

Het vragen AEM het gebruiken van GraphQL die vragen (in tegenstelling tot [voortgeduurde vragen GraphQL](#persisted-graphql-queries)) stelt ontwikkelaars in staat query&#39;s in code te definiëren en op te geven welke inhoud van AEM moet worden aangevraagd.

GraphQL-query&#39;s zijn meestal minder presterend dan aanhoudend query&#39;s, omdat ze worden uitgevoerd met HTTP-POST, wat minder cache-able is bij de CDN- en AEM Dispatcher-lagen.

### Codevoorbeelden{#graphql-queries-code-examples}

Hieronder ziet u codevoorbeelden voor het uitvoeren van een GraphQL-query op AEM.

+++ JavaScript-voorbeeld

Installeer de [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) door de `npm install` bevel van de wortel van uw project Node.js.

```
$ npm i @adobe/aem-headless-client-js
```

In dit codevoorbeeld wordt getoond hoe u een query kunt uitvoeren AEM het [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) npm-module gebruiken `async/await` syntaxis. De AEM Headless SDK voor JavaScript ondersteunt ook [Promise syntaxis](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchQuery(query, queryParams) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runQuery(query, queryParams);
        // The GraphQL data is stored on the response's data key
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Define the GraphQL query in-code
const adventureNamesQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let data = fetchQuery(adventureNamesQuery);
```

+++


+++ UseEffect(..) Reageren voorbeeld

Installeer de [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) door de `npm install` bevel van de wortel van uw project van de Reactie.

```
$ npm i @adobe/aem-headless-client-js
```

In dit codevoorbeeld wordt getoond hoe u de [UseEffect(..) Reageren haak](https://reactjs.org/docs/hooks-effect.html) om een asynchrone vraag aan AEM GraphQL uit te voeren.

Gebruiken `useEffect` om de asynchrone Vraag GraphQL in React te maken is nuttig omdat het:

1. Verstrekt synchrone omslag voor de asynchrone vraag aan AEM.
1. Vermindert onnodig AEM.

```javascript
// src/useGraphQL.js

import { useState, useEffect } from 'react';
import AEMHeadless from '@adobe/aem-headless-client-js';

const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/global/endpoint.json'       // The AEM GraphQL endpoint, this can be pulled out to env variables
});

export function useGraphQL(query, queryParams) {
    let [data, setData] = useState(null);
    let [errors, setErrors] = useState(null);
  
    useEffect(() => {
        async function fetchData() {
            try {
                const response = await aemHeadlessClient.runQuery(query, queryParams);
                setData(response.data);
            } catch(error) {
                setErrors(error);
            };
        }
        fetchData();
    }, [query, queryParams]);
  
    return { data, errors }
}
```

Importeren en gebruiken `useGraphQL` haak in de React component aan vraag AEM.

```javascript
import useGraphQL from 'useGraphQL';

const adventuresQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let { data, errors } = useGraphQL(adventuresQuery);
```

+++

<p> </p>

## Blijvende query&#39;s voor GraphQL

Het vragen AEM het gebruiken van GraphQL die gepersisteerde vragen (in tegenstelling tot [Reguliere GraphQL-query&#39;s](#graphl-queries)) staat ontwikkelaars toe om een vraag (maar niet zijn resultaten) in AEM voort te zetten, en dan te verzoeken om de vraag die door naam moet worden uitgevoerd. De aangehouden vragen is gelijkaardig aan het concept opgeslagen procedures in SQL gegevensbestanden.

De gepersisteerde vragen neigen uitvoeriger te zijn dan regelmatige vragen GraphQL, aangezien de voortgezette vragen gebruikend de GET van HTTP worden uitgevoerd, die meer cache-able bij CDN en AEM de rijen van de Verzender is. Blijvende query&#39;s zijn ook van kracht, definiëren een API en ontkoppelen de noodzaak voor de ontwikkelaar om de details van elk Content Fragment Model te begrijpen.

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
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchPersistedQuery(persistedQueryName) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Execute the persisted query using its name
let data = fetchPersistedQuery('wknd/adventureNames');
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

In deze code wordt ervan uitgegaan dat de query met de naam doorloopt `wknd/adventureNames` is gemaakt op AEM-auteur en gepubliceerd naar AEM-publicatie.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd/endpoint.json'         // The AEM GraphQL endpoint
})

export function fetchPersistedQuery(persistedQueryName) {
  let [data, setData] = useState(null);
  let [errors, setErrors] = useState(null);

  useEffect(async () => {
    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax 
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        setData(response.data);
    }.catch((error) => {
        setErrors(error);
    });

  }, [persistedQueryName]);

  return { data, errors }
}
```

Deze code ergens anders in de antwoordcode aanroepen.

```javascript
import useGraphL from '...';

let { data, errors } = fetchPersistedQuery('wknd/adventureNames');
```

+++

<p> </p>
