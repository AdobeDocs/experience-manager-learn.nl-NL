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
duration: 937
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 0%

---

# Sitemaps

Leer hoe u uw SEO kunt verhogen door sitemaps voor AEM Sites te maken.

>[!WARNING]
>
>Deze video demonstreert het gebruik van relatieve URL&#39;s in de sitemap. Sitemaps [ zou absolute URLs ](https://sitemaps.org/protocol.html) moeten gebruiken. Zie [ Configuraties ](#absolute-sitemap-urls) voor hoe te om absolute URLs toe te laten, aangezien dit niet in de video hieronder wordt behandeld.

>[!VIDEO](https://video.tv.adobe.com/v/337960?quality=12&learn=on)

## Configuraties

### Absolute sitemap-URL&#39;s{#absolute-sitemap-urls}

AEM sitemap steunt absolute URL&#39;s door [ het Verdelen afbeelding ](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html) te gebruiken. Dit wordt gedaan door toewijzingsknopen op de AEM diensten te creëren die sitemaps (typisch de dienst van AEM Publish) produceren.

Een voorbeelddefinitie van het toewijzingsknooppunt voor `https://wknd.com` kan onder `/etc/map/https` als volgt worden gedefinieerd:

| Pad | Eigenschapnaam | Type eigenschap | Waarde van eigenschap |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | String | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | String | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | String | `wknd.com/$1` |

De onderstaande schermafbeelding illustreert een vergelijkbare configuratie, maar voor `http://wknd.local` (een lokale hostnaamtoewijzing die wordt uitgevoerd op `http` ).

![ Sitemap absolute configuratie URLs ](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### OSGi-configuratie Sitemap-planner

Bepaalt de [ OSGi fabrieksconfiguratie ](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) voor de frequentie (gebruikend [ cron uitdrukkingen ](https://cron.help/)) sitemaps wordt re/geproduceerd en in AEM in het voorgeheugen ondergebracht.

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Dispatcher-regel voor toegestane filter

HTTP-aanvragen voor de sitemapindex en sitemapbestanden toestaan.

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Herschrijfregel voor Apache-webserver

Zorg ervoor dat de HTTP-aanvragen voor `.xml` sitemap naar de juiste onderliggende AEM pagina worden gerouteerd. Als er geen URL-kortere weg wordt gebruikt of Sling Mappings wordt gebruikt om URL-kortere weg te bereiken, is deze configuratie niet nodig.

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```

## Bronnen

+ [ AEM de Documentatie van de Sitemap ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/seo-and-url-management.html?lang=en)
+ [ Apache Sling Sitemap documentatie ](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [ Sitemap.org de documentatie van de Sitemap ](https://www.sitemaps.org/protocol.html)
+ [ Sitemap.org de documentatie van het de indexdossier van de Sitemap ](https://www.sitemaps.org/protocol.html#index)
+ [ Helper van het Gewas ](https://cron.help/)