---
title: Gelokaliseerde inhoud gebruiken met AEM zonder kop
description: Leer hoe te om GraphQL te gebruiken om AEM voor gelokaliseerde inhoud te vragen.
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
kt: 10254
thumbnail: KT-10254.jpeg
source-git-commit: 4966a48c29ae1b5d0664cb43feeb4ad94f43b4e1
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 0%

---


# Gelokaliseerde inhoud met AEM zonder kop

AEM biedt een [Omzettingsintegratiekader](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html) voor inhoud zonder kop, zodat Content Fragments en ondersteunende elementen eenvoudig kunnen worden vertaald voor gebruik in verschillende landinstellingen. Dit is hetzelfde framework dat wordt gebruikt om andere AEM inhoud te vertalen, zoals Pagina&#39;s, Experience Fragments, Assets en Forms. Eenmaal [inhoud zonder kop is omgezet](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html), en gepubliceerd, is klaar voor consumptie door headless toepassingen.

## Structuur van middelenmap{#assets-folder-structure}

Zorg ervoor dat de gelokaliseerde inhoudsfragmenten in AEM de [aanbevolen lokalisatiestructuur](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure).

![Gelokaliseerde mappen met AEM middelen](./assets/localized-content/asset-folders.jpg)

De mappen met landinstellingen moeten op hetzelfde niveau staan en de mapnaam moet in plaats van de titel een geldige naam zijn [ISO 639-1-code](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) die de landinstelling van de inhoud in de map vertegenwoordigt.

De landinstellingscode is ook de waarde die wordt gebruikt om de inhoudsfragmenten te filteren die door de query GraphQL worden geretourneerd.

| Landinstellingscode | AEM pad | Landinstelling inhoud |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | Duitse inhoud |
| en | /content/dam/.../**en**/... | Engelse inhoud |
| es | /content/dam/.../**es**/... | Spaanse inhoud |

## GraphQL-query

AEM biedt een `_locale` GraphQL-filter dat inhoud automatisch filtert op landinstellingscode. U kunt bijvoorbeeld alle Engelse avonturen opvragen in het dialoogvenster [WKND-project voor demo-referentie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/create-site.html) zou er als volgt uitzien:

```graphql
{
  adventureList(_locale: "en") {
    items {      
      _path
      adventureTitle
    }
  }
}
```

De `_locale` filter vereist het gebruik van [Localisatie-conventie voor het lokaliseren van bestanden AEM](#assets-folder-structure).

## Voorbeeld Reageren

Laten we een eenvoudige React-toepassing maken die bepaalt welke Adventure-inhoud van AEM wordt gevraagd op basis van een locale-kiezer met behulp van de `_locale` filter.

Wanneer __Engels__ is geselecteerd in de locatieselector, dan Engelse Adventure Content Fragments onder `/content/dam/wknd/en` worden geretourneerd, wanneer __Spaans__ is geselecteerd, dan Spaanse inhoudsfragmenten onder `/content/dam/wknd/es`enzovoort.

![Lokale React-voorbeeldtoepassing](./assets/localized-content/react-example.png)

### Een `LocaleContext`{#locale-context}

Maak eerst een [Reageercontext](https://reactjs.org/docs/context.html) om toe te staan dat de landinstelling over de componenten van React wordt gebruikt.

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

### Een `LocaleSwitcher` Reageren, component{#locale-switcher}

Vervolgens maakt u een component Reactie via een locale-switch die de instelling [LocaleContext&#39;s](#locale-context) aan de selectie van de gebruiker.

Deze landinstellingswaarde wordt gebruikt om de query GraphQL uit te voeren, zodat deze alleen inhoud retourneert die overeenkomt met de geselecteerde landinstelling.

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

### Inhoud query uitvoeren met de opdracht `_locale` filter{#adventures}

De Adventures-component zoekt AEM naar alle avonturen per landinstelling en geeft een overzicht van de titels ervan. Dit wordt bereikt door de waarde van de landinstelling die is opgeslagen in de context Reageren, aan de query door te geven met de opdracht `_locale` filter.

Deze benadering kan tot andere vragen in uw toepassing worden uitgebreid, die ervoor zorgt dat alle vragen slechts inhoud omvatten die door de scèneselectie van een gebruiker wordt gespecificeerd.

Het vragen tegen AEM wordt uitgevoerd in de haak van de douanereactie [useGraphQL, die meer in detail wordt beschreven op de Vraag AEM documentatie GraphQL](./aem-headless-sdk.md).

```javascript
// src/Adventures.js

import { useContext } from "react"
import { useGraphQL } from './useGraphQL'
import LocaleContext from './LocaleContext'

export default function Adventures() {
    const { locale } = useContext(LocaleContext);

    let {data} = useGraphQL(`{
            adventureList(_locale: "${locale}") {
                items {      
                _path
                adventureTitle
             }
        }
    }`);

    return (
        <ul>
            {data?.adventureList?.items?.map((adventure, index) => { 
                return <li key={index}>{adventure.adventureTitle}</li>
            })}
        </ul>
    )
}
```

### Definieer de `App.js`{#app-js}

Als laatste koppelt u dit alles door de React-toepassing in te pakken met de `LanguageContext.Provider` en de landinstellingswaarde instellen. Dit staat de andere componenten van het Reageren toe, [LocaleSwitcher](#locale-switcher), en [avonturen](#adventures) om de status van de landinstellingsselectie te delen.

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