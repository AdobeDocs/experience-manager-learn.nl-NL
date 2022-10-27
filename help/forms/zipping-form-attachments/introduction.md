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
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# Inleiding



De gebruikelijke methode is om de adaptieve formulierbijlagen te verzenden met de functie E-mail verzenden in een AEM workflow.
Klanten zouden de formulierbijlagen meestal comprimeren of de bijlagen als afzonderlijke bestanden verzenden met de component E-mail verzenden.

## De formulierbijlagen verzenden in een ZIP-bestand

Om het gebruik te verwezenlijken werd een stap van het douanewerkschemaproces geschreven. In deze aangepaste processtap wordt een ZIP-bestand met de formulierbijlagen gemaakt en opgeslagen in de payload-map in een bestand met de naam *zipped_attachments.zip*

![send-form-attachments](assets/send-form-attachments.JPG)

## De formulierbijlagen afzonderlijk verzenden

Voor dit doel is een stap voor een aangepast workflowproces geschreven. In deze stap van het douaneproces bevolken wij werkschemariabelen van type ArrayList van Documenten en ArrayList van Koorden.

![send-list-of-documents](assets/send-list-of-documents.JPG)
