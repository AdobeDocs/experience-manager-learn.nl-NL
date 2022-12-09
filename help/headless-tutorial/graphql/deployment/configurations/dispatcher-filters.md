---
title: Dispatcher-filters voor AEM GraphQL
description: Leer hoe u AEM Publish Dispatcher-filters configureert voor gebruik met AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10829
thumbnail: kt-10829.jpg
source-git-commit: 442020d854d8f42c5d8a1340afd907548875866e
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---


# Verzendingsfilters

Adobe Experience Manager as a Cloud Service gebruikt de filters van de Verzender van de Publicatie AEM om slechts verzoeken te verzekeren die zouden moeten bereiken AEM bereiken AEM. Standaard worden alle aanvragen geweigerd en moeten patronen voor toegestane URL&#39;s expliciet worden toegevoegd.

| Type client | [App van één pagina (SPA)](../spa.md) | [Webcomponent/JS](../web-component.md) | [Mobiel](../mobile.md) | [Server-naar-server](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Vereist configuratie Dispatcher-filters | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> De volgende configuraties zijn voorbeelden. Zorg ervoor dat u de instellingen aanpast aan de vereisten van uw project.

## Configuratie van filter Dispatcher

De AEM-filterconfiguratie van Dispatcher publiceren definieert de URL-patronen die zijn toegestaan om AEM te bereiken, en moet het URL-voorvoegsel bevatten voor het AEM voortgezette queryeindpunt.

| Client maakt verbinding met | AEM-auteur | AEM-publicatie | Voorvertoning AEM |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Vereist configuratie Dispatcher-filters | ✘ | ✔ | ✔ |

Een `allow` regel met het URL-patroon `/graphql/execute.json/*`en controleer de bestands-id (bijvoorbeeld `/0600`, is uniek in het dossier van het voorbeeldlandbouwbedrijf).
Dit staat HTTP- GET- verzoek aan het persistente vraageindpunt toe, zoals `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` tot en met AEM Publish.

Als het gebruiken van de Fragmenten van de Ervaring in uw AEM Headless ervaring, doe het zelfde voor deze wegen.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
# Allow headless requests for Persisted Query endpoints
/0600 { /type "allow" /method '(POST|OPTIONS)' /url "/graphql/execute.json/*" }
# Allow headless requests for Experience Fragments
/0601 { /type "allow" /method '(GET|OPTIONS)' /url "/content/experience-fragments/*" }
...
```

### Voorbeeldfilterconfiguratie

+ [Een voorbeeld van de filter van de Verzender kan in het WKND project worden gevonden.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
