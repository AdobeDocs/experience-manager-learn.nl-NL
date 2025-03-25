---
title: Source van gegevens configureren
description: Gegevensbron maken die naar de MySQL-database verwijst
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
duration: 64
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 0%

---

# Source van gegevens configureren

Er zijn veel manieren waarop AEM integratie met een externe database mogelijk maakt. Één van de gemeenschappelijkste &amp; standaardpraktijken van gegevensbestandintegratie is door Apache Te gebruiken die Verbinding Gepoolde eigenschappen van de Configuratie DataSource door [ configMgr ](http://localhost:4502/system/console/configMgr) ruilt.
De eerste stap is de aangewezen [ bestuurders MySQL ](https://mvnrepository.com/artifact/mysql/mysql-connector-java) aan AEM te downloaden en op te stellen.
Vervolgens stelt u de specifieke eigenschappen voor de gegevensbron van uw database in voor de verzamelverbinding. In de volgende schermafbeelding ziet u de instellingen die voor deze zelfstudie worden gebruikt. Het databaseschema wordt als onderdeel van deze zelfstudie-elementen aan u verstrekt.

>[!NOTE]
>Gelieve te zorgen u uw gegevensbron `StoreAndRetrieveAfData` noemt aangezien dit de naam in de dienst OSGi wordt gebruikt.


![ gegeven-bron ](assets/data-source.JPG)

| Eigenschapnaam | Waarde van eigenschap |   |
|---------------------|------------------------------------------------------------------------------------|---|
| Naam gegevensbron | `StoreAndRetrieveAfData` |   |
| JDBC-schijfklasse | `jdbc:mysql://localhost:3306/aemformstutorial` |   |
| URI voor JDBC-verbinding | `jdbc:mysql://localhost:3306/aemformstutorial?serverTimezone=UTC&autoReconnect=true` |   |
|                     |                                                                                    |   |


## Database maken


De volgende database is gebruikt voor dit gebruiksgeval. De database heeft één tabel met de naam `formdatawithattachments` en de vier kolommen, zoals in de onderstaande schermafbeelding wordt weergegeven.
![ gegeven-basis ](assets/table-schema.JPG)

* De kolom **afdata** zal de adaptieve vormgegevens houden.
* De kolom **attachmentsInfo** zal de informatie over de vormgehechtheid houden.
* De kolommen **phoneNumber** zullen het mobiele aantal van de persoon houden die de vorm invult.

Gelieve te creëren het gegevensbestand door het [ gegevensbestandschema ](assets/data-base-schema.sql) in te voeren
MySQL-workbench gebruiken.

## Formuliergegevensmodel maken

Creeer het model van vormgegevens en baseer het op de gegevensbron die in de vorige stap wordt gecreeerd.
Vorm de **krijgen** dienst van dit model van vormgegevens zoals aangetoond in hieronder het schermschot.
Zorg ervoor u geen serie in de **terugkeert krijgt** dienst.

Het doel van deze **krijgt** dienst moet het telefoonaantal halen verbonden aan toepassings identiteitskaart.

![ get-service ](assets/get-service.JPG)

Dit model van vormgegevens zal dan in **MyAccountForm** worden gebruikt om het telefoonaantal te halen verbonden aan toepassings identiteitskaart.

## Volgende stappen

[Code schrijven om formulierbijlagen op te slaan](./store-form-attachments.md)
