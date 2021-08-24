---
title: Triggerwerkstroom AEM HTML5-formulierverzending
seo-title: Trigger AEM workflow voor het verzenden van HTML5-formulieren
description: Mobiel formulier blijven invullen in offline modus en mobiel formulier verzenden om AEM workflow te activeren
seo-description: Mobiel formulier blijven invullen in offline modus en mobiel formulier verzenden om AEM workflow te activeren
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Ontwikkeling
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 0%

---


# Workflow voor het reviseren en goedkeuren van de verzonden PDF

De laatste en laatste stap bestaat uit het maken van AEM workflow die een statische of niet-interactieve PDF genereert voor revisie en goedkeuring. De workflow wordt geactiveerd via een AEM Launcher die op het knooppunt `/content/pdfsubmissions` is geconfigureerd.

In de volgende schermafbeelding worden de stappen weergegeven die bij de workflow horen.

![werkstroom](assets/workflow.PNG)

## Niet-interactieve PDF-werkstroomstap genereren

Hier worden de XDP-sjabloon en de gegevens die met de sjabloon moeten worden samengevoegd, opgegeven. De gegevens die moeten worden samengevoegd, zijn de verzonden gegevens uit de PDF. Deze verzonden gegevens worden opgeslagen onder het knooppunt `/content/pdfsubmissions`.

![werkstroom](assets/generate-pdf1.PNG)

De gegenereerde PDF wordt toegewezen aan de werkstroomvariabele `submittedPDF`.

![werkstroom](assets/generate-pdf2.PNG)

### De gegenereerde pdf ter controle en goedkeuring toewijzen

De taakworkflowcomponent toewijzen wordt hier gebruikt om het gegenereerde PDF-bestand toe te wijzen voor revisie en goedkeuring. De variabele `submittedPDF` wordt gebruikt op het lusje van Forms en van Documenten van de het werkschemacomponent van de Taak toewijzen.

![werkstroom](assets/assign-task.PNG)
