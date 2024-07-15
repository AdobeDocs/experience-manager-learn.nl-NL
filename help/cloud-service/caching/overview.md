---
title: AEM as a Cloud Service caching
description: Algemeen overzicht van AEM as a Cloud Service in cache plaatsen.
version: Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: e76ed4c5-3220-4274-a315-a75e549f8b40
duration: 36
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 0%

---

# AEM as a Cloud Service caching

In AEM as a Cloud Service is het begrijpen van caching cruciaal. In cache plaatsen houdt in dat eerder opgehaalde gegevens worden opgeslagen en opnieuw gebruikt om de efficiëntie van het systeem te verbeteren en de laadtijden te verkorten. Dit mechanisme versnelt de levering van inhoud aanzienlijk, verhoogt de prestaties van de website en optimaliseert de gebruikerservaring.

AEM as a Cloud Service heeft meerdere lagen in cache en strategieën die verschillen tussen de services van Auteur en Publish.

![ AEM as a Cloud Service caching overzicht ](./assets/overview/all.png){align="center"}

## AEM caching

AEM as a Cloud Service heeft een robuuste, configureerbare multi-layer caching strategie, met inbegrip van een CDN, AEM Dispatcher, en naar keuze een klant beheerde CDN. In cache plaatsen over lagen kan worden aangepast om de prestaties te optimaliseren, zodat AEM alleen de beste ervaringen oplevert. AEM heeft verschillende problemen met caching voor de services van Auteur en Publish. Ontdek de caching strategieën voor elke hieronder dienst.


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="AEM Publish-service" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="AEM Publish service caching">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="AEM Publish service caching">AEM Publish service caching</a></p>
            <p class="is-size-6">AEM Publish-service gebruikt een beheerde CDN en AEM Dispatcher om webervaringen voor eindgebruikers te optimaliseren.</p>
            <a href="./publish.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leren </span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Author service caching">
        <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
            <div class="card-image">
            <figure class="image is-16by9">
                <a href="./author.md" title="AEM Auteur-service in cache plaatsen" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="AEM Auteur-service in cache plaatsen">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="AEM Auteur-service in cache plaatsen">AEM Auteur-service in cache plaatsen</a></p>
                <p class="is-size-6">AEM de dienst van de Auteur gebruikt beheerde CDN om geoptimaliseerde auteurservaringen te verstrekken.</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leren </span>
                </a>
            </div>
            </div>
        </div>
    </div>
</div>
