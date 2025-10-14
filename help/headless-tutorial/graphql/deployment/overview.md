---
title: AEM Headless-implementaties
description: Meer informatie over de verschillende implementatieoverwegingen voor AEM Headless-apps.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
exl-id: 6de58ca0-9444-4272-9487-15a9e3c89231
duration: 59
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---

# AEM Headless-implementaties

AEM Headless-clientimplementaties hebben vele vormen: door AEM gehoste SPA, externe SPA, website, mobiele app of zelfs server-naar-server proces.

Afhankelijk van de client en de manier waarop deze wordt geïmplementeerd, hebben AEM Headless-implementaties verschillende overwegingen.

## AEM-servicesarchitectuur

Alvorens plaatsingsoverwegingen te onderzoeken, is het noodzakelijk om de logische architectuur van AEM, en de scheiding en de rollen van de de dienstrijen van AEM as a Cloud Service te begrijpen. AEM as a Cloud Service bestaat uit twee logische services:

+ __de Auteur van AEM__ is de dienst waar de teams creeren, samenwerken, en de Fragmenten van de Inhoud (en andere activa) publiceren.
+ __AEM publiceert__ is de dienst die de gepubliceerde Fragmenten van de Inhoud (en andere activa) voor algemene consumptie worden herhaald.
+ __de Voorproef van AEM__ is de dienst die AEM in gedrag mimiteert, maar inhoud heeft die aan het voor voorproef of overzichtsdoeleinden wordt gepubliceerd. AEM Preview is bedoeld voor intern publiek en niet voor de algemene levering van inhoud. Het gebruik van AEM Preview is optioneel op basis van de gewenste workflow.

![&#x200B; de dienstarchitectuur van AEM &#x200B;](./assets/overview/aem-service-architecture.png)

Typische AEM as a Cloud Service-implementatie zonder kop_

AEM Headless-clients die in een productiecapaciteit werken, communiceren doorgaans met AEM Publish, dat de goedgekeurde, gepubliceerde inhoud bevat. Clients die met AEM Author communiceren, moeten speciale aandacht besteden, aangezien de AEM Author standaard veilig is, waarbij toestemming voor alle aanvragen vereist is, en kunnen ook werk in uitvoering of niet-goedgekeurde inhoud bevatten.

## Hoofdloze clientimplementaties

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="Single-page App (SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="Apps van één pagina (SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="Single-page App (SPA)">App van één pagina (SPA)</a></p>
                   <p class="is-size-6">Leer over plaatsingsoverwegingen voor enig-pagina apps (SPA).</p>
                   <a href="./spa.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leren </span>
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
               <p class="is-size-6">Meer informatie over implementatieoverwegingen voor webcomponenten en op een browser gebaseerde JavaScript-gebruikers zonder kop.</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leren </span>
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
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leren </span>
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
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leren </span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
