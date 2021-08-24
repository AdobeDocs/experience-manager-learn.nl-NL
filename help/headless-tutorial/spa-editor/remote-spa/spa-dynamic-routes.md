---
title: Bewerkbare componenten toevoegen aan externe SPA dynamische routes
description: Leer hoe te om editable componenten aan dynamische routes in een verre SPA toe te voegen.
topic: Zwaardeloze, SPA, ontwikkeling
feature: SPA Editor, kerncomponenten, API's, ontwikkelen
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '958'
ht-degree: 0%

---


# Dynamische routes en bewerkbare componenten

In dit hoofdstuk, laten wij twee dynamische routes van het Detail van het avontuur toe om editable componenten te steunen; __Bali Surf Camp__ en __Beervana in Portland__.

![Dynamische routes en bewerkbare componenten](./assets/spa-dynamic-routes/intro.png)

De route van het Detail van het avontuur wordt bepaald als `/adventure:path` waar `path` de weg aan het Adventure WKND (het Fragment van de Inhoud) is om details over te tonen.

## De SPA URL&#39;s toewijzen aan AEM pagina&#39;s

In de vorige twee hoofdstukken, hebben wij editable componenteninhoud van de SPA mening van het Huis aan overeenkomstige Verre SPA wortelpagina in AEM op `/content/wknd-app/us/en/` in kaart gebracht.

Het bepalen van afbeelding voor editable componenten voor de SPA dynamische routes is gelijkaardig maar wij moeten omhoog 1:1 toewijzingsschema tussen instanties van de route en AEM pagina&#39;s komen.

In dit leerprogramma, zullen wij de naam van het Fragment van de Inhoud van het avontuur WKND nemen, dat het laatste segment van de weg is, en het in kaart brengen aan een eenvoudig weg onder `/content/wknd-app/us/en/adventure`.

| Externe SPA | Paginapad AEM |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/nl/home |
| /adventure:/content/dam/wknd/nl/adventures/bali-surf-kamp/__bali-surf-kamp__ | /content/wknd-app/us/nl/home/adventure/__bali-surf-kamp__ |
| /adventure:/content/dam/wknd/nl/adventures/beervana-portland/__beervana-portland__ | /content/wknd-app/us/nl/home/adventure/__beervana-in-portland__ |

Op basis van deze afbeelding moeten we dus twee nieuwe AEM maken op:

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Externe SPA

De afbeelding voor verzoeken die de verre SPA verlaten wordt gevormd via de `setupProxy` configuratie die in [Bootstrap wordt gedaan SPA](./spa-bootstrap.md).

## Toewijzing SPA Editor

De afbeelding voor SPA verzoeken wanneer de SPA via AEM SPA Editor wordt geopend, wordt geconfigureerd via de configuratie Sling Mappings die wordt uitgevoerd in [AEM configureren](./aem-configure.md).

## Inhoudspagina&#39;s maken in AEM

Maak eerst het tussenliggende paginasegment `adventure`:

1. Aanmelden bij AEM-auteur
1. Navigeer naar __Sites > WKND App > us > nl > WKND App Home Page__
   + Deze AEM pagina wordt in kaart gebracht als wortel van de SPA, zodat is dit waar wij beginnen de AEM paginastructuur voor andere SPA routes te bouwen.
1. Tik __Maken__ en selecteer __Pagina__
1. Selecteer de sjabloon __Externe SPA Pagina__ en tik __Volgende__
1. Pagina-eigenschappen invullen
   + __Titel__: Adventure
   + __Naam__:  `adventure`
      + Deze waarde bepaalt URL van de AEM pagina, en moet daarom het de routesegment van de SPA aanpassen.
1. Tik __Gereed__

Maak vervolgens de AEM pagina&#39;s die overeenkomen met elk van de SPA URL&#39;s waarvoor bewerkbare gebieden nodig zijn.

1. Navigeer in de nieuwe __Adventure__ pagina in Admin van de Plaats
1. Tik __Maken__ en selecteer __Pagina__
1. Selecteer de sjabloon __Externe SPA Pagina__ en tik __Volgende__
1. Pagina-eigenschappen invullen
   + __Titel__: Bali Surf Camp
   + __Naam__:  `bali-surf-camp`
      + Deze waarde bepaalt URL van de AEM pagina, en moet daarom het laatste segment van de SPA&#39; route aanpassen
1. Tik __Gereed__
1. Herhaal de stappen 3-6 om de pagina __Beervana in Portland__ te maken, met:
   + __Titel__: Beervana in Portland
   + __Naam__:  `beervana-in-portland`
      + Deze waarde bepaalt URL van de AEM pagina, en moet daarom het laatste segment van de SPA&#39; route aanpassen

Deze twee AEM pagina&#39;s houden de respectieve authored inhoud voor hun passende SPA routes. Als andere SPA routes auteursrecht vereisen, moeten de nieuwe AEMPagina&#39;s bij hun SPA URL onder de Verre de wortelpagina van SPA pagina (`/content/wknd-app/us/en/home`) in AEM worden gecreeerd.

