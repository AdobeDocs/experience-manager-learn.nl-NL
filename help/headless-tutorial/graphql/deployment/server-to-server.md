---
title: Server-naar-server implementaties zonder koptelefoon AEM
description: Leer over plaatsingsoverwegingen voor server-aan-server AEM Headless plaatsingen.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10798
thumbnail: kt-10798.jpg
exl-id: d4ae08d9-dc43-4414-ab75-26853186a301
duration: 48
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 0%

---

# Server-naar-server implementaties zonder koptelefoon AEM

AEM Headless server-aan-server plaatsingen impliceren server-zijtoepassingen of processen die en met inhoud in AEM op een krantenloze manier verbruiken en in wisselwerking staan.

Server-aan-server plaatsingen vereisen minimale configuratie, aangezien de verbindingen van HTTP aan AEM Zwaarloze APIs niet in de context van browser in werking worden gesteld.

## Implementatieconfiguraties

De volgende plaatsingsconfiguratie moet op zijn plaats voor server-aan-server toepassingsplaatsen zijn.

| Server-naar-server-app maakt verbinding met | AEM auteur | AEM Publish | Voorvertoning AEM |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [ de filters van Dispatcher ](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Delen van bronnen van oorsprong (CORS) | ✘ | ✘ | ✘ |
| [ AEM gastheren ](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Vergunningsvereisten

Geautoriseerde verzoeken om GraphQL APIs te AEM zij voorkomen typisch in de context van server-aan-server apps, aangezien andere app types, zoals [ single-page apps ](./spa.md), [ mobiele ](./mobile.md), of [ Componenten van het Web ](./web-component.md), typisch vergunning gebruiken aangezien het moeilijk is om de geloofsbrieven te beveiligen.

Wanneer het autoriseren van verzoeken aan AEM as a Cloud Service, gebruik [ de dienst op geloofsbrieven-gebaseerde symbolische authentificatie ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html). Meer over het voor authentiek verklaren van verzoeken aan AEM as a Cloud Service leren, herzie het [ op teken-gebaseerde authentificatieleerprogramma ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). Het leerprogramma verkent op teken-gebaseerde authentificatie gebruikend [ AEM Assets HTTP APIs ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html) maar de zelfde concepten en de benaderingen zijn van toepassing op apps die met AEM Headless GraphQL APIs in wisselwerking staan.

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
