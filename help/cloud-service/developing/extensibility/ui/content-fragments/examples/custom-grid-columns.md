---
title: Kolommen met het aangepaste raster in de inhoudsfragmentconsole
description: Leer hoe u een aangepaste rasterkolom kunt toevoegen aan de Content Fragment Console.
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13453
thumbnail: KT-13453.jpeg
doc-type: article
last-substantial-update: 2023-06-07T00:00:00Z
exl-id: 87143cf9-e932-4ad6-afe2-cce093c520f4
duration: 198
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# Aangepaste rasterkolommen

![ de kolom van het het douaneraster van de Console van het Fragment van de Inhoud ](./assets/custom-grid-columns/hero.png){align="center"}

U kunt aangepaste rasterkolommen toevoegen aan de Content Fragment Console met het extensiepunt `contentFragmentGrid` . In dit voorbeeld ziet u hoe u een aangepaste kolom toevoegt waarmee de pagina Inhoudsfragmenten op basis van de laatste gewijzigde datum in een leesbare indeling wordt weergegeven.

## Extensiepunt

In dit voorbeeld wordt het uitbreidingspunt `contentFragmentGrid` uitgebreid om een aangepaste kolom toe te voegen aan de Content Fragment Console.

| AEM-gebruikersinterface uitgebreid | Extensiepunt |
| ------------------------ | --------------------- | 
| [ de Console van het Fragment van de Inhoud ](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/) | [ Kolommen van het Net ](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/) |

## Voorbeeldextensie

In het volgende voorbeeld wordt een aangepaste kolom `Age` gemaakt waarmee de pagina van het inhoudsfragment in een leesbare indeling wordt weergegeven. De pagina wordt berekend vanaf de laatste gewijzigde datum van het inhoudsfragment.

De code laat zien hoe de metagegevens van het inhoudsfragment kunnen worden verkregen in het registratiebestand van de extensie en hoe de JSON-inhoud van het inhoudsfragment kan worden getransformeerd.

