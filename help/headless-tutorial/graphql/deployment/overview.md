---
title: Implementaties zonder kop AEM
description: Meer informatie over de verschillende implementatieoverwegingen voor AEM Headless-apps.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
exl-id: 6de58ca0-9444-4272-9487-15a9e3c89231
duration: 59
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---

# Implementaties zonder kop AEM

AEM Clientimplementaties zonder koptelefoon hebben vele vormen: SPA, externe SPA, websites, mobiele apps of zelfs server-naar-server processen die door AEM worden gehost.

Afhankelijk van de client en hoe deze wordt geïmplementeerd, hebben AEM headless-implementaties verschillende overwegingen.

## AEM

Alvorens plaatsingsoverwegingen te onderzoeken, is het noodzakelijk om AEM logische architectuur, en de scheiding en de rollen van AEM as a Cloud Service de dienstrijen te begrijpen. AEM as a Cloud Service bestaat uit twee logische diensten:

+ __AEM auteur__ Dit is de service waar teams Content Fragments (en andere elementen) maken, samenwerken en publiceren.
+ __AEM publiceren__ is de dienst die gepubliceerde Contentfragmenten (en andere activa) werden herhaald voor algemeen gebruik.
+ __Voorvertoning AEM__ Dit is de service die AEM Publiceren in gedrag bootst, maar inhoud heeft die er voor voorvertoning of revisiedoeleinden aan wordt gepubliceerd. AEM Voorvertoning is bedoeld voor intern publiek en niet voor de algemene levering van inhoud. Het gebruik van AEM voorvertoning is optioneel op basis van de gewenste workflow.

![AEM](./assets/overview/aem-service-architecture.png)

Typische AEM as a Cloud Service implementatiearchitectuur zonder kop_

AEM Koploze klanten die in een productiecapaciteit werken, werken doorgaans samen met AEM Publish, dat de goedgekeurde, gepubliceerde inhoud bevat. Clients die met AEM auteur werken, moeten speciale aandacht besteden, aangezien AEM auteur standaard veilig is en toestemming voor alle aanvragen vereist, en kunnen ook werk in uitvoering of niet-goedgekeurde inhoud bevatten.

## Hoofdloze clientimplementaties

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="App van één pagina (SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="Apps van één pagina (SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="App van één pagina (SPA)">App van één pagina (SPA)</a></p>
                   <p class="is-size-6">Meer informatie over implementatieoverwegingen voor apps van één pagina (SPA).</p>
                   <a href="./spa.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Meer informatie</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
<!-- Web component/JS -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web component/JS" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./web-component.md" title="Webcomponent/JS" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/web-component/web-component-card.png" alt="Webcomponent/JS">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./web-component.md" title="Webcomponent/JS">Webcomponent/JS</a></p>
               <p class="is-size-6">Meer informatie over implementatieoverwegingen voor webcomponenten en JavaScript-gebruikers zonder kop in een browser.</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Meer informatie</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Mobile apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Mobile apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./mobile.md" title="Mobiele apps" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/mobile/mobile-card.png" alt="Mobiele apps">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./mobile.md" title="Mobiele apps">Mobiele app</a></p>
               <p class="is-size-6">Meer informatie over implementatieoverwegingen voor mobiele apps.</p>
               <a href="./mobile.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Meer informatie</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Server-to-server apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./server-to-server.md" title="Server-naar-server apps" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/server-to-server/server-to-server-card.png" alt="Server-naar-server apps">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./server-to-server.md" title="Server-naar-server apps">Server-naar-server-app</a></p>
               <p class="is-size-6">Meer informatie over implementatieoverwegingen voor server-naar-server-apps</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Meer informatie</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
