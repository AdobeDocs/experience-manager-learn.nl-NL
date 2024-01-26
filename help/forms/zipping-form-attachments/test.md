---
title: Test de oplossing - Stappen die nodig zijn om de twee benaderingen te testen
description: Test de oplossing door bijlagen aan het formulier toe te voegen en activeer de workflow om de e-mail te verzenden.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: ce9b9203-b44c-4a52-821c-ae76e93312d2
duration: 53
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---

# Benodigde stappen om de twee naderingen te testen

* Configureren [Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) om e-mails te verzenden vanaf de AEM Forms-server
* Implementeer de [bijlagen](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) bundelen met [felix-webconsole](http://localhost:4502/system/console/bundles)

## ZIP-bestand verzenden als e-mailbijlage



* Implementeer de [Workflow SendFormAttachmentsViaEmail.](assets/zipped-form-attachments-model.zip) Deze workflow gebruikt de component send email om het bestand zipped_attachments.zip te verzenden. Dit bestand wordt door de stap custom process opgeslagen in de map payload. Configureer de e-mailadressen van de afzenders en de ontvangers naar wens.
* Het dialoogvenster Importeren [voorbeeldformulier](assets/zip-form-attachments-form.zip) van [UI Forms en Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Een voorbeeld van het formulier bekijken](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) en voeg een aantal bijlagen toe en verzend het formulier.
* De workflow moet worden geactiveerd en er moet een e-mailmelding met het ZIP-bestand worden verzonden.

## Formulierbijlagen verzenden als afzonderlijke bestanden

* Implementeer de [Workflow SendForm.](assets/send-form-attachments-model.zip) In deze workflow wordt e-mailcomponent verzonden om de formulierbijlagen als afzonderlijke bestanden te verzenden. Configureer het e-mailadres van de afzenders en ontvangers naar wens.
* Het dialoogvenster Importeren [voorbeeldformulier](assets/send-list-attachments-form.zip) van [UI Forms en Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Een voorbeeld van het formulier bekijken](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) en voeg een aantal bijlagen toe en verzend het formulier.
* De workflow moet worden geactiveerd en er moet een e-mailmelding met de formulierbijlagen worden verzonden.
