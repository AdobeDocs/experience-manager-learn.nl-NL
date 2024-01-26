---
title: Configuratie van batchgegevens configureren
description: Configuratie van batchgegevens configureren
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9673
exl-id: db25e5a2-e1a8-40ad-af97-35604d515450
duration: 239
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# Batchconfiguratie maken

Als u een batch-API wilt gebruiken, maakt u een batchconfiguratie en voert u op basis van die configuratie een uitvoering uit. In de volgende video ziet u een demonstratie van het maken van batchconfiguraties met behulp van de API.

>[!NOTE]
>Zorg ervoor dat de AEM gebruiker tot ```forms-users``` groep om API-aanroepen uit te voeren.


>[!VIDEO](https://video.tv.adobe.com/v/340241?quality=12&learn=on)

## Batchconfiguratie maken

Het volgende is het eindpunt van de POST voor het creëren van Batch config

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

Om de succesvolle verwezenlijking van partijconfiguratie te verifiëren, kunt u een vraag van de GET tot het volgende eindpunt richten


```xml
<baseURL>/config/monthlystatements
```

U hoeft alleen een leeg JSON-object in de hoofdtekst van de HTTP-aanvraag door te geven
