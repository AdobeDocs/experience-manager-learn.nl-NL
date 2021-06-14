---
title: Aangepaste formulierbijlagen verzenden
description: Aangepaste formulierbijlagen verzenden met e-mailcomponent
feature: adaptieve formulieren
topics: adaptive forms
audience: developer
doc-type: article
activity: setup
version: 6.5
topic: Ontwikkeling
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: 540e11c0861eacc795122328b2359c7db6378aec
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 1%

---


# Inleiding



De gebruikelijke methode is om de adaptieve formulierbijlagen te verzenden met de functie E-mail verzenden in een AEM workflow.
Klanten zouden de formulierbijlagen meestal comprimeren of de bijlagen als afzonderlijke bestanden verzenden met de component E-mail verzenden.

## De formulierbijlagen verzenden in een ZIP-bestand

Om het gebruik te verwezenlijken werd een stap van het douanewerkschemaproces geschreven. In deze aangepaste processtap wordt een ZIP-bestand met de formulierbijlagen gemaakt en opgeslagen in de map payload in een bestand met de naam *zipped_attachments.zip*

![send-form-attachments](assets/send-form-attachments.JPG)

## De formulierbijlagen afzonderlijk verzenden

Voor dit doel is een stap voor een aangepast workflowproces geschreven. In deze stap van het douaneproces bevolken wij werkschemariabelen van type ArrayList van Documenten en ArrayList van Koorden.

![send-list-of-documents](assets/send-list-of-documents.JPG)



