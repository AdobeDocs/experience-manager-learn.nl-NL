---
title: Werken met sets met grote resultaten in AEM Headless
description: Leer hoe u met grote resultaatsets werkt met AEM Headless.
version: Experience Manager as a Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-04-14T00:00:00Z
jira: KT-13102
thumbnail: 3418381.jpeg
exl-id: 304b4d80-27bd-4336-b2ff-4b613a30f712
duration: 308
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 0%

---

# Grote resultaatsets in AEM Headless

AEM Headless GraphQL-query&#39;s kunnen grote resultaten opleveren. In dit artikel wordt beschreven hoe u met grote resultaten kunt werken in AEM Headless voor de beste prestaties voor uw toepassing.

De Hoofdloze steunen van AEM a [&#x200B; verschuiving/grens &#x200B;](#list-query) en [&#x200B; op curseur-gebaseerde paginering &#x200B;](#paginated-query) vragen aan kleinere ondergroepen van een grotere resultaatreeks. Er kunnen meerdere verzoeken worden ingediend om zoveel resultaten te verzamelen als nodig is.

In de onderstaande voorbeelden worden kleine subsets van resultaten (vier records per aanvraag) gebruikt om de technieken aan te tonen. In een echte toepassing zou u een groter aantal records per aanvraag gebruiken om de prestaties te verbeteren. 50 records per verzoek is een goede basislijn.

## Inhoudsfragmentmodel

Paginering en sortering kunnen worden gebruikt tegen elk Content Fragment Model.

## GraphQL blijft vragen

Wanneer het werken met grote datasets, zowel verschuiving en grens als curseur-gebaseerde paginering kan worden gebruikt om een specifieke ondergroep van de gegevens terug te winnen. Er zijn echter enkele verschillen tussen de twee technieken die de ene in bepaalde situaties geschikter kunnen maken dan de andere.

### Verschuiven/beperken

De vragen van de lijst, gebruikend `limit` en `offset` verstrekken een ongecompliceerde benadering die het uitgangspunt (`offset`) en het aantal verslagen specificeert om terug te winnen (`limit`). Op deze manier kan een subset van resultaten worden geselecteerd vanuit een willekeurige locatie binnen de volledige resultaatset, zoals naar een specifieke pagina met resultaten gaan. Hoewel het gemakkelijk is uit te voeren, kan het langzaam en inefficiënt zijn wanneer het behandelen van groot resultaat, aangezien het terugwinnen van vele verslagen aftasten door alle vorige verslagen vereist. Deze aanpak kan ook prestatieproblemen opleveren wanneer de verschuivingswaarde hoog is, omdat hiervoor veel resultaten moeten worden opgehaald en verwijderd.

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

De resulterende JSON-respons bevat de tweede, derde, vierde en vijfde duurste avonturen. De eerste twee avonturen in de resultaten hebben de zelfde prijs (`4500` zodat specificeert de [&#x200B; lijstvraag &#x200B;](#list-queries) avonturen met de zelfde prijs dan door titel in het stijgen orde.)

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

De op curseur-gebaseerde paginering, beschikbaar in Gepagineerde vragen, impliceert het gebruiken van een curseur (een verwijzing naar een specifiek verslag) om de volgende reeks resultaten terug te winnen. Deze benadering is efficiënter omdat het de behoefte vermijdt om door alle vorige verslagen af te tasten om de vereiste ondergroep van gegevens terug te winnen. De gepagineerde vragen zijn groot voor het herhalen door grote resultaatreeksen van het begin, aan één of ander punt in het midden, of aan het eind. De vragen van de lijst, gebruikend `limit` en `offset` verstrekken een ongecompliceerde benadering die het uitgangspunt (`offset`) en het aantal verslagen specificeert om terug te winnen (`limit`). Op deze manier kan een subset van resultaten worden geselecteerd vanuit een willekeurige locatie binnen de volledige resultaatset, zoals naar een specifieke pagina met resultaten gaan. Hoewel het gemakkelijk is uit te voeren, kan het langzaam en inefficiënt zijn wanneer het behandelen van groot resultaat, aangezien het terugwinnen van vele verslagen aftasten door alle vorige verslagen vereist. Deze aanpak kan ook prestatieproblemen opleveren wanneer de verschuivingswaarde hoog is, omdat hiervoor veel resultaten moeten worden opgehaald en verwijderd.

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

De resulterende JSON-respons bevat de tweede, derde, vierde en vijfde duurste avonturen. De eerste twee avonturen in de resultaten hebben de zelfde prijs (`4500` zodat specificeert de [&#x200B; lijstvraag &#x200B;](#list-queries) avonturen met de zelfde prijs dan door titel in het stijgen orde.)

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

De volgende set resultaten kan worden opgehaald met de parameter `after` en de waarde `endCursor` van de vorige query. Als er geen resultaten meer te halen zijn, is `hasNextPage` `false` .

##### Query-variabelen

```json
{
  "first": 3,
  "after": "NDUwMC4w...kwMzE5Njc="
}
```

## Voorbeelden Reageren

Het volgende is React voorbeelden die aantonen hoe te om [&#x200B; te gebruiken verschuiven en te beperken &#x200B;](#offset-and-limit) en [&#x200B; op curseur-gebaseerde paginering &#x200B;](#cursor-based-pagination) benaderingen. Doorgaans is het aantal resultaten per aanvraag groter, maar voor deze voorbeelden is de limiet ingesteld op 5.

### Voorbeeld van verschuiven en beperken

>[!VIDEO](https://video.tv.adobe.com/v/3418381/?quality=12&learn=on)

Met verschuiving en beperking kunnen subsets van resultaten gemakkelijk worden opgehaald en weergegeven.

#### useEffect-haak

De `useEffect` haak roept een voortgezette vraag (`adventures-by-offset-and-limit`) aan die een lijst van avonturen terugwint. De query gebruikt de parameters `offset` en `limit` om het beginpunt en het aantal resultaten op te geven dat moet worden opgehaald. De `useEffect` -haak wordt aangeroepen wanneer de `page` -waarde verandert.


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

De component gebruikt de `useOffsetLimitAdventures` haak om een lijst met avonturen op te halen. De waarde `page` wordt verhoogd en verlaagd om de volgende en vorige reeks resultaten op te halen. De `hasMore` -waarde wordt gebruikt om te bepalen of de knop Volgende pagina moet worden ingeschakeld.

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

![&#x200B; Gepagineerd voorbeeld &#x200B;](./assets/large-results/paginated-example.png)

_Elk rood vakje vertegenwoordigt een discrete gepagineerde vraag van HTTP GraphQL._

Gebruikend op curseur-gebaseerde paginering, kunnen de grote resultaatreeksen gemakkelijk worden teruggewonnen en worden getoond, door de resultaten incrementeel te verzamelen en hen aan de bestaande resultaten aan te passen.


#### UseEffect-haak

De `useEffect` haak roept een voortgezette vraag (`adventures-by-paginated`) aan die een lijst van avonturen terugwint. De query gebruikt de parameters `first` en `after` om het aantal resultaten op te geven dat moet worden opgehaald en de cursor waaruit moet worden gestart. `fetchData` wordt voortdurend herhaald, waarbij de volgende reeks gepagineerde resultaten wordt verzameld, totdat er geen resultaten meer zijn om op te halen.

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

De component gebruikt de `usePaginatedAdventures` haak om een lijst met avonturen op te halen. De waarde `queryCount` wordt gebruikt om het aantal HTTP-aanvragen weer te geven dat is gemaakt om de lijst met avonturen op te halen.

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
