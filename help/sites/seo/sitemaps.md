---
title: Sitemaps
description: Leer hoe u uw SEO kunt verhogen door sitemaps voor AEM Sites te maken.
version: Experience Manager as a Cloud Service
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
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 0%

---

# Sitemaps

Leer hoe u uw SEO kunt verhogen door sitemaps voor AEM Sites te maken.

>[!WARNING]
>
>Deze video demonstreert het gebruik van relatieve URL&#39;s in de sitemap. Sitemaps [&#x200B; zou absolute URLs &#x200B;](https://sitemaps.org/protocol.html) moeten gebruiken. Zie [&#x200B; Configuraties &#x200B;](#absolute-sitemap-urls) voor hoe te om absolute URLs toe te laten, aangezien dit niet in de video hieronder wordt behandeld.

>[!VIDEO](https://video.tv.adobe.com/v/3454367?captions=dut&quality=12&learn=on)

## Configuraties

### Absolute sitemap-URL&#39;s{#absolute-sitemap-urls}

De sitemap van AEM steunt absolute URL&#39;s door [&#x200B; het Verdelen afbeelding &#x200B;](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html) te gebruiken. Dit gebeurt door toewijzingsknooppunten te maken op de AEM-services die sitemaps genereren (doorgaans de AEM-publicatieservice).

Een voorbeelddefinitie van het toewijzingsknooppunt voor `https://wknd.com` kan onder `/etc/map/https` als volgt worden gedefinieerd:

| Pad | Eigenschapnaam | Type eigenschap | Waarde van eigenschap |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | String | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | String | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | String | `wknd.com/$1` |

De onderstaande schermafbeelding illustreert een vergelijkbare configuratie, maar voor `http://wknd.local` (een lokale hostnaamtoewijzing die wordt uitgevoerd op `http` ).

![&#x200B; Sitemap absolute configuratie URLs &#x200B;](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### OSGi-configuratie Sitemap-planner

Bepaalt de [&#x200B; OSGi fabrieksconfiguratie &#x200B;](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) voor de frequentie (gebruikend [&#x200B; cron uitdrukkingen &#x200B;](https://cron.help/)) sitemaps wordt re/geproduceerd en in het voorgeheugen ondergebracht in AEM.

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.author`

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

Zorg ervoor dat `.xml` sitemap HTTP-aanvragen naar de juiste onderliggende AEM-pagina worden gerouteerd. Als er geen URL-kortere weg wordt gebruikt of Sling Mappings wordt gebruikt om URL-kortere weg te bereiken, is deze configuratie niet nodig.

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```

## Bronnen

+ [&#x200B; de Documentatie van AEM Sitemap &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/seo-and-url-management.html?lang=nl-NL)
+ [&#x200B; Apache Sling Sitemap documentatie &#x200B;](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [&#x200B; Sitemap.org de documentatie van de Sitemap &#x200B;](https://www.sitemaps.org/protocol.html)
+ [&#x200B; Sitemap.org de documentatie van het de indexdossier van de Sitemap &#x200B;](https://www.sitemaps.org/protocol.html#index)
+ [&#x200B; Helper van het Gewas &#x200B;](https://cron.help/)
