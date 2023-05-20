---
title: De AEM van de tewerkstelling bij de verzending van HTML5-formulieren - Gebruiksscenario's aan het werk krijgen
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Mobiel formulier blijven invullen in offline modus en mobiel formulier verzenden om AEM workflow te activeren
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 0%

---

# Gebruiksscenario&#39;s voor dit gebruik aan uw systeem laten werken

>[!NOTE]
>
>De voorbeeldmiddelen werken alleen op uw systeem als AEM Author and Publish-instantie wordt uitgevoerd op respectievelijk poort 4502 en 4503. Er wordt ook aangenomen dat de AEM-auteur toegankelijk is via `admin`/`admin`. Als de poortnummers of het beheerwachtwoord zijn gewijzigd, werken deze voorbeeldbestanden niet. U moet uw eigen elementen maken met behulp van de opgegeven voorbeeldcode.

Ga als volgt te werk om deze kwestie van het gebruik op uw lokale systeem te laten werken:

* AEM-auteur-instantie installeren op poort 4502 en AEM-publicatie-instantie op poort 4503
* [Volg de instructies die zijn opgegeven bij het ontwikkelen met een servicegebruiker in AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). Zorg ervoor dat u de servicegebruiker maakt en implementeer de bundel op uw AEM-auteur- en -publicatieexemplaar.
* [De osgi-configuratie openen ](http://localhost:4503/system/console/configMgr).
* Zoeken naar  **Filter Apache Sling Referrer**. Controleer of het selectievakje Lege waarden toestaan is ingeschakeld.
* [De aangepaste AEMFormDocumentService-bundel implementeren](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).Deze bundel moet op uw publicatie-instantie AEM worden opgesteld. Deze bundel bevat de code voor het genereren van interactieve PDF van een mobiel formulier.
* [Download en decomprimeer de middelen die aan dit artikel zijn gekoppeld.](assets/offline-pdf-submission-assets.zip) U krijgt het volgende:
   * **offline-verzending-profile.zip** - Dit AEM pakket bevat het aangepaste profiel waarmee u de interactieve PDF naar uw lokale bestandssysteem kunt downloaden. Implementeer dit pakket op uw AEM-publicatieexemplaar.
   * **xdp-form-and-workflow.zip** - Dit AEM pakket bevat XDP, voorbeeldwerkstroom, draagprogramma geconfigureerd op knoopinhoud/pdfsubmission. Implementeer dit pakket op zowel uw AEM-auteur- als -publicatieexemplaar.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar** - Dit is de AEM bundel die het grootste deel van het werk doet. Deze bundel bevat de servlet die op wordt gemonteerd `/bin/startworkflow`. Deze server slaat de ingediende formuliergegevens op onder `/content/pdfsubmissions` knooppunt in AEM repository. Implementeer deze bundel op zowel uw AEM-auteur- als -publicatieexemplaar.
* [Een voorbeeld van het mobiele formulier bekijken](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Vul verschillende velden in en klik op de knop op de werkbalk om de interactieve PDF te downloaden.
* Vul de gedownloade PDF in met Acrobat en klik op Verzenden.
* Je moet een succesbericht krijgen
* Aanmelden bij instantie AEM-auteur als beheerder
* [De AEM inbox controleren](http://localhost:4502/aem/inbox)
* Je moet een werkitem hebben om de verzonden PDF te bekijken

>[!NOTE]
>
>In plaats van de PDF naar servlet te verzenden die op publicatieexemplaar loopt, hebben sommige klanten servlet in servlet container zoals Tomcat opgesteld. Het hangt allemaal van de topologie af de klant met.for het doel van dit leerprogramma vertrouwd is wij servlet gebruiken die bij wordt opgesteld om instantie te publiceren voor de behandeling van pdf- voorlegging wordt opgesteld.
