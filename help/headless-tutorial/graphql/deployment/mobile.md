---
title: Mobiele implementaties zonder koptelefoon AEM
description: Meer informatie over implementatieoverwegingen voor mobiele AEM Headless-implementaties.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10796
thumbnail: KT-10796.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---


# Mobiele implementaties zonder koptelefoon AEM

AEM mobiele implementaties zonder koptelefoon zijn systeemeigen mobiele apps voor iOS, Android™, enz. die op een krantenloze manier inhoud verbruiken en interageren.

Mobiele implementaties vereisen een minimale configuratie, aangezien HTTP-verbindingen met AEM headless API&#39;s niet in de context van een browser worden gestart.

## Implementatieconfiguraties

De volgende implementatieconfiguratie moet op zijn plaats zijn voor mobiele app-implementaties.

| Mobiele app maakt verbinding met | AEM-auteur | AEM-publicatie | Voorvertoning AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Verzendingsfilters](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Delen van bronnen van oorsprong (CORS) | ✘ | ✘ | ✘ |
| [AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

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
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Voorbeeld weergeven</span>
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
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Voorbeeld weergeven</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>

