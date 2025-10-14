---
title: Implementaties van AEM Headless Web Component
description: Leer over plaatsingsoverwegingen voor de Component van het Web/zuivere op JS-Gebaseerde plaatsingen van AEM Headless.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10797
thumbnail: kt-10797.jpg
exl-id: 9d4aab4c-82af-4917-8c1b-3935f19691e6
duration: 31
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 0%

---

# Implementaties van AEM Headless Web Component

De Plaatsingen van de Component 1&rbrace;/JS van de Component van het Web van AEM Headless [&#x200B; zijn zuivere JavaScript apps die in Webbrowser lopen, die op een krantenloze manier verbruiken en met inhoud in AEM in wisselwerking staan. &#x200B;](https://developer.mozilla.org/en-US/docs/Web/Web_Components) De plaatsingen van de Component/JS van het Web verschillen van [&#x200B; plaatsingen van het KUUROORD &#x200B;](./spa.md) in die zin dat zij geen robuust kader van het KUUROORD gebruiken, en worden verwacht om in de context van om het even welke website worden ingebed, leveren, aan oppervlakte inhoud van AEM.


## Implementatieconfiguraties

De volgende plaatsingsconfiguratie moet op zijn plaats voor de plaatsingen van de Component/JS van het Web zijn.

| Web Component/JS-toepassing maakt verbinding met → | AEM-auteur | AEM Publiceren | AEM Preview |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [&#x200B; de filters van Dispatcher &#x200B;](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [&#x200B; bron het delen van de kruis-oorsprong (CORS) &#x200B;](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [&#x200B; de gastheren van AEM &#x200B;](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

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
                   <p class="is-size-6">Een voorbeeld van een webcomponent, geschreven in pure JavaScript, die inhoud gebruikt van AEM Headless GraphQL-API's.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Voorbeeld van de Mening </span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
