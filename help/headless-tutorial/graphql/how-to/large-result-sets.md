---
title: Hoe te met grote resultaatreeksen in AEM Zwaartepunt werken
description: Leer hoe u met grote resultaatsets werkt met AEM Headless.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-04-14T00:00:00Z
jira: KT-13102
thumbnail: 3418381.jpeg
exl-id: 304b4d80-27bd-4336-b2ff-4b613a30f712
duration: 439
source-git-commit: d7f3c5193cc53f050d24dd66705a3979fb710c36
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 0%

---

# Grote resultaatsets in AEM headless

AEM GraphQL-query&#39;s zonder koppen kunnen grote resultaten opleveren. In dit artikel wordt beschreven hoe u met grote resultaten kunt werken in AEM Headless voor de beste prestaties voor uw toepassing.

AEM Headless ondersteunt een [offset/limiet](#list-query) en [cursorpaginering](#paginated-query) vragen aan kleinere subsets van een grotere resultaatset. Er kunnen meerdere verzoeken worden ingediend om zoveel resultaten te verzamelen als nodig is.

In de onderstaande voorbeelden worden kleine subsets van resultaten (vier records per aanvraag) gebruikt om de technieken aan te tonen. In een echte toepassing zou u een groter aantal records per aanvraag gebruiken om de prestaties te verbeteren. 50 records per verzoek is een goede basislijn.

## Inhoudsfragmentmodel

Paginering en sortering kunnen worden gebruikt tegen elk Content Fragment Model.

## GraphQL blijft vragen

Wanneer het werken met grote datasets, zowel verschuiving en grens als curseur-gebaseerde paginering kan worden gebruikt om een specifieke ondergroep van de gegevens terug te winnen. Er zijn echter enkele verschillen tussen de twee technieken die de ene in bepaalde situaties geschikter kunnen maken dan de andere.

### Verschuiven/beperken

Lijstquery&#39;s gebruiken `limit` en `offset` een eenvoudige benadering bieden die het beginpunt aangeeft (`offset`) en het aantal records dat moet worden opgehaald (`limit`). Op deze manier kan een subset van resultaten worden geselecteerd vanuit een willekeurige locatie binnen de volledige resultaatset, zoals naar een specifieke pagina met resultaten gaan. Hoewel het gemakkelijk is uit te voeren, kan het langzaam en inefficiënt zijn wanneer het behandelen van groot resultaat, aangezien het terugwinnen van vele verslagen aftasten door alle vorige verslagen vereist. Deze aanpak kan ook prestatieproblemen opleveren wanneer de verschuivingswaarde hoog is, omdat hiervoor veel resultaten moeten worden opgehaald en verwijderd.

#### GraphQL-query

```graphql
# Retrieves a list of Adventures sorted price descending, and title ascending if there is the prices are the same.
query adventuresByOffetAndLimit($offset:Int!, $limit:Int) {
    adventureList(offset: $offset, limit: $limit, sort: "price DESC, title ASC", ) {
      items {
        _path
        title
        price
      }
    }
  }
```

##### Query-variabelen

```json
{
  "offset": 1,
  "limit": 4
}
```

#### GraphQL-reactie

De resulterende JSON-respons bevat de tweede, derde, vierde en vijfde duurste avonturen. De eerste twee avonturen in de resultaten hebben dezelfde prijs (`4500` dus de [lijstquery](#list-queries) verwijst naar avonturen met dezelfde prijs en wordt vervolgens in oplopende volgorde gesorteerd op titel.)

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
          "title": "Cycling Tuscany",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
          "title": "West Coast Cycling",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/surf-camp-in-costa-rica/surf-camp-costa-rica",
          "title": "Surf Camp in Costa Rica",
          "price": 3400
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/cycling-southern-utah",
          "title": "Cycling Southern Utah",
          "price": 3000
        }
      ]
    }
  }
}
```

### Gepagineerde query

De op curseur-gebaseerde paginering, beschikbaar in Gepagineerde vragen, impliceert het gebruiken van een curseur (een verwijzing naar een specifiek verslag) om de volgende reeks resultaten terug te winnen. Deze benadering is efficiënter omdat het de behoefte vermijdt om door alle vorige verslagen af te tasten om de vereiste ondergroep van gegevens terug te winnen. De gepagineerde vragen zijn groot voor het herhalen door grote resultaatreeksen van het begin, aan één of ander punt in het midden, of aan het eind. Lijstquery&#39;s gebruiken `limit` en `offset` een eenvoudige benadering bieden die het beginpunt aangeeft (`offset`) en het aantal records dat moet worden opgehaald (`limit`). Op deze manier kan een subset van resultaten worden geselecteerd vanuit een willekeurige locatie binnen de volledige resultaatset, zoals naar een specifieke pagina met resultaten gaan. Hoewel het gemakkelijk is uit te voeren, kan het langzaam en inefficiënt zijn wanneer het behandelen van groot resultaat, aangezien het terugwinnen van vele verslagen aftasten door alle vorige verslagen vereist. Deze aanpak kan ook prestatieproblemen opleveren wanneer de verschuivingswaarde hoog is, omdat hiervoor veel resultaten moeten worden opgehaald en verwijderd.

#### GraphQL-query

```graphql
# Retrieves the most expensive Adventures (sorted by title ascending if there is the prices are the same)
query adventuresByPaginated($first:Int, $after:String) {
 adventurePaginated(first: $first, after: $after, sort: "price DESC, title ASC") {
       edges {
          cursor
          node {
            _path
            title
            price
          }
        }
        pageInfo {
          endCursor
          hasNextPage
        }
    }
  }