## De WKND-app bijwerken

Plaats de `<AEMResponsiveGrid...>` component die in [last hoofdstuk](./spa-container-component.md), in onze `AdventureDetail` SPA component wordt gecreeerd, die tot een editable container leiden.

### Plaats de SPA AEMResponsiveGrid

Wanneer u `<AEMResponsiveGrid...>` in de component `AdventureDetail` plaatst, wordt een bewerkbare container in die route gemaakt. De truc is omdat de veelvoudige routes de `AdventureDetail` component gebruiken om terug te geven, moeten wij dynamisch het `<AEMResponsiveGrid...>'s pagePath` attribuut aanpassen. `pagePath` moet worden afgeleid om aan de overeenkomstige AEM pagina te richten, die op het avontuur wordt gebaseerd de instantie van de route toont.

1. `react-app/src/components/AdventureDetail.js` openen en bewerken
1. Voeg de volgende lijn vóór `AdventureDetail(..)'s` tweede `return(..)` verklaring toe, die de avontuurnaam uit de weg van het inhoudsfragment afleidt.

   ```
   ...
   // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
   const adventureName = adventureData._path.split('/').pop();
   ...
   ```

1. Importeer de component `AEMResponsiveGrid` en plaats deze boven de component `<h2>Itinerary</h2>`.
1. De volgende kenmerken instellen voor de `<AEMResponsiveGrid...>`-component
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${adventureName}'`
   + `itemPath = 'root/responsivegrid'`

   Dit instrueert de `AEMResponsiveGrid` component om zijn inhoud van het AEM middel terug te winnen:

   + `/content/wknd-app/us/en/home/adventure/${adventureName}/jcr:content/root/responsivegrid`


`AdventureDetail.js` bijwerken met de volgende regels:

```
...
import AEMResponsiveGrid from '../components/aem/AEMResponsiveGrid';
...

function AdventureDetail(props) {
    ...
    // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
    const adventureName = adventureData._path.split('/').pop();

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

Het `AdventureDetail.js`-bestand moet er als volgt uitzien:

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## Auteur van de container in AEM

Met `<AEMResponsiveGrid...>` op zijn plaats, en zijn `pagePath` dynamisch geplaatst die op het avontuur wordt teruggegeven, proberen wij creërend inhoud daarin.

1. Aanmelden bij AEM-auteur
1. Navigeer naar __Sites > WKND App > us > en__
1. __De startpagina van de__ WKND- ____ app bewerken
   + Navigeer naar de __Bali Surf Camp__ route in de SPA om het uit te geven
1. Selecteer __Voorvertoning__ in de modus-kiezer in de rechterbovenhoek
1. Tik op de __Bali Surf Camp__ kaart in de SPA om naar zijn route te navigeren
1. Selecteer __Bewerken__ in de modus-kiezer
1. Zoek het bewerkbare gebied __Indelingscontainer__ recht boven de __route__
1. Open de zijbalk __Pagina-editor__ en selecteer de __Componentweergave__
1. Sleep een aantal van de ingeschakelde componenten naar de __Indelingscontainer__
   + Afbeelding
   + Tekst
   + Titel

   En maak wat promotiemateriaal. Het zou er ongeveer als volgt kunnen uitzien:

   ![Bali Adventure Detail Authoring](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __Wijzigingen__ voorvertonen in AEM Pagina-editor
1. Vernieuw de WKND App die plaatselijk op [http://localhost:3000](http://localhost:3000) loopt, navigeer aan __Bali Surf Camp__ route om de authored veranderingen te zien!

   ![Externe SPA Bali](./assets/spa-dynamic-routes/remote-spa-final.png)

Wanneer het navigeren aan een adventure detailroute die geen in kaart gebrachte AEM Pagina heeft, zal er geen auteursbaarheid op die routeinstantie zijn. Als u het ontwerpen op deze pagina&#39;s wilt inschakelen, maakt u gewoon een AEM pagina met de overeenkomende naam onder de pagina __Adventure__!

## Gefeliciteerd!

Gefeliciteerd! U hebt authoring vermogen toegevoegd aan dynamische routes in de SPA!

+ De component ResponsiveGrid van de component React Editable AEM toegevoegd aan een dynamische route
+ Opgezette AEM pagina&#39;s ter ondersteuning van het ontwerpen van twee specifieke routes in de SPA (Bali Surf Camp en Beervana in Portland)
+ Geautoriseerde inhoud op de dynamische Bali Surf Camp route!

U hebt nu het onderzoeken van de eerste stappen van hoe AEM SPA Redacteur kan worden gebruikt om specifieke editable gebieden aan een Verre SPA toe te voegen!


>[!NOTE]
>
>Blijf op de hoogte! Dit leerprogramma zal worden uitgebreid om Adobe beste praktijken en aanbevelingen op te stellen over hoe de oplossing van de SPARedacteur aan AEM als Cloud Service, en productiemilieu&#39;s op te stellen.