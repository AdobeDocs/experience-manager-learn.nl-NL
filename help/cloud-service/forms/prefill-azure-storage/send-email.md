---
title: E-mail verzenden met SendGrid
description: Een e-mail activeren met een koppeling naar het opgeslagen formulier
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-8474
duration: 25
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# E-mail verzenden

Nadat de gegevens van het formulier zijn opgeslagen in Azure Blob Storage, wordt een e-mail met een koppeling naar het opgeslagen formulier verzonden naar de gebruiker. Deze e-mail wordt verzonden gebruikend REST API SendGrid.

Het kwikbestand, het formuliergegevensmodel en de configuratie van de cloudservices die nodig zijn om e-mails te verzenden, worden als onderdeel van de artikelelementen aan u geleverd.

U moet een SendGrid-account maken, een dynamische sjabloon die gegevens kan opnemen die zijn vastgelegd in het adaptieve formulier.


Hier volgt een fragment van HTML-code die in de dynamische sjabloon wordt gebruikt. De waarde van de parameter blobID wordt doorgegeven met behulp van het formuliergegevensmodel invoke service.

```html
You can  <a href='https://forms.enablementadobe.com/content/dam/formsanddocuments/azureportalstorage/creditcardapplication/jcr:content?wcmmode=disabled&ampguid={{blobID}}'>access your application here</a> and complete it.
```


