---
title: Configuratie van batchgegevens configureren
description: Configuratie van batchgegevens configureren
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9673
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: db25e5a2-e1a8-40ad-af97-35604d515450
duration: 233
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Batchconfiguratie maken

Als u een batch-API wilt gebruiken, maakt u een batchconfiguratie en voert u op basis van die configuratie een uitvoering uit. In de volgende video ziet u een demonstratie van het maken van batchconfiguraties met behulp van de API.

>[!NOTE]
>Controleer of de AEM-gebruiker tot de ```forms-users``` -groep behoort om API-aanroepen uit te voeren.


>[!VIDEO](https://video.tv.adobe.com/v/340241?quality=12&learn=on)

## Batchconfiguratie maken

Het volgende is het POST eindpunt voor het creëren van Batch config

```xml
<baseURL>/config
```

Het volgende is de minimumconfiguratie die moet worden gespecificeerd wanneer het creëren van partijconfiguratie. Dit moet worden doorgegeven als JSON-object in de hoofdtekst van de HTTP-aanvraag

```
{
    "configName": "monthlystatements",
    "dataSourceConfigUri": "/conf/batchapi/settings/forms/usc/batch/batchapitutorial",
    "outputTypes": [
        "PDF"
    ],
    "template": "crx:///content/dam/formsanddocuments/formtemplates/custom_fonts.xdp"

}
```

## Batchconfiguratie controleren

Om de succesvolle verwezenlijking van partijconfiguratie te verifiëren, kunt u een GET- verzoekvraag aan het volgende eindpunt richten


```xml
<baseURL>/config/monthlystatements
```

U hoeft alleen een leeg JSON-object in de hoofdtekst van de HTTP-aanvraag door te geven
