---
title: Workflowopmerkingen vastleggen in Adaptive Forms Workflow
description: Workflowopmerkingen vastleggen in AEM workflow
feature: Workflow
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '380'
ht-degree: 0%

---

# Workflowopmerkingen vastleggen in Adaptive Forms Workflow{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[Alleen van toepassing op AEM Forms 6.4. Gebruik in AEM Forms 6.5 de functie Variabelen om dit geval te gebruiken]

Een veelvoorkomend verzoek is om de opmerkingen die de taakcontroleur heeft ingevoerd, in een e-mail op te nemen. In AEM Forms 6.4 is er geen mechanisme buiten het vak om de gebruiker in te voeren opmerkingen en deze opmerkingen in e-mail op te nemen.

Om aan dit vereiste te voldoen, wordt een bundel van steekproef OSGi verstrekt die kan worden gebruikt om commentaren vast te leggen en deze commentaren op te slaan als werkschemabezit.

In de volgende schermafbeelding ziet u hoe u processtap in [AEM](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) om opmerkingen vast te leggen en op te slaan als eigenschap metadata. De &quot;Commentaar van het Werkschema van de Vangst&quot;is de naam van de klasse java die in de processtap moet worden gebruikt. U moet de naam van de metagegevenseigenschap doorgeven die de opmerkingen bevat. In de onderstaande schermafbeelding is managerComments de eigenschap metadata waarin de opmerkingen worden opgeslagen.

![workflowcomments1](assets/workflowcomments1.gif)

Voer de volgende stappen uit om deze mogelijkheid op uw systeem te testen:
* [Zorg ervoor dat de processtap in de workflow zodanig is geconfigureerd dat de Opmerkingen van de workflow voor vastleggen worden gebruikt](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [De gebruikersbundel DevelopingWithService implementeren](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [De SetValue-bundel implementeren](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Deze bundel bevat de voorbeeldcode voor het vastleggen van de opmerkingen en het opslaan als een eigenschap voor metagegevens

* [De aan dit artikel gerelateerde elementen downloaden en uitpakken naar uw bestandssysteem](assets/capturecomments.zip) De elementen bevatten workflowmodel en voorbeeld Adaptief formulier.

* De 2 zip-bestanden met pakketbeheer in AEM importeren

* [Bekijk een voorbeeld van het formulier door naar deze URL te bladeren](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* De formuliervelden invullen en het formulier verzenden

* [Controleer uw AEM](http://localhost:4502/aem/inbox)

* Open de taak vanuit het Postvak IN en verzend het formulier. Voer opmerkingen in wanneer hierom wordt gevraagd.

De opmerkingen worden in crx opgeslagen in de metagegevenseigenschap met de naam managerComments. Als u wilt controleren op de opmerkingen, meldt u zich aan bij crx als beheerder. De workflowinstanties worden opgeslagen in het volgende pad

/var/workflow/instances/server0

Selecteer de aangewezen werkschemainstantie en controleer bezitsmanagerComments in de meta-gegevensknoop.
