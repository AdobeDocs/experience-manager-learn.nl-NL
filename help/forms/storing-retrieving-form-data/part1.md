---
title: Opslaan en ophalen van formuliergegevens uit MySQL-database - Gegevensbron configureren
description: Zelfstudie met meerdere onderdelen om door de stappen te bladeren die nodig zijn voor het opslaan en ophalen van formuliergegevens
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
duration: 49
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---

# Gegevensbron configureren

Er zijn vele manieren waarmee AEM integratie met externe gegevensbestand toelaat. Één van de gemeenschappelijkste &amp; standaardpraktijk van gegevensbestandintegratie is door Apache het Verdelen van Verbinding Gepoolde eigenschappen van de Configuratie DataSource door [configMgr](http://localhost:4502/system/console/configMgr).
De eerste stap bestaat uit het downloaden en implementeren van de juiste [MySql-stuurprogramma&#39;s](https://mvnrepository.com/artifact/mysql/mysql-connector-java) in AEM.
Maak Apache Sling Connection Pooled DataSource en geef de eigenschappen op die zijn opgegeven in de onderstaande schermafbeelding. Het databaseschema wordt als onderdeel van deze zelfstudie-elementen aan u verstrekt.

![gegevensbron](assets/save-continue.PNG)

Het gegevensbestand heeft één lijst genoemd formdata met de 3 kolommen zoals aangetoond in het scherm-schot hieronder.

![gegevensbank](assets/data-base-tables.PNG)

Het sql-bestand waarmee het schema wordt gemaakt, kan [hier gedownload](assets/form-data-db.sql). U zult dit dossier gebruikend MySql werkbank moeten invoeren om het schema en de lijst tot stand te brengen.

>[!NOTE]
>Zorg ervoor dat u uw gegevensbron een naam geeft **Opslaan en doorgaan**. De voorbeeldcode gebruikt de naam om verbinding te maken met de database.

| Eigenschapnaam | Waarde |
| ------------------------|---------------------------------------|
| Naam gegevensbron | Opslaan en doorgaan |
| JDBC-stuurprogramma, klasse | com.mysql.cj.jdbc.Driver |
| JDBC-verbindingsuri | jdbc:mysql://localhost:3306/aemformstutorial |
