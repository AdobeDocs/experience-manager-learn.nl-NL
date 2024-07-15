---
title: Gelokaliseerde inhoud gebruiken met AEM zonder kop
description: Leer hoe u GraphQL gebruikt om te zoeken naar AEM voor gelokaliseerde inhoud.
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
jira: KT-10254
thumbnail: KT-10254.jpeg
exl-id: 5e3d115b-f3a1-4edc-86ab-3e0713a36d54
duration: 130
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 0%

---

# Gelokaliseerde inhoud met AEM zonder kop

AEM verstrekt het Kader van de Integratie van de a [ Vertaling ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html) voor hoofdloze inhoud, toestaand de Fragmenten van de Inhoud en ondersteunende activa om voor gebruik over scènes gemakkelijk worden vertaald. Dit is hetzelfde framework dat wordt gebruikt om andere AEM inhoud te vertalen, zoals Pagina&#39;s, Experience Fragments, Assets en Forms. Zodra [ hoofdloze inhoud is vertaald ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html), en gepubliceerd, is het klaar voor consumptie door hoofdloze toepassingen.

## Assets-mapstructuur{#assets-folder-structure}

Zorg ervoor dat de gelokaliseerde Fragmenten van de Inhoud in AEM de [ geadviseerde localisatiestructuur ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure) volgen.

![ Gelokaliseerde AEM activa omslagen ](./assets/localized-content/asset-folders.jpg)

De scèneomslagen moeten siblings zijn, en de omslagnaam, eerder dan titel, moet een geldige [ ISO 639-1 code ](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) zijn die de scène van de inhoud vertegenwoordigt in de omslag.

De landinstellingscode is ook de waarde die wordt gebruikt om de inhoudsfragmenten te filteren die door de GraphQL-query worden geretourneerd.

| Landinstellingscode | AEM pad | Landinstelling van inhoud |
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

De `$locale` variabele die in de `_locale` filter wordt gebruikt vereist de scènecode (bijvoorbeeld `en`, `en_us`, of `de`) zoals die in [ wordt gespecificeerd AEM de activa omslag-basis localisatieconcept ](#assets-folder-structure).

## Voorbeeld Reageren

Laten we een eenvoudige React-toepassing maken die bepaalt welke Adventure-inhoud van AEM moet worden opgehaald op basis van een locale-kiezer met het `_locale` -filter.

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

De Adventures-component zoekt AEM naar alle avonturen per landinstelling en geeft een overzicht van de titels ervan. Dit wordt bereikt door de landinstellingswaarde die is opgeslagen in de context React, door te geven aan de query met het filter `_locale` .

Deze benadering kan tot andere vragen in uw toepassing worden uitgebreid, die ervoor zorgt dat alle vragen slechts inhoud omvatten die door de scèneselectie van een gebruiker wordt gespecificeerd.

Het vragen tegen AEM wordt uitgevoerd in de verbinding van het douaneantwoord [ getAdventuresByLocale, die in meer detail op het Vragen AEM de documentatie van GraphQL ](./aem-headless-sdk.md) wordt beschreven.

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
