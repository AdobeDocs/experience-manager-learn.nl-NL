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
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---


# Deze op uw server implementeren

>[!NOTE]
>
>Hieronder vindt u de volgende vereisten om dit op uw systeem uit te voeren
>
>* AEM Forms (versie 6.3 of hoger)
>* MYSQL-database


Voer de volgende stappen uit om deze mogelijkheid te testen op uw AEM Forms-exemplaar

* Download en decomprimeer de [lesbestanden](assets/store-retrieve-form-data.zip) naar uw lokale systeem
* Implementeer en start de bundels techmarketingdemos.jar en mysqldriver.jar met behulp van de [Felix-webconsole](http://localhost:4502/system/console/configMgr)
* Importeer het bestand aemformstutorial.sql met MYSQL Workbench. Dit zal tot het noodzakelijke schema en de lijsten in uw gegevensbestand voor dit leerprogramma leiden te werken.
* Import StoreAndRetrieve.zip using [AEM package manager.](http://localhost:4502/crx/packmgr/index.jsp) Dit pakket bevat de adaptieve formuliersjabloon, de client lib van de paginacomponent en een voorbeeldconfiguratie voor adaptieve formulieren en gegevensbronnen.
* Meld u aan bij [configMgr.](http://localhost:4502/system/console/configMgr) Zoek naar &quot;Apache Sling Connection Pooled DataSource. Open de gegevensbroningang verbonden aan een modelstudie en ga de gebruikersbenaming en het wachtwoord in specifiek voor uw gegevensbestandinstantie.
* Het [adaptieve formulier openen](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Vul enkele details in en klik op de knop &quot;Opslaan en verdergaan&quot;
* U zou terug een URL met een GUID in het moeten krijgen.
* Kopieer de URL en plak deze in een nieuw browsertabblad. **Zorg ervoor dat er geen lege ruimte aan het einde van de URL is**
* Het adaptieve formulier moet worden gevuld met de gegevens uit de vorige stap
