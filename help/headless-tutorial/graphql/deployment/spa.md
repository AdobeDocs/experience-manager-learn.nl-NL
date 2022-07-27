---
title: SPA voor AEM GraphQL implementeren
description: Leer SPA plaatsingsopties met betrekking tot AEM GraphQL, Headless.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: c7d2e69a9039cfbaa43d6d9b65b9fa6f69378716
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---


# Een SPA implementeren

In deze sectie, zullen wij een benadering voor het opstellen van SPA (Reageren, Waarde, Angular, enz.) herzien die AEM GraphQL APIs aanhaalt om de gegevens te laden, echter, alvorens dat de high-level artefacten moeten begrijpen die moeten worden opgesteld.

**SPA App-artefacten samenstellen:**

De bestanden die door het SPA-framework worden gemaakt, **HTML, CSS, JS**, ook bekend als statische bouwartefacten. In het geval van React app, de artefacten van `build` directory en voor Vue-artefacten van de `dist` directory.
Het verzoek aan uw SPA (bijvoorbeeld https://HOST/my-aem-spa.html) zal worden betekend gebruikend deze bouwartefacten.

**AEM GraphQL-API&#39;s:**

Duidelijk, dit parameter GraphQL API (`/graphql/execute.json/<PROJECT-CONFIG>/<PERSISTED-QUERY-NAME>`) moet worden gehost op het AEM domein.

Samenvattend heeft SPA implementatieschitectuur twee onderdelen *1. SPA 2. AEM GraphQL API-laag*, dus laten wij de plaatsingsopties voor deze twee delen herzien.


## Implementatieopties

| Implementatieoptie | URL SPA | AEM GraphQL API-URL | Config. CORS vereist? |
| ---------|---------- | ---------|---------- |
| **Hetzelfde domein** | https://**HOST**/my-aem-spa.html | https://**HOST**/graphql/execute.json/... | ✘ |
| **Verschillend domein** | https://**SPA-HOST**/my-aem-spa.html | https://**AEM-HOST**/graphql/execute.json/... | ✔ |

**Hetzelfde domein:**\
Beide *SPA en AEM GraphQL API-laag* in deze optie wordt ingezet op **Hetzelfde domein**. Betekenis van verzoek aan SPA URI `/my-aem-spa.html` en GraphQL API-laag `/graphql/execute.json/` vanuit exact hetzelfde domein worden aangeboden.

**Verschillend domein:**\
Beide *SPA en AEM GraphQL API-laag* in deze optie wordt opgesteld aan **Verschillend domein**. Betekenis van verzoek aan SPA URI `/my-aem-spa.html` wordt aangeboden **verschillend domein** than GraphQL API layer `/graphql/execute.json/` verzoeken. Houd er rekening mee dat als onderdeel van deze implementatieoptie: [CORS configureren](cors.md) op de AEM instantie.

>[!NOTE]
>
>U MOET CORS op de juiste manier configureren bij AEM instantie, [zie hier stappen](cors.md).

### Implementeren op hetzelfde domein

Wanneer het opstellen op het Zelfde Domein, zou het domein een **primair AEM** (Op AEM domein) of **primair SPA** (Buiten AEM Domein), en de inkomende SPA, kunnen AEM verzoeken GraphQL API bij om het even welke plaatsingscomponenten zoals CDN ([Snel](https://docs.fastly.com/en/guides/routing-assets-to-different-origins), Akamai, [CloudFront](https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-distribution-serve-content/)), [HTTPD met reverse-proxy](https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html). Met andere woorden, stelt u nog de SPA bouwt artefacten en AEM GraphQL APIs aan verschillende servers op maar voor eind - gebruikers, die van één enkel domein en achter de scène worden geleverd die aan een verschillende bestemming of oorsprongservers wordt verpletterd.

Ook kunnen SPA constructieartefacten worden gehost in AEM *maar worden niet aanbevolen.*

| Dezelfde domeinimplementatie | CDN splitsen | HTTPD + reverse-proxy | AEM gehoste SPA artefacten |
| ---------|---------- | ---------|---------- |
| **ON AEM** | ✔ | ✔ | ✔ |
| **UIT AEM domein** | ✔ | ✔ | **N.v.t.** |


**HTTPD + reverse-proxy**

Een voorbeeld config zal hieronder kijken als

>[!TIP]
>
> De volgende configuraties zijn voorbeelden. Zorg ervoor dat u de instellingen aanpast aan de vereisten van uw project.

OP AEM DOMAIN

    &quot;
    ProxyPass &quot;/${UW-SPA-URI}&quot; &quot;http://${SPA-HOST}/&quot;
    ProxyPassReverse &quot;/${UW-SPA-URI}&quot; &quot;http://${SPA-HOST}/&quot;
    &quot;

UIT AEM domein

    &quot;
    ProxyPass &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    ProxyPassReverse &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    &quot;




### Implementeren op ander domein

In dit scenario, SPA bouwstijlartefacten worden opgesteld aan een verschillend domein dan het AEM domein GraphQL APIs en voor eind - gebruikers, zullen zij van twee afzonderlijke domeinen zo worden geleverd [CORS-configuratie](cors.md) AEM.

**Toepassingsverzoeken SPA via verschillende domeinen**

![Verschillende SPA](assets/spa/different-domain-spa-delivery.png)


**CORS Response Header on AEM GraphQL API**

![Koptekst CORS-reactie AEM GraphQL API](assets/spa/CORS-response-header-aem-graphql-api.png)


