---
title: Test de Forms-assemblageoplossing
description: Voer de ExecuteAssemblerService.java uit om de oplossing te testen
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
duration: 55
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# Eclipproject importeren

* Download en unzip het [ zip dossier ](./assets/pdf-manipulation.zip)
* Eclipse starten en het project importeren in Eclipse
* Het project bevat de volgende mappen in de bronnenmap:
   * ddxFiles - Deze map bevat het ddx-bestand om de uitvoer te beschrijven die u wilt genereren
   * pdffiles - Deze map bevat de PDF-bestanden die u wilt samenstellen en pdf-bestanden om PDFA-hulpprogramma&#39;s te testen
   * credentials - Deze map bevat het bestand pdfa-options.json

![ middelen-dossier ](./assets/resources.png)

## PDF-bestanden samenstellen testen

* Kopieer en plak uw de dienstgeloofsbrieven in het dienst_token.json middeldossier in het project.
* Open het bestand AssemblePDFFiles.java en geef de map op waarin u de gegenereerde PDF-bestanden wilt opslaan
* Open ExecuteAssemblerService.java. Plaats de waarde van veranderlijke _AEM_FORMS_CS_ aan punt aan uw instantie.
* Verwijder de commentaarmarkering van de desbetreffende regels om het samenvoegen van twee of meer PDF-bestanden te testen
* Voer de toepassing ExecuteAssemblerService.java uit als Java

### PDFA-hulpprogramma&#39;s testen

* Kopieer en plak uw de dienstgeloofsbrieven in het dienst_token.json middeldossier in het project.
* Open het bestand PDFAUtilities.java en geef de map op waarin u de gegenereerde PDF-bestanden wilt opslaan.
* Open ExecuteAssemblerService.java. Plaats de waarde van veranderlijke _AEM_FORMS_CS_ aan punt aan uw instantie.
* Verwijder de commentaarmarkering van de desbetreffende regels om PDFA-bewerkingen te testen.
* Voer de toepassing ExecuteAssemblerService.java uit als Java.



>[!NOTE]
> De allereerste keer dat u het Java-programma uitvoert, wordt een HTTP 403-fout gegenereerd. Om voorbij dit te krijgen zorg u de [ aangewezen toestemmingen aan de technische rekeningsgebruiker in AEM ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem) geeft.

**de Gebruikers van AEM Forms** is de rol ik voor deze cursus heb gebruikt.
