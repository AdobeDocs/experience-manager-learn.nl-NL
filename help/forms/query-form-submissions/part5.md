---
title: Het voorbeeld op uw lokale server implementeren
description: Multi-part zelfstudie om u door de stappen te leiden die betrokken zijn bij het opvragen van formulierverzendingen die zijn opgeslagen in Azure Portal
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: 44841a3c-85e0-447f-85e2-169a451d9c68
duration: 20
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Het voorbeeld op uw lokale server implementeren

Volg de onderstaande stappen om deze toepassing geschikt te maken voor uw lokale server. Er wordt aangenomen dat uw AEM-exemplaar wordt uitgevoerd op localhost, 4502-poort.

* [ installeer het pakket ](assets/azuredemo.all-1.0.0-SNAPSHOT.zip) gebruikend pakketmanager.

* Verstrek de Azure poortgeloofsbrieven gebruikend OSGi configMgr
  ![ azure-portaal ](assets/azure-portal-config.png)
Zorg ervoor dat de opslag-URI eindigt in slash en dat het SAS-token begint met een ?
* Navigeer aan [ AzureDemo ](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/azuredemo)

* Geef de authentificatiemontages van de volgende 3 gegevensbronnen uit om uw milieu aan te passen
  ![ gegevens-bronnen ](assets/fdm-data-sources.png)

* De voorproef en legt [ vorm ContactUs ](http://localhost:4502/content/dam/formsanddocuments/azureportal/contactus/jcr:content?wcmmode=disabled) voor

* [ Vraag uw vormvoorlegging ](http://localhost:4502/content/dam/formsanddocuments/azureportal/queryformsubmissions/jcr:content?wcmmode=disabled)
