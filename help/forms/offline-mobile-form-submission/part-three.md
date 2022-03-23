---
title: Trigger AEM workflow voor HTML5-formulierverzending - PDF controleren en goedkeuren
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
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 0%

---

# Workflow voor het beoordelen en goedkeuren van de ingediende PDF

De laatste en laatste stap bestaat uit het maken van AEM workflow die een statische of niet-interactieve PDF voor controle en goedkeuring genereert. Workflow wordt geactiveerd via een AEM Launcher die op het knooppunt is geconfigureerd `/content/pdfsubmissions`.

In de volgende schermafbeelding worden de stappen weergegeven die bij de workflow horen.

![werkstroom](assets/workflow.PNG)

## Niet-interactieve stap voor PDF-workflow genereren

Hier worden de XDP-sjabloon en de gegevens die met de sjabloon moeten worden samengevoegd, opgegeven. De gegevens die moeten worden samengevoegd, zijn de verzonden gegevens van de PDF. Deze verzonden gegevens worden opgeslagen onder het knooppunt `/content/pdfsubmissions`.

![werkstroom](assets/generate-pdf1.PNG)

De gegenereerde PDF wordt toegewezen aan de workflowvariabele `submittedPDF`.

![werkstroom](assets/generate-pdf2.PNG)

### De gegenereerde pdf ter controle en goedkeuring toewijzen

De taakwerkstroomcomponent toewijzen wordt hier gebruikt om de gegenereerde PDF toe te wijzen voor revisie en goedkeuring. De variabele `submittedPDF` wordt gebruikt op het tabblad Forms en Documenten van de werkstroomcomponent Taak toewijzen.

![werkstroom](assets/assign-task.PNG)
