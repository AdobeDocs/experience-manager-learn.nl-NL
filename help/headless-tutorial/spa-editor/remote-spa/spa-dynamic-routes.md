---
title: Bewerkbare componenten toevoegen aan externe SPA dynamische routes
description: Leer hoe te om editable componenten aan dynamische routes in een verre SPA toe te voegen.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '947'
ht-degree: 0%

---

# Dynamische routes en bewerkbare componenten

In dit hoofdstuk, laten wij twee dynamische routes van het Detail van het avontuur toe om editable componenten te steunen; __Bali Surf Camp__ en __Beervana in Portland__.

![Dynamische routes en bewerkbare componenten](./assets/spa-dynamic-routes/intro.png)

De route van het Detail van het avontuur SPA wordt bepaald zoals `/adventure:path` waar `path` is de weg aan het Adventure WKND (het Fragment van de Inhoud) om details over te tonen.

## De SPA URL&#39;s toewijzen aan AEM pagina&#39;s

In de vorige twee hoofdstukken hebben we bewerkbare componentinhoud vanuit de SPA Home-weergave toegewezen aan de bijbehorende externe SPA hoofdpagina in AEM op `/content/wknd-app/us/en/`.

Het bepalen van afbeelding voor editable componenten voor de SPA dynamische routes is gelijkaardig maar wij moeten omhoog 1:1 toewijzingsschema tussen instanties van de route en AEM pagina&#39;s komen.

In dit leerprogramma, nemen wij de naam van het Fragment van de Inhoud van het avontuur WKND, dat het laatste segment van de weg is, en brengen het aan een eenvoudig weg onder `/content/wknd-app/us/en/adventure`.

| Externe SPA | Paginapad AEM |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/nl/home |
| /adventure:/content/dam/wknd/nl/adventures/bali-surf-kamp/__bali-surf-kamp__ | /content/wknd-app/us/nl/home/adventure/__bali-surf-kamp__ |
| /adventure:/content/dam/wknd/nl/adventures/beervana-portland/__beervana-portland__ | /content/wknd-app/us/nl/home/adventure/__beervana-in-portland__ |

Op basis van deze afbeelding moeten we dus twee nieuwe AEM maken op:

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Externe SPA

De toewijzing voor verzoeken die de Verre SPA verlaten wordt gevormd via `setupProxy` configuratie uitgevoerd in [Bootstrap de SPA](./spa-bootstrap.md).

## Toewijzing SPA Editor

De afbeelding voor SPA verzoeken wanneer de SPA via AEM SPA Editor wordt geopend, wordt geconfigureerd via de configuratie Sling Mappings die in [AEM configureren](./aem-configure.md).

## Inhoudspagina&#39;s maken in AEM

Maak eerst de tussenpersoon `adventure` Paginasegment:

1. Aanmelden bij AEM-auteur
1. Navigeren naar __Sites > WKND App > us > en > WKND App Home Page__
   + Deze AEM pagina wordt in kaart gebracht als wortel van de SPA, zodat is dit waar wij beginnen de AEM paginastructuur voor andere SPA routes te bouwen.
1. Tikken __Maken__ en selecteert u __Pagina__
1. Selecteer __Externe SPA__ sjabloon, en tikken __Volgende__
1. Pagina-eigenschappen invullen
   + __Titel__: Adventure
   + __Naam__: `adventure`
      + Deze waarde bepaalt URL van de AEM pagina, en moet daarom het de routesegment van de SPA aanpassen.
1. Tikken __Gereed__

Maak vervolgens de AEM pagina&#39;s die overeenkomen met elk van de SPA URL&#39;s waarvoor bewerkbare gebieden nodig zijn.

1. Navigeer in het nieuwe __Adventure__ pagina in Sitebeheer
1. Tikken __Maken__ en selecteert u __Pagina__
1. Selecteer __Externe SPA__ sjabloon, en tikken __Volgende__
1. Pagina-eigenschappen invullen
   + __Titel__: Bali Surf Camp
   + __Naam__: `bali-surf-camp`
      + Deze waarde bepaalt URL van de AEM pagina, en moet daarom het laatste segment van de SPA&#39; route aanpassen
1. Tikken __Gereed__
1. Herhaal stap 3-6 om de __Beervana in Portland__ pagina, met:
   + __Titel__: Beervana in Portland
   + __Naam__: `beervana-in-portland`
      + Deze waarde bepaalt URL van de AEM pagina, en moet daarom het laatste segment van de SPA&#39; route aanpassen

Deze twee AEM pagina&#39;s houden de respectieve-authored inhoud voor hun passende SPA routes. Als andere SPA routes auteursrecht vereisen, moeten de nieuwe AEMPagina&#39;s bij hun SPA URL onder de Verre de wortelpagina van SPA van de pagina (`/content/wknd-app/us/en/home`) in AEM.

## De WKND-app bijwerken

Laten we de `<AEMResponsiveGrid...>` component die in de [laatste hoofdstuk](./spa-container-component.md), in onze `AdventureDetail` SPA component, een bewerkbare container maken.

