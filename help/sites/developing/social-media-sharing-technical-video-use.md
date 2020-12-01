---
title: Delen van sociale media gebruiken in AEM Sites
description: Ontdek het instellen en gebruiken van de component Sociale media delen.
feature: core-components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 3%

---


# Delen van sociale media gebruiken {#using-social-media-sharing-in-aem-sites}

Ontdek het instellen en gebruiken van de component Sociale media delen.

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

In deze video worden de volgende faciliteiten van de component voor sociale media-uitwisseling (onderdeel van [AEM Core Components](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html)) besproken met de voorbeeldwebsite [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail).

* 0:00 - De component voor sociale media delen toevoegen en configureren
* 1:00 - Delen op Facebook
* 3:10 - Delen naar Pinterest
* 6:25 - De component Sociale media delen gebruiken op een productpagina

## Setup van externe alizer {#externalizer-setup}

![Day CQ Link ExternalAlizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) extern moet worden ingesteld op zowel AEM Auteur als AEM Publish, om de publicatierunmode toe te wijzen aan het openbaar toegankelijke domein dat wordt gebruikt om toegang te krijgen tot AEM Publish.

In deze video gebruiken wij `/etc/hosts` aan spoof *www.example.com* om aan localhost op te lossen, en gebruiken een [basis AEM Dispatcher configuratie](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) om www.example.com toe te staan om te publiceren AEM.

## Ondersteunende materialen {#supporting-materials}

* [Download de AEM Core Components](https://github.com/adobe/aem-core-wcm-components/releases)
* [We downloaden.Handelsversie](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Dispatcher installeren](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
