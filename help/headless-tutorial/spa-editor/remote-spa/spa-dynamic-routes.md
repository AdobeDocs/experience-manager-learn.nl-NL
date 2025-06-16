---
title: Voeg Bewerkbare Componenten aan de Dynamische Routes van het KUUROORD toe
description: Leer hoe te om editable componenten aan dynamische routes in een verre KUUROORD toe te voegen.
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
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# Dynamische routes en bewerkbare componenten

{{spa-editor-deprecation}}

In dit hoofdstuk, laten wij twee dynamische routes van het Detail van het avontuur toe om editable componenten te steunen; __Bali Surf Camp__ en __Beervana in Portland__.

![ Dynamische routes en editable componenten ](./assets/spa-dynamic-routes/intro.png)

De route van het KUUROORD van het Detail van het Avontuur wordt gedefinieerd als `/adventure/:slug` waar `slug` een unieke herkenningstekenbezit op het Fragment van de Inhoud van de Avontuur is.

## De SPA-URL&#39;s toewijzen aan AEM-pagina&#39;s

In de vorige twee hoofdstukken, hebben wij editable componenteninhoud van de mening van het Huis van het KUUROORD aan overeenkomstige Verre de wortelpagina van het KUUROORD in AEM bij `/content/wknd-app/us/en/` in kaart gebracht.

Het bepalen van afbeelding voor editable componenten voor de dynamische routes van het KUUROORD is gelijkaardig maar wij moeten omhoog 1:1 kaartschema tussen instanties van de route en de pagina&#39;s van AEM komen.

In deze zelfstudie nemen we de naam van het WKND Adventure Content Fragment, het laatste segment van het pad, en wijzen we het toe aan een eenvoudig pad onder `/content/wknd-app/us/en/adventure` .

| Externe SPA-route | AEM-paginapad |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/nl/home |
| /adventure/__bali-surf-kamp__ | /content/wknd-app/us/nl/home/adventure/__bali-surf-kamp__ |
| /adventure/__beervana-portland__ | /content/wknd-app/us/nl/home/adventure/__beervana-in-portland__ |

Op basis van deze afbeelding moeten we dus twee nieuwe AEM-pagina&#39;s maken op:

* `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
* `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Externe SPA-toewijzing

De afbeelding voor verzoeken die het Verre KUUROORD verlaten wordt gevormd via de `setupProxy` configuratie die in [ Bootstrap wordt gedaan het KUUROORD ](./spa-bootstrap.md).

## SPA-editor toewijzen

De afbeelding voor de verzoeken van het KUUROORD wanneer het KUUROORD via de Redacteur van het KUUROORD wordt geopend wordt gevormd via het Verdelen van de configuratie die in [ wordt gedaan vormt AEM ](./aem-configure.md).

## Inhoudspagina&#39;s maken in AEM

Maak eerst het tussenliggende paginasegment `adventure` :

1. Aanmelden bij AEM-auteur
1. Navigeer aan __Plaatsen > WKND App > gebruiken > en > WKND App Homepage__
   1. Deze pagina van AEM wordt in kaart gebracht als wortel van SPA, zodat is dit waar wij beginnen de de paginastructuur van AEM voor andere routes van het KUUROORD te bouwen.
1. Tik __creeer__ en selecteer __Pagina__
1. Selecteer het __Verre malplaatje van de Pagina van het KUUROORD__, en Tik __daarna__
1. Pagina-eigenschappen invullen
   1. __Titel__: Adventure
   1. __Naam__: `adventure`
      1. Deze waarde bepaalt URL van de pagina van AEM, en moet daarom het de routesegment van het KUUROORD aanpassen.
1. Tik __Gereed__

Dan, creeer de pagina&#39;s van AEM die aan elk van URLs van het KUUROORD beantwoorden die editable gebieden vereisen.

1. Navigeer in de nieuwe __pagina van het Adventure__ in Plaats Admin
1. Tik __creeer__ en selecteer __Pagina__
1. Selecteer het __Verre malplaatje van de Pagina van het KUUROORD__, en Tik __daarna__
1. Pagina-eigenschappen invullen
   1. __Titel__: Bali Surf Camp
   1. __Naam__: `bali-surf-camp`
      1. Deze waarde bepaalt URL van de pagina van AEM, en moet daarom het laatste segment van de route van SPA aanpassen
1. Tik __Gereed__
1. Herhaal de stappen 3-6 om __Beervana in Portland__ pagina tot stand te brengen, met:
   1. __Titel__: Beervana in Portland
   1. __Naam__: `beervana-in-portland`
      1. Deze waarde bepaalt URL van de pagina van AEM, en moet daarom het laatste segment van de route van SPA aanpassen

