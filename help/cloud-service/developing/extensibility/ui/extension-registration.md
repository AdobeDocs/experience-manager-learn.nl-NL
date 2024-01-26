---
title: Registratie van AEM UI-extensie
description: Leer hoe u een AEM UI-extensie registreert.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
duration: 96
source-git-commit: 6f1245e804f0311c3f833ea8b2324cbc95272f52
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 0%

---

# Registratie van extensies

AEM UI-extensies zijn gespecialiseerde App Builder-apps, gebaseerd op React en gebruiken de extensies [Spectrum reageren](https://react-spectrum.adobe.com/react-spectrum/) UI-framework.

Om te bepalen waar en hoe de AEM UI uitbreiding verschijnt, worden twee configuraties vereist in App Builder app van de uitbreiding: app het verpletteren en de uitbreidingsregistratie.

## Toepassingsroutes{#app-routes}

De extensies `App.js` declareert de [Reageren router](https://reactrouter.com/en/main) dat een indexroute omvat die de uitbreiding in AEM UI registreert.

De indexroute wordt aangehaald wanneer AEM UI aanvankelijk laadt, en het doel van deze route bepaalt hoe de uitbreiding in de console wordt blootgesteld.

+ `./src/aem-ui-extension/web-src/src/components/App.js`

```javascript
import ExtensionRegistration from "./ExtensionRegistration"
...            
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          {/* The index route maps to the extension registration */}
          <Route index element={<ExtensionRegistration />} />
          ...                                   
        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## Registratie van extensies

`ExtensionRegistration.js` moet onmiddellijk via de indexroute van de uitbreiding worden geladen, en handelt het registratiepunt van de uitbreiding in werking.

Gebaseerd op de geselecteerde AEM UI-extensiesjabloon wanneer [initialiseren App Builder-app-extensie](./app-initialization.md), worden verschillende extensiepunten ondersteund.

+ [UI-extensiepunten voor inhoudsfragmenten](./content-fragments/overview.md#extension-points)

## Extensies voorwaardelijk opnemen

AEM UI-extensies kunnen aangepaste logica uitvoeren om de AEM omgevingen waarin de extensie wordt weergegeven, te beperken. Deze controle wordt uitgevoerd vóór de `register` oproepen in `ExtensionRegistration` en wordt onmiddellijk geretourneerd als de extensie niet moet worden weergegeven.

Voor deze controle is een beperkte context beschikbaar:

+ De AEM host waarop de extensie wordt geladen.
+ De AEM toegangstoken van de huidige gebruiker.

De meest gebruikte controles voor het laden van een extensie zijn:

+ De AEM host (`new URLSearchParams(window.location.search).get('repo')`) om te bepalen of de extensie moet worden geladen.
   + Alleen de extensie weergeven in AEM omgevingen die deel uitmaken van een specifiek programma (zoals in het onderstaande voorbeeld wordt getoond).
   + Alleen de extensie weergeven in een specifieke AEM (AEM host).
+ Een [Adobe I/O Runtime-actie](./runtime-action.md) om een vraag van HTTP aan AEM te maken om te bepalen of de huidige gebruiker de uitbreiding zou moeten zien.

In het onderstaande voorbeeld ziet u hoe u de extensie beperkt tot alle omgevingen in het programma `p12345`.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const PROGRAM_ID = 'p12345';

  // Get the current AEM Host (author-pXXX-eYYY.adobeaemcloud.com) the extension is loading on
  const aemHost = new URLSearchParams(window.location.search).get('repo');

  // Create a check to determine if the current AEM host matches the AEM program that uses this extension 
  const aemHostRegex = new RegExp(`^author-${PROGRAM_ID}-e[\\d]+\\.adobeaemcloud\\.com$`)

  // Disable the extension if the Cloud Manager Program Id doesn't match the regex.
  if (!aemHostRegex.test(aemHost)) {
    return; // Skip extension registration if the environment is not in program p12345.
  }

  // Else, continue initializing the extension
  const init = async () => { .. };
  
  init().catch(console.error);
}
```
