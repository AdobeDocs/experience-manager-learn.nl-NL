---
title: Dispatcher-filters voor AEM GraphQL
description: Leer hoe u AEM Publish Dispatcher filters configureert voor gebruik met AEM GraphQL.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
jira: KT-10829
thumbnail: kt-10829.jpg
exl-id: b76b7c46-5cbd-4039-8fd6-9f0f10a4a84f
duration: 48
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# Dispatcher-filters

Adobe Experience Manager as a Cloud Service gebruikt AEM Publish Dispatcher filters om ervoor te zorgen slechts verzoeken die AEM zouden moeten bereiken AEM bereiken. Standaard worden alle aanvragen geweigerd en moeten patronen voor toegestane URL&#39;s expliciet worden toegevoegd.

| Type client | [&#x200B; Enige-pagina app (SPA) &#x200B;](../spa.md) | [&#x200B; Component/JS van het Web &#x200B;](../web-component.md) | [&#x200B; Mobiel &#x200B;](../mobile.md) | [&#x200B; server-aan-server &#x200B;](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Dispatcher-filterconfiguratie vereist | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> De volgende configuraties zijn voorbeelden. Zorg ervoor dat u de instellingen aanpast en aanpast aan de vereisten van uw project.

## Dispatcher-filterconfiguratie

De AEM-filterconfiguratie voor Dispatcher publiceren definieert de URL-patronen die AEM mag bereiken en moet het URL-voorvoegsel bevatten voor het AEM-voortgeduurde queryeindpunt.

| Client maakt verbinding met | AEM-auteur | AEM Publiceren | AEM Preview |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Dispatcher-filterconfiguratie vereist | ✘ | ✔ | ✔ |

Voeg een `allow` -regel toe met het URL-patroon `/graphql/execute.json/*` en controleer of de bestands-id (bijvoorbeeld `/0600` , uniek is in het voorbeeldbestand van de farm).
Dit staat HTTP GET- verzoek aan het voortgezette vraageindpunt toe, zoals `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` door aan AEM publiceren.

Als u Experience Fragments gebruikt in uw AEM Headless-ervaring, doe dan hetzelfde voor deze paden.

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

+ [&#x200B; een voorbeeld van de filter van Dispatcher kan in het project worden gevonden WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
