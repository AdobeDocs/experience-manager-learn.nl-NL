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
last-substantial-update: 2022-10-03T00:00:00Z
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 2%

---

# Sitemaps

Leer hoe u uw SEO kunt verhogen door sitemaps voor AEM Sites te maken.

>[!WARNING]
>
>Deze video demonstreert het gebruik van relatieve URL&#39;s in de sitemap. Sitemaps [absolute URL&#39;s gebruiken](https://sitemaps.org/protocol.html). Zie [Configuraties](#absolute-sitemap-urls) voor hoe u absolute URL&#39;s kunt inschakelen, aangezien dit niet in de onderstaande video wordt besproken.

>[!VIDEO](https://video.tv.adobe.com/v/337960?quality=12&learn=on)

## Configuraties

### Absolute sitemap-URL&#39;s{#absolute-sitemap-urls}

AEM sitemap ondersteunt absolute URL&#39;s door [Verspreiding toewijzen](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html). Dit wordt gedaan door toewijzingsknopen op de AEM diensten te creëren die sitemaps (typisch de publicatiedienst AEM) produceren.

Een voorbeeld van Sling-toewijzingsknoopdefinitie voor `https://wknd.com` kunnen worden gedefinieerd onder `/etc/map/https` als volgt:

| Pad | Eigenschapnaam | Type eigenschap | Waarde van eigenschap |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | String | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | String | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | String | `wknd.com/$1` |

De onderstaande schermafbeelding illustreert een vergelijkbare configuratie, maar voor `http://wknd.local` (lokale hostname-toewijzing uitgevoerd op `http`).

![Configuratie absolute URL&#39;s van Sitemap](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### OSGi-configuratie Sitemap-planner

Definieert de [OSGi-fabrieksconfiguratie](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) voor de frequentie (gebruik [uitsnijdexpressies](http://www.cronmaker.com)) sitemaps worden opnieuw/gegenereerd en in AEM opgeslagen.

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Filterregel toestaan voor verzending

HTTP-aanvragen voor de sitemap-index en sitemap-bestanden toestaan.

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Herschrijfregel voor Apache-webserver

Zorgen `.xml` HTTP-aanvragen van sitemap worden gerouteerd naar de juiste onderliggende AEM pagina. Als er geen URL-kortere weg wordt gebruikt of Sling Mappings wordt gebruikt om URL-kortere weg te bereiken, is deze configuratie niet nodig.

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```

## Bronnen

+ [AEM Sitemap-documentatie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Documentatie Apache Sling Sitemap](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Documentatie Sitemap.org Sitemap](https://www.sitemaps.org/protocol.html)
+ [Documentatie Sitemap.org Sitemap-indexbestand](https://www.sitemaps.org/protocol.html#index)
+ [Cronmaker](http://www.cronmaker.com/)