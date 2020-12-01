---
title: Triggerwerkstroom AEM HTML5-formulierverzending
seo-title: Trigger AEM workflow voor het verzenden van HTML5-formulieren
description: Mobiel formulier blijven invullen in offline modus en mobiel formulier verzenden om AEM workflow te activeren
seo-description: Mobiel formulier blijven invullen in offline modus en mobiel formulier verzenden om AEM workflow te activeren
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---


# Gebruiksscenario&#39;s voor dit gebruik aan uw systeem laten werken

>[!NOTE]
>
>De voorbeeldmiddelen werken alleen op uw systeem als AEM Author and Publish-instantie wordt uitgevoerd op respectievelijk poort 4502 en 4503. Er wordt ook aangenomen dat de AEM-auteur toegankelijk is via `admin`/`admin`. Als de poortnummers of het beheerwachtwoord zijn gewijzigd, werken deze voorbeeldbestanden niet. U moet uw eigen elementen maken met behulp van de opgegeven voorbeeldcode.

Ga als volgt te werk om deze kwestie van het gebruik op uw lokale systeem te laten werken:

* AEM-auteur-instantie installeren op poort 4502 en AEM-publicatie-instantie op poort 4503
* [Volg de instructies die u opgeeft bij het ontwikkelen met servicegebruikers in AEM Forms](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). Zorg ervoor dat u de servicegebruiker maakt en implementeer de bundel op uw AEM-auteur- en -publicatieexemplaar.
* [Open de osgi-configuratie  ](http://localhost:4503/system/console/configMgr).
* Zoeken naar **filter Apache Sling Referrer**. Controleer of het selectievakje Lege waarden toestaan is ingeschakeld.
* [Implementeer de aangepaste AEMFormDocumentService-bundel](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Deze bundel moet worden geïmplementeerd op uw AEM-publicatie-instantie. Deze bundel bevat de code voor het genereren van een interactieve PDF van een mobiel formulier.
* [Download en decomprimeer de middelen die aan dit artikel zijn gekoppeld.](assets/offline-pdf-submission-assets.zip) U krijgt het volgende:
   * **offline-submit-profile.zip**  - Dit AEM pakket bevat het douaneprofiel dat u toestaat om interactieve pdf aan uw lokaal dossiersysteem te downloaden. Implementeer dit pakket op uw AEM-publicatieexemplaar.
   * **xdp-form-and-workflow.zip** - Dit AEM pakket bevat XDP, voorbeeldwerkstroom,lanceerinrichting geconfigureerd op knoopinhoud/pdfsubmission. Implementeer dit pakket op zowel uw AEM-auteur- als -publicatieexemplaar.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar**  - Dit is de AEM bundel die het grootste deel van het werk doet. Deze bundel bevat de servlet die op `/bin/startworkflow` wordt gemonteerd. Deze server slaat de verzonden formuliergegevens op onder het knooppunt `/content/pdfsubmissions` in AEM opslagplaats. Implementeer deze bundel op zowel uw AEM-auteur- als -publicatieexemplaar.
* [Een voorbeeld van het mobiele formulier bekijken](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Vul verschillende velden in en klik op de knop op de werkbalk om de interactieve PDF te downloaden.
* Vul de gedownloade PDF in met Acrobat en klik op Verzenden.
* Je moet een succesbericht krijgen
* Aanmelden bij instantie AEM-auteur als beheerder
* [De AEM inbox controleren](http://localhost:4502/aem/inbox)
* U moet een werkitem hebben om de verzonden PDF te reviseren

>[!NOTE]
>
>In plaats van de PDF naar servlet te verzenden die wordt uitgevoerd op het publicatieexemplaar, hebben sommige klanten de servlet geïmplementeerd in servlet-container zoals Tomcat. Het hangt allemaal van de topologie af de klant met.for het doel van dit leerprogramma vertrouwd is wij servlet gebruiken die bij wordt opgesteld om instantie te publiceren voor de behandeling van pdf- voorlegging wordt opgesteld.

