---
title: Gegevensbron configureren
description: Gegevensbron maken die naar de MySQL-database verwijst
feature: Adaptieve Forms
type: Tutorial
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: Ontwikkeling
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---


# Gegevensbron configureren

Er zijn vele manieren waarmee AEM integratie met externe gegevensbestand toelaat. Één van de gemeenschappelijkste &amp; standaardpraktijk van gegevensbestandintegratie is door Apache het Verdelen van Verbinding Gepoolde configuratieeigenschappen DataSource door [configMgr](http://localhost:4502/system/console/configMgr) te gebruiken.
De eerste stap bestaat uit het downloaden en implementeren van de juiste [MySQL-stuurprogramma&#39;s](https://mvnrepository.com/artifact/mysql/mysql-connector-java) om te AEM.
Stel vervolgens de specifieke eigenschappen voor de gegevensbron van de verzamelverbinding in. In de volgende schermafbeelding ziet u de instellingen die voor deze zelfstudie worden gebruikt. Het databaseschema wordt als onderdeel van deze zelfstudie-elementen aan u verstrekt.

![gegevensbron](assets/data-source.JPG)


* JDBC-stuurprogramma-klasse: `com.mysql.cj.jdbc.Driver`
* URI JDBC-verbinding: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>Gelieve te zorgen u uw gegevensbron `StoreAndRetrieveAfData` aangezien dit de naam is die in de dienst OSGi wordt gebruikt.


## Database maken


De volgende database is gebruikt voor dit gebruiksgeval. De database heeft één tabel met de naam `formdatawithattachments` met de vier kolommen, zoals hieronder in de schermafbeelding wordt weergegeven.
![gegevensbank](assets/table-schema.JPG)

* De kolom **afdata** bevat de adaptieve formuliergegevens.
* De kolom **attachmentsInfo** bevat de informatie over de formulierbijlagen.
* De kolommen **phoneNumber** bevatten het mobiele nummer van de persoon die het formulier invult.

Maak de database door het [databaseschema](assets/data-base-schema.sql) te importeren
MySQL-workbench gebruiken.

## Formuliergegevensmodel maken

Creeer het model van vormgegevens en baseer het op de gegevensbron die in de vorige stap wordt gecreeerd.
Configureer de **get**-service van dit formuliergegevensmodel, zoals wordt weergegeven in de onderstaande schermafbeelding.
Zorg ervoor u geen serie in de **get** dienst terugkeert.

Deze **get** service wordt gebruikt om het telefoonnummer op te halen dat aan de toepassings-id is gekoppeld.

![getService](assets/get-service.JPG)

Dit formuliergegevensmodel wordt vervolgens gebruikt in **MyAccountForm** om het telefoonnummer op te halen dat aan de toepassings-id is gekoppeld.
