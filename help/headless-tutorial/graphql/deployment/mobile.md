---
title: AEM Headless mobile-implementaties
description: Meer informatie over implementatieoverwegingen voor mobiele AEM Headless-implementaties.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10796
thumbnail: KT-10796.jpg
exl-id: 1f536079-b3ce-4807-be88-804378e75d37
duration: 31
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 0%

---

# AEM Headless mobile-implementaties

Mobiele AEM Headless-implementaties zijn systeemeigen mobiele apps voor iOS, Android™, enz. die inhoud in AEM op een krantenloze manier verbruiken en interageren.

Mobiele implementaties vereisen een minimale configuratie, aangezien HTTP-verbindingen met AEM Headless API&#39;s niet worden geïnitieerd in de context van een browser.

## Implementatieconfiguraties

De volgende implementatieconfiguratie moet op zijn plaats zijn voor mobiele apps.

| Mobiele app maakt verbinding met → | AEM-auteur | AEM Publiceren | AEM Preview |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [&#x200B; de filters van Dispatcher &#x200B;](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Delen van bronnen van oorsprong (CORS) | ✘ | ✘ | ✘ |
| [&#x200B; de gastheren van AEM &#x200B;](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Voorbeeld van mobiele apps

Adobe biedt voorbeelden van mobiele apps voor iOS en Android™.

<div class="columns is-multiline">
    <!-- iOS app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="iOS app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/ios-swiftui-app.md" title="iOS-app" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/ios-swiftui-app/ios-app-card.png" alt="iOS-app">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/ios-swiftui-app.md" title="iOS-app">iOS-app</a></p>
                   <p class="is-size-6">Een voorbeeld van een iOS-app, geschreven in SwiftUI, die inhoud van AEM Headless GraphQL-API's gebruikt.</p>
                   <a href="../example-apps/ios-swiftui-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Voorbeeld van de Mening </span>
                   </a>
               </div>
           </div>
       </div>
    </div>
    <!-- Android app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Android app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/android-app.md" title="Android™-app" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/android-java-app/android-app-card.png" alt="Android-app">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/android-app.md" title="Android™-app">Android™-app</a></p>
                   <p class="is-size-6">Een voorbeeld van een Java™ Android™-app die inhoud van AEM Headless GraphQL-API's gebruikt.</p>
                   <a href="../example-apps/android-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Voorbeeld van de Mening </span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
