---
title: Gelokaliseerde inhoud gebruiken met AEM Headless
description: Leer hoe u GraphQL gebruikt om AEM te vragen naar gelokaliseerde inhoud.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
jira: KT-10254
thumbnail: KT-10254.jpeg
exl-id: 5e3d115b-f3a1-4edc-86ab-3e0713a36d54
duration: 130
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 0%

---

# Gelokaliseerde inhoud met AEM Headless

AEM verstrekt het Kader van de Integratie van de a [ Vertaling ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html) voor hoofdloze inhoud, toestaand de Fragmenten van de Inhoud en het steunen van activa om voor gebruik over scènes gemakkelijk worden vertaald. Dit is hetzelfde framework dat wordt gebruikt voor het vertalen van andere AEM-inhoud, zoals Pagina&#39;s, Experience Fragments, Assets en Forms. Zodra [ hoofdloze inhoud is vertaald ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html), en gepubliceerd, is het klaar voor consumptie door hoofdloze toepassingen.

## Assets-mapstructuur{#assets-folder-structure}

Zorg ervoor dat de gelokaliseerde Fragments van de Inhoud in AEM de [ geadviseerde localisatiestructuur ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure) volgen.

![ Gelokaliseerde de activaomslagen van AEM ](./assets/localized-content/asset-folders.jpg)

De scèneomslagen moeten siblings zijn, en de omslagnaam, eerder dan titel, moet een geldige [ ISO 639-1 code ](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) zijn die de scène van de inhoud vertegenwoordigt in de omslag.

De landinstellingscode is ook de waarde die wordt gebruikt om de inhoudsfragmenten te filteren die door de GraphQL-query worden geretourneerd.

| Landinstellingscode | AEM-pad | Landinstelling van inhoud |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | Duitse inhoud |
| en | /content/dam/.../**nl**/... | Engelse inhoud |
| es | /content/dam/.../**es**/... | Spaanse inhoud |

## GraphQL-query voortgezet

AEM biedt een `_locale` GraphQL-filter dat inhoud automatisch filtert op landinstellingscode. Bijvoorbeeld, die alle Engelse avonturen in het [ project van de Plaats WKND ](https://github.com/adobe/aem-guides-wknd) vragen kan met een nieuwe persisted vraag `wknd-shared/adventures-by-locale` worden gedaan die als wordt bepaald:

```graphql
query($locale: String!) {
  adventureList(_locale: $locale) {
    items {      
      _path
      title
    }
  }
}
```

De `$locale` variabele die in de `_locale` filter wordt gebruikt vereist de scènecode (bijvoorbeeld `en`, `en_us`, of `de`) zoals die in [ wordt gespecificeerd AEM element omslag-basis localisatieovereenkomst ](#assets-folder-structure).

## Voorbeeld Reageren

Laten we een eenvoudige React-toepassing maken die bepaalt welke Adventure-inhoud van AEM moet worden opgevraagd op basis van een lokale kiezer die het filter `_locale` gebruikt.

Wanneer __Engels__ in de scèneselecteur wordt geselecteerd, dan zijn de Engelse Fragmenten van de Inhoud van het Avontuur onder `/content/dam/wknd/en` teruggekeerd, wanneer __Spaans__ wordt geselecteerd, dan de Spaanse Fragmenten van de Inhoud onder `/content/dam/wknd/es`, etc., etc.

![ Gelokaliseerde Reactie voorbeeld app ](./assets/localized-content/react-example.png)

### Een `LocaleContext` maken{#locale-context}

Eerst, creeer de context van het Reageren van a [ ](https://reactjs.org/docs/context.html) om de scène toe te staan om over de componenten van de React toepassing worden gebruikt.

```javascript
// src/LocaleContext.js

import React from 'react'

const DEFAULT_LOCALE = 'en';

const LocaleContext = React.createContext({
    locale: DEFAULT_LOCALE, 
    setLocale: () => {}
});

export default LocaleContext;
```

### Een component `LocaleSwitcher` React maken{#locale-switcher}

Daarna, creeer een component van het Reageren van de scènewisselaar die de ](#locale-context) waarde van 0} LocaleContext aan de selectie van de gebruiker is.[

Deze landinstellingswaarde wordt gebruikt om de GraphQL-query&#39;s aan te sturen, zodat deze alleen inhoud retourneren die overeenkomt met de geselecteerde landinstelling.

```javascript
// src/LocaleSwitcher.js

import { useContext } from "react";
import LocaleContext from "./LocaleContext";

export default function LocaleSwitcher() {
  const { locale, setLocale } = useContext(LocaleContext);

  return (
    <select value={locale}
            onChange={e => setLocale(e.target.value)}>
      <option value="de">Deutsch</option>
      <option value="en">English</option>
      <option value="es">Español</option>
    </select>
  );
}
```

### Vraag inhoud met behulp van het filter `_locale`{#adventures}

De component Adventures zoekt AEM naar alle avonturen per landinstelling en geeft de titels ervan weer. Dit wordt bereikt door de landinstellingswaarde die is opgeslagen in de context React, door te geven aan de query met het filter `_locale` .

Deze benadering kan tot andere vragen in uw toepassing worden uitgebreid, die ervoor zorgt dat alle vragen slechts inhoud omvatten die door de scèneselectie van een gebruiker wordt gespecificeerd.

Het vragen tegen AEM wordt uitgevoerd in de haken van het douaneantwoord [ getAdventuresByLocale, die in meer detail op de het vragen van AEM GraphQL documentatie ](./aem-headless-sdk.md) wordt beschreven.

```javascript
// src/Adventures.js

import { useContext } from "react"
import { useAdventuresByLocale } from './api/persistedQueries'
import LocaleContext from './LocaleContext'

export default function Adventures() {
    const { locale } = useContext(LocaleContext);

    // Get data from AEM using GraphQL persisted query as defined above 
    // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
    let { data, error } = useAdventuresByLocale(locale);

    return (
        <ul>
            {data?.adventureList?.items?.map((adventure, index) => { 
                return <li key={index}>{adventure.title}</li>
            })}
        </ul>
    )
}
```

### Definieer de `App.js`{#app-js}

Als laatste koppelt u de toepassing React aan elkaar door de toepassing React in de `LanguageContext.Provider` te plaatsen en de waarde voor de landinstelling in te stellen. Dit staat de andere componenten van het Reageren, [ LocaleSwitcher ](#locale-switcher), en [ avonturen ](#adventures) toe om de staat van de scèneselectie te delen.

```javascript
// src/App.js

import { useState, useContext } from "react";
import LocaleContext from "./LocaleContext";
import LocaleSwitcher from "./LocaleSwitcher";
import Adventures from "./Adventures";

export default function App() {
  const [locale, setLocale] = useState(useContext(LocaleContext).locale);

  return (
    <LocaleContext.Provider value={{locale, setLocale}}>
      <LocaleSwitcher />
      <Adventures />
    </LocaleContext.Provider>
  );
}
```
