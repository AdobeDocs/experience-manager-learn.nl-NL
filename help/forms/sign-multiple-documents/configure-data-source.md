---
title: Gegevensbron AEM configureren
description: Door MySQL ondersteunde gegevensbron configureren om formuliergegevens op te slaan en op te halen
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---

# Gegevensbron configureren

Er zijn vele manieren waarmee AEM integratie met een externe gegevensbestand toelaat. Één van de gemeenschappelijkste manieren om een gegevensbestand te integreren is door de configuratieeigenschappen van de Verbinding van Apache Gepoolde DataSource door [configMgr](http://localhost:4502/system/console/configMgr).
De eerste stap bestaat uit het downloaden en implementeren van de juiste [MySql-stuurprogramma&#39;s](https://mvnrepository.com/artifact/mysql/mysql-connector-java) in AEM.
Maak Apache Sling Connection Pooled DataSource en geef de eigenschappen op die zijn opgegeven in de onderstaande schermafbeelding. Het databaseschema wordt als onderdeel van deze zelfstudie-elementen aan u verstrekt.

![gegevensbron](assets/data-source.PNG)

Het gegevensbestand heeft één lijst genoemd formdata met de 3 kolommen zoals aangetoond in het scherm-schot hieronder.

![gegevensbank](assets/data-base.PNG)


>[!NOTE]
>Zorg ervoor dat u uw gegevensbron een naam geeft **vormgeving**. De voorbeeldcode gebruikt de naam om verbinding te maken met de database.

| Eigenschapnaam | Waarde |
| ------------------------|--------------------------------------- |
| Naam gegevensbron | Opslaan en doorgaan |
| JDBC-stuurprogramma, klasse | com.mysql.cj.jdbc.Driver |
| JDBC-verbindingsuri | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

Het SQL-bestand om het schema te maken, kan [hier gedownload](assets/sign-multiple-forms.sql). U zult dit dossier gebruikend MySql werkbank moeten invoeren om het schema en de lijst tot stand te brengen.

## Volgende stappen

[Creeer de dienst OSGi om gegevens in gegevensbestand op te slaan en te halen](./create-osgi-service.md)