Deze twee pagina&#39;s van AEM houden de respectieve-authored inhoud voor hun passende routes van het KUUROORD. Als andere routes van het KUUROORD auteursrecht vereisen, moeten de nieuwe Pagina&#39;s van AEM bij hun KUUROORD URL onder de Verre de wortelpagina van de KUUROORD (`/content/wknd-app/us/en/home`) in AEM worden gecreeerd.

## De WKND-app bijwerken

Plaats de `<ResponsiveGrid...>` component die in het [ laatste hoofdstuk ](./spa-container-component.md) wordt gecreeerd, in onze `AdventureDetail` component van het KUUROORD, die tot een editable container leiden.

### De SPA-component ResponsiveGrid plaatsen

Door `<ResponsiveGrid...>` in de `AdventureDetail` -component te plaatsen, wordt een bewerkbare container in die route gemaakt. De truc is dat meerdere routes de `AdventureDetail` -component gebruiken om te renderen, we het `<ResponsiveGrid...>'s pagePath` -kenmerk dynamisch moeten aanpassen. `pagePath` moet worden afgeleid om aan de overeenkomstige pagina van AEM te richten, die op het avontuur wordt gebaseerd de instantie van de route toont.

1. Openen en bewerken `react-app-/src/components/AdventureDetail.js`
1. Importeer de component `ResponsiveGrid` en plaats deze boven de component `<h2>Itinerary</h2>` .
1. Stel de volgende kenmerken in voor de component `<ResponsiveGrid...>` . Het kenmerk `pagePath` voegt de huidige `slug` toe die aan de avontuurpagina wordt toegewezen volgens de bovenstaande toewijzing.
   1. `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   1. `itemPath = 'root/responsivegrid'`

   Hierdoor krijgt de component `ResponsiveGrid` de opdracht om de inhoud ervan op te halen uit de AEM-bron:

   1. `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`

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

1. Aanmelden bij AEM-auteur
1. Navigeer aan __Plaatsen > App WKND > gebruiken > en__
1. __geeft__ __WKND de pagina van het Huis van de App__ uit
   1. Navigeer aan de __Bali Surf Camp__ route in het KUUROORD om het uit te geven
1. Selecteer __Voorproef__ van de wijze-selecteur in top-right
1. Tik op __Bali Surf Camp__ kaart in het KUUROORD om aan zijn route te navigeren
1. Selecteer __uitgeven__ van de wijze-selecteur
1. Bepaal de plaats van de __Container van de Lay-out__ editable gebied recht boven de __Reis__
1. Open de __zijbar van de Redacteur van de Pagina__, en selecteer de __mening van Componenten__
1. Sleep enkele toegelaten componenten in de __Container van de Lay-out__
   1. Afbeelding
   1. Tekst
   1. Titel

   En creeer wat promotiemateriaal. Het zou er ongeveer als volgt kunnen uitzien:

   ![ Bali Adventure Detail Authoring ](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __Voorproef__ uw veranderingen in de Redacteur van de Pagina van AEM
1. Vernieuw de WKND App die plaatselijk op [ http://localhost:3000 ](http://localhost:3000) loopt, navigeer aan de __Bali Surf Camp__ route om de authored veranderingen te zien!

   ![ Verre SPA Bali ](./assets/spa-dynamic-routes/remote-spa-final.png)

Wanneer het navigeren aan een avontuurdetailroute die geen in kaart gebrachte Pagina van AEM heeft, is er geen auteursbaarheid op die routeinstantie. Om auteursrecht op deze pagina&#39;s toe te laten, maak eenvoudig een Pagina van AEM met de passende naam onder de __pagina van het Vontuur__!

## Gefeliciteerd!

Gefeliciteerd! U hebt auteurscapaciteit aan dynamische routes in het KUUUROORD toegevoegd!

* De component ResponsiveGrid van de component AEM React Editable toegevoegd aan een dynamische route
* AEM-pagina&#39;s gemaakt ter ondersteuning van het ontwerpen van twee specifieke routes in het SPA (Bali Surf Camp en Beervana in Portland)
* Geautoriseerde inhoud op de dynamische Bali Surf Camp route!

U hebt nu het onderzoeken van de eerste stappen van hoe de Redacteur van AEM SPA kan worden gebruikt om specifieke editable gebieden aan een Verre KUUROORD toe te voegen!
