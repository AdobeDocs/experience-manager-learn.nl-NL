---
title: Gegevensbron AEM configureren
description: Door MySQL ondersteunde gegevensbron configureren om formuliergegevens op te slaan en op te halen
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 2%

---

# Gegevensbron configureren

Er zijn vele manieren waarmee AEM integratie met een externe gegevensbestand toelaat. Één van de gemeenschappelijkste manieren om een gegevensbestand te integreren is door Apache het Verdelen van Verbinding Gepoolde configuratieeigenschappen DataSource door [configMgr](http://localhost:4502/system/console/configMgr) te gebruiken.
De eerste stap bestaat uit het downloaden en implementeren van de juiste [MySql drivers](https://mvnrepository.com/artifact/mysql/mysql-connector-java) in AEM.
Maak Apache Sling Connection Pooled DataSource en geef de eigenschappen op die zijn opgegeven in de onderstaande schermafbeelding. Het databaseschema wordt als onderdeel van deze zelfstudie-elementen aan u verstrekt.

![gegevensbron](assets/data-source.PNG)

Het gegevensbestand heeft één lijst genoemd formdata met de 3 kolommen zoals aangetoond in het scherm-schot hieronder.

![gegevensbank](assets/data-base.PNG)


>[!NOTE]
>Gelieve te zorgen u uw gegevensbron **aemformstutorial** noemen. De voorbeeldcode gebruikt de naam om verbinding te maken met de database.

| Eigenschapnaam | Waarde |
------------------------|---------------------------------------
| Naam gegevensbron | Opslaan en doorgaan |
| JDBC-stuurprogramma, klasse | com.mysql.cj.jdbc.Driver |
| JDBC-verbindingsuri | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

Het sql- dossier om het schema tot stand te brengen kan [van hier ](assets/sign-multiple-forms.sql) worden gedownload. U zult dit dossier gebruikend MySql werkbank moeten invoeren om het schema en de lijst tot stand te brengen.


