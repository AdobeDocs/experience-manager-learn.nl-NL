---
title: Workflowopmerkingen vastleggen in Adaptive Forms Workflow
seo-title: Workflowopmerkingen vastleggen in Adaptive Forms Workflow
description: Workflowopmerkingen vastleggen in AEM workflow
seo-description: Workflowopmerkingen vastleggen in AEM workflow
uuid: df41fc6f-9abf-47b4-a014-b3b9fb58b6f7
feature: Workflow
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4
discoiquuid: d5e40650-3b1f-4875-91b2-e22d932b5e7c
topic: Ontwikkeling
role: Developer
level: Ervaren
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 1%

---


# Workflowopmerkingen vastleggen in Adaptive Forms Workflow{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[Alleen van toepassing op AEM Forms 6.4. Gebruik in AEM Forms 6.5 de functie Variabelen om dit geval te gebruiken]

Een veelvoorkomend verzoek is om de opmerkingen die de taakcontroleur heeft ingevoerd, in een e-mail op te nemen. In AEM Forms 6.4 is er geen mechanisme buiten het vak om de gebruiker in te voeren opmerkingen en deze opmerkingen in e-mail op te nemen.

Om aan dit vereiste te voldoen, wordt een bundel van steekproef OSGi verstrekt die kan worden gebruikt om commentaren vast te leggen en deze commentaren op te slaan als werkschemabezit.

Het volgende schermafbeelding laat zien hoe u processtap in [AEM Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) kunt gebruiken om opmerkingen vast te leggen en deze op te slaan als eigenschap metadata. De &quot;Commentaar van het Werkschema van de Vangst&quot;is de naam van de klasse java die in de processtap moet worden gebruikt. U moet de naam van de metagegevenseigenschap doorgeven die de opmerkingen bevat. In de onderstaande schermafbeelding is managerComments de eigenschap metadata waarin de opmerkingen worden opgeslagen.

![workflowcomments1](assets/workflowcomments1.gif)

Voer de volgende stappen uit om deze mogelijkheid op uw systeem te testen:
* [Zorg ervoor dat de processtap in de workflow zodanig is geconfigureerd dat de Opmerkingen van de workflow voor vastleggen worden gebruikt](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [De gebruikersbundel DevelopingWithService implementeren](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Implementeer de SetValue-bundel](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Deze bundel bevat de voorbeeldcode voor het vastleggen van de opmerkingen en het opslaan als een eigenschap voor metagegevens

* [Download en decomprimeer de elementen die aan dit artikel zijn gerelateerd naar uw bestandssysteem. ](assets/capturecomments.zip) De elementen bevatten workflowmodel en voorbeeld Adaptief formulier.

* De 2 zip-bestanden met pakketbeheer in AEM importeren

* [Bekijk een voorbeeld van het formulier door naar deze URL te bladeren](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* De formuliervelden invullen en het formulier verzenden

* [Controleer uw AEM](http://localhost:4502/aem/inbox)

* Open de taak vanuit het Postvak IN en verzend het formulier. Voer opmerkingen in wanneer hierom wordt gevraagd.

De opmerkingen worden in crx opgeslagen in de metagegevenseigenschap met de naam managerComments. Als u wilt controleren op de opmerkingen, meldt u zich aan bij crx als beheerder. De workflowinstanties worden opgeslagen in het volgende pad

/var/workflow/instances/server0

Selecteer de aangewezen werkschemainstantie en controleer bezitsmanagerComments in de meta-gegevensknoop.

