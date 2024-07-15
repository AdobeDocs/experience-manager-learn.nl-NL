---
title: Delen van sociale media gebruiken in AEM Sites
description: Ontdek het instellen en gebruiken van de component voor sociale media delen.
feature: Core Components
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
duration: 511
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 0%

---

# Delen via sociale media gebruiken {#using-social-media-sharing-in-aem-sites}

Ontdek het instellen en gebruiken van de component voor sociale media delen.

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

Deze video onderzoekt de volgende faciliteiten van de Sociale Media die component (deel van [ AEM de Componenten van de Kern ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)) delen gebruikend [ Wij.Retail ](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) steekproefwebsite.

* 0:00 - De component voor sociale media delen toevoegen en configureren
* 1:00 - Delen naar Facebook
* 3:10 - Delen naar Pinterest
* 6:25 - De component Sociale media delen gebruiken op een productpagina

## ExternalAlizer instellen {#externalizer-setup}

![ CQ van de Dag Verbinding Externalzer ](assets/externalizer.png)

[ http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[ AEM externalizer ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) zou opstelling op zowel AEM Auteur als AEM Publish moeten zijn, om publiceer runmode aan het openbaar toegankelijke domein in kaart te brengen dat wordt gebruikt om tot AEM Publish toegang te hebben.

In deze video gebruiken wij `/etc/hosts` aan spoof *www.example.com* om aan localhost op te lossen, en de configuratie van a [ basis AEM Dispatcher ](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) gebruiken om www.example.com toe te staan om AEM Publish.

## Ondersteunende materialen {#supporting-materials}

* [ Download de Componenten van de AEMKern ](https://github.com/adobe/aem-core-wcm-components/releases)
* [ Download wij.Retail ](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [ Installerend Dispatcher ](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
