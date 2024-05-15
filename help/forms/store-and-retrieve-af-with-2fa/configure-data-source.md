---
title: Gegevensbron configureren
description: Gegevensbron maken die naar de MySQL-database verwijst
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
duration: 64
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 0%

---

# Gegevensbron configureren

Er zijn vele manieren waarin AEM integratie met een externe gegevensbestand toelaat. Één van de gemeenschappelijkste &amp; standaardpraktijken van gegevensbestandintegratie is door de configuratieeigenschappen van de Verbinding van Apache Gepoolde DataSource door [configMgr](http://localhost:4502/system/console/configMgr).
De eerste stap bestaat uit het downloaden en implementeren van de juiste [MySQL-stuurprogramma&#39;s](https://mvnrepository.com/artifact/mysql/mysql-connector-java) tot AEM.
Vervolgens stelt u de specifieke eigenschappen voor de gegevensbron van uw database in voor de verzamelverbinding. In de volgende schermafbeelding ziet u de instellingen die voor deze zelfstudie worden gebruikt. Het databaseschema wordt als onderdeel van deze zelfstudie-elementen aan u verstrekt.

>[!NOTE]
>Zorg ervoor dat u uw gegevensbron een naam geeft `StoreAndRetrieveAfData` aangezien dit de naam is die in de dienst OSGi wordt gebruikt.


![gegevensbron](assets/data-source.JPG)

| Eigenschapnaam | Waarde van eigenschap |   |
|---------------------|------------------------------------------------------------------------------------|---|
| Naam gegevensbron | `StoreAndRetrieveAfData` |   |
| JDBC-schijfklasse | `jdbc:mysql://localhost:3306/aemformstutorial` |   |
| URI voor JDBC-verbinding | `jdbc:mysql://localhost:3306/aemformstutorial?serverTimezone=UTC&autoReconnect=true` |   |
|                     |                                                                                    |   |


## Database maken


De volgende database is gebruikt voor dit gebruiksgeval. De database heeft één tabel met de naam `formdatawithattachments` met de vier kolommen zoals weergegeven in de onderstaande schermafbeelding.
![gegevensbank](assets/table-schema.JPG)

* De kolom **gegevens** bevat de aangepaste formuliergegevens.
* De kolom **attachmentsInfo** bevat de informatie over de formulierbijlagen.
* De kolommen **phoneNumber** bevat het mobiele nummer van de persoon die het formulier invult.

Maak een database door de [databaseschema](assets/data-base-schema.sql)
MySQL-workbench gebruiken.

## Formuliergegevensmodel maken

Creeer het model van vormgegevens en baseer het op de gegevensbron die in de vorige stap wordt gecreeerd.
Vorm **get** service van dit formuliergegevensmodel, zoals wordt getoond in de onderstaande schermafbeelding.
Zorg ervoor dat u geen array retourneert in het dialoogvenster **get** service.

Het doel van deze **get** de dienst moet het telefoonaantal halen verbonden aan toepassings identiteitskaart

![getService](assets/get-service.JPG)

Dit formuliergegevensmodel wordt vervolgens gebruikt in het dialoogvenster **MyAccountForm** om het telefoonnummer op te halen dat aan toepassings-id is gekoppeld.

## Volgende stappen

[Code schrijven om formulierbijlagen op te slaan](./store-form-attachments.md)
