---
title: Opslaan en ophalen van formuliergegevens uit MySQL-database - Implementeren
description: Zelfstudie met meerdere onderdelen om door de stappen te bladeren die nodig zijn voor het opslaan en ophalen van formuliergegevens
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: Experience Manager 6.4, Experience Manager 6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
duration: 47
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# Deze op uw server implementeren

>[!NOTE]
>
>Hieronder vindt u de volgende vereisten om dit op uw systeem uit te voeren
>
>* AEM Forms (versie 6.3 of hoger)
>* MySQL-database

Voer de volgende stappen uit om deze mogelijkheid te testen op uw AEM Forms-exemplaar

* Download en stel de [&#x200B; Jar van de Bestuurder MySql &#x200B;](assets/mysqldriver.jar) dossiers op gebruikend de [&#x200B; felix Webconsole &#x200B;](http://localhost:4502/system/console/bundles)
* Download en stel de [&#x200B; bundel OSGi &#x200B;](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) op gebruikend de [&#x200B; felix Webconsole &#x200B;](http://localhost:4502/system/console/bundles)
* Download en installeer het [&#x200B; pakket dat cliëntlib, adaptief vormmalplaatje, en de component van de douanepagina &#x200B;](assets/store-and-fetch-af-with-data.zip) bevat gebruikend de [&#x200B; pakketmanager &#x200B;](http://localhost:4502/crx/packmgr/index.jsp)
* Invoer de [&#x200B; steekproef Aangepaste vorm &#x200B;](assets/sample-adaptive-form.zip) gebruikend de [&#x200B; interface FormsAndDocuments &#x200B;](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Importeer [&#x200B; vorm-gegeven-db.sql &#x200B;](assets/form-data-db.sql) gebruikend MySql Workbench. Dit zal tot het noodzakelijke schema en de lijsten in uw gegevensbestand voor dit leerprogramma leiden te werken.
* Login aan [&#x200B; configMgr.](http://localhost:4502/system/console/configMgr) Zoek naar &quot;Apache Sling Connection Pooled DataSource. Creeer een nieuwe Apache het Schelen Verbinding Gepoolde ingang DataSource genoemd **SaveAndContinue** gebruikend de volgende eigenschappen:

| Eigenschapnaam | Waarde |
| ------------------------|---------------------------------------|
| Naam gegevensbron | `SaveAndContinue` |
| JDBC-stuurprogramma, klasse | `com.mysql.cj.jdbc.Driver` |
| JDBC-verbindingsuri | `jdbc:mysql://localhost:3306/aemformstutorial` |

* Open de [&#x200B; Aangepaste Vorm &#x200B;](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Vul enkele details in en klik op de knop &quot;Opslaan en verdergaan&quot;.
* U zou terug een URL met een GUID in het moeten krijgen.
* Kopieer de URL en plak deze in een nieuw browsertabblad. **zorg ervoor er geen lege ruimte aan het eind van URL zijn.**
* Het adaptieve formulier moet worden gevuld met de gegevens uit de vorige stap.
