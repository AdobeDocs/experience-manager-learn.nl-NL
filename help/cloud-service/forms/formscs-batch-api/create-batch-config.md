---
title: Configuratie van batchgegevens configureren
description: Configuratie van batchgegevens configureren
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9673
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# Batchconfiguratie maken

Als u een batch-API wilt gebruiken, maakt u een batchconfiguratie en voert u op basis van die configuratie een uitvoering uit. In de volgende video ziet u een demonstratie van het maken van batchconfiguraties met behulp van de API.

>[!NOTE]
>Zorg ervoor dat de AEM gebruiker tot ```forms-users``` groep om API-aanroepen uit te voeren.


>[!VIDEO](https://video.tv.adobe.com/v/340241/?quality=12&learn=on)

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
