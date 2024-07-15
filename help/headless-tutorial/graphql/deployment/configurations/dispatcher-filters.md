---
title: Dispatcher-filters voor AEM GraphQL
description: Leer hoe u AEM Publish Dispatcher-filters configureert voor gebruik met AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10829
thumbnail: kt-10829.jpg
exl-id: b76b7c46-5cbd-4039-8fd6-9f0f10a4a84f
duration: 48
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# Dispatcher-filters

Adobe Experience Manager as a Cloud Service gebruikt AEM Publish Dispatcher-filters om ervoor te zorgen dat alleen aanvragen die AEM bereiken, AEM bereiken. Standaard worden alle aanvragen geweigerd en moeten patronen voor toegestane URL&#39;s expliciet worden toegevoegd.

| Type client | [ Enige-pagina app (SPA) ](../spa.md) | [ Component/JS van het Web ](../web-component.md) | [ Mobiel ](../mobile.md) | [ server-aan-server ](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Dispatcher-filterconfiguratie vereist | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> De volgende configuraties zijn voorbeelden. Zorg ervoor dat u de instellingen aanpast en aanpast aan de vereisten van uw project.

## Dispatcher-filterconfiguratie

De AEM Publish Dispatcher filterconfiguratie bepaalt de patronen URL die worden toegestaan om AEM te bereiken, en moet het prefix URL voor het AEM voortgezette vraageindpunt omvatten.

| Client maakt verbinding met | AEM auteur | AEM Publish | Voorvertoning AEM |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Dispatcher-filterconfiguratie vereist | ✘ | ✔ | ✔ |

Voeg een `allow` -regel toe met het URL-patroon `/graphql/execute.json/*` en controleer of de bestands-id (bijvoorbeeld `/0600` , uniek is in het voorbeeldbestand van de farm).
Hierdoor kan HTTP-GET-aanvraag worden uitgevoerd naar het voortgezette queryeindpunt, bijvoorbeeld `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` tot en met AEM Publish.

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

+ [ een voorbeeld van de filter van Dispatcher kan in het project worden gevonden WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
