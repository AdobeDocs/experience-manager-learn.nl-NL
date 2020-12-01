---
title: Annotaties aanpassen in AEM Assets
seo-title: Annotaties aanpassen in AEM Assets
description: AEM Assets-indeling en -stijl bij uitvoer naar PDF kunnen worden geconfigureerd door AEM ontwikkelaars.
feature: asset-share
topics: authoring, collaboration, operations, sharing
audience: developer
doc-type: feature video
activity: developer
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '65'
ht-degree: 0%

---


# Annotaties aanpassen in AEM Assets{#using-annotations-in-aem-assets}

AEM ondersteunt het aanpassen van de uitvoer van de annotatie naar PDF.

## PDF-annotatie instellen:definitie van OsgiConfig

Als u PDF-annotaties wilt aanpassen, maakt u een **sling:OsgiConfig**-knooppunt in het AEM project onder

`/apps/my-project/config.author/com.day.cq.dam.core.impl.annotation.pdf.AnnotationPdfConfig.xml` en pas zo nodig de waarden aan:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"

    cq.dam.config.annotation.pdf.document.padding.vertical="{Long}20"
    cq.dam.config.annotation.pdf.reviewStatus.color.changesRequested="fad269"
    cq.dam.config.annotation.pdf.document.height="{Long}792"
    cq.dam.config.annotation.pdf.document.width="{Long}612"
    cq.dam.config.annotation.pdf.reviewStatus.width="{Long}150"
    cq.dam.config.annotation.pdf.font.size="{Long}7"
    cq.dam.config.annotation.pdf.font.color="3B3B3B"
    cq.dam.config.annotation.pdf.font.light="A4A4A4"
    cq.dam.config.annotation.pdf.reviewStatus.color.rejected="fa7d73"
    cq.dam.config.annotation.pdf.minImageHeight="{Long}100"
    cq.dam.config.annotation.pdf.reviewStatus.color.approved="009933"
    cq.dam.config.annotation.pdf.marginTextImage="{Long}14"
    cq.dam.config.annotation.pdf.document.padding.horizontal="{Long}20"
    cq.dam.config.annotation.pdf.annotationMarker.width="{Long}5"
    />
```
