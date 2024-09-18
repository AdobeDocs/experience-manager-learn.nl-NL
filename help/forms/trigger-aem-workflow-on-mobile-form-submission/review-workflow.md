---
title: Trigger AEM workflow voor het verzenden van HTML5-formulieren - PDF controleren en goedkeuren
description: Workflow om de ingediende PDF te beoordelen
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: 5f42678502a785ead29982044d1f3f5ecf023e0f
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 0%

---

# Workflow voor het beoordelen en goedkeuren van de ingediende PDF

De laatste en laatste stap bestaat uit het maken van AEM workflow die een statische of niet-interactieve PDF voor controle en goedkeuring genereert. De workflow wordt geactiveerd via een AEM Launcher die op het knooppunt `/content/formsubmissions` is geconfigureerd.

In de volgende schermafbeelding worden de stappen weergegeven die bij de workflow horen.

![ werkschema ](assets/workflow.PNG)

## Niet-interactieve stap voor PDF-workflow genereren

Hier worden de XDP-sjabloon en de gegevens die met de sjabloon moeten worden samengevoegd, opgegeven. De gegevens die moeten worden samengevoegd, zijn de verzonden gegevens van de PDF. Deze verzonden gegevens worden opgeslagen onder het knooppunt ```/content/formsubmissions```

![ werkschema ](assets/generate-pdf1.PNG)

De gegenereerde PDF wordt toegewezen aan de werkstroomvariabele `submittedPDF` .

![ werkschema ](assets/generate-pdf2.PNG)

### De gegenereerde pdf ter controle en goedkeuring toewijzen

De taakwerkstroomcomponent toewijzen wordt hier gebruikt om de gegenereerde PDF toe te wijzen voor revisie en goedkeuring. De variabele `submittedPDF` wordt gebruikt op het tabblad Forms en Documenten van de werkstroomcomponent Taak toewijzen.

![ werkschema ](assets/assign-task.PNG)


## Volgende stappen

[De middelen in uw omgeving implementeren](./deploy-assets.md)