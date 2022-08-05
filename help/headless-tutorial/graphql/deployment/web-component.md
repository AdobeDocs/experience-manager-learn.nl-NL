---
title: Implementaties van onderdelen van het web zonder koppen AEM
description: Leer over plaatsingsoverwegingen voor de Component van het Web/zuivere op JS-Gebaseerde AEM Headless plaatsingen.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 0%

---


# Implementaties van onderdelen van het web zonder koppen AEM

AEM zonder kop [Webcomponent](https://developer.mozilla.org/en-US/docs/Web/Web_Components)/JS-implementaties zijn pure JavaScript-toepassingen die in een webbrowser worden uitgevoerd en die zonder kop inhoud verbruiken en interageren. Webcomponenten/JS-implementaties verschillen van [Implementaties SPA](./spa.md) in die zin dat ze geen robuust SPA gebruiken en dat ze naar verwachting in de context van een website worden ingesloten, leveren, aan oppervlakinhoud van AEM.


## Implementatieconfiguraties

De volgende plaatsingsconfiguratie moet op zijn plaats voor de plaatsingen van de Component/JS van het Web zijn.

| Webcomponent/JS-app maakt verbinding met | AEM-auteur | AEM-publicatie | Voorvertoning AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Verzendingsfilters](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Delen van bronnen van oorsprong (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Voorbeeld van webcomponent

Adobe verstrekt een component van het voorbeeldWeb.

<div class="columns is-multiline">
    <!-- Web Component -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/web-component.md" title="Webcomponent" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/web-component/web-component-card.png" alt="Webcomponent">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/web-component.md" title="Webcomponent">Webcomponent</a></p>
                   <p class="is-size-6">Een voorbeeldcomponent van het Web, die in zuiver JavaScript wordt geschreven, die inhoud van AEM Headless GraphQL APIs verbruikt.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Voorbeeld weergeven</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
