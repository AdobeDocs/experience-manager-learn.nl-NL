---
title: AEM Inhoudsfragmentconsole-extensie registreren
description: Leer hoe u extensies voor een Content Fragment-console registreert.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: f19cdc7d551f20b35550e7d25bd168a2eaa43b6a
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---


# Registratie van extensies

AEM extensies van Content Fragment Console zijn speciale App Builder-app, gebaseerd op React en gebruikt de extensie [Spectrum reageren](https://react-spectrum.adobe.com/react-spectrum/) UI-framework.

Om te bepalen waar en hoe de AEM Content Fragment Console de uitbreiding verschijnt, worden twee specifieke configuraties vereist in app Builder van de uitbreiding: app routing en de extensieregistratie.

## Toepassingsroutes{#app-routes}

De extensies `App.js` declareert de [Reageren router](https://reactrouter.com/en/main) die een indexroute bevat die de extensie registreert in de AEM Content Fragment Console.

De indexroute wordt aangehaald wanneer dan AEM de Console van het Fragment van de Inhoud aanvankelijk laadt, en het doel van deze route bepaalt hoe de uitbreiding in de console wordt blootgesteld.

+ `./src/aem-cf-console-admin-1/web-src/src/components/App.js`

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

`ExtensionRegistration.js` moet onmiddellijk worden geladen via de indexroute van de extensie en treedt op als registratiepunt van de extensie, waarin wordt gedefinieerd:

1. het extensietype; a [kopmenu](./header-menu.md) of [actiebalk](./action-bar.md) knop.
   + [Menu Koptekst](./header-menu.md#extension-registration) extensies worden aangeduid door de `headerMenu` eigendom onder `methods`.
   + [Actiebalk](./action-bar.md#extension-registration) extensies worden aangeduid door de `actionBar` eigendom onder `methods`.
1. De definitie van de extensieknop, in `getButton()` functie. Deze functie retourneert een object met velden:
   + `id` is een unieke id voor de knop
   + `label` is het label van de extensieknop in de AEM Content Fragment-console
   + `icon` Dit is het pictogram van de extensieknop in de AEM Content Fragment-console. Het pictogram is een [Spectrum reageren](https://spectrum.adobe.com/page/icons/) pictogramnaam, met spaties verwijderd.
1. De klikmanager voor de knoop, in die in wordt bepaald `onClick()` functie.
   + [Menu Koptekst](./header-menu.md#extension-registration) extensies geven geen parameters door aan de click-handler.
   + [Actiebalk](./action-bar.md#extension-registration) extensies bieden een lijst met geselecteerde inhoudfragmentpaden in de `selections` parameter.

### Extensie Menu Koptekst

![Extensie Menu Koptekst](./assets/extension-registration/header-menu.png)

De extensieknoppen Menu Koptekst worden weergegeven wanneer er geen inhoudsfragmenten zijn geselecteerd. Omdat extensies in het menu Koptekst geen effect hebben op de selectie van een inhoudsfragment, worden er geen inhoudsfragmenten opgegeven `onClick()` handler.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-extension', // Unique ID for the button
              'label': 'My header menu extension',      // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Header Menu extensions do not pass parameters to the click handler
          onClick() { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Overslaan om een extensie van het kopmenu te maken</p>
      <p class="has-text-blackest">Leer hoe u een extensie van het headermenu registreert en definieert in de AEM Content Fragments Console.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./header-menu.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Leer om een uitbreiding van het kopbalmenu te bouwen">Leer om een uitbreiding van het kopbalmenu te bouwen</span>
        </a>
      </div>
    </div>
  </div>
</div>

### Handelingenbalk, extensie

![Handelingenbalk, extensie](./assets/extension-registration/action-bar.png)

De knoppen voor de extensie van de actiebalk worden weergegeven wanneer een of meer inhoudsfragmenten zijn geselecteerd. De paden van het geselecteerde inhoudsfragment worden beschikbaar gemaakt voor de extensie via de `selections` in de knoppen `onClick(..)` handler.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'example.my-action-bar-extension',  // Unique ID for the button
              'label': 'My action bar extension',       // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Only Action Bar buttons populate the selections parameter
          onClick(selections) { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Overslaan naar het bouwen van een extensie voor een actiebalk</p>
      <p class="has-text-blackest">Leer hoe u een extensie voor een actiebalk registreert en definieert in de AEM Content Fragments Console.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./action-bar.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Leer een extensie voor een actiebalk te maken">Leer een extensie voor een actiebalk te maken</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Extensies voorwaardelijk opnemen

AEM extensies van Content Fragment Console kunnen aangepaste logica uitvoeren om te beperken wanneer de extensie wordt weergegeven in de AEM Content Fragment Console. Deze controle wordt uitgevoerd vóór de `register` oproepen in `ExtensionRegistration` en wordt onmiddellijk geretourneerd als de extensie niet moet worden weergegeven.

Voor deze controle is een beperkte context beschikbaar:

+ De AEM host waarop de extensie wordt geladen.
+ De AEM toegangstoken van de huidige gebruiker.

De meest gebruikte controles voor het laden van een extensie zijn:

+ De AEM host gebruiken (`new URLSearchParams(window.location.search).get('repo')`) om te bepalen of de extensie moet worden geladen.
   + Alleen de extensie weergeven in AEM omgevingen die deel uitmaken van een specifiek programma (zoals in het onderstaande voorbeeld wordt getoond).
   + Alleen de extensie weergeven in een specifieke AEM (d.w.z. AEM host).
+ Een [Adobe I/O Runtime-actie](./runtime-action.md) om een HTTP- vraag aan AEM te maken om te bepalen of de huidige gebruiker de uitbreiding zou moeten zien.

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
