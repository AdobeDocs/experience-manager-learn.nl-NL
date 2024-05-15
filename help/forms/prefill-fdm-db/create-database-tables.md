---
title: Databasetabellen maken
description: Database maken voor gebruik door formuliergegevensmodel
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5811
thumbnail: kt-5811.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 1136244a-c3e6-45f6-8af8-eb3c100f838e
duration: 21
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 0%

---

# Databasetabellen maken

Het gegevensmodel van het formulier kan worden gebaseerd op bronnen van RDBMS, RESTfull, SOAP of OData. De focus van deze cursus ligt op het vooraf instellen van een adaptief formulier met behulp van een formuliergegevensmodel dat wordt ondersteund door de RDBMS-gegevensbron. Voor deze zelfstudie werd MYSQL-database gebruikt. We hebben de volgende twee tabellen gemaakt om het gebruiksgeval aan te tonen

* **newheren** table - Deze lijst slaat de juiste informatie op

  ![newheren](assets/newhire-table.png)


* **begunstigden** Tabel - Hiermee worden alle begunstigden opgeslagen

  ![begunstigden](assets/beneficiaries-table.png)

U kunt de [sql-bestand](assets/db-schema.sql) met MySQL-workbench maken van tabellen met voorbeeldgegevens.

## Volgende stappen

[Formuliergegevensmodel configureren](./configuring-form-data-model.md)
