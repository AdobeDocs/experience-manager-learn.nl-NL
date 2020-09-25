---
title: Formuliergegevens opslaan en ophalen vanuit MySQL-database
description: Zelfstudie met meerdere onderdelen om u door de stappen te laten lopen die nodig zijn voor het opslaan en ophalen van formuliergegevens
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 6ae8110d4f4bc80682c35b0dab3fe7a62cad88f3
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Gegevensbron configureren

Er zijn vele manieren waarmee AEM integratie met externe gegevensbestand toelaat. Één van de gemeenschappelijkste &amp; standaardpraktijk van gegevensbestandintegratie is door Apache het Verdelen van Verbinding Gepoolde eigenschappen van de Configuratie DataSource door [configMgr](http://localhost:4502/system/console/configMgr)te gebruiken.
De eerste stap bestaat uit het downloaden en implementeren van de juiste [My SQL-stuurprogramma](https://mvnrepository.com/artifact/mysql/mysql-connector-java) &#39;s in AEM.
Vervolgens stelt u de eigenschappen voor het samenvoegen van gebooste gegevensbrongegevens in voor de verbinding. Deze eigenschappen zijn specifiek voor uw database. In de volgende schermafbeelding ziet u de instellingen die voor deze zelfstudie worden gebruikt. Het databaseschema wordt als onderdeel van deze zelfstudie-elementen aan u verstrekt.
![gegevensbron](assets/data-source.png)

Het gegevensbestand heeft één lijst genoemd formdata met de 3 kolommen zoals aangetoond in scherm-ontsproten hieronder![gegevensbestand](assets/data-base-tables.PNG)
