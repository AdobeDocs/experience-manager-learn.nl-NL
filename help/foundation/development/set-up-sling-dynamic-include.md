---
title: Dynamische include-bestanden voor AEM instellen
description: Een videodemo over het installeren en gebruiken van Apache Sling Dynamic Include met AEM Dispatcher die op Apache HTTP Web Server wordt uitgevoerd.
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
duration: 863
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# Instellen [!DNL Sling Dynamic Include]

Een videolooppas-door van het installeren van en het gebruiken van [!DNL Apache Sling Dynamic Include] met [ AEM Dispatcher ](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=nl-NL) lopend op [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040?quality=12&learn=on)

>[!NOTE]
>
> Controleer of de nieuwste versie van AEM Dispatcher lokaal is geïnstalleerd.

1. Download en installeer de [[!DNL Sling Dynamic Include]  bundel ](https://sling.apache.org/downloads.cgi).
1. Vorm [!DNL Sling Dynamic Include] via [!DNL OSGi Configuration Factory] in **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   Of, om aan een code-basis van AEM toe te voegen, creeer de aangewezen **gooi:OsgiConfig** knoop bij:

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

1. (Facultatief) herhaal de laatste stap om voor componenten op [ gesloten (aanvankelijke) inhoud van editable malplaatjes ](https://helpx.adobe.com/nl/experience-manager/6-5/sites/developing/using/page-templates-editable.html) toe te staan om via [!DNL SDI] eveneens te worden gediend. De reden voor de extra configuratie is dat vergrendelde inhoud van bewerkbare sjablonen wordt aangeboden vanuit `/conf` in plaats van `/content` .

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

1. Werk het `httpd.conf` -bestand van [!DNL Apache HTTPD Web server] bij om de [!DNL Include] -module in te schakelen.

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. Werk het [!DNL vhost] -bestand bij om instructies te respecteren.

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

1. Werk het dispatcher.any-configuratiebestand bij ter ondersteuning van (1) `nocache` kiezers en (2) schakel TTL-ondersteuning in.

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
   > Als u de `*` volgende regel in de algemene regel `*.nocache.html*` hierboven weglaat, kan dit resulteren in [ problemen in verzoeken om subresources ](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16) .

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. Start [!DNL Apache HTTP Web Server] altijd opnieuw nadat u wijzigingen hebt aangebracht in de configuratiebestanden of in `dispatcher.any` .

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Als u [!DNL Sling Dynamic Includes] voor het dienen van rand-zij omvat (ESI) gebruikt, dan zorg ervoor om relevante [ reactiekopballen in het berichtchermgeheime voorgeheugen ](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=nl-NL#CachingHTTPResponseHeaders) in het voorgeheugen onder te brengen. Mogelijke kopteksten zijn onder andere:
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

* [ Download Sling Dynamische Include bundel ](https://sling.apache.org/downloads.cgi)
* [ Apache Sling Dynamic Include documentatie ](https://github.com/Cognifide/Sling-Dynamic-Include)
