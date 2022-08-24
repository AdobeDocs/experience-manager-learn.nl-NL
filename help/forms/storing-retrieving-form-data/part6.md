---
title: Opslaan en ophalen van formuliergegevens uit MySQL-database - Implementeren
description: Zelfstudie met meerdere onderdelen om u door de stappen te laten lopen die nodig zijn voor het opslaan en ophalen van formuliergegevens
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: 6.4,6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# Deze op uw server implementeren

>[!NOTE]
>
>Hieronder vindt u de volgende vereisten om dit op uw systeem uit te voeren
>
>* AEM Forms (versie 6.3 of hoger)
>* MySql-database


Voer de volgende stappen uit om deze mogelijkheid te testen op uw AEM Forms-exemplaar

* Download en implementeer de [MySql Driver Jar](assets/mysqldriver.jar) bestanden met de [felix-webconsole](http://localhost:4502/system/console/bundles)
* Download en implementeer de [OSGi-bundel](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) met de [felix-webconsole](http://localhost:4502/system/console/bundles)
* Download en installeer de [pakket met client-lib, adaptieve formuliersjabloon en de aangepaste pagina-component](assets/store-and-fetch-af-with-data.zip) met de [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* Het dialoogvenster Importeren [Adaptief voorbeeldformulier](assets/sample-adaptive-form.zip) met de [Interface FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Het dialoogvenster Importeren [form-data-db.sql](assets/form-data-db.sql) gebruiken van MySql Workbench. Dit zal tot het noodzakelijke schema en de lijsten in uw gegevensbestand voor dit leerprogramma leiden te werken.
* Aanmelden bij [configMgr.](http://localhost:4502/system/console/configMgr) Zoek naar &quot;Apache Sling Connection Pooled DataSource. Maak een nieuw genaamd Apache Sling Connection Pooled Datasource-item **Opslaan en doorgaan** de volgende eigenschappen gebruiken:

| Eigenschapnaam | Waarde |
| ------------------------|---------------------------------------|
| Naam gegevensbron | Opslaan en doorgaan |
| JDBC-stuurprogramma, klasse | com.mysql.cj.jdbc.Driver |
| JDBC-verbindingsuri | jdbc:mysql://localhost:3306/aemformstutorial |

* Open de [Adaptief formulier](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Vul enkele details in en klik op de knop &quot;Opslaan en verdergaan&quot;.
* U zou terug een URL met een GUID in het moeten krijgen.
* Kopieer de URL en plak deze in een nieuw browsertabblad. **Zorg ervoor dat er geen lege ruimte aan het einde van de URL is.**
* Het adaptieve formulier moet worden gevuld met de gegevens uit de vorige stap.
