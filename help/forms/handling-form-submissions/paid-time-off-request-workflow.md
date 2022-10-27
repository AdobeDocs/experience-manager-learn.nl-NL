---
title: Workflow voor eenvoudig uitstel van betaling
description: Deelvensters Adaptief formulier verbergen en tonen in AEM workflow
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: Adaptive Forms
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: Development
role: Developer
level: Experienced
exl-id: 9342bd2f-2ba9-42ee-9224-055649ac3c90
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Workflow voor eenvoudig uitstel van betaling

In dit artikel bekijken we een eenvoudige workflow die wordt gebruikt voor het aanvragen van Betaalde tijd uit. De bedrijfsvereisten zijn als volgt:

* De gebruiker A vraagt tijd uit door een adaptief formulier in te vullen.
* Het formulier wordt omgeleid naar AEM gebruiker (in real-time wordt het omgeleid naar de manager van de verzender)
* Admin opent het formulier. Beheerders mogen de door de indiener ingevulde gegevens niet bewerken.
* De sectie Fiatteur moet zichtbaar zijn voor de fiatteur (In dit geval is dit de AEM gebruiker).

Voor het uitvoeren van de bovenstaande vereiste gebruiken we een verborgen veld met de naam **initialstep** in het formulier en de standaardwaarde ervan is ingesteld op Ja. Wanneer het formulier wordt verzonden, stelt de eerste stap in de workflow de waarde van de eerste stap in op Nee. Het formulier bevat bedrijfsregels voor het verbergen en weergeven van de juiste secties op basis van de beginstapwaarde.

**Formulier configureren voor activering AEM workflow**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=9&learn=on)

**Workflowanalyse**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=9&learn=on)

**Weergave van de verzender van het aanvraagformulier**

![initialstep](assets/initialstep.gif)

**Weergave fiatteur van het formulier**

![overzicht](assets/approversview.gif)

In de fiattweergave kan de fiatteur de verzonden gegevens niet bewerken. Er is ook een nieuwe sectie die alleen voor fiatteurs is bedoeld.

Volg onderstaande stappen om deze workflow op uw systeem te testen:
* [DevelopingWithServiceUserBundle downloaden en implementeren](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [De SetValue Custom OSGI-bundel downloaden en implementeren](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [De aan dit artikel gerelateerde elementen importeren in AEM](assets/helpxworkflow.zip)
* Open de [Formulier Verzoek om time-out](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Vul de details in en verzend
* Open de [inbox](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). Er moet een nieuwe taak worden toegewezen. Open het formulier. De gegevens van de indiener moeten alleen-lezen zijn en er moet een nieuwe fiatteur-sectie zichtbaar zijn.
* Ontdek de [workflowmodel](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* Verken de processtap. Dit is de stap die de waarde van eerste stap op Nee instelt.
