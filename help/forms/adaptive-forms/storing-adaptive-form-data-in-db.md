---
title: Adaptieve formuliergegevens opslaan
description: Adaptieve formuliergegevens opslaan in DataBase als onderdeel van uw AEM workflow
feature: Adaptief Forms, formuliergegevensmodel
version: 6.3,6.4,6.5
topic: Ontwikkeling
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '380'
ht-degree: 0%

---


# Aangepaste formulierverzendingen opslaan in database

Er zijn verschillende manieren om de verzonden formuliergegevens op te slaan in de database van uw keuze. Een JDBC-gegevensbron kan worden gebruikt om de gegevens rechtstreeks in de database op te slaan. U kunt een aangepaste OSGI-bundel schrijven om de gegevens in de database op te slaan. Dit artikel gebruikt een aangepaste processtap in AEM workflow om de gegevens op te slaan.
Het gebruiksscenario is dat een AEM workflow wordt geactiveerd voor het verzenden van een adaptief formulier en dat de verzonden gegevens in de database worden opgeslagen in een stap in de workflow.

**Volg de onderstaande stappen om dit op uw systeem te laten werken**

* [Download het Zip-bestand en extraheer de inhoud ervan naar de vaste schijf](assets/storeafdataindb.zip)

   * Importeer de StoreAFInDBWorkflow.zip in AEM met behulp van pakketbeheer. Pakket bevat een voorbeeldworkflow waarin de AF-gegevens worden opgeslagen in de database. Open het workflowmodel. De workflow heeft slechts één stap. Deze stap roept de code die in de bundel wordt geschreven om de gegevens van AF in het Gegevensbestand op te slaan. Ik geef één enkel argument door aan het proces. Dit is de naam van het adaptieve formulier waarvan de gegevens worden opgeslagen.
   * Implementeer de insertData.core-0.0.1-SNAPSHOT.jar met behulp van de Felix-webconsole. Deze bundel heeft de code om de ingediende formuliergegevens naar de database te schrijven

* Ga naar [ConfigMgr](http://localhost:4502/system/console/configMgr)

   * Zoek naar &quot;Pool van de Verbinding JDBC&quot;. Maak een nieuwe dag gemeenschappelijke JDBC-verbindingspool. Geef de specifieke instellingen voor uw database op.

   * ![jdbc-verbindingspool](assets/jdbc-connection-pool.png)
   * Zoeken naar &quot;**Formuliergegevens invoegen in DB**&quot;
   * Geef de specifieke eigenschappen voor uw database op.
      * DataSourceName:Naam van de gegevensbron u eerder vormde.
      * TableName - Naam van de lijst waarin u de Gegevens van AF wilt opslaan
      * Formuliernaam - Kolomnaam voor de naam van het formulier
      * Kolomnaam - Kolomnaam om de AF-gegevens te bevatten

   ![insertData](assets/insertdata.PNG)

* Maak een adaptief formulier.

* Koppel het adaptieve formulier aan AEM Workflow (StoreAFValuesinDB), zoals hieronder in de schermafbeelding wordt weergegeven.

* Zorg ervoor dat u &quot;data.xml&quot; opgeeft in het pad naar het gegevensbestand, zoals wordt weergegeven in de onderstaande schermafbeelding.

   ![indiening](assets/submissionafforms.png)

* Een voorbeeld van het formulier bekijken en verzenden

* Als alles goed is gegaan, moet u zien dat de formuliergegevens worden opgeslagen in de tabel en kolom die door u zijn opgegeven



