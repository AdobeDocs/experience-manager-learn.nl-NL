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
duration: 83
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 0%

---

# Server-naar-server implementaties zonder koptelefoon AEM

AEM Headless server-aan-server plaatsingen impliceren server-zijtoepassingen of processen die en met inhoud in AEM op een krantenloze manier verbruiken en in wisselwerking staan.

Server-aan-server plaatsingen vereisen minimale configuratie, aangezien de verbindingen van HTTP aan AEM Zwaarloze APIs niet in de context van browser in werking worden gesteld.

## Implementatieconfiguraties

De volgende plaatsingsconfiguratie moet op zijn plaats voor server-aan-server toepassingsplaatsen zijn.

| Server-naar-server-app maakt verbinding met | AEM auteur | AEM publiceren | Voorvertoning AEM |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Verzendingsfilters](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Delen van bronnen van oorsprong (CORS) | ✘ | ✘ | ✘ |
| [AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Vergunningsvereisten

Aangeautoriseerde aanvragen voor AEM GraphQL API&#39;s vinden doorgaans plaats in de context van server-naar-server-apps, aangezien andere app-typen, zoals [apps van één pagina](./spa.md), [mobiel](./mobile.md), of [Webcomponenten](./web-component.md), gebruikt doorgaans geen autorisatie omdat het moeilijk is om de gegevens te beveiligen.

Wanneer het autoriseren van verzoeken om as a Cloud Service te AEM, gebruik [op referenties gebaseerde tokenverificatie van de service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html). Als u meer wilt weten over het verifiëren van aanvragen voor AEM as a Cloud Service, raadpleegt u de [tokengebaseerde zelfstudie over verificatie](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). De zelfstudie verkent tokengebaseerde verificatie met behulp van [AEM Assets HTTP-API&#39;s](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html) Maar dezelfde concepten en benaderingen zijn van toepassing op toepassingen die werken met AEM Headless GraphQL-API&#39;s.

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
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Voorbeeld weergeven</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
