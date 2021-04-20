---
title: Formuliergegevens opslaan en ophalen vanuit MySQL-database
description: Zelfstudie met meerdere onderdelen om u door de stappen te laten lopen die nodig zijn voor het opslaan en ophalen van formuliergegevens
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 1%

---

# Gegevensbron configureren

Er zijn vele manieren waarmee AEM integratie met externe gegevensbestand toelaat. Één van de gemeenschappelijkste &amp; standaardpraktijk van gegevensbestandintegratie is door Apache het Verdelen van Verbinding Gepoolde configuratieeigenschappen DataSource door [configMgr](http://localhost:4502/system/console/configMgr) te gebruiken.
De eerste stap bestaat uit het downloaden en implementeren van de juiste [MySql drivers](https://mvnrepository.com/artifact/mysql/mysql-connector-java) in AEM.
Maak Apache Sling Connection Pooled DataSource en geef de eigenschappen op die zijn opgegeven in de onderstaande schermafbeelding. Het databaseschema wordt als onderdeel van deze zelfstudie-elementen aan u verstrekt.

![gegevensbron](assets/save-continue.PNG)

Het gegevensbestand heeft één lijst genoemd formdata met de 3 kolommen zoals aangetoond in het scherm-schot hieronder.

![gegevensbank](assets/data-base-tables.PNG)

Het sql- dossier om het schema tot stand te brengen kan [van hier ](assets/form-data-db.sql) worden gedownload. U zult dit dossier gebruikend MySql werkbank moeten invoeren om het schema en de lijst tot stand te brengen.

>[!NOTE]
>Geef de gegevensbron **SaveAndContinue** een naam. De voorbeeldcode gebruikt de naam om verbinding te maken met de database.

| Eigenschapnaam | Waarde |
------------------------|---------------------------------------
| Naam gegevensbron | Opslaan en doorgaan |
| JDBC-stuurprogramma, klasse | com.mysql.cj.jdbc.Driver |
| JDBC-verbindingsuri | jdbc:mysql://localhost:3306/aemformstutorial |


