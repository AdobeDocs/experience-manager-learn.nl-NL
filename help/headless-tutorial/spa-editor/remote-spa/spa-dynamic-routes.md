---
title: Bewerkbare componenten toevoegen aan externe SPA dynamische routes
description: Leer hoe te om editable componenten aan dynamische routes in een verre SPA toe te voegen.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7636
thumbnail: kt-7636.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
duration: 202
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# Dynamische routes en bewerkbare componenten

In dit hoofdstuk, laten wij twee dynamische routes van het Detail van het avontuur toe om editable componenten te steunen; __Bali Surf Camp__ en __Beervana in Portland__.

![ Dynamische routes en editable componenten ](./assets/spa-dynamic-routes/intro.png)

De route van het Detail van het avontuur SPA wordt bepaald als `/adventure/:slug` waar `slug` een unieke herkenningstekenbezit op het Fragment van de Inhoud van het Avontuur is.

## De SPA URL&#39;s toewijzen aan AEM pagina&#39;s

In de vorige twee hoofdstukken hebben we bewerkbare componentinhoud vanuit de SPA Home-weergave toegewezen aan de corresponderende Remote SPA hoofdpagina in AEM op `/content/wknd-app/us/en/` .

Het bepalen van afbeelding voor editable componenten voor de SPA dynamische routes is gelijkaardig maar wij moeten omhoog 1:1 toewijzingsschema tussen instanties van de route en AEM pagina&#39;s komen.

In deze zelfstudie nemen we de naam van het WKND Adventure Content Fragment, het laatste segment van het pad, en wijzen we het toe aan een eenvoudig pad onder `/content/wknd-app/us/en/adventure` .

| Externe SPA | Pad AEM |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/nl/home |
| /adventure/__bali-surf-kamp__ | /content/wknd-app/us/nl/home/adventure/__bali-surf-kamp__ |
| /adventure/__beervana-portland__ | /content/wknd-app/us/nl/home/adventure/__beervana-in-portland__ |

Op basis van deze afbeelding moeten we dus twee nieuwe AEM maken op:

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Externe SPA

De afbeelding voor verzoeken die de Verre SPA verlaten wordt gevormd via de `setupProxy` configuratie die in [ wordt gedaan Bootstrap de SPA ](./spa-bootstrap.md).

## Toewijzing SPA Editor

De afbeelding voor SPA verzoeken wanneer de SPA via AEM SPA Redacteur wordt geopend wordt gevormd via het Verdelen configuratie van Toewijzingen die in [ wordt gedaan vormt AEM ](./aem-configure.md).

## Inhoudspagina&#39;s maken in AEM

Maak eerst het tussenliggende paginasegment `adventure` :

1. Aanmelden bij AEM auteur
1. Navigeer aan __Plaatsen > WKND App > gebruiken > en > WKND App Homepage__
   + Deze AEM pagina wordt in kaart gebracht als wortel van de SPA, zodat is dit waar wij beginnen de AEM paginastructuur voor andere SPA routes te bouwen.
1. Tik __creeer__ en selecteer __Pagina__
1. Selecteer het __Verre malplaatje van de SPA van de Pagina__, en Tik __daarna__
1. Pagina-eigenschappen invullen
   + __Titel__: Adventure
   + __Naam__: `adventure`
      + Deze waarde bepaalt URL van de AEM pagina, en moet daarom het de routesegment van de SPA aanpassen.
1. Tik __Gereed__

Maak vervolgens de AEM pagina&#39;s die overeenkomen met elk van de SPA URL&#39;s waarvoor bewerkbare gebieden nodig zijn.

1. Navigeer in de nieuwe __pagina van het Adventure__ in Plaats Admin
1. Tik __creeer__ en selecteer __Pagina__
1. Selecteer het __Verre malplaatje van de SPA van de Pagina__, en Tik __daarna__
1. Pagina-eigenschappen invullen
   + __Titel__: Bali Surf Camp
   + __Naam__: `bali-surf-camp`
      + Deze waarde bepaalt URL van de AEM pagina, en moet daarom het laatste segment van de SPA&#39; route aanpassen
1. Tik __Gereed__
1. Herhaal de stappen 3-6 om __Beervana in Portland__ pagina tot stand te brengen, met:
   + __Titel__: Beervana in Portland
   + __Naam__: `beervana-in-portland`
      + Deze waarde bepaalt URL van de AEM pagina, en moet daarom het laatste segment van de SPA&#39; route aanpassen

Deze twee AEM pagina&#39;s houden de respectieve-authored inhoud voor hun passende SPA routes. Als andere SPA routes auteursrecht vereisen, moeten de nieuwe AEMPagina&#39;s bij hun SPA URL onder de Verre de wortelpagina van SPA pagina (`/content/wknd-app/us/en/home`) in AEM worden gecreeerd.

## De WKND-app bijwerken

