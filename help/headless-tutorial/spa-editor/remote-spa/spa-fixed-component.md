---
title: Bewerkbare vaste componenten toevoegen aan een externe SPA
description: Leer hoe u bewerkbare vaste componenten aan een externe SPA kunt toevoegen.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---

# Bewerkbare vaste componenten

Bewerkbare React-componenten kunnen &#39;vast&#39; zijn of hard gecodeerd in de SPA weergaven. Op deze manier kunnen ontwikkelaars SPA met de Editor compatibele componenten in de SPA weergaven plaatsen en kunnen gebruikers de inhoud van de componenten schrijven in AEM SPA Editor.

![Vaste componenten](./assets/spa-fixed-component/intro.png)

In dit hoofdstuk vervangen we de titel &quot;Huidige avonturen&quot; van de weergave Home. Dit is een hard gecodeerde tekst in `Home.js` met een vaste, maar bewerkbare component Titel. Vaste componenten garanderen de plaatsing van de titel, maar staan ook toe dat de tekst van de titel wordt geschreven en dat de titel buiten de ontwikkelingscyclus wordt gewijzigd.

## De WKND-app bijwerken

Als u een __Vast__ aan de mening van het Huis:

+ Importeer de AEM component React Core Component Title en registreer deze aan het bronnentype van Titel van project
+ Plaats de bewerkbare component Titel in de SPA Home-weergave

### Importeren in de component Titel van AEM React Core-component

Vervang de tekst met harde codes in de SPA Home `<h2>Current Adventures</h2>` met de component AEM React Core Components&#39; Title. Voordat de component Titel kan worden gebruikt, moeten we:

1. De component Title importeren uit `@adobe/aem-core-components-react-base`
1. Registreren met `withMappable` zodat ontwikkelaars het in de SPA kunnen plaatsen
1. Registreer ook bij `MapTo` zodat het kan worden gebruikt in [containercomponent later](./spa-container-component.md).

Dit doet u als volgt:

1. Externe SPA openen bij `~/Code/wknd-app/aem-guides-wknd-graphql/react-app` in uw IDE
1. Een component React maken op `react-app/src/components/aem/AEMTitle.js`
1. De volgende code toevoegen aan `AEMTitle.js`.

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

De `AEMTitle.js` bestand moet er als volgt uitzien:

![AEMTitle.js](./assets/spa-fixed-component/aem-title-js.png)

### De component React AEMTitle gebruiken

Nu de component Titel van de AEM React Core-component is geregistreerd in en beschikbaar voor gebruik in de React-app, vervangt u de tekst van de hard-gecodeerde titel in de weergave Home.

1. Bewerken `react-app/src/Home.js`
1. In de `Home()` onderaan vervangt u de hard-gecodeerde titel door de nieuwe `AEMTitle` component:

   ```
   <h2>Current Adventures</h2>
   ```

   with

   ```
   <AEMTitle
       pagePath='/content/wknd-app/us/en/home' 
       itemPath='root/title'/>
   ```

   Bijwerken `Home.js` met de volgende code:

   ```
   ...
   import { AEMTitle } from './aem/AEMTitle';
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

De `Home.js` bestand moet er als volgt uitzien:

![Home.js](./assets/spa-fixed-component/home-js.png)

## De component Titel in AEM maken

1. Aanmelden bij AEM-auteur
1. Navigeren naar __Sites > WKND App__
1. Tikken __Home__ en selecteert u __Bewerken__ van de bovenste actiebalk
1. Selecteren __Bewerken__ in de bewerkingsmoduskiezer rechtsboven in de Pagina-editor
1. Houd de muisaanwijzer boven de standaardtiteltekst onder het WKND-logo en boven de lijst met avonturen totdat de omtrek voor blauwe bewerking wordt weergegeven
1. Tik om de actiebalk van de component zichtbaar te maken en tik vervolgens op de __moersleutel__  bewerken

   ![De component Title, actiebalk](./assets/spa-fixed-component/title-action-bar.png)

1. Auteur van de component Title:
   + Titel: __WKND Adventures__
   + Type/grootte: __H2__

      ![Dialoogvenster Titelcomponent](./assets/spa-fixed-component/title-dialog.png)

1. Tikken __Gereed__ opslaan
1. Wijzigingen voorvertonen in AEM SPA Editor
1. De WKND-app vernieuwen die lokaal wordt uitgevoerd [http://localhost:3000](http://localhost:3000) en zie de veranderingen van de geschreven titel onmiddellijk weerspiegeld.

   ![Titelcomponent in SPA](./assets/spa-fixed-component/title-final.png)

## Gefeliciteerd!

U hebt een vaste, bewerkbare component toegevoegd aan de WKND-app! Nu weet u hoe:

+ Importeren uit en opnieuw gebruiken van een AEM React Core-component in de SPA
+ Een vaste, maar bewerkbare component aan de SPA toevoegen
+ Auteur van de vaste component in AEM
+ Zie de geschreven inhoud in de Verre SPA

## Volgende stappen

De volgende stappen zijn: [een AEM component ResponsiveGrid toevoegen](./spa-container-component.md) aan de SPA die auteur toestaat om en editable componenten aan de SPA toe te voegen!
