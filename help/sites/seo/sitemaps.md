---
title: Sitemaps
description: Leer hoe u uw SEO kunt verhogen door sitemaps voor AEM Sites te maken.
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
kt: 9165
thumbnail: 337960.jpeg
source-git-commit: 5bdff2eafaa28aff722b12607b1278539072be62
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---


# Sitemaps

Leer hoe u uw SEO kunt verhogen door sitemaps voor AEM Sites te maken.

>[!VIDEO](https://video.tv.adobe.com/v/337960/?quality=12&learn=on)

## Bronnen

+ [AEM Sitemap-documentatie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Documentatie Apache Sling Sitemap](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [AEM Core WCM Components Github](https://github.com/adobe/aem-core-wcm-components)
   + Sitemapfunctionaliteit toegevoegd in v2.17.6
+ [Documentatie Sitemap.org Sitemap](https://www.sitemaps.org/protocol.html)
+ [Documentatie Sitemap.org Sitemap-indexbestand](https://www.sitemaps.org/protocol.html#index)
+ [Cronmaker](http://www.cronmaker.com/)

## Configuraties

### org.apache.sling.sitemap.impl.SitemapScheduler~wknd.cfg.json

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

Definieert de [OSGi-fabrieksconfiguratie](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) voor de frequentie (gebruik [uitsnijdexpressies](http://www.cronmaker.com)) sitemaps worden opnieuw/gegenereerd en in AEM opgeslagen.

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### filters.any

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

HTTP-aanvragen voor de sitemap-index en sitemap-bestanden toestaan.

```
...

# Allow AEM WCM Core Components sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### rewrite.rules

`dispatcher/src/conf.d/rewrites/rewrite.rules`

Zorgen `.xml` HTTP-aanvragen van sitemap worden gerouteerd naar de juiste onderliggende AEM pagina. Als er geen URL-kortere weg wordt gebruikt of Sling Mappings wordt gebruikt om URL-kortere weg te bereiken, is deze configuratie niet nodig.

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```
