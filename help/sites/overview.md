---
title: AEM Sites-video's en-tutorials
description: Bekijk video's en zelfstudies over de functies en mogelijkheden van Adobe Experience Manager Sites. AEM Sites is een toonaangevend platform voor ervaringsbeheer.
solution: Experience Manager, Experience Manager Sites
sub-product: Experience Manager Sites
topic: Content Management
doc-type: Catalog
exl-id: cde4ce7f-0afe-4632-8c1c-354586f296d5
source-git-commit: 36917be459162e5399620c976bfe953cc5553c82
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 2%

---

# AEM Sites - video&#39;s en zelfstudies {#overview}

{{edge-delivery-services}}

Adobe Experience Manager (AEM) Sites is een toonaangevend platform voor ervaringsbeheer. Deze gebruikershandleiding bevat video&#39;s en zelfstudies over de vele functies en mogelijkheden van AEM Sites.

## Drie manieren om te bouwen met AEM Sites

AEM Sites biedt drie manieren om ervaringen op te bouwen, te schrijven en te leveren. Of u nu volledige pagina&#39;s samenstelt, optimaliseert voor de prestaties van de Edge-server of toepassingen zonder kop gebruikt, AEM Sites biedt flexibele opties die aan uw projectbehoeften voldoen:

1. **Edge Delivery Services** de websites hefboomwerkings op document-gebaseerde auteur of de Universele Redacteur van Adobe aan auteursinhoud, die dan wordt geactiveerd, en dan aan eind - gebruikers door Edge Delivery Services als Webpagina&#39;s van HTML wordt geleverd. Deze optie is hoofdzakelijk voor _nieuwe en bestaande projecten_ die hoge prestaties, scalability, en snelheid vereisen.
1. **Headless/API-eerste** de gebruiksRedacteur van het Fragment van de Inhoud of Universele Redacteur aan auteursinhoud, die dan wordt geactiveerd, en door AEM wordt geleverd publiceren als JSON. Deze optie is hoofdzakelijk voor _nieuwe en bestaande projecten_ die hoofdloze levering van inhoud aan mobiele apps, enig-paginatoepassingen (SPAs), of andere toepassingen zonder kop vereisen.
1. **Traditionele AEM** is niet de huidigste benadering van de ervaringen van het bouwerWeb gebruikend AEM Sites. Traditionele AEM gebruikt de Pagina-editor van de AEM Auteur om inhoud te ontwerpen. Deze wordt vervolgens geactiveerd en door AEM Publish als HTML-webpagina&#39;s aan eindgebruikers geleverd. Traditionele AEM wordt geadviseerd voor _bestaande projecten_.

Deze opties zijn ontworpen om aan de uiteenlopende behoeften van marketingorganisaties te voldoen, om persoonlijke, boeiende ervaringen met hoge snelheid en schaal te bieden voor elk kanaal of apparaat.

>[!IMPORTANT]
>
> **Edge Delivery Services** is de recentste manier om met AEM Sites te bouwen. Het is ontworpen om krachtige websites op grote schaal te leveren, waarbij de kracht van Adobe Edge Network wordt benut.

In het volgende diagram worden de verschillende paden geïllustreerd:

![ AEM-Sites-Content-Authoring-and-Experience-Delivery-Paths.png ](./assets/aem-sites-authoring-and-experience-delivery-paths.png){width="700" zoomable="yes"}

### Vergelijk de manieren om te bouwen met AEM Sites

De volgende tabel biedt een vergelijking op hoog niveau van de drie paden. Het richt zich op de inhoud creatie en ervaart leveringsnuances van elk weg.

|            | Edge Delivery Services | Koploos / API-eerste | Traditioneel AEM |
|---------------------|------------------------------|---------------------------------|---------------------------------------------|
| **Best voor** | Websites met hoge vereisten voor verkeer, prestaties en schaalbaarheid | Mobiele apps, SPA&#39;s en andere toepassingen zonder kop | Bestaande projecten (niet de meest actuele aanpak) |
| **Authoring Hulpmiddelen** | Op documenten gebaseerde authoring, Universal Editor | Inhoudsfragmenten, Universele editor | Pagina-editor |
| **Geautoriseerde Opslag van de Inhoud** | Documenten of AEM-auteur (JCR) | AEM-auteur (JCR) | AEM-auteur (JCR) |
| **Levering** | Edge Delivery Services | AEM Publiceren (met Adobe CDN + Dispatcher) | AEM Publiceren (met Adobe CDN + Dispatcher) |
| **Opslag van de Inhoud van de Levering** | Edge Delivery Services | AEM Publish (JCR) | AEM Publish (JCR) |
| **Indeling van de Levering** | HTML | JSON | HTML |
| **Technologie van de Ontwikkeling** | JavaScript, CSS | Willekeurig (bv. Swift, Reageren, enz.) | Java™, JavaScript, CSS |
| **het Stadium van de Implementatie** | Nieuwe en bestaande projecten | Nieuwe en bestaande projecten | Alleen bestaande projecten |

