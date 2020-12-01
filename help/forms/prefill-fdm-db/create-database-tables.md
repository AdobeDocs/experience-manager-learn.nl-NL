---
title: Databasetabellen maken
description: Database maken voor gebruik door formuliergegevensmodel
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
translation-type: tm+mt
source-git-commit: b085a2c75f8e0b4860d503774ea01a108773ad09
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 0%

---


# Databasetabellen maken

Het formuliergegevensmodel kan worden gebaseerd op RDBMS-, RESTfull-, SOAP- of OData-bronnen. De focus van deze cursus ligt op het vooraf instellen van een adaptief formulier met behulp van een formuliergegevensmodel dat wordt ondersteund door de RDBMS-gegevensbron. Voor deze zelfstudie werd MYSQL-database gebruikt. We hebben de volgende twee tabellen gemaakt om het gebruiksgeval aan te tonen

* **** newhiretable - Deze lijst slaat de nieuwe informatie op

   ![newheren](assets/newhire-table.png)


* **Begunstigd -** Deze slaat de begunstigden op

   ![begunstigden](assets/beneficiaries-table.png)

U kunt het [sql dossier](assets/db-schema.sql) invoeren gebruikend werkbank MySQL om aan lijsten met sommige steekproefgegevens tot stand te brengen.