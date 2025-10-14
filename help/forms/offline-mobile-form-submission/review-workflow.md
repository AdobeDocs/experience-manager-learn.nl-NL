---
title: AEM-workflow activeren op HTML5-formulierverzending - PDF controleren en goedkeuren
description: Workflow voor het beoordelen van de verzonden PDF
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16133
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
duration: 32
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 0%

---

# Workflow voor het beoordelen en goedkeuren van de ingediende PDF

De laatste en laatste stap bestaat uit het maken van een AEM-workflow die een statische of niet-interactieve PDF genereert voor controle en goedkeuring. Workflow wordt geactiveerd via een AEM Launcher die op het knooppunt `/content/formsubmissions` is geconfigureerd.

In de volgende schermafbeelding worden de stappen weergegeven die bij de workflow horen.

![&#x200B; werkschema &#x200B;](assets/workflow.PNG)

## Niet-interactieve PDF-workflowstap genereren

Hier worden de XDP-sjabloon en de gegevens die met de sjabloon moeten worden samengevoegd, opgegeven. De gegevens die moeten worden samengevoegd, zijn de ingediende gegevens van de PDF. Deze verzonden gegevens worden opgeslagen onder het knooppunt ```/content/formsubmissions```

![&#x200B; werkschema &#x200B;](assets/generate-pdf1.PNG)

De gegenereerde PDF wordt toegewezen aan de workflowvariabele `submittedPDF` .

![&#x200B; werkschema &#x200B;](assets/generate-pdf2.PNG)

### De gegenereerde pdf ter controle en goedkeuring toewijzen

De taakworkflowcomponent toewijzen wordt hier gebruikt om de gegenereerde PDF toe te wijzen voor revisie en goedkeuring. De variabele `submittedPDF` wordt gebruikt op het tabblad Forms en Documenten van de werkstroomcomponent Taak toewijzen.

![&#x200B; werkschema &#x200B;](assets/assign-task.PNG)


## Volgende stappen

[De middelen in uw omgeving implementeren](./deploy-assets.md)