Dit voorbeeld gebruikt de [&#128279;](https://moment.github.io/luxon/) bibliotheek van de Luxon om de leeftijd van het Fragment van de Inhoud te berekenen, die via `npm i luxon` wordt geïnstalleerd.

### Registratie van extensies

`ExtensionRegistration.js` , toegewezen aan de route index.html, is het ingangspunt voor de uitbreiding van AEM en bepaalt:

+ De locatie van de extensie injecteert zichzelf (`contentFragmentGrid`) in de AEM-ontwerpervaring
+ De definitie van de aangepaste kolom in de functie `getColumns()`
+ De waarden voor elke aangepaste kolom, op rij

```javascript
import React from "react";
import { generatePath } from "react-router";
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";
import { Duration } from "luxon";

/**
 * Set up a in-memory cache of the custom column data.
 * This is important if the work to contain column data is expensive, such as making HTTP requests to obtain the value.
 * 
 * The caching of computed value is optional, but recommended if the work to compute the column data is expensive.
 */
const COLUMN_AGE_ID = "age";
const cache = {
  [COLUMN_AGE_ID]: {},
};

function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        contentFragmentGrid: {
          getColumns() {
            return [
              {
                id: COLUMN_AGE_ID,          // Give the column a unique ID.
                label: "Age",               // The label to display
                render: async function (fragments) {
                  // This function is run for each row in the grid.
                  // The fragments parameter is an array of all the fragments (aka each row) in the grid.
                  const context = await guestConnection.hostConnection.getRemoteApi().getSharedContext();

                  // Iterate over each fragment in the grid
                  for (const fragment of fragments) {
                    // Check if a previous pass has computed the value for this fragment.id. If it has, we can skip it.
                    if (!cache[COLUMN_AGE_ID][fragment.id]) {
                      // If the fragment.id has not been computed and cached, then compute the value and cache it.
                      cache[COLUMN_AGE_ID][fragment.id] = await computeAgeColumnValue(fragment, context);
                    }
                  }
                  // Return the populated cache of the custom column data.
                  return cache[COLUMN_AGE_ID];
                }
              },
              // Add other custom columns here...
            ];
          },
        },
      },
    });
  };
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

async function computeAgeColumnValue(fragment, context) {
  // Various data is available in the sharedContext, such as the AEM host, the IMS token, and the fragment itself.
  
  // Accessing AEM APIs requires the IMS token, which is available in the sharedContext, and also supporting CORS configurations deployed to AEM Author.
  //const aemHost = context.aemHost;
  //const accessToken = context.auth.imsToken;
  
  // Get the user's locale to format the value of the column.
  const locale = context.locale;

  // Compute the value of the column, in this case we are computing the age of the fragment from its last modified date.
  const duration = Duration.fromMillis(Date.now() - fragment.modifiedDate).rescale();

  let largestUnit = {};

  if (duration.years > 0) {
    largestUnit = {years: duration.years};
  } else if (duration.months > 0) {
    largestUnit = {months: duration.months};
  } else if (duration.days > 0) {
    largestUnit = {days: duration.days};
  } else if (duration.hours > 0) {
    largestUnit = {hours: duration.hours};
  } else if (duration.minutes > 0) {
    largestUnit = {minutes: duration.minutes};
  } else if (duration.seconds > 0) {
    largestUnit = {seconds: duration.seconds};
  }

  // Convert the largest unit of the age to human readable format.
  const columnValue = Duration.fromObject(largestUnit, {locale: locale}).toHuman();

  // Return the value of the column.
  return columnValue;
}

export default ExtensionRegistration;
```

#### Gegevens van inhoudsfragment

De methode `render(..)` in `getColumns()` wordt doorgegeven aan een array met fragmenten. Elk object in de array vertegenwoordigt een rij in het raster en bevat de volgende metagegevens over het inhoudsfragment. Deze metagegevens kunnen worden gebruikt voor populaire aangepaste kolommen in het raster.


```javascript
render: async function (fragments) {
    for (const fragment of fragments) {
        // An example value from this console log is displayed below.
        console.log(fragment);
    }
}
```

Voorbeeld van een JSON-inhoudsfragment dat beschikbaar is als element van de parameter `fragments` in de methode `render(..)` .

```json
{
    "id": "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures",
    "name": "alaskan-adventures",
    "title": "Alaskan Adventures",
    "status": "draft",
    "statusPreview": null,
    "model": {
        "name": "Article",
        "id": "/conf/wknd-shared/settings/dam/cfm/models/article"
    },
    "folderId": "/content/dam/wknd-shared/en/magazine/alaska-adventure",
    "folderName": "Alaska Adventure",
    "createdBy": "admin",
    "createdDate": 1684875665786,
    "modifiedBy": "admin",
    "modifiedDate": 1654104774889,
    "publishedBy": "",
    "publishedDate": 0,
    "locale": "",
    "main": true,
    "translations": {
        "locale": "en",
        "languageCopies": [
            {
                "path": "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures",
                "title": "Alaskan Adventures",
                "locale": "en",
                "model": "Article",
                "status": "DRAFT",
                "id": "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures"
            }
        ]
    },
    "transientStatus": {},
    "extensions": {
        "age": "1 year old"
    }
}
```

Als er andere gegevens nodig zijn om de aangepaste kolom te vullen, kunnen HTTP-aanvragen worden ingediend bij AEM Author om de gegevens op te halen.

>[!IMPORTANT]
>
> Zorg ervoor dat de instantie van de Auteur van AEM wordt gevormd om [ verzoeken van de dwars-oorsprong ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=nl-NL) van de oorsprong toe te staan app AppBuilder loopt. Tot de toegestane oorsprong behoren `https://localhost:9080`, de oorsprong van het werkgebied van AppBuilder en de oorsprong van de AppBuilder-productie.
>
> Alternatief, kan de uitbreiding een actie van douane [ AppBuilder ](../../runtime-action.md) roepen die het verzoek aan de Auteur van AEM namens de uitbreiding doet.


```javascript
const context = await guestConnection.hostConnection.getRemoteApi().getSharedContext();
...
// Fetch the Content Fragment JSON from AEM Author using the AEM Assets HTTP API
const response = await fetch(`${context.aemHost}${fragment.id.slice('/content/dam'.length)}.json`, {
    headers: {
        'Authorization': `Bearer ${context.auth.imsToken}`
    }
})
...
```

#### Kolomdefinitie

Het resultaat van de rendermethode is een JavaScript-object waarvan de sleutels het pad van het inhoudsfragment (of de `fragment.id` ) zijn en de waarde de waarde die in de kolom wordt weergegeven.

De resultaten van deze extensie voor de kolom `age` zijn bijvoorbeeld:

```json
{
    "/content/dam/wknd-shared/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks": "22 minutes",
    "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp": "1 hour",
    "/content/dam/wknd-shared/en/magazine/western-australia/western-australia-by-camper-van": "1 hour",
    "/content/dam/wknd-shared/en/adventures/climbing-new-zealand/climbing-new-zealand": "10 months",
    "/content/dam/wknd-shared/en/magazine/skitouring/skitouring": "1 year",
    "/content/dam/wknd-shared/en/adventures/beervana-portland/beervana-in-portland": "1 year",
    "/content/dam/wknd-shared/en/magazine/alaska-adventure/alaskan-adventures": "1 year",
    "/content/dam/wknd-shared/en/magazine/arctic-surfing/aloha-spirits-in-northern-norway": "1 year",
    "/content/dam/wknd-shared/en/magazine/san-diego-surf-spots/san-diego-surfspots": "1 year",
    "/content/dam/wknd-shared/en/magazine/fly-fishing-amazon/fly-fishing": "1 year",
    "/content/dam/wknd-shared/en/adventures/napa-wine-tasting/napa-wine-tasting": "1 year",
    "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/cycling-southern-utah": "1 year",
    "/content/dam/wknd-shared/en/adventures/gastronomic-marais-tour/gastronomic-marais-tour": "1 year",
    "/content/dam/wknd-shared/en/adventures/tahoe-skiing/tahoe-skiing": "1 year",
    "/content/dam/wknd-shared/en/adventures/surf-camp-in-costa-rica/surf-camp-costa-rica": "1 year",
    "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking": "1 year",
    "/content/dam/wknd-shared/en/adventures/whistler-mountain-biking/whistler-mountain-biking": "1 year",
    "/content/dam/wknd-shared/en/adventures/colorado-rock-climbing/colorado-rock-climbing": "1 year",
    "/content/dam/wknd-shared/en/adventures/ski-touring-mont-blanc/ski-touring-mont-blanc": "1 year",
    "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany": "1 year",
    "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling": "1 year",
    "/content/dam/wknd-shared/en/adventures/downhill-skiing-wyoming/downhill-skiing-wyoming": "1 year",
    "/content/dam/wknd-shared/en/adventures/riverside-camping-australia/riverside-camping-australia": "1 year",
    "/content/dam/wknd-shared/en/contributors/ian-provo": "1 year",
    "/content/dam/wknd-shared/en/contributors/sofia-sj-berg": "1 year",
    "/content/dam/wknd-shared/en/contributors/justin-barr": "1 year",
    "/content/dam/wknd-shared/en/contributors/jake-hammer": "1 year",
    "/content/dam/wknd-shared/en/contributors/jacob-wester": "1 year",
    "/content/dam/wknd-shared/en/contributors/stacey-roswells": "1 year",
    "/content/dam/wknd-shared/en/contributors/kumar-selveraj": "1 year"
}
```
