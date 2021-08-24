---
title: Dynamische include-bestanden voor AEM instellen
description: Een videodemo over het installeren en gebruiken van Apache Sling Dynamic Include met AEM Dispatcher die op Apache HTTP Web Server wordt uitgevoerd.
version: 6.3, 6.4, 6.5
sub-product: stichting, sites
feature: API's
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
topic: Ontwikkeling
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---


# [!DNL Sling Dynamic Include] instellen

Een videodemthrough van het installeren en gebruiken van [!DNL Apache Sling Dynamic Include] met [AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html) die op [!DNL Apache HTTP Web Server] loopt.

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> Zorg ervoor dat de nieuwste versie van AEM Dispatcher lokaal is geïnstalleerd.

1. Download en installeer de [[!DNL Sling Dynamic Include] bundle](https://sling.apache.org/downloads.cgi).
1. Configureer [!DNL Sling Dynamic Include] via [!DNL OSGi Configuration Factory] op **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   Of, om aan een AEM code-basis toe te voegen, creeer de aangewezen **sling:OsgiConfig** knoop bij:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/content"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. (Optioneel) Herhaal de laatste stap zodat componenten op [vergrendelde (initiële) inhoud van bewerkbare sjablonen](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) ook via [!DNL SDI] kunnen worden weergegeven. De reden voor de extra configuratie is dat vergrendelde inhoud van bewerkbare sjablonen wordt aangeboden vanaf `/conf` in plaats van `/content`.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/conf"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. [!DNL Apache HTTPD Web server] `httpd.conf` dossier bijwerken om [!DNL Include] module toe te laten.

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. Werk het [!DNL vhost] dossier bij om te respecteren omvat richtlijnen.

   ```shell
   $ sudo vi .../vhosts/aem-publish.local.conf
   ```

   ```shell
   <VirtualHost *:80>
   ...
      <Directory /Library/WebServer/docroot/publish>
         ...
         # Add Includes to enable SSI Includes used by Sling Dynamic Include
         Options FollowSymLinks Includes
   
         # Required to have dispatcher-handler process includes
         ModMimeUsePathInfo On
   
         # Set includes to process .html files
         AddOutputFilter INCLUDES .html
         ...
      </Directory>
   ...
   </VirtualHost>
   ```

1. Werk het dispatcher.any configuratiedossier bij om (1) `nocache` selecteurs te steunen en (2) laat de steun van TTL toe.

   ```shell
   $ sudo vi .../conf/dispatcher.any
   ```

   ```shell
   /rules {
     ...
     /0009 {
       /glob "*.nocache.html*"
       /type "deny"
     } 
   }
   ```

   >[!TIP]
   >
   > Als u de onderliggende `*` in de bovenstaande glob `*.nocache.html*`-regel weglaat, kan dit leiden tot [problemen in aanvragen voor subbronnen](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. Start [!DNL Apache HTTP Web Server] altijd opnieuw nadat u wijzigingen hebt aangebracht in de configuratiebestanden of `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Als u [!DNL Sling Dynamic Includes] voor het dienen van edge-side omvat (ESI) gebruikt, dan zorg ervoor om relevante [reactiekoppen in het verzendergeheime voorgeheugen ](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders) in het voorgeheugen onder te brengen. Mogelijke kopteksten zijn onder andere:
>
>* &quot;Cache-control&quot;
>* &quot;Content-Disposition&quot;
>* &quot;Content-Type&quot;
>* &quot;Verloopt&quot;
>* &quot;Laatst gewijzigd&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;Laatst gewijzigd&quot;

>



## Ondersteunende materialen

* [Dynamic Include-bundel downloaden](https://sling.apache.org/downloads.cgi)
* [Apache Sling Dynamic Include-documentatie](https://github.com/Cognifide/Sling-Dynamic-Include)
