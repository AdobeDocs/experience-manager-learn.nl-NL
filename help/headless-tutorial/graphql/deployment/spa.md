---
title: SPA voor AEM GraphQL implementeren
description: Leer over plaatsingsoverwegingen voor enig-pagina app (SPA) AEM Headless plaatsingen.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
exl-id: 3fe175f7-6213-439a-a02c-af3f82b6e3b7
duration: 136
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '655'
ht-degree: 0%

---

# AEM Headless SPA-implementaties

AEM Headless single-page app (SPA)-implementaties omvatten JavaScript-gebaseerde toepassingen die zijn gebouwd met behulp van frameworks zoals React of Vue, die op een krantenloze manier inhoud in AEM verbruiken en ermee communiceren.

Het opstellen van een KUUROORD die AEM op een headless manier in wisselwerking staat impliceert het ontvangen van het KUUROORD en het toegankelijk maken via Webbrowser.

## Gastheer het SPA

Een KUUROORD bestaat uit een inzameling van inheemse Webmiddelen: **HTML, CSS, en JavaScript**. Deze middelen worden geproduceerd tijdens _bouwt_ proces (bijvoorbeeld, `npm run build`) en aan een gastheer voor consumptie door eind - gebruikers opgesteld.

Er zijn diverse **het ontvangen** opties afhankelijk van de vereisten van uw organisatie:

1. **de leveranciers van de Wolk** zoals **Azure** of **AWS**.

2. **op gebouw** het ontvangen in een collectief **gegevenscentrum**

3. **front-end het ontvangen platforms** zoals **AWS breidt**, **Azure App Service** uit, **verbeter**, **Heroku**, **Vercel**, enz.

## Implementatieconfiguraties

De belangrijkste overweging wanneer het ontvangen van een KUUROORD die met de hoofdtelefoon van AEM in wisselwerking staat, is als het KUUROORD via het domein van AEM (of gastheer), of op een verschillend domein wordt betreden.  De reden is SPAs is Webtoepassingen die in Webbrowsers lopen, en is daarom onderworpen aan Webbrowsers veiligheidsbeleid.

### Gedeeld domein

Een SPA en AEM delen domeinen wanneer allebei toegang door eind - gebruikers van het zelfde domein zijn. Bijvoorbeeld:

+ AEM is toegankelijk via: `https://wknd.site/`
+ SPA is toegankelijk via `https://wknd.site/spa`

Aangezien zowel AEM als het KUUROORD van het zelfde domein worden betreden, staan Webbrowsers het KUUROORD toe om XHR aan de Eindpunten van AEM Headless zonder de behoefte aan CORS te maken, en het delen van de koekjes van HTTP (zoals het koekje van AEM `login-token`) toe te staan.

Hoe het verkeer van het KUUROORD en van AEM op het gedeelde domein wordt verpletterd, is tot u: CDN met veelvoudige oorsprong, de server van HTTP met omgekeerde volmacht, die direct het KUUROORD in AEM ontvangen, etc.

Hieronder zijn plaatsingsconfiguraties die voor de plaatsingen van de productie van het KUUROORD worden vereist, wanneer ontvangen op het zelfde domein zoals AEM.

| SPA verbindt met → | AEM-auteur | AEM Publiceren | AEM Preview |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [ de filters van Dispatcher ](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Delen van bronnen van oorsprong (CORS) | ✘ | ✘ | ✘ |
| AEM-hosts | ✘ | ✘ | ✘ |

### Verschillende domeinen

Een SPA en AEM hebben verschillende domeinen wanneer zij door eind - gebruikers van het verschillende domein worden betreden. Bijvoorbeeld:

+ AEM is toegankelijk via: `https://wknd.site/`
+ SPA is toegankelijk via `https://wknd-app.site/`

Aangezien AEM en het KUUROORD van verschillende domeinen worden betreden, dwingen Webbrowsers veiligheidspolitiek zoals [ dwars-oorsprong middel het delen (CORS) ](./configurations/cors.md) af, en verhinderen het delen van de koekjes van HTTP (zoals AEM `login-token` koekje).

Hieronder zijn plaatsingsconfiguraties die voor de plaatsingen van de productie van het KUUROORD worden vereist, wanneer ontvangen op een verschillend domein dan AEM.

| SPA verbindt met → | AEM-auteur | AEM Publiceren | AEM Preview |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [ de filters van Dispatcher ](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [ bron het delen van de kruis-oorsprong (CORS) ](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [ de gastheren van AEM ](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### Voorbeeld-SPA-implementatie op verschillende domeinen

In dit voorbeeld, wordt het KUUROORD opgesteld aan een Netlify domein (`https://main--sparkly-marzipan-b20bf8.netlify.app/`) en het KUUROORD gebruikt AEM GraphQL APIs van het AEM publiceer domein (`https://publish-p65804-e666805.adobeaemcloud.com`). In de onderstaande schermafbeeldingen wordt de vereiste CORS benadrukt.

1. Het KUUROORD wordt gediend van een Netlify domein, maar maakt een XHR vraag aan AEM GraphQL APIs op een verschillend domein. Dit dwars-plaats verzoek vereist [ CORS ](./configurations/cors.md) om opstelling op AEM worden geplaatst om verzoek van het domein toe te staan Netlify om tot zijn inhoud toegang te hebben.

   ![ verzoek van het KUUROORD van gastheren van SPA &amp; van AEM ](assets/spa/cors-requirement.png)

2. Wanneer de XHR-aanvraag wordt gecontroleerd op de AEM GraphQL API, is de `Access-Control-Allow-Origin` aanwezig en wordt aan de webbrowser doorgegeven dat AEM een aanvraag van dit Netlify-domein toestaat om toegang te krijgen tot de inhoud ervan.

   Als AEM [ CORS ](./configurations/cors.md) ontbrak of niet het Netlify domein omvatte, zou Webbrowser het XHR- verzoek ontbreken, en een fout melden CORS.

   ![ de Kopbal van de Reactie van CORS AEM GraphQL API ](assets/spa/cors-response-headers.png)

## Voorbeeld-app van één pagina

Adobe biedt een voorbeeld van een app van één pagina die in React is gecodeerd.

<div class="columns is-multiline">
<!-- React app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/react-app.md" title="Toepassingen Reageren" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app-card.png" alt="Toepassingen Reageren">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/react-app.md" title="Toepassingen Reageren">Toepassingen Reageren</a></p>
               <p class="is-size-6">Een voorbeeld van een app van één pagina, geschreven in React, die inhoud van AEM Headless GraphQL APIs verbruikt.</p>
               <a href="../example-apps/react-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Voorbeeld van de Mening </span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Next.js app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Next.js app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/next-js.md" title="De app Next.js" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/next-js/next-js-card.png" alt="De app Next.js">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/next-js.md" title="De app Next.js">De app Next.js</a></p>
               <p class="is-size-6">Een voorbeeld van een app van één pagina, geschreven in Next.js, die inhoud van AEM Headless GraphQL APIs verbruikt.</p>
               <a href="../example-apps/next-js.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Voorbeeld van de Mening </span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
