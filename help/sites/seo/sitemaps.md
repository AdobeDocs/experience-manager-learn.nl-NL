---
title: Sitemaps
description: Leer hoe u uw SEO kunt verhogen door sitemaps voor AEM Sites te maken.
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
jira: KT-9165
thumbnail: 337960.jpeg
last-substantial-update: 2022-10-03T00:00:00Z
doc-type: Technical Video
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
duration: 957
source-git-commit: 970093bb54046fee49e2ac209f1588e70582ab67
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 0%

---

# Sitemaps

Leer hoe u uw SEO kunt verhogen door sitemaps voor AEM Sites te maken.

>[!WARNING]
>
>Deze video demonstreert het gebruik van relatieve URL&#39;s in de sitemap. Sitemaps [absolute URL&#39;s gebruiken](https://sitemaps.org/protocol.html). Zie [Configuraties](#absolute-sitemap-urls) voor hoe u absolute URL&#39;s kunt inschakelen, aangezien dit niet in de onderstaande video wordt besproken.

>[!VIDEO](https://video.tv.adobe.com/v/337960?quality=12&learn=on)

## Configuraties

### Absolute sitemap-URL&#39;s{#absolute-sitemap-urls}

AEM sitemap ondersteunt absolute URL&#39;s met behulp van [Verspreiding toewijzen](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html). Dit wordt gedaan door toewijzingsknopen op de AEM diensten te creëren die sitemaps (typisch de AEM publiceren dienst) produceren.

Een voorbeeld van het splitsen van toewijzingsknoopdefinitie voor `https://wknd.com` kunnen worden gedefinieerd onder `/etc/map/https` als volgt:

| Pad | Eigenschapnaam | Type eigenschap | Waarde van eigenschap |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | String | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | String | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | String | `wknd.com/$1` |

De onderstaande schermafbeelding illustreert een vergelijkbare configuratie, maar voor `http://wknd.local` (lokale hostname-toewijzing uitgevoerd op `http`).

![Configuratie absolute URL&#39;s van Sitemap](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### OSGi-configuratie Sitemap-planner

Definieert de [OSGi-fabrieksconfiguratie](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) voor de frequentie (gebruik [uitsnijdexpressies](https://cron.help/)) sitemaps worden opnieuw/gegenereerd en in AEM in cache geplaatst.

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Filterregel toestaan voor verzending

HTTP-aanvragen voor de sitemapindex en sitemapbestanden toestaan.

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

+ [AEM Sitemap-documentatie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/seo-and-url-management.html?lang=en)
+ [Documentatie Apache Sling Sitemap](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Sitemap.org Sitemap-documentatie](https://www.sitemaps.org/protocol.html)
+ [Sitemap.org Documentatie van indexbestand Sitemap](https://www.sitemaps.org/protocol.html#index)
+ [Cron Helper](https://cron.help/)