---
title: Aangepaste formulierbijlagen verzenden
description: Aangepaste formulierbijlagen verzenden met e-mailcomponent
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: bd9e1fc1-2fc7-452c-9a4a-2e16f6821760
last-substantial-update: 2020-07-07T00:00:00Z
duration: 28
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%

---

# Inleiding



De gebruikelijke methode is om de adaptieve formulierbijlagen te verzenden met de functie E-mail verzenden in een AEM workflow.
Klanten zouden de formulierbijlagen meestal comprimeren of de bijlagen als afzonderlijke bestanden verzenden met de component E-mail verzenden.

## De formulierbijlagen verzenden in een ZIP-bestand

Om het gebruik te verwezenlijken werd een stap van het douanewerkschemaproces geschreven. In deze stap van het douaneproces een gecomprimeerd dossier met de vormgehechtheid in gecreeerd en opgeslagen onder de ladingsomslag in een dossier genoemd *zipped_attachments.zip*

![ verzenden-vorm-gehechtheid ](assets/send-form-attachments.JPG)

## De formulierbijlagen afzonderlijk verzenden

Voor dit doel is een stap voor een aangepast workflowproces geschreven. In deze stap van het douaneproces bevolken wij werkschemariabelen van type ArrayList van Documenten en ArrayList van Koorden.

![ send-list-of-documents ](assets/send-list-of-documents.JPG)

## Volgende stappen

[Formulierbijlagen comprimeren](./custom-process-step.md)
