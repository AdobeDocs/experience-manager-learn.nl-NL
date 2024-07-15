---
title: De AEM van de tewerkstelling bij de verzending van HTML5-formulieren - Gebruiksscenario's worden gebruikt
description: Mobiel formulier blijven invullen in offline modus en mobiel formulier verzenden om AEM workflow te activeren
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
duration: 90
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 0%

---

# Gebruiksscenario&#39;s voor dit gebruik aan uw systeem laten werken

>[!NOTE]
>
>De voorbeeldmiddelen werken alleen op uw systeem als u AEM Author- en Publish-instantie hebt uitgevoerd op poort 4502 respectievelijk 4503. Er wordt ook aangenomen dat de AEM Auteur toegankelijk is via `admin`/`admin` . Als de poortnummers of het beheerwachtwoord zijn gewijzigd, werken deze voorbeeldbestanden niet. U moet uw eigen elementen maken met behulp van de opgegeven voorbeeldcode.

Ga als volgt te werk om deze kwestie van het gebruik op uw lokale systeem te laten werken:

* Installeer AEM Author-instantie op poort 4502 en AEM Publish-instantie op poort 4503
* [ volg de instructies die in het ontwikkelen met de dienstgebruiker in AEM Forms ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html) worden gespecificeerd. Maak de servicegebruiker en implementeer de bundel op de AEM Author en Publish-instantie.
* [ open de configuratie van osgi ](http://localhost:4503/system/console/configMgr).
* Onderzoek naar **Apache het Verdelen Filter van de Verwijzing**. Controleer of het selectievakje Lege waarden toestaan is ingeschakeld.
* [ stelt de Bundle van de douaneAEMFormDocumentService ](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar) op.Deze bundel moet op uw AEM instantie van Publish worden opgesteld. Deze bundel heeft de code om interactieve PDF van mobiel formulier te produceren.
* [ Download en unzip de activa met betrekking tot dit artikel.](assets/offline-pdf-submission-assets.zip) U krijgt het volgende:
   * **offline-voorlegging-profile.zip** - Dit AEM pakket bevat het douaneprofiel dat u toestaat om interactieve pdf aan uw lokaal dossiersysteem te downloaden. Implementeer dit pakket op uw AEM Publish-exemplaar.
   * **xdp-form-and-workflow.zip** - Dit AEM pakket bevat XDP, steekproefwerkschema, lanceerinrichting die op knoopinhoud/pdfsubmission wordt gevormd. Implementeer dit pakket op zowel de AEM Auteur als de Publish-instantie.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar** - dit is de AEM bundel die het grootste deel van het werk doet. Deze bundel bevat de servlet die op `/bin/startworkflow` is gemonteerd. Deze server slaat de verzonden formuliergegevens op onder het knooppunt `/content/pdfsubmissions` in AEM gegevensopslagruimte. Implementeer deze bundel op zowel de AEM Auteur als de Publish-instantie.
* [ Voorproef de mobiele vorm ](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Vul verschillende velden in en klik op de knop op de werkbalk om de interactieve PDF te downloaden.
* Vul de gedownloade PDF in met Acrobat en klik op Verzenden.
* Je moet een succesbericht krijgen
* Aanmelden bij AEM instantie Auteur als beheerder
* [ Controle de AEM Inbox ](http://localhost:4502/aem/inbox)
* Je moet een werkitem hebben om de verzonden PDF te bekijken

>[!NOTE]
>
>In plaats van de PDF naar servlet te verzenden die op publicatieexemplaar loopt, hebben sommige klanten servlet in servlet container zoals Tomcat opgesteld. Het hangt allemaal van de topologie af de klant met.for het doel van dit leerprogramma vertrouwd is wij servlet gebruiken die bij wordt opgesteld om instantie te publiceren voor de behandeling van pdf- voorlegging wordt opgesteld.
