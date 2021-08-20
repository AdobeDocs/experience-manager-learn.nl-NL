---
title: De oplossing testen
description: Test de oplossing door bijlagen aan het formulier toe te voegen en activeer de workflow om de e-mail te verzenden.
feature: Adaptieve Forms
version: 6.5
topic: Ontwikkeling
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---


# Benodigde stappen om de twee naderingen te testen

* Configureer [Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) om e-mails te verzenden vanaf de AEM Forms-server
* Implementeer de [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar)-bundel met behulp van [felix webconsole](http://localhost:4502/system/console/bundles)

## ZIP-bestand verzenden als e-mailbijlage



* Implementeer de [SendFormAttachmentsViaEmail-workflow.](assets/zipped-form-attachments-model.zip) Deze workflow gebruikt de component send email om het bestand zipped_attachments.zip te verzenden. Dit bestand wordt door de stap custom process opgeslagen in de map payload. Configureer de e-mailadressen van de afzenders en de ontvangers naar wens.
* Importeer het [voorbeeldformulier](assets/zip-form-attachments-form.zip) uit [Forms en Document UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Geef een voorbeeld van het ](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) formulier weer en voeg enkele bijlagen toe en verzend het formulier.
* De workflow moet worden geactiveerd en er moet een e-mailmelding met het ZIP-bestand worden verzonden.

## Formulierbijlagen verzenden als afzonderlijke bestanden

* Implementeer de [SendForm-workflow.](assets/send-form-attachments-model.zip) In deze workflow wordt e-mailcomponent verzonden om de formulierbijlagen als afzonderlijke bestanden te verzenden. Configureer het e-mailadres van de afzenders en ontvangers naar wens.
* Importeer het [voorbeeldformulier](assets/send-list-attachments-form.zip) uit [Forms en Document UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Geef een voorbeeld van het ](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) formulier weer en voeg enkele bijlagen toe en verzend het formulier.
* De workflow moet worden geactiveerd en er wordt een e-mailbericht met de formulierbijlagen verzonden.