```

##### Query-variabelen

```json
{
  "first": 3
}
```

#### GraphQL-reactie

De resulterende JSON-respons bevat de tweede, derde, vierde en vijfde duurste avonturen. De eerste twee avonturen in de resultaten hebben dezelfde prijs (`4500` dus de [lijstquery](#list-queries) verwijst naar avonturen met dezelfde prijs en wordt vervolgens in oplopende volgorde gesorteerd op titel.)

```json
{
  "data": {
    "adventurePaginated": {
      "edges": [
        {
          "cursor": "NTAwMC4...Dg0ZTUwN2FkOA==",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
            "title": "Bali Surf Camp",
            "price": 5000
          }
        },
        {
          "cursor": "SFNDUwMC4wC...gyNWUyMWQ5M2Q=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
            "title": "Cycling Tuscany",
            "price": 4500
          }
        },
        {
          "cursor": "AVUwMC4w...0ZTYzMjkwMzE5Njc=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
            "title": "West Coast Cycling",
            "price": 4500
          }
        }
      ],
      "pageInfo": {
        "endCursor": "NDUwMC4w...kwMzE5Njc=",
        "hasNextPage": true
      }
    }
  }
}
```

#### Volgende set gepagineerde resultaten

De volgende set resultaten kan worden opgehaald met de opdracht `after` en de `endCursor` waarde van de vorige query. Als er geen resultaten meer te halen zijn, `hasNextPage` is `false`.

##### Query-variabelen

```json
{
  "first": 3,
  "after": "NDUwMC4w...kwMzE5Njc="
}
```

## Voorbeelden Reageren

Hieronder volgen React-voorbeelden die aantonen hoe u kunt werken [verschuiven en beperken](#offset-and-limit) en [cursorpaginering](#cursor-based-pagination) benaderingen. Doorgaans is het aantal resultaten per aanvraag groter, maar voor deze voorbeelden is de limiet ingesteld op 5.

### Voorbeeld van verschuiven en beperken

>[!VIDEO](https://video.tv.adobe.com/v/3418381/?quality=12&learn=on)

Met verschuiving en beperking kunnen subsets van resultaten gemakkelijk worden opgehaald en weergegeven.

#### useEffect-haak

De `useEffect` de hak roept een persistente vraag (`adventures-by-offset-and-limit`) die een lijst met avonturen ophaalt. De query gebruikt de `offset` en `limit` parameters om het uitgangspunt en het aantal resultaten te specificeren om terug te winnen. De `useEffect` haak wordt aangeroepen wanneer de `page` de waarde verandert.


```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function useOffsetLimitAdventures(page, limit) {
    const [adventures, setAdventures] = useState([]);
    const [hasMore, setHasMore] = useState(true);

    useEffect(() => {
      async function fetchData() {
        const queryParameters = {
          offset: page * limit, // Calculate the offset based on the current page and the limit
          limit: limit + 1,     // Add 1 to the limit to determine if there are more adventures to fetch
        };

        // Invoke the persisted query with the offset and limit parameters
        const response = await aemHeadlessClient.runPersistedQuery(
          "wknd-shared/adventures-by-offset-and-limit",
          queryParameters
        );        
        const data = response?.data;

        if (data?.adventureList?.items?.length > 0) {
          // Collect the adventures - slice off the last item since the last item is used to determine if there are more adventures to fetch
          setAdventures([...data.adventureList.items].slice(0, limit));
          // Determine if there are more adventures to fetch
          setHasMore(data.adventureList.items.length > limit);
        } else {
          setHasMore(false);
        }
      }
      fetchData();
    }, [page]);

    return { adventures, hasMore };
}
```

#### Component

De component gebruikt de `useOffsetLimitAdventures` haak om een lijst van avonturen terug te winnen. De `page` De waarde wordt verhoogd en verlaagd om de volgende en vorige reeks resultaten op te halen. De `hasMore` wordt gebruikt om te bepalen of de volgende paginaknoop zou moeten worden toegelaten.

```javascript
import { useState } from "react";
import { useOffsetLimitAdventures } from "./api/persistedQueries";

