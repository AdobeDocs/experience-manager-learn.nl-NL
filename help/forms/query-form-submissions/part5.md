---
title: Het voorbeeld op uw lokale server implementeren
description: Multi-part zelfstudie om u door de stappen te leiden die betrokken zijn bij het opvragen van formulierverzendingen die zijn opgeslagen in Azure Portal
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
source-git-commit: ae2a2cbde1bf21314cc77863014cb0f013b6e0bb
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Het voorbeeld op uw lokale server implementeren

Volg de onderstaande stappen om deze toepassing op uw lokale server te laten werken. Er wordt aangenomen dat uw AEM op localhost, 4502-poort, wordt uitgevoerd.

* [Het pakket installeren](assets/azuredemo.all-1.0.0-SNAPSHOT.zip) met pakketbeheer.

* Verstrek de Azure poortgeloofsbrieven gebruikend OSGi configMgr
  ![azure-portaal](assets/azure-portal-config.png)
Zorg ervoor dat de opslag-URI eindigt in slash en dat het SAS-token begint met een ?
* Navigeren naar [AzureDemo](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/azuredemo)

* Geef de authentificatiemontages van de volgende 3 gegevensbronnen uit om uw milieu aan te passen
  ![gegevensbronnen](assets/fdm-data-sources.png)

* Voorvertonen en verzenden [ContactU-formulier](http://localhost:4502/content/dam/formsanddocuments/azureportal/contactus/jcr:content?wcmmode=disabled)

* [Vraag uw formulierverzending](http://localhost:4502/content/dam/formsanddocuments/azureportal/queryformsubmissions/jcr:content?wcmmode=disabled)

