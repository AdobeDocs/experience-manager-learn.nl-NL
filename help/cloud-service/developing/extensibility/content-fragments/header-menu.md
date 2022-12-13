---
title: AEM extensies in het headermenu van de Content Fragment-console
description: Leer hoe u een extensie voor het kopmenu van de AEM Content Fragment-console maakt.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---


# Extensie van menu Koptekst

![Extensie van menu Koptekst](./assets/header-menu/header-menu.png){align="center"}

Extensies die een headermenu bevatten, introduceren een knop in de koptekst van de AEM Content Fragment Console die wordt weergegeven wanneer __nee__ Inhoudsfragmenten worden geselecteerd. Omdat de knoppen voor de extensie van het kopmenu alleen worden weergegeven wanneer er geen inhoudsfragmenten zijn geselecteerd, werken ze gewoonlijk niet op bestaande inhoudsfragmenten. In plaats daarvan worden doorgaans extensies voor headermenu&#39;s gebruikt:

+ Maak nieuwe inhoudsfragmenten met behulp van aangepaste logica, zoals een set inhoudsfragmenten maken die via inhoudsverwijzingen zijn gekoppeld.
+ Handelend op een programmatically geselecteerde reeks van de Fragmenten van de Inhoud, zoals het uitvoeren van alle die Fragments van de Inhoud in de vorige week worden gecreeerd.

## Registratie van extensies

`ExtensionRegistration.js` is het ingangspunt voor de AEM uitbreiding en bepaalt:

1. het extensietype; in het geval van een knop in het headermenu.
1. De definitie van de extensieknop, in `getButton()` functie.
1. De klikmanager voor de knoop, in `onClick()` functie.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // Must be unique
      methods: {
        // Configure your Header Menu button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-button',    // Unique ID for the button
              'label': 'My header menu button',         // Button label 
              'icon': 'Bookmark'                        // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the Header Menu extension button
          onClick() {
            // Header Menu buttons are not associated with selected Content Fragment, and thus are not provided a selection parameter.        
            // Do work like importing data from a well known location, or exporting a welll known set of data
            doWork();            
          },
        }
      }
    }
  }
  init().catch(console.error);
}
```

## Modal

![Modal](./assets/modal/modal.png)

AEM extensies in het headermenu van de Content Fragment Console zijn mogelijk vereist:

+ Extra invoer van de gebruiker om de gewenste actie uit te voeren.
+ De mogelijkheid om de gebruiker gedetailleerde informatie over het resultaat van de actie te verstrekken.

Ter ondersteuning van deze vereisten staat de extensie AEM Content Fragment Console een aangepast modaal model toe dat wordt weergegeven als een React-toepassing.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/content-fragment/my-extension";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Overslaan naar het maken van een modaal</p>
      <p class="has-text-blackest">Leer hoe u een modaal object maakt dat wordt weergegeven wanneer u op de knop voor extensie van het headermenu klikt.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Leer een modaal model te maken">Leer een modaal model te maken</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Geen modaal

Soms vereisen AEM extensies in het headermenu van de Content Fragment-console geen verdere interactie met de gebruiker, bijvoorbeeld:

+ Een back-end proces aanroepen waarvoor geen gebruikersinvoer nodig is, zoals importeren of exporteren.
+ Een nieuwe webpagina openen, zoals interne documentatie over inhoudsrichtlijnen.

In deze gevallen vereist de extensie AEM Content Fragment Console geen [modaal](#modal)en kan het werk rechtstreeks in de knoop van het kopbalmenu uitvoeren `onClick` handler.

Met de extensie AEM Content Fragment Console kan een voortgangsindicator de AEM Content Fragment Console bedekken terwijl het werk wordt uitgevoerd, zodat de gebruiker geen verdere handelingen meer kan uitvoeren. Het gebruik van de voortgangsindicator is optioneel, maar nuttig om de voortgang van synchroon werk aan de gebruiker mee te delen.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      headerMenu: { ...
        onClick() {
          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork();
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```