Plaats de `<ResponsiveGrid...>` component die in het [ laatste hoofdstuk ](./spa-container-component.md) wordt gecreeerd, in onze `AdventureDetail` SPA component, die tot een editable container leiden.

### De SPA ResponsiveGrid plaatsen

Door `<ResponsiveGrid...>` in de `AdventureDetail` -component te plaatsen, wordt een bewerkbare container in die route gemaakt. De truc is dat meerdere routes de `AdventureDetail` -component gebruiken om te renderen, we het `<ResponsiveGrid...>'s pagePath` -kenmerk dynamisch moeten aanpassen. `pagePath` moet worden afgeleid om aan de overeenkomstige AEM pagina te richten, die op het avontuur wordt gebaseerd de instantie van de route toont.

1. Openen en bewerken `react-app-/src/components/AdventureDetail.js`
1. Importeer de component `ResponsiveGrid` en plaats deze boven de component `<h2>Itinerary</h2>` .
1. Stel de volgende kenmerken in voor de component `<ResponsiveGrid...>` . Het kenmerk `pagePath` voegt de huidige `slug` toe die aan de avontuurpagina wordt toegewezen volgens de bovenstaande toewijzing.
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   + `itemPath = 'root/responsivegrid'`

   Hierdoor krijgt de component `ResponsiveGrid` de opdracht om de inhoud ervan op te halen uit de AEM bron:

   + `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`

Werk `AdventureDetail.js` bij met de volgende regels:

```javascript
...
import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
...

function AdventureDetailRender(props) {
    ...
    // Get the slug from the React route parameter, this will be used to specify the AEM Page to store/read editable content from
    const { slug } = useParams();

    return(
        ...
        // Pass the slug in
        function AdventureDetailRender({ title, primaryImage, activity, adventureType, tripLength, 
                groupSize, difficulty, price, description, itinerary, references, slug }) {
            ...
            return (
                ...
                <ResponsiveGrid 
                    pagePath={`/content/wknd-app/us/en/home/adventure/${slug}`}
                    itemPath="root/responsivegrid"/>
                    
                <h2>Itinerary</h2>
                ...
            )
        }
    )
}
```

Het bestand `AdventureDetail.js` moet er als volgt uitzien:

![ AdventureDetail.js ](./assets/spa-dynamic-routes/adventure-detail-js.png)

## Auteur van de container in AEM

Nu `<ResponsiveGrid...>` op zijn plaats is en de `pagePath` dynamisch is ingesteld op basis van het avontuur dat wordt gerenderd, proberen we de inhoud ervan te ontwerpen.

1. Aanmelden bij AEM auteur
1. Navigeer aan __Plaatsen > App WKND > gebruiken > en__
1. __geeft__ __WKND de pagina van het Huis van de App__ uit
   + Navigeer aan de __Bali Surf Camp__ route in de SPA om het uit te geven
1. Selecteer __Voorproef__ van de wijze-selecteur in top-right
1. Tik op __Bali Surf Camp__ kaart in de SPA om aan zijn route te navigeren
1. Selecteer __uitgeven__ van de wijze-selecteur
1. Bepaal de plaats van de __Container van de Lay-out__ editable gebied recht boven de __Reis__
1. Open de __zijbar van de Redacteur van de Pagina__, en selecteer de __mening van Componenten__
1. Sleep enkele toegelaten componenten in de __Container van de Lay-out__
   + Afbeelding
   + Tekst
   + Titel

   En creeer wat promotiemateriaal. Het zou er ongeveer als volgt kunnen uitzien:

   ![ Bali Adventure Detail Authoring ](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __Voorproef__ uw veranderingen in AEM Redacteur van de Pagina
1. Vernieuw de WKND App die plaatselijk op [ http://localhost:3000 ](http://localhost:3000) loopt, navigeer aan de __Bali Surf Camp__ route om de authored veranderingen te zien!

   ![ Verre SPA Bali ](./assets/spa-dynamic-routes/remote-spa-final.png)

Wanneer het navigeren aan een adventure detailroute die geen in kaart gebrachte AEM Pagina heeft, is er geen auteursbaarheid op die routeinstantie. Om auteursrecht op deze pagina&#39;s toe te laten, maak eenvoudig een AEM Pagina met de passende naam onder de __pagina van het Vontuur__!

## Gefeliciteerd!

Gefeliciteerd! U hebt authoring vermogen toegevoegd aan dynamische routes in de SPA!

+ De component ResponsiveGrid van de component React Editable AEM toegevoegd aan een dynamische route
+ Opgezette AEM pagina&#39;s ter ondersteuning van het ontwerpen van twee specifieke routes in de SPA (Bali Surf Camp en Beervana in Portland)
+ Geautoriseerde inhoud op de dynamische Bali Surf Camp route!

U hebt nu het onderzoeken van de eerste stappen van hoe AEM SPA Redacteur kan worden gebruikt om specifieke editable gebieden aan een Verre SPA toe te voegen!
