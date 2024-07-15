---
title: Workflowopmerkingen vastleggen in Adaptive Forms Workflow
description: Workflowopmerkingen vastleggen in AEM workflow
feature: Workflow
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
last-substantial-update: 2020-10-10T00:00:00Z
duration: 73
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---

# Workflowopmerkingen vastleggen in Adaptive Forms Workflow{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[ is slechts op AEM Forms 6.4 van toepassing. Gebruik in AEM Forms 6.5 de functie Variabelen om dit te gebruiken ]

Een veelvoorkomend verzoek is om de opmerkingen die de taakcontroleur heeft ingevoerd, in een e-mail op te nemen. In AEM Forms 6.4 is er geen mechanisme buiten het vak om de gebruiker in te voeren opmerkingen en deze opmerkingen in e-mail op te nemen.

Om aan dit vereiste te voldoen, wordt een bundel van steekproef OSGi verstrekt die kan worden gebruikt om commentaren vast te leggen en deze commentaren op te slaan als werkschemabezit.

Het volgende schermafbeelding toont u hoe te om processtap in [ AEM Werkschema ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) te gebruiken om commentaren te vangen en hen op te slaan als meta-gegevensbezit. De &quot;Commentaar van het Werkschema van de Vangst&quot;is de naam van de klasse java die in de processtap moet worden gebruikt. U moet de naam van de metagegevenseigenschap doorgeven die de opmerkingen bevat. In de onderstaande schermafbeelding is managerComments de eigenschap metadata waarin de opmerkingen worden opgeslagen.

![ workflowcomments1 ](assets/workflowcomments1.gif)

Voer de volgende stappen uit om deze mogelijkheid op uw systeem te testen:
* [ zorg ervoor de processtap in het werkschema wordt gevormd om de Commentaren van het Werkschema van de Vangst te gebruiken ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [De gebruikersbundel DevelopingWithService implementeren](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [ stelt de bundel SetValue ](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) op. Deze bundel bevat de voorbeeldcode voor het vastleggen van de opmerkingen en het opslaan als eigenschap metadata

* [ Download en unzip de activa met betrekking tot dit artikel op uw dossiersysteem ](assets/capturecomments.zip) De activa bevatten werkschemamodel en steekproef Aangepaste Vorm.

* De 2 zip-bestanden met pakketbeheer in AEM importeren

* [ Voorproef de vorm door aan dit URL te doorbladeren ](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* De formuliervelden invullen en het formulier verzenden

* [ Controle uw AEM inbox ](http://localhost:4502/aem/inbox)

* Open de taak vanuit het Postvak IN en verzend het formulier. Voer opmerkingen in wanneer hierom wordt gevraagd.

De opmerkingen worden opgeslagen in de eigenschap metadata met de naam `managerComments` in AEM opslagplaats. Als u wilt controleren op de opmerkingen, meldt u zich aan bij crx als beheerder. De workflowinstanties worden opgeslagen in het volgende pad:

`/var/workflow/instances/server0`

Selecteer de aangewezen werkschemainstantie en controleer bezitsmanagerComments in de meta-gegevensknoop.
