---
title: Delen van sociale media gebruiken in AEM Sites
description: Ontdek het instellen en gebruiken van de component voor sociale media delen.
feature: Core Components
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
duration: 511
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 0%

---

# Delen via sociale media gebruiken {#using-social-media-sharing-in-aem-sites}

Ontdek het instellen en gebruiken van de component voor sociale media delen.

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

Deze video onderzoekt de volgende faciliteiten van de Sociale Media die component (deel van [&#x200B; de Componenten van de Kern van AEM &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=nl-NL)) delen gebruikend [&#x200B; Wij.Retail &#x200B;](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) steekproefwebsite.

* 0:00 - De component voor sociale media delen toevoegen en configureren
* 1:00 - Delen op Facebook
* 3:10 - Delen naar Pinterest
* 6:25 - De component Sociale media delen gebruiken op een productpagina

## ExternalAlizer instellen {#externalizer-setup}

![&#x200B; CQ van de Dag Verbinding Externalzer &#x200B;](assets/externalizer.png)

[&#x200B; http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[&#x200B; externalizer van AEM &#x200B;](https://helpx.adobe.com/nl/experience-manager/6-5/sites/developing/using/externalizer.html) zou opstelling op zowel de Auteur van AEM als AEM moeten zijn publiceren, om publiceer runmode aan het openbaar toegankelijke domein in kaart te brengen dat wordt gebruikt om tot AEM toegang te hebben publiceer.

In deze video gebruiken wij `/etc/hosts` aan spoof *www.example.com* om aan localhost op te lossen, en de configuratie van a [&#x200B; basisAEM Dispatcher &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html?lang=nl-NL) gebruiken om www.example.com toe te staan om AEM te publiceren.

## Ondersteunende materialen {#supporting-materials}

* [&#x200B; Download de Componenten van de Kern van AEM &#x200B;](https://github.com/adobe/aem-core-wcm-components/releases)
* [&#x200B; Download wij.Retail &#x200B;](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [&#x200B; Installerend Dispatcher &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html?lang=nl-NL)
