---
title: Formuliergegevens opslaan en ophalen vanuit MySQL-database
description: Zelfstudie met meerdere onderdelen om u door de stappen te laten lopen die nodig zijn voor het opslaan en ophalen van formuliergegevens
feature: Adaptieve Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Ontwikkeling
role: Developer
level: Ervaren
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 1%

---


# Deze op uw server implementeren

>[!NOTE]
>
>Hieronder vindt u de volgende vereisten om dit op uw systeem uit te voeren
>
>* AEM Forms (versie 6.3 of hoger)
>* MySql-database


Voer de volgende stappen uit om deze mogelijkheid te testen op uw AEM Forms-exemplaar

* Download en implementeer de [MySql Driver Jar](assets/mysqldriver.jar)-bestanden met de [felix webconsole](http://localhost:4502/system/console/bundles)
* Download en implementeer de [OSGi-bundel](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) met de [felix webconsole](http://localhost:4502/system/console/bundles)
* Download en installeer het [pakket met client lib, adaptieve formuliersjabloon en de aangepaste pagina component](assets/store-and-fetch-af-with-data.zip) met behulp van [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* Importeer het [voorbeeld adaptief formulier](assets/sample-adaptive-form.zip) met behulp van de interface [FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Importeer de [form-data-db.sql](assets/form-data-db.sql) met MySql Workbench. Dit zal tot het noodzakelijke schema en de lijsten in uw gegevensbestand voor dit leerprogramma leiden te werken.
* Meld u aan bij [configMgr.](http://localhost:4502/system/console/configMgr) Zoek naar &quot;Apache Sling Connection Pooled DataSource. Maak een nieuw item voor de gegevensbron van Apache Sling Connection met de naam **SaveAndContinue** met de volgende eigenschappen:

| Eigenschapnaam | Waarde |
------------------------|---------------------------------------
| Naam gegevensbron | Opslaan en doorgaan |
| JDBC-stuurprogramma, klasse | com.mysql.cj.jdbc.Driver |
| JDBC-verbindingsuri | jdbc:mysql://localhost:3306/aemformstutorial |


* Open het [Aangepaste formulier](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Vul enkele details in en klik op de knop &quot;Opslaan en verdergaan&quot;.
* U zou terug een URL met een GUID in het moeten krijgen.
* Kopieer de URL en plak deze in een nieuw browsertabblad. **Zorg ervoor dat er geen lege ruimte aan het einde van de URL is.**
* Het adaptieve formulier moet worden gevuld met de gegevens uit de vorige stap.
