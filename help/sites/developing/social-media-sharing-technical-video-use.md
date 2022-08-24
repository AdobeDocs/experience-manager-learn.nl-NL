---
title: Delen van sociale media gebruiken in AEM Sites
description: Ontdek het instellen en gebruiken van de component Sociale media delen.
feature: Core Components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---

# Delen via sociale media gebruiken {#using-social-media-sharing-in-aem-sites}

Ontdek het instellen en gebruiken van de component Sociale media delen.

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

In deze video worden de volgende mogelijkheden van de component voor het delen van sociale media besproken (onderdeel van [AEM kerncomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)) met de [Wij.Detailhandel](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) voorbeeldwebsite.

* 0:00 - De component voor sociale media delen toevoegen en configureren
* 1:00 - Delen naar Facebook
* 3:10 - Delen naar Pinterest
* 6:25 - De component Sociale media delen gebruiken op een productpagina

## ExternalAlizer instellen {#externalizer-setup}

![Day CQ Link ExternalAlizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) moet worden ingesteld op zowel AEM-auteur als AEM-publicatie, om de publicatierunmode toe te wijzen aan het openbaar toegankelijke domein dat wordt gebruikt voor toegang tot AEM-publicatie.

In deze video gebruiken we `/etc/hosts` naar spul *www.example.com* om op te lossen aan localhost, en gebruik [basis AEM Dispatcher-configuratie](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) om toe te staan dat www.example.com voor publicatie in AEM wordt weergegeven.

## Ondersteunende materialen {#supporting-materials}

* [Download de AEM Core Components](https://github.com/adobe/aem-core-wcm-components/releases)
* [We downloaden.Handelsversie](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Dispatcher installeren](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
