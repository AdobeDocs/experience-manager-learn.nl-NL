---
title: Bewerkbare containercomponenten toevoegen aan een externe SPA
description: Leer hoe u bewerkbare containercomponenten aan een externe SPA kunt toevoegen waarmee AEM auteurs componenten naar deze componenten kunnen slepen en neerzetten.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7635
thumbnail: kt-7635.jpeg
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '1169'
ht-degree: 0%

---

# Bewerkbare containercomponenten

[Vaste componenten](./spa-fixed-component.md) biedt enige flexibiliteit voor het ontwerpen SPA inhoud, maar deze aanpak is star en vereist dat ontwikkelaars de exacte samenstelling van de bewerkbare inhoud definiëren. Om het creëren van uitzonderlijke ervaringen door auteurs te steunen, steunt SPA Redacteur het gebruik van containercomponenten in de SPA. Met containercomponenten kunnen auteurs toegestane componenten naar de container slepen en neerzetten, en ze ontwerpen, net als bij traditionele AEM Sites-ontwerpen!

![Bewerkbare containercomponenten](./assets/spa-container-component/intro.png)

In dit hoofdstuk, voegen wij een editable container aan de huismening toe die auteurs toestaat om rijke inhoudservaring samen te stellen en op te maken gebruikend AEM React de Componenten van de Kern in de SPA direct.

## De WKND-app bijwerken

Een containercomponent toevoegen aan de weergave Home:

+ De component ResponsiveGrid van de component React Editable AEM importeren
+ Importeren en registreren AEM React Core Components (Tekst en afbeelding) voor gebruik in de containercomponent

### Importeren in de container component ResponsiveGrid

Om een bewerkbaar gebied aan de mening van het Huis te plaatsen, moeten wij:

1. De component ResponsiveGrid importeren uit `@adobe/aem-react-editable-components`
1. Registreren met `withMappable` zodat ontwikkelaars het in de SPA kunnen plaatsen
1. Registreer ook bij `MapTo` zodat het opnieuw kan worden gebruikt in andere Container-componenten, waardoor containers effectief kunnen worden genest.

Dit doet u als volgt:

1. Open het SPA project in uw winde
1. Een component React maken op `src/components/aem/AEMResponsiveGrid.js`
1. De volgende code toevoegen aan `AEMResponsiveGrid.js`

   ```
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the base ResponsiveGrid component
   import { ResponsiveGrid } from "@adobe/aem-react-editable-components";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wcm/foundation/components/responsivegrid";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Layout Container",  // The component placeholder in AEM SPA Editor
       isEmpty: function(props) { 
           return props.cqItemsOrder == null || props.cqItemsOrder.length === 0;
       },                              // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE     // The sling:resourceType this SPA component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(ResponsiveGrid, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMResponsiveGrid .../>
   const AEMResponsiveGrid = withMappable(ResponsiveGrid, EditConfig);
   
   export default AEMResponsiveGrid;
   ```

