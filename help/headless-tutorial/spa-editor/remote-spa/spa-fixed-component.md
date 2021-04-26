---
title: Bewerkbare vaste componenten toevoegen aan een externe SPA
description: Leer hoe u bewerkbare vaste componenten aan een externe SPA kunt toevoegen.
topic: Zwaardeloze, SPA, ontwikkeling
feature: SPA Editor, kerncomponenten, API's, ontwikkelen
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
translation-type: tm+mt
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 0%

---


# Bewerkbare vaste componenten

Bewerkbare React-componenten kunnen &#39;vast&#39; zijn of hard gecodeerd in de SPA weergaven. Op deze manier kunnen ontwikkelaars SPA met de Editor compatibele componenten in de SPA weergaven plaatsen en kunnen gebruikers de inhoud van de componenten schrijven in AEM SPA Editor.

![Vaste componenten](./assets/spa-fixed-component/intro.png)

In dit hoofdstuk vervangen wij de titel van de mening van het Huis, &quot;Huidige avonturen&quot;, die hard-gecodeerde tekst in `Home.js` met een vaste, maar editable component van de Titel is. Vaste componenten garanderen de plaatsing van de titel, maar staan ook toe dat de tekst van de titel wordt geschreven en dat de titel buiten de ontwikkelingscyclus wordt gewijzigd.

## De WKND-app bijwerken

Een component Fixed toevoegen aan de weergave Home:

+ Importeer de AEM component React Core Component Title en registreer deze aan het bronnentype van Titel van project
+ Plaats de bewerkbare component Titel in de SPA Home-weergave

### Importeren in de component Titel van AEM React Core-component

In de SPA mening van het Huis, vervang de hard-gecodeerde tekst `<h2>Current Adventures</h2>` met de AEM component van de Titel van de Component van de Kern React. Voordat de component Titel kan worden gebruikt, moeten we:

1. De component Title importeren uit `@adobe/aem-core-components-react-base`
1. Registreer het gebruikend `withMappable` zodat kunnen de ontwikkelaars het in de SPA plaatsen
1. Registreer u ook bij `MapTo`, zodat u deze kunt gebruiken in [containercomponent later](./spa-container-component.md).

Dit doet u als volgt:

1. Open Verre SPA project bij `~/Code/wknd-app/aem-guides-wknd-graphql/react-app` in uw winde
1. Een component React maken op `react-app/src/components/aem/AEMTitle.js`
1. Voeg de volgende code toe aan `AEMTitle.js`.

   ```
   // Import the withMappable API provided by the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the AEM React Core Components' Title component implementation and it's Empty Function 
   import { TitleV2, TitleV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {    
       emptyLabel: "Title",  // The component placeholder in AEM SPA Editor
       isEmpty: TitleV2IsEmptyFn, // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(TitleV2, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMTitle .../>
   const AEMTitle = withMappable(TitleV2, EditConfig);
   
   export default AEMTitle;
   ```

Lees de opmerkingen van de code voor de implementatiedetails.

Het `AEMTitle.js`-bestand moet er als volgt uitzien:

![AEMTitle.js](./assets/spa-fixed-component/aem-title-js.png)

### De component React AEMTitle gebruiken

Nu de component Titel van de AEM React Core-component is geregistreerd in en beschikbaar voor gebruik in de React-app, vervangt u de tekst van de hard-gecodeerde titel in de weergave Home.

1. Bewerken `react-app/src/App.js`
1. in `Home()` onderaan, vervang de hard-gecodeerde titel met de nieuwe `AEMTitle` component:

   ```
   <h2>Current Adventures</h2>
   ```

   with

   ```
   <AEMTitle
       pagePath='/content/wknd-app/us/en/home' 
       itemPath='root/title'/>
   ```

   `Apps.js` bijwerken met de volgende code:

   ```
   ...
   import { AEMTitle } from './components/aem/AEMTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
               <AEMTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

Het `Apps.js`-bestand moet er als volgt uitzien:

![App.js](./assets/spa-fixed-component/app-js.png)

## De component Titel in AEM maken

1. Aanmelden bij AEM-auteur
1. Navigeer naar __Sites > WKND App__
1. Tik __Home__ en selecteer __Bewerken__ in de bovenste actiebalk
1. Selecteer __Bewerken__ in de bewerkingsmoduskiezer rechtsboven in de Pagina-editor
1. Houd de muisaanwijzer boven de standaardtiteltekst onder het WKND-logo en boven de lijst met avonturen totdat de omtrek voor blauwe bewerking wordt weergegeven
1. Tik om de actiebalk van de component zichtbaar te maken en tik op __moersleutel__ om te bewerken

   ![De component Title, actiebalk](./assets/spa-fixed-component/title-action-bar.png)

1. Auteur van de component Title:
   + Titel: __WKND-avonturen__
   + Type/grootte: __H2__

      ![Dialoogvenster Titelcomponent](./assets/spa-fixed-component/title-dialog.png)

1. Tik __Gereed__ om op te slaan
1. Wijzigingen voorvertonen in AEM SPA Editor
1. Vernieuw de WKND App die plaatselijk op [http://localhost:3000](http://localhost:3000) loopt en zie de authored titelveranderingen onmiddellijk weerspiegeld.

   ![Titelcomponent in SPA](./assets/spa-fixed-component/title-final.png)

## Gefeliciteerd!

U hebt een vaste, bewerkbare component toegevoegd aan de WKND-app! Nu weet u hoe:

+ Importeren uit en opnieuw gebruiken van een AEM React Core-component in de SPA
+ Een vaste, maar bewerkbare component aan de SPA toevoegen
+ Auteur van de vaste component in AEM
+ Zie de geschreven inhoud in de Verre SPA

## Volgende stappen

De volgende stappen moeten [een AEM component van de container ResponsiveGrid](./spa-container-component.md) aan de SPA toevoegen die auteur toestaat om en editable componenten aan de SPA toe te voegen!
