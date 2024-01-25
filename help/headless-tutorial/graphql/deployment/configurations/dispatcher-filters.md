---
title: Dispatcher-filters voor AEM GraphQL
description: Leer hoe u AEM filters voor publicatieverzending configureert voor gebruik met AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10829
thumbnail: kt-10829.jpg
exl-id: b76b7c46-5cbd-4039-8fd6-9f0f10a4a84f
duration: 64
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# Verzendingsfilters

Adobe Experience Manager as a Cloud Service gebruikt AEM de filters van de Verzender van de Publicatie om slechts verzoeken te verzekeren die AEM zouden moeten bereiken AEM. Standaard worden alle aanvragen geweigerd en moeten patronen voor toegestane URL&#39;s expliciet worden toegevoegd.

| Type client | [App van één pagina (SPA)](../spa.md) | [Webcomponent/JS](../web-component.md) | [Mobiel](../mobile.md) | [Server-naar-server](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Vereist configuratie Dispatcher-filters | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> De volgende configuraties zijn voorbeelden. Zorg ervoor dat u de instellingen aanpast en aanpast aan de vereisten van uw project.

## Configuratie van filter Dispatcher

De AEM publiceer de filterconfiguratie van de Verzender bepaalt de patronen URL die worden toegestaan om AEM te bereiken, en moet de prefix URL voor het AEM persisted vraageindpunt omvatten.

| Client maakt verbinding met | AEM auteur | AEM publiceren | Voorvertoning AEM |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Vereist configuratie Dispatcher-filters | ✘ | ✔ | ✔ |

Een `allow` regel met het URL-patroon `/graphql/execute.json/*`en controleer de bestands-id (bijvoorbeeld `/0600`, is uniek in het dossier van het voorbeeldlandbouwbedrijf).
Dit staat HTTP- GET- verzoek aan het persistente vraageindpunt toe, zoals `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` tot AEM Publiceren.

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
