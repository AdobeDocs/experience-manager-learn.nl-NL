---
title: Dynamische include-bestanden voor AEM instellen
description: Een videodemo over het installeren en gebruiken van Apache Sling Dynamic Include met AEM Dispatcher die op Apache HTTP Web Server wordt uitgevoerd.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---

# Instellen [!DNL Sling Dynamic Include]

Een videodemo van het installeren en gebruiken [!DNL Apache Sling Dynamic Include] with [AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html) wordt uitgevoerd op [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040?quality=12&learn=on)

>[!NOTE]
>
> Zorg ervoor dat de nieuwste versie van AEM Dispatcher lokaal is geïnstalleerd.

1. Download en installeer de [[!DNL Sling Dynamic Include] bundelen](https://sling.apache.org/downloads.cgi).
1. Configureren [!DNL Sling Dynamic Include] via de [!DNL OSGi Configuration Factory] om **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   Of, om aan AEM code-basis toe te voegen, creeer aangewezen **sling:OsgiConfig** knooppunt bij:

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

1. (Optioneel) Herhaal de laatste stap om componenten toe te staan op [vergrendelde (initiële) inhoud van bewerkbare sjablonen](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) via [!DNL SDI] ook. De reden voor de extra configuratie is dat vergrendelde inhoud van bewerkbare sjablonen wordt aangeboden vanuit `/conf` in plaats van `/content`.

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

1. Bijwerken [!DNL Apache HTTPD Web server]s `httpd.conf` bestand om het [!DNL Include] module.

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. Werk de [!DNL vhost] dossier om richtlijnen te respecteren.

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

1. Update het dispatcher.any-configuratiebestand dat moet worden ondersteund (1) `nocache` selecteurs en (2) laat de steun van TTL toe.

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
   > De slede verlaten `*` in de wereld `*.nocache.html*` lijn boven, kan resulteren in [kwesties in verzoeken om submiddelen](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. Altijd opnieuw opstarten [!DNL Apache HTTP Web Server] na het aanbrengen van wijzigingen in de configuratiebestanden of de `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Als u [!DNL Sling Dynamic Includes] voor het dienen van edge-side include (ESI), dan zorg ervoor om relevant in het voorgeheugen op te slaan [antwoordheaders in de verzendercache](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). Mogelijke kopteksten zijn onder andere:
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
