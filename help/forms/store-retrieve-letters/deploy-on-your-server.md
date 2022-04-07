---
title: Stel de steekproefactiva op uw server op
description: Test sparen als ontwerpfunctionaliteit voor Interactieve Mededelingen
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
source-git-commit: db99787c48e49a9861de893e6cb7fbb7b31807b8
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# Stel de steekproefactiva op uw server op

Volg de onderstaande instructies om deze functionaliteit te laten werken op uw AEM

* [Het databaseschema maken](assets/icdrafts.sql)
* [De clientbibliotheek importeren](assets/icdrafts.zip)
* [Het adaptieve formulier importeren](assets/SavedDraftsAdaptiveForm.zip)
* Gegevensbron genaamd _Opslaan en doorgaan_

![Gegevensbron maken](assets/data-source.png)

| Eigenschapnaam | Waarde van eigenschap |
|---|---|
| Naam gegevensbron | Opslaan en doorgaan |
| JDBC-stuurprogrammaklasse | com.mysql.cj.jdbc.Driver |
| URL JDBC-verbinding | jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&amp;useSSL=false&amp;characterEncoding=utf8&amp;useUnicode=true |

* [De bundel voor pictogrammen implementeren](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Zorg ervoor dat u _Opslaan inschakelen met CCRDocumentInstanceService_ in OSGI config zoals hieronder weergegeven
   ![Concepten inschakelen](assets/enable-drafts.png)
* Open interactieve communicatie. Klik op Opslaan als concept om op te slaan
* [Opgeslagen concepten weergeven](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>De xml-bestanden worden opgeslagen in de hoofdmap van de installatie van de AEM. Het ovaalproject >wordt aan u geleverd om de oplossing aan uw vereisten aan te passen.

Het eclipse-project met voorbeeldimplementatie kan [hier gedownload](assets/icdrafts-eclipse-project.zip)
