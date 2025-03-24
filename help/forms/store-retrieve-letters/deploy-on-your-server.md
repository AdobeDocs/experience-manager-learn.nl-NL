---
title: Stel de steekproefactiva op uw server op
description: Test sparen als ontwerpfunctionaliteit voor Interactieve Mededelingen
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
duration: 28
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 0%

---

# Stel de steekproefactiva op uw server op

Volg onderstaande instructies om deze functionaliteit te laten werken op uw AEM Server

* [Het databaseschema maken](assets/icdrafts.sql)
* [De clientbibliotheek importeren](assets/icdrafts.zip)
* [Het adaptieve formulier importeren](assets/SavedDraftsAdaptiveForm.zip)
* Creeer gegevensbron genoemd _SaveAndContinue_

![ creeer Gegevens Source ](assets/data-source.png)

| Eigenschapnaam | Waarde van eigenschap |
|---|---|
| Naam gegevensbron | `SaveAndContinue` |
| JDBC-stuurprogrammaklasse | `com.mysql.cj.jdbc.Driver` |
| URL JDBC-verbinding | `jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&useSSL=false&characterEncoding=utf8&useUnicode=true` |

* [De bundel voor pictogrammen implementeren](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Zorg ervoor u _sparen gebruikend CCRDocumentInstanceService_ in config OSGI zoals hieronder getoond toelaat
  ![ laat Concepten ](assets/enable-drafts.png) toe
* Open interactieve communicatie. Klik op Opslaan als concept om op te slaan
* [ Mening Bewaarde Concepten ](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>De xml-bestanden worden opgeslagen in de hoofdmap van de AEM-serverinstallatie. Het ovaalproject >wordt aan u geleverd om de oplossing aan uw vereisten aan te passen.

Het eclipse- project met steekproefimplementatie kan [ van hier worden gedownload ](assets/icdrafts-eclipse-project.zip)
