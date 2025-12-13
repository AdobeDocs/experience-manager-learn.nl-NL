---
title: AEM Headless server-aan-server plaatsingen
description: Meer informatie over implementatieoverwegingen voor server-naar-server AEM Headless-implementaties.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
jira: KT-10798
thumbnail: kt-10798.jpg
exl-id: d4ae08d9-dc43-4414-ab75-26853186a301
duration: 48
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# AEM Headless server-aan-server plaatsingen

AEM Headless server-aan-server plaatsingen impliceren server-zijtoepassingen of processen die en met inhoud in AEM op een krantenloze manier verbruiken en in wisselwerking staan.

Server-naar-server plaatsingen vereisen minimale configuratie, aangezien de verbindingen van HTTP aan AEM Headless APIs niet in de context van browser in werking worden gesteld.

## Implementatieconfiguraties

De volgende plaatsingsconfiguratie moet op zijn plaats voor server-aan-server toepassingsplaatsen zijn.

| Server-naar-server toepassing verbindt met → | AEM-auteur | AEM Publiceren | AEM Preview |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [&#x200B; de filters van Dispatcher &#x200B;](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Delen van bronnen van oorsprong (CORS) | ✘ | ✘ | ✘ |
| [&#x200B; de gastheren van AEM &#x200B;](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Vergunningsvereisten

Erkende verzoeken aan AEM GraphQL APIs voorkomen zij typisch in de context van server-aan-server apps, aangezien andere app types, zoals [&#x200B; single-page apps &#x200B;](./spa.md), [&#x200B; mobiele &#x200B;](./mobile.md), of [&#x200B; Componenten van het Web &#x200B;](./web-component.md), typisch vergunning gebruiken aangezien het moeilijk is om de geloofsbrieven te beveiligen.

Wanneer het autoriseren van verzoeken aan AEM as a Cloud Service, gebruik [&#x200B; de dienst op geloofsbrieven-gebaseerde symbolische authentificatie &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html). Meer over het voor authentiek verklaren van verzoeken aan AEM as a Cloud Service leren, herzie het [&#x200B; op teken-gebaseerde authentificatieleerprogramma &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). Het leerprogramma verkent op teken-gebaseerde authentificatie gebruikend [&#x200B; AEM Assets HTTP APIs &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html) maar de zelfde concepten en de benaderingen zijn van toepassing op apps die met AEM Headless GraphQL APIs in wisselwerking staan.

## Voorbeeld van een server-naar-server-app

Adobe biedt een voorbeeld van een server-naar-server-app die is gecodeerd in Node.js.

<div class="columns is-multiline">
    <!-- Server-to-server app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/server-to-server-app.md" title="Server-naar-server-app" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/server-to-server-app/server-to-server-card.png" alt="Server-naar-server-app">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/server-to-server-app.md" title="Server-naar-server-app">Server-naar-server-app</a></p>
                   <p class="is-size-6">Een voorbeeld van een server-naar-server-app, geschreven in Node.js, die inhoud van AEM Headless GraphQL APIs verbruikt.</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Voorbeeld van de Mening </span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
