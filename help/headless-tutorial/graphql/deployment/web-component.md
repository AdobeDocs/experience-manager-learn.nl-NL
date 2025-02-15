---
title: Implementaties van onderdelen van het web zonder koppen AEM
description: Leer over plaatsingsoverwegingen voor de Component van het Web/zuivere op JS-Gebaseerde AEM Headless plaatsingen.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10797
thumbnail: kt-10797.jpg
exl-id: 9d4aab4c-82af-4917-8c1b-3935f19691e6
duration: 31
source-git-commit: 089bcf71f03bdbb6d21337cc23452afb33ce8098
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 0%

---

# Implementaties van onderdelen van het web zonder koppen AEM

AEM Koploze ](https://developer.mozilla.org/en-US/docs/Web/Web_Components) /JS plaatsingen van de Component van het Web [ de Zonder kop zijn zuivere JavaScript apps die in Webbrowser lopen, die en met inhoud in AEM op een krantenloze manier verbruiken en in wisselwerking staan. De plaatsingen van de Component/JS van het Web verschillen van [ SPA plaatsingen ](./spa.md) in zoverre dat zij geen robuust SPA kader gebruiken, en naar verwachting ingebed in de context van om het even welke website, leveren, aan oppervlakte inhoud van AEM.


## Implementatieconfiguraties

De volgende plaatsingsconfiguratie moet op zijn plaats voor de plaatsingen van de Component/JS van het Web zijn.

| Web Component/JS-toepassing maakt verbinding met → | AEM auteur | AEM Publish | Voorvertoning AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [ de filters van Dispatcher ](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [ bron het delen van de kruis-oorsprong (CORS) ](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [ AEM gastheren ](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Voorbeeld van webcomponent

Adobe biedt een voorbeeld van een webcomponent.

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
                   <p class="is-size-6">Een voorbeeld van een webcomponent, geschreven in pure JavaScript, die inhoud gebruikt van AEM GraphQL-API's zonder koppen.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Voorbeeld van de Mening </span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
