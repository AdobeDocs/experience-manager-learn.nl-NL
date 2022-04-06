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
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# Stel de steekproefactiva op uw server op

Volg de onderstaande instructies om deze functionaliteit te laten werken op uw AEM

* Maak een map met de naam icconcepten in uw c-station
* [Het databaseschema maken](assets/icdrafts.sql)
* [De clientbibliotheek importeren](assets/icdrafts.zip)
* [Het adaptieve formulier importeren](assets/SavedDraftsAdaptiveForm.zip)
* Gegevensbron genaamd _Opslaan en doorgaan_

![Gegevensbron maken](assets/data-source.png)

* [De bundel voor pictogrammen implementeren](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Zorg ervoor dat u _Opslaan inschakelen met CCRDocumentInstanceService_ in OSGI config zoals hieronder weergegeven
   ![Concepten inschakelen](assets/enable-drafts.png)
* Open interactieve communicatie. Klik op Opslaan als concept om op te slaan
* [Opgeslagen concepten weergeven](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>De xml-bestanden worden opgeslagen in de hoofdmap van de installatie van de AEM. Het ovaalproject >wordt aan u geleverd om de oplossing aan uw vereisten aan te passen.

Het eclipse-project met voorbeeldimplementatie kan [hier gedownload](assets/icdrafts-eclipse-project.zip)
