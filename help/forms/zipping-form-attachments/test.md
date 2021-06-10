---
title: De oplossing testen
description: Test de oplossing door bijlagen aan het formulier toe te voegen en activeer de workflow om de e-mail te verzenden.
sub-product: formulieren
feature: Workflow
topics: adaptive forms
audience: developer
doc-type: article
activity: develop
version: 6.5
topic: Ontwikkeling
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: e82cc5e5de6db33e82b7c71c73bb606f16b98ea6
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 1%

---


# De oplossing testen


* Configureer [Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) om e-mails te verzenden vanaf de AEM Forms-server
* Implementeer de [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar)-bundel met behulp van [felix webconsole](http://localhost:4502/system/console/bundles)
* Implementeer de [SendFormAttachmentsViaEmail-workflow.](assets/zipped-form-attachments-model.zip) In deze workflow wordt een e-mailcomponent verzonden om het bestand zipped_attachments.zip te verzenden dat met de stap voor het aangepaste proces in de map payload wordt opgeslagen. Configureer het e-mailadres van de afzenders en ontvangers naar wens.
* Importeer het [voorbeeldformulier](assets/zip-form-attachments-form.zip) uit [Forms en Document UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Geef een voorbeeld van het ](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) formulier weer en voeg enkele bijlagen toe en verzend het formulier.
* De workflow moet worden geactiveerd en er moet een e-mailmelding met het ZIP-bestand worden verzonden.

