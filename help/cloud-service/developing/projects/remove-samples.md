---
title: Het verwijderen van Steekproeven van een AEM Maven project
description: Leer hoe u voorbeeldcode kunt opschonen en verwijderen uit een AEM Project dat is gegenereerd door het AEM Project Archetype.
version: Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
kt: 9092
thumbnail: 337263.jpeg
source-git-commit: e8b3bcaeee40b4bfd4f967f929ad664e8d168cb0
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 2%

---


# Verwijderen van monsters uit een AEM Maven-project

Leer hoe u gegenereerde voorbeeldcode kunt opschonen en verwijderen uit een AEM Project dat is gegenereerd door het AEM Project Archetype.

>[!VIDEO](https://video.tv.adobe.com/v/337263/?quality=12&learn=on)


## Bronnen

+ [AEM Maven Project Archetype](https://github.com/adobe/aem-project-archetype)

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

Verwijder de `<div class="helloworld" ...></div>` van:

```
ui.frontend/src/main/webpack/static/index.html
```

Verwijder de `<helloworld>` definitie van componentinstantie van:

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
