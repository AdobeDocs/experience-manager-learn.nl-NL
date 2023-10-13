---
title: as a Cloud Service caching AEM
description: Algemeen overzicht van AEM as a Cloud Service caching.
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
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 0%

---

# as a Cloud Service caching AEM

In AEM as a Cloud Service is het begrijpen van caching cruciaal. In cache plaatsen houdt in dat eerder opgehaalde gegevens worden opgeslagen en opnieuw gebruikt om de efficiëntie van het systeem te verbeteren en de laadtijden te verkorten. Dit mechanisme versnelt de levering van inhoud aanzienlijk, verhoogt de prestaties van de website en optimaliseert de gebruikerservaring.

AEM as a Cloud Service heeft veelvoudige caching lagen, en strategieën die tussen de auteur en de Publish diensten verschillen.

![Overzicht van as a Cloud Service caching AEM](./assets/overview/all.png){align="center"}

## AEM caching

AEM as a Cloud Service heeft een robuuste, configureerbare multi-layer caching strategie, met inbegrip van een CDN, AEM Dispatcher, en naar keuze een klant beheerde CDN. In cache plaatsen over lagen kan worden aangepast om de prestaties te optimaliseren, zodat AEM alleen de beste ervaringen oplevert. AEM heeft verschillende caching-zorgen voor de auteur- en publicatieservices. Ontdek de caching strategieën voor elke hieronder dienst.


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="AEM-publicatieservice" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="AEM in cache plaatsen van publicatieservice">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="AEM in cache plaatsen van publicatieservice">AEM in cache plaatsen van publicatieservice</a></p>
            <p class="is-size-6">AEM publicatieservice gebruikt een beheerde CDN en AEM Dispatcher om webervaringen van eindgebruikers te optimaliseren.</p>
            <a href="./publish.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Meer informatie</span>
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
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Meer informatie</span>
                </a>
            </div>
            </div>
        </div>
    </div>
</div>
