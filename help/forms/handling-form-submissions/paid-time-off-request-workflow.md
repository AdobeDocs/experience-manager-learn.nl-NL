---
title: Workflow voor eenvoudig uitstel van betaling
description: Deelvensters Adaptief formulier verbergen en tonen in AEM workflow
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: integratie
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---


# Workflow voor eenvoudig uitstel van betaling

In dit artikel bekijken we een eenvoudige workflow die wordt gebruikt voor het aanvragen van Betaalde tijd uit. De bedrijfsvereisten zijn als volgt:

* De gebruiker A vraagt tijd uit door een adaptief formulier in te vullen.
* Het formulier wordt omgeleid naar AEM gebruiker (in real-time wordt het omgeleid naar de manager van de verzender)
* Admin opent het formulier. Beheerders mogen de door de indiener ingevulde gegevens niet bewerken.
* De sectie Fiatteur moet zichtbaar zijn voor de fiatteur (In dit geval is dit de AEM gebruiker).

Om het bovenstaande vereiste te verwezenlijken, gebruiken wij een verborgen gebied genoemd **initialstep** in de vorm en zijn standaardwaarde wordt geplaatst aan ja.Wanneer de vorm wordt voorgelegd, plaatst de eerste stap in het werkschema de waarde van aanvankelijke stap aan Nr. Het formulier bevat bedrijfsregels voor het verbergen en weergeven van de juiste secties op basis van de beginstapwaarde.

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
* Open het [Verzoek om tijd uit formulier](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Vul de details in en verzend
* Open [inbox](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). Er moet een nieuwe taak worden toegewezen. Open het formulier. De gegevens van de indiener moeten alleen-lezen zijn en er moet een nieuwe fiatteur-sectie zichtbaar zijn.
* Ontdek het [workflowmodel](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* Verken de processtap. Dit is de stap die de waarde van eerste stap op Nee instelt.