## Tutorials

Bekijk de volgende zelfstudies voor meer informatie over de drie paden die u met AEM Sites wilt bouwen:

<!-- CARDS

* https://www.aem.live/docs/
  {title = Edge Delivery Services - Guides}
  {description = Explore Edge Delivery Services with comprehensive guides. The Build, Publish, and Launch guides cover everything you need to get started with EDS.}
  {image = ./assets/edge-delivery-services.png}
  {target = _blank}
* https://experienceleague.adobe.com/nl/docs/experience-manager-learn/getting-started-with-aem-headless/overview
  {title = Headless/API-First - Tutorials}
  {description = Learn how to build headless applications powered by AEM content. Tutorials cover frameworks like iOS, Android, and React—choose what fits your stack.}
  {image = ./assets/headless.png}
  {target = _self}
* https://experienceleague.adobe.com/nl/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview
  {title = Traditional AEM - WKND Tutorial}
  {description = Learn how to build a sample AEM Sites project using the WKND tutorial. This guide walks you through project setup, Core Components, Editable Templates, client-side libraries, and component development.}
  {image = ./assets/aem-wknd-spa-editor-tutorial.png}
  {target = _self}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Edge Delivery Services - Guides">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://www.aem.live/docs/" title="Edge Delivery Services - Hulplijnen" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/edge-delivery-services.png" alt="Edge Delivery Services - Hulplijnen"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" title="Edge Delivery Services - Hulplijnen"> Edge Delivery Services - Gidsen </a>
                    </p>
                    <p class="is-size-6">Ontdek Edge Delivery Services met uitgebreide gidsen. De gidsen van de Bouwstijl, van de Publicatie, en van de Lancering behandelen alles u met EDS moet beginnen.</p>
                </div>
                <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Headless/API-First - Tutorials">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/nl/docs/experience-manager-learn/getting-started-with-aem-headless/overview" title="Headless/API-First - Lesbestanden" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/headless.png" alt="Headless/API-First - Lesbestanden"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/nl/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" title="Headless/API-First - Lesbestanden"> Headless/API-Eerste - Leerprogramma's </a>
                    </p>
                    <p class="is-size-6">Leer hoe u toepassingen zonder koppen kunt maken met AEM-inhoud. De leerprogramma's behandelen kaders zoals iOS, Android, en React-kiezen wat uw stapel past.</p>
                </div>
                <a href="https://experienceleague.adobe.com/nl/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Traditional AEM - WKND Tutorial">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/nl/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" title="Traditionele AEM - WKND-zelfstudie" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/aem-wknd-spa-editor-tutorial.png" alt="Traditionele AEM - WKND-zelfstudie"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/nl/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" title="Traditionele AEM - WKND-zelfstudie"> Traditionele AEM - WKND Leerprogramma </a>
                    </p>
                    <p class="is-size-6">Leer hoe u een voorbeeld-AEM Sites-project maakt aan de hand van de WKND-zelfstudie. Deze gids begeleidt u door projectopstelling, de Componenten van de Kern, Bewerkbare Malplaatjes, cliënt-zijbibliotheken, en componentenontwikkeling.</p>
                </div>
                <a href="https://experienceleague.adobe.com/nl/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## Aanvullende bronnen

* [ AEM Sites Authoring documentatie ](https://experienceleague.adobe.com/nl/docs/experience-manager-65/content/sites/authoring/essentials/first-steps)
* [ AEM Sites het Ontwikkelen documentatie ](https://experienceleague.adobe.com/nl/docs/experience-manager-65/content/implementing/developing/introduction/getting-started)
* [ AEM Sites die documentatie beheren ](https://experienceleague.adobe.com/nl/docs/experience-manager-65/content/sites/administering/home)
* [ AEM Sites die documentatie ](https://experienceleague.adobe.com/nl/docs/experience-manager-65/content/implementing/deploying/introduction/platform) opstelt
* [AEM as a Cloud Service-zelfstudies](/help/cloud-service/overview.md)
* [AEM Assets-zelfstudies](/help/assets/overview.md)
* [AEM Forms-zelfstudies](/help/forms/overview.md)
* [Zelfstudies voor AEM Foundation](/help/foundation/overview.md)
