---
title: Sociale postering instellen met AEM ervaringsfragmenten
description: Met ervaringsfragmenten kunnen marketers ervaringen die in AEM zijn gemaakt, op sociale-mediaplatforms plaatsen. In de onderstaande video ziet u de instellingen en configuratie die nodig zijn om Experience Fragments te publiceren naar Facebook en Pinterest.
sub-product: sites, content-services
feature: experience-fragments
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 0%

---


# Sociale post instellen met ervaringsfragmenten {#set-up-social-posting-with-experience-fragments}

Met ervaringsfragmenten kunnen marketers ervaringen die in AEM zijn gemaakt, op sociale-mediaplatforms plaatsen. In de onderstaande video ziet u de instellingen en configuratie die nodig zijn om Experience Fragments te publiceren naar Facebook en Pinterest.

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)
*[Ervaar Fragments]- Opstelling en configuratie aan sociale post aan Facebook en Pinterest*

## Checklist voor het configureren van Experience Fragments voor post naar Facebook en Pinterest

1. Instantie AEM-auteur wordt uitgevoerd op HTTPS
2. Facebook-account + Facebook Developer App
3. Pinterest-account + Pinterest-ontwikkelaarsapp
4. [!UICONTROL AEM Cloud Services] Configuratie - Facebook
5. [!UICONTROL AEM Cloud Services] Configuratie - Pinterest
6. AEM Ervaar Fragmentatie met Cloud Services voor Facebook + Pinterest
7. Ervaar fragmentvariatie met Facebook-sjabloon
8. Ervaar fragmentvariatie met Pinterest-sjabloon

## URI met omleiding van fragment ervaren

Deze URI wordt gebruikt voor Facebook- en Pinterest-apps als onderdeel van de Oauth-flow.

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

