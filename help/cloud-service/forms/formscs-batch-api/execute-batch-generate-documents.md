---
title: De batchconfiguratie uitvoeren
description: Start het genereren van het document door de batch uit te voeren
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9674
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
duration: 221
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 0%

---

# Batchconfiguratie uitvoeren

Als u de batch wilt uitvoeren, vraagt u een POST naar de volgende API

```xml
<baseURL>/confi/<configName>/execution
```

Deze API verwacht een leeg json-object als een parameter in de aanvraaginstantie.
Deze API retourneert een unieke URL in de responsheader die wordt bepaald door **locatie** toets.
Een GET-aanvraag voor deze unieke URL geeft de status van de uitvoering van de batch

In de volgende video ziet u hoe de batchconfiguratie wordt geactiveerd

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
