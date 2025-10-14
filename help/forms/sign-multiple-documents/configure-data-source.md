---
title: AEM Data Source configureren
description: Door MySQL ondersteunde gegevensbron configureren om formuliergegevens op te slaan en op te halen
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
duration: 39
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 0%

---

# Gegevensbron configureren

Er zijn vele manieren waarmee AEM integratie met een externe database mogelijk maakt. Één van de gemeenschappelijkste manieren om een gegevensbestand te integreren is door Apache het Schipen Verbinding Gepoolde eigenschappen van de Configuratie DataSource door [&#x200B; configMgr &#x200B;](http://localhost:4502/system/console/configMgr) te gebruiken.
De eerste stap is de aangewezen [&#x200B; bestuurders MySql &#x200B;](https://mvnrepository.com/artifact/mysql/mysql-connector-java) in AEM te downloaden en op te stellen.
Maak Apache Sling Connection Pooled DataSource en geef de eigenschappen op die zijn opgegeven in de onderstaande schermafbeelding. Het databaseschema wordt als onderdeel van deze zelfstudie-elementen aan u verstrekt.

![&#x200B; gegeven-bron &#x200B;](assets/data-source.PNG)

Het gegevensbestand heeft één lijst genoemd formdata met de 3 kolommen zoals aangetoond in het scherm-schot hieronder.

![&#x200B; gegeven-basis &#x200B;](assets/data-base.PNG)


>[!NOTE]
>Gelieve te zorgen u uw gegevensbron **aemformstutorial** noemt. De voorbeeldcode gebruikt de naam om verbinding te maken met de database.

| Eigenschapnaam | Waarde |
| ------------------------|--------------------------------------- |
| Naam gegevensbron | `SaveAndContinue` |
| JDBC-stuurprogramma, klasse | `com.mysql.cj.jdbc.Driver` |
| JDBC-verbindingsuri | `jdbc:mysql://localhost:3306/aemformstutorial` |

## Assets

Het SQL dossier om het schema tot stand te brengen kan [&#x200B; van hier worden gedownload &#x200B;](assets/sign-multiple-forms.sql). U zult dit dossier gebruikend MySql werkbank moeten invoeren om het schema en de lijst tot stand te brengen.

## Volgende stappen

[Creeer de dienst OSGi om gegevens in gegevensbestand op te slaan en te halen](./create-osgi-service.md)
