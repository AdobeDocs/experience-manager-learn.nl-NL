---
title: Acrobat Sign Cloud Configuration Cloud Service maken
description: Maak de integratie met AEM Forms en Acrobat Sign met behulp van de configuratie van cloudservices.
solution: Experience Manager,Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-7428
thumbnail: 332437.jpg
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: a55773a5-0486-413f-ada6-bb589315f0b1
duration: 222
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 0%

---

# Acrobat Sign Cloud-configuratie maken

Met de configuratie van cloudservices in AEM kunt u integratie tot stand brengen tussen AEM en andere cloudtoepassingen.

In de volgende video ziet u de stappen die nodig zijn om de configuratie van cloudservices te maken voor de integratie van AEM met Acrobat Sign

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## Problemen oplossen

Als er een fout optreedt bij het configureren van de cloudconfiguratie voor bovenstaande ondertekening, kunt u de volgende stappen uitvoeren om problemen op te lossen
* Zorg ervoor dat de URL voor omleiding die is opgegeven in de Acrobat Sign API-toepassing de volgende notatie heeft
&lt;uw instantienaam>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;container>.
Bijvoorbeeld: https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS. FormsCS is de naam van de container die de cloudconfiguratie zal bevatten
* Controleer of de Auth-URL juist is
* Controleer uw Client-id en clientgeheim
* De modus van het venster Incognito proberen