De code is vergelijkbaar `AEMTitle.js` dat [de component AEM Reach Core Components&#39; Title geïmporteerd](./spa-fixed-component.md).


De `AEMResponsiveGrid.js` bestand moet er als volgt uitzien:

![AEMResponsiveGrid.js](./assets/spa-container-component/aem-responsive-grid-js.png)

### De SPA AEMResponsiveGrid gebruiken

Nu AEM component ResponsiveGrid is geregistreerd in en beschikbaar voor gebruik binnen de SPA, kunnen we deze in de weergave Home plaatsen.

1. Openen en bewerken `react-app/src/Home.js`
1. Het dialoogvenster Importeren `AEMResponsiveGrid` en plaatst deze boven de component `<AEMTitle ...>` component.
1. Stel de volgende kenmerken in op de knop `<AEMResponsiveGrid...>` component
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   Dit draagt het `AEMResponsiveGrid` component om zijn inhoud van het AEM middel terug te winnen:

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   De `itemPath` worden toegewezen aan `responsivegrid` knooppunt gedefinieerd in `Remote SPA Page` AEM Sjabloon en wordt automatisch gemaakt op nieuwe AEM pagina&#39;s die zijn gemaakt op basis van de `Remote SPA Page` AEM sjabloon.

   Bijwerken `Home.js` om de `<AEMResponsiveGrid...>` component.

   ```
   ...
   import AEMResponsiveGrid from './aem/AEMResponsiveGrid';
   ...
   
   function Home() {
       return (
           <div className="Home">
               <AEMResponsiveGrid
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/responsivegrid'/>
   
               <AEMTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='title'/>
               <Adventures />
           </div>
       );
   }
   ```

De `Home.js` bestand moet er als volgt uitzien:

![Home.js](./assets/spa-container-component/home-js.png)

## Bewerkbare componenten maken

Om het volledige effect van de flexibele auteurservaringscontainers in SPA Redacteur te krijgen. We hebben al een bewerkbare component Titel gemaakt, maar laten we er nog een paar maken waarmee auteurs tekst en afbeelding kunnen gebruiken AEM WCM Core-componenten in de nieuw toegevoegde containercomponent.

### Tekstcomponent

1. Open het SPA project in uw winde
1. Een component React maken op `src/components/aem/AEMText.js`
1. De volgende code toevoegen aan `AEMText.js`

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { TextV2, TextV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {    
       emptyLabel: "Text",
       isEmpty: TextV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(TextV2, EditConfig);
   
   const AEMText = withMappable(TextV2, EditConfig);
   
   export default AEMText;
   ```

De `AEMText.js` bestand moet er als volgt uitzien:

![AEMText.js](./assets/spa-container-component/aem-text-js.png)

### Afbeeldingscomponent

1. Open het SPA project in uw winde
1. Een component React maken op `src/components/aem/AEMImage.js`
1. De volgende code toevoegen aan `AEMImage.js`

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { ImageV2, ImageV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/image";
   
   const EditConfig = {    
       emptyLabel: "Image",
       isEmpty: ImageV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(ImageV2, EditConfig);
   
   const AEMImage = withMappable(ImageV2, EditConfig);
   
   export default AEMImage;
   ```

1. Een SCSS-bestand maken `src/components/aem/AEMImage.scss` die aangepaste stijlen voor de `AEMImage.scss`. Deze stijlen zijn gericht op de CSS-klassen van de AEM React Core Component BEM-notation.
1. Voeg volgende SCSS toe aan `AEMImage.scss`

   ```
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. Importeren `AEMImage.scss` in `AEMImage.js`

   ```
   ...
   import './AEMImage.scss';
   ...
   ```

De `AEMImage.js` en `AEMImage.scss` zou als moeten kijken:

![AEMImage.js en AEMImage.scss](./assets/spa-container-component/aem-image-js-scss.png)

### De bewerkbare componenten importeren

De nieuwe `AEMText` en `AEMImage` In de SPA wordt verwezen naar SPA componenten en deze worden dynamisch geïnstantieerd op basis van de JSON die door AEM wordt geretourneerd. Als u ervoor wilt zorgen dat deze componenten beschikbaar zijn voor de SPA, maakt u voor deze componenten importinstructies in `Home.js`

1. Open het SPA project in uw winde
1. Het bestand openen `src/Home.js`
1. Importeerinstructies toevoegen voor `AEMText` en `AEMImage`

   ```
   ...
   import AEMText from './components/aem/AEMText';
   import AEMImage from './components/aem/AEMImage';
   ...
   ```


Het resultaat moet er als volgt uitzien:

![Home.js](./assets/spa-container-component/home-js-imports.png)

Indien deze invoer _niet_ toegevoegd, de `AEMText` en `AEMImage` de code wordt niet aangehaald door SPA, en zo, worden de componenten niet geregistreerd tegen de verstrekte middeltypes.

## De container configureren in AEM

AEM containercomponenten gebruiken beleid om hun toegestane componenten te dicteren. Dit is een kritieke configuratie wanneer het gebruiken van SPA Redacteur, aangezien slechts AEM de Componenten van de Kern WCM die SPA componententegenhangers in kaart hebben gebracht door de SPA renderbaar zijn. Zorg ervoor dat alleen de onderdelen waarvoor we SPA implementaties hebben geleverd, zijn toegestaan:

+ `AEMTitle` toegewezen aan `wknd-app/components/title`
+ `AEMText` toegewezen aan `wknd-app/components/text`
+ `AEMImage` toegewezen aan `wknd-app/components/image`

De reponsivegrid-container van de sjabloon Externe SPA pagina configureren:

1. Aanmelden bij AEM-auteur
1. Navigeren naar __Gereedschappen > Algemeen > Sjablonen > WKND App__
1. Bewerken __SPA pagina rapporteren__

   ![Beleid voor responsieve rasters](./assets/spa-container-component/templates-remote-spa-page.png)

1. Selecteren __Structuur__ in de modusschakelaar rechtsboven
1. Tik om de __Layout Container__
1. Tik op de knop __Beleid__ pictogram in de pop-upbalk

   ![Beleid voor responsieve rasters](./assets/spa-container-component/templates-policies-action.png)

1. Rechts, onder de __Toegestane componenten__ tab, uitvouwen __WKND APP - INHOUD__
1. Zorg ervoor dat alleen het volgende is geselecteerd:
   + Afbeelding
   + Tekst
   + Titel

   ![Externe SPA](./assets/spa-container-component/templates-allowed-components.png)

1. Tikken __Gereed__

## De container ontwerpen in AEM

Nadat de SPA is bijgewerkt om de `<AEMResponsiveGrid...>`, omvat drie AEM React Core-componenten (`AEMTitle`, `AEMText`, en `AEMImage`), en AEM wordt bijgewerkt met een passend beleid van het Malplaatje, kunnen wij beginnen inhoud in de containercomponent te ontwerpen.

1. Aanmelden bij AEM-auteur
1. Navigeren naar __Sites > WKND App__
1. Tikken __Home__ en selecteert u __Bewerken__ van de bovenste actiebalk
   + De componentenvertoningen van de Tekst van de &quot;Wereld van Hello&quot;aangezien dit automatisch werd toegevoegd toen het produceren van het project van het archetype van het AEM Project
1. Selecteren __Bewerken__ in de modus Selecteren rechtsboven in de Pagina-editor
1. Zoek de __Layout Container__ bewerkbaar gebied onder Titel
1. Open de __Zijbalk van de pagina-editor__ en selecteert u de __Weergave Componenten__
1. Sleep de volgende componenten naar de __Layout Container__
   + Afbeelding
   + Titel
1. Sleep de componenten om deze in de volgende volgorde te plaatsen:
   1. Titel
   1. Afbeelding
   1. Tekst
1. __Auteur__ de __Titel__ component
   1. Tik op de component Title en tik op de knop __moersleutel__ pictogram naar __bewerken__ de component Title
   1. Voeg de volgende tekst toe:
      + Titel: __De zomer komt eraan. Laten we er het beste van maken!__
      + Type: __H1__
   1. Tikken __Gereed__
1. __Auteur__ de __Afbeelding__ component
   1. Sleep een afbeelding vanuit de zijbalk (na het schakelen naar de weergave Elementen) naar de component Afbeelding
   1. Tik op de component Image en tik op de __moersleutel__ pictogram om te bewerken
   1. Controleer de __Afbeelding is decoratief__ selectievakje
   1. Tikken __Gereed__
1. __Auteur__ de __Tekst__ component
   1. Bewerk de component Text door op de component Text te tikken en op de component Text te tikken __moersleutel__ pictogram
   1. Voeg de volgende tekst toe:
      + _Op dit moment kunt u 15% krijgen voor alle avonturen van één week, en 20% voor alle avonturen die 2 weken of langer zijn! Voeg bij de kassa de campagnecode SUMMERISCOMING toe om uw kortingen te krijgen!_
   1. Tikken __Gereed__

1. Uw componenten zijn nu ontworpen, maar worden verticaal gestapeld.

   ![Gemaakte componenten](./assets/spa-container-component/authored-components.png)

   Gebruik AEM lay-outmodus om de grootte en lay-out van de componenten aan te passen.

1. Overschakelen op __Lay-outmodus__ met de moduskiezer rechtsboven
1. __Formaat wijzigen__ de componenten Afbeelding en Tekst, zodat deze naast elkaar staan
   + __Afbeelding__ onderdeel moet __8 kolommen breed__
   + __Tekst__ onderdeel moet __3 kolommen breed__

   ![Indelingscomponenten](./assets/spa-container-component/layout-components.png)

1. __Voorvertoning__ uw wijzigingen in AEM paginaeditor
1. De WKND-app vernieuwen die lokaal wordt uitgevoerd [http://localhost:3000](http://localhost:3000) om de gemaakte wijzigingen te zien!

   ![Containercomponent in SPA](./assets/spa-container-component/localhost-final.png)


## Gefeliciteerd!

U hebt een containercomponent toegevoegd waarmee bewerkbare componenten door auteurs aan de WKND App kunnen worden toegevoegd! Nu weet u hoe:

+ De component ResponsiveGrid van de component React Editable AEM gebruiken in de SPA
+ Registreer AEM React Core Components (Tekst en afbeelding) voor gebruik in de SPA via de containercomponent
+ Vorm het Verre malplaatje van de SPA van de Pagina om de SPA toegelaten Componenten van de Kern toe te staan
+ Bewerkbare componenten toevoegen aan de containercomponent
+ Auteur- en indelingscomponenten in SPA Editor

## Volgende stappen

In de volgende stap wordt dezelfde techniek gebruikt voor [Voeg een editable component aan een route van de Details van het Avontuur toe](./spa-dynamic-routes.md) in de SPA.