### Plaats de SPA AEMResponsiveGrid

Het plaatsen van `<AEMResponsiveGrid...>` in de `AdventureDetail` maakt een bewerkbare container in die route. De truc is omdat de veelvoudige routes gebruiken `AdventureDetail` -component die moet worden gerenderd, moeten we de  `<AEMResponsiveGrid...>'s pagePath` kenmerk. De `pagePath` moet worden afgeleid om aan de overeenkomstige AEM pagina te richten, die op het avontuur wordt gebaseerd de instantie van de route toont.

1. Openen en bewerken `react-app/src/components/AdventureDetail.js`
1. Voeg de volgende regel toe voor `AdventureDetail(..)'s` seconde `return(..)` die de avontuurnaam afleidt van het pad van het inhoudsfragment.

   ```
   ...
   // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
   const adventureName = _path.split('/').pop();
   ...
   ```

1. Het dialoogvenster Importeren `AEMResponsiveGrid` en plaatst deze boven de component `<h2>Itinerary</h2>` component.
1. Stel de volgende kenmerken in op de knop `<AEMResponsiveGrid...>` component
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${adventureName}'`
   + `itemPath = 'root/responsivegrid'`

   Dit draagt het `AEMResponsiveGrid` component om zijn inhoud van het AEM middel terug te winnen:

   + `/content/wknd-app/us/en/home/adventure/${adventureName}/jcr:content/root/responsivegrid`


Bijwerken `AdventureDetail.js` met de volgende regels:

```
...
import AEMResponsiveGrid from '../components/aem/AEMResponsiveGrid';
...

function AdventureDetail(props) {
    ...
    // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
    const adventureName = _path.split('/').pop();

    return(
        ...
        <AEMResponsiveGrid 
            pagePath={`/content/wknd-app/us/en/home/adventure/${adventureName}`}
            itemPath="root/responsivegrid"/>
            
        <h2>Itinerary</h2>
        ...
    )
}
```

De `AdventureDetail.js` bestand moet er als volgt uitzien:

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## Auteur van de container in AEM

Met de `<AEMResponsiveGrid...>` en de `pagePath` dynamisch ingesteld op basis van het avontuur dat wordt gerenderd, proberen we er inhoud in te ontwerpen.

1. Aanmelden bij AEM-auteur
1. Navigeren naar __Sites > WKND App > us > en__
1. __Bewerken__ de __WKND App Home Page__ page
   + Ga naar de __Bali Surf Camp__ route in de SPA om het uit te geven
1. Selecteren __Voorvertoning__ in de moduskiezer rechtsboven
1. Tik op de knop __Bali Surf Camp__ kaart in de SPA om aan zijn route te navigeren
1. Selecteren __Bewerken__ in de modus Selector
1. Zoek de __Layout Container__ bewerkbaar gebied rechts boven __Urinertie__
1. Open de __Zijbalk van de pagina-editor__ en selecteert u de __Weergave Componenten__
1. Sleep een aantal van de ingeschakelde componenten naar de __Layout Container__
   + Afbeelding
   + Tekst
   + Titel

   En maak wat promotiemateriaal. Het zou er ongeveer als volgt kunnen uitzien:

   ![Bali Adventure Detail Authoring](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __Voorvertoning__ uw wijzigingen in AEM paginaeditor
1. De WKND-app vernieuwen die lokaal wordt uitgevoerd [http://localhost:3000](http://localhost:3000), navigeert u naar de __Bali Surf Camp__ route om de authored veranderingen te zien!

   ![Externe SPA Bali](./assets/spa-dynamic-routes/remote-spa-final.png)

Wanneer het navigeren aan een adventure detailroute die geen in kaart gebrachte AEM Pagina heeft, is er geen auteursbaarheid op die routeinstantie. Als u het ontwerpen op deze pagina&#39;s wilt inschakelen, maakt u gewoon een AEM pagina met de overeenkomende naam onder de __Adventure__ pagina!

## Gefeliciteerd!

Gefeliciteerd! U hebt authoring vermogen toegevoegd aan dynamische routes in de SPA!

+ De component ResponsiveGrid van de component React Editable AEM toegevoegd aan een dynamische route
+ Opgezette AEM pagina&#39;s ter ondersteuning van het ontwerpen van twee specifieke routes in de SPA (Bali Surf Camp en Beervana in Portland)
+ Geautoriseerde inhoud op de dynamische Bali Surf Camp route!

U hebt nu het onderzoeken van de eerste stappen van hoe AEM SPA Redacteur kan worden gebruikt om specifieke editable gebieden aan een Verre SPA toe te voegen!


>[!NOTE]
>
>Blijf op de hoogte! Dit leerprogramma zal worden uitgebreid om Adobe beste praktijken en aanbevelingen op te stellen over hoe de oplossing van de SPARedacteur aan AEM as a Cloud Service, en productiemilieu&#39;s op te stellen.
