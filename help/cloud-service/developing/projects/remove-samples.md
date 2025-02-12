---
title: Het verwijderen van Steekproeven van een AEM Maven project
description: Leer hoe u voorbeeldcode kunt opschonen en verwijderen uit een AEM Project dat is gegenereerd door het AEM Project Archetype.
version: Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
jira: KT-9092
thumbnail: 337263.jpeg
exl-id: 4e10c2b7-41b6-41a0-b8d4-9207a9d3f9c8
duration: 341
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '85'
ht-degree: 1%

---

# Verwijderen van monsters uit een AEM Maven-project

Leer hoe u gegenereerde voorbeeldcode kunt opschonen en verwijderen uit een AEM Project dat is gegenereerd door het AEM Project Archetype.

>[!VIDEO](https://video.tv.adobe.com/v/337263?quality=12&learn=on)


## Bronnen

+ [AEM Maven Project Archetype ](https://github.com/adobe/aem-project-archetype)

## Opdrachten

De volgende bevelen kunnen worden uitgevoerd om de geproduceerde steekproefdossiers uit het AEM Gemaakt Project te verwijderen:

```
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/filters \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/listeners \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/models \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/schedulers \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/servlets \
rm -rf core/src/test/java/com/adobe/aem/wknd/examples/core/* \
rm -rf ui.apps/src/main/content/jcr_root/apps/wknd-examples/components/helloworld \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.js \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.css
```

## Bewerkingen

Verwijder `<div class="helloworld" ...></div>` uit:

```
ui.frontend/src/main/webpack/static/index.html
```

Verwijder de definitie van de componentinstantie `<helloworld>` uit:

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