export default function OffsetLimitAdventures() {
  const LIMIT = 5;
  const [page, setPage] = useState(0);

  let { adventures, hasMore } = useOffsetLimitAdventures(page, LIMIT);

  return (
    <section className="offsetLimit">
      <h2>Offset/limit query</h2>
      <p>Collect sub-sets of adventures using offset and limit.</p>

      <h4>Page: {page + 1}</h4>
      <p>
        Query variables:
        <em>
          <code>
            &#123; offset: {page * LIMIT}, limit: {LIMIT} &#125;
          </code>
        </em>
      </p>

      <hr />

      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure._path}>
              {adventure.title} <em>(${adventure.price})</em>
            </li>
          );
        })}
      </ul>

      <hr />

      <ul className="buttons">
        <li>
          <button disabled={page === 0} onClick={() => setPage(page - 1)}>
            Previous
          </button>
        </li>
        <li>
          <button disabled={!hasMore} onClick={() => setPage(page + 1)}>
            Next
          </button>
        </li>
      </ul>
    </section>
  );
}
```

### Voorbeeld van paginering

![Voorbeeld van paginering](./assets/large-results/paginated-example.png)

_Elk rood vak vertegenwoordigt een afzonderlijke gepagineerde HTTP GraphQL-query._

Gebruikend op curseur-gebaseerde paginering, kunnen de grote resultaatreeksen gemakkelijk worden teruggewonnen en worden getoond, door de resultaten incrementeel te verzamelen en hen aan de bestaande resultaten aan te passen.


#### UseEffect-haak

De `useEffect` de hak roept een persistente vraag (`adventures-by-paginated`) die een lijst met avonturen ophaalt. De query gebruikt de `first` en `after` parameters om het aantal resultaten op te geven dat moet worden opgehaald en de cursor waaruit moet worden gestart. `fetchData` onophoudelijk lijnen, die de volgende reeks gepagineerde resultaten verzamelen, tot er geen meer resultaten zijn om te halen.

```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function usePaginatedAdventures() {
    const LIMIT = 5;
    const [adventures, setAdventures] = useState([]);
    const [queryCount, setQueryCount] = useState(0);

    useEffect(() => {
      async function fetchData() {
        let paginatedAdventures = [];
        let paginatedCount = 0;
        let hasMore = false;
        let after = null;
        
        do {
          const response = await aemHeadlessClient.runPersistedQuery(
            "wknd-shared/adventures-by-paginated",
            {
                first: LIMIT,
                after: after
            }
          );
          // The GraphQL data is stored on the response's data field
          const data = response?.data;

          paginatedCount = paginatedCount + 1;

          if (data?.adventurePaginated?.edges?.length > 0) {
            // Add the next set page of adventures to full list of adventures
            paginatedAdventures = [...paginatedAdventures, ...data.adventurePaginated.edges];
          }

          // If there are more adventures, set the state to fetch them
          hasMore = data.adventurePaginated?.pageInfo?.hasNextPage;
          after = data.adventurePaginated.pageInfo.endCursor;

        } while (hasMore);

        setQueryCount(paginatedCount);
        setAdventures(paginatedAdventures);
      }

      fetchData();
    }, []);

    return { adventures, queryCount };
}
```

#### Component

De component gebruikt de `usePaginatedAdventures` haak om een lijst van avonturen terug te winnen. De `queryCount` Deze waarde wordt gebruikt om het aantal HTTP-aanvragen weer te geven dat is gemaakt om de lijst met avonturen op te halen.

```javascript
import { useState } from "react";
import { usePaginatedAdventures } from "./api/persistedQueries";
...
export default function PaginatedAdventures() {
  let { adventures, queryCount } = usePaginatedAdventures();

  return (
    <section className="paginated">
      <h2>Paginated query</h2>
      <p>Collect all adventures using {queryCount} cursor-paginated HTTP GraphQL requests</p>

      <hr/>
      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure.node._path}>
              {adventure.node.title} <em>(${adventure.node.price})</em>
            </li>
          );
        })}
      </ul>
      <hr/>
    </section>
  );
}
```
