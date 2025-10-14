---
title: Test de oplossing - Stappen die nodig zijn om de twee benaderingen te testen
description: Test de oplossing door bijlagen aan het formulier toe te voegen en activeer de workflow om de e-mail te verzenden.
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: ce9b9203-b44c-4a52-821c-ae76e93312d2
duration: 41
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---

# Benodigde stappen om de twee naderingen te testen

* Vorm [&#128279;](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=nl-NL#configuring-the-mail-service) de Dienst van de Post van 0&rbrace; Dag CQ om e-mail van de server van AEM Forms te verzenden
* Stel de [&#x200B; formattachments &#x200B;](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) bundel op gebruikend [&#x200B; felix Webconsole &#x200B;](http://localhost:4502/system/console/bundles)

## ZIP-bestand verzenden als e-mailbijlage



* Stel het [&#x200B; SendFormAttachmentsViaEmail werkschema op.](assets/zipped-form-attachments-model.zip) In deze workflow wordt de component send email gebruikt om het bestand zipped_attachments.zip te verzenden. Dit bestand wordt door de stap custom process opgeslagen in de map payload. Configureer de e-mailadressen van de afzenders en de ontvangers naar wens.
* Invoer de [&#x200B; steekproefvorm &#x200B;](assets/zip-form-attachments-form.zip) van [&#x200B; Forms en Document UI &#x200B;](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [&#x200B; voorproef de vorm &#x200B;](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) en voeg een paar gehechtheid toe en verzend de vorm.
* De workflow moet worden geactiveerd en er moet een e-mailmelding met het ZIP-bestand worden verzonden.

## Formulierbijlagen verzenden als afzonderlijke bestanden

* Stel het [&#x200B; werkschema SendForm op.](assets/send-form-attachments-model.zip) In deze werkstroom wordt het onderdeel E-mail verzenden gebruikt om de formulierbijlagen als afzonderlijke bestanden te verzenden. Configureer het e-mailadres van de afzenders en ontvangers naar wens.
* Invoer de [&#x200B; steekproefvorm &#x200B;](assets/send-list-attachments-form.zip) van [&#x200B; Forms en Document UI &#x200B;](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [&#x200B; voorproef de vorm &#x200B;](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) en voeg een paar gehechtheid toe en verzend de vorm.
* De workflow moet worden geactiveerd en er moet een e-mailmelding met de formulierbijlagen worden verzonden.
