---
title: Workflow voor eenvoudig uitstel van betaling
description: Adaptieve formulierdeelvensters verbergen en tonen in de AEM-workflow
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: Development
role: Developer
level: Experienced
exl-id: 9342bd2f-2ba9-42ee-9224-055649ac3c90
last-substantial-update: 2020-07-07T00:00:00Z
duration: 592
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 0%

---

# Workflow voor eenvoudig uitstel van betaling

In dit artikel bekijken we een eenvoudige workflow die wordt gebruikt voor het aanvragen van Betaalde tijd uit. De bedrijfsvereisten zijn als volgt:

* De gebruiker A vraagt tijd uit door een adaptief formulier in te vullen.
* Het formulier wordt omgeleid naar een AEM-beheerder (het wordt in real-time omgeleid naar de manager van de verzender)
* Admin opent het formulier. Beheerders mogen de door de indiener ingevulde gegevens niet bewerken.
* De sectie Fiatteur moet zichtbaar zijn voor de fiatteur (In dit geval is dit de AEM-beheerder).

Om het bovengenoemde vereiste te verwezenlijken, gebruiken wij een verborgen gebied genoemd **initialstep** in de vorm en zijn standaardwaarde wordt geplaatst aan Yes.When de vorm wordt voorgelegd, plaatst de eerste stap in het werkschema de waarde van eerste stap aan Nr. Het formulier bevat bedrijfsregels voor het verbergen en weergeven van de juiste secties op basis van de beginstapwaarde.

**Vorm vormen om het Werkschema van AEM van de Trekker** te teweegbrengen

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=12&learn=on)

**Analyse van het Werkschema**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=12&learn=on)

**de mening van de Verzender van de Tijd van de Vorm van het Verzoek**

![&#x200B; initialstep &#x200B;](assets/initialstep.gif)

**Approver mening van de vorm**

![&#x200B; Approverview &#x200B;](assets/approversview.gif)

In de fiattweergave kan de fiatteur de verzonden gegevens niet bewerken. Er is ook een nieuwe sectie die alleen voor fiatteurs is bedoeld.

Volg onderstaande stappen om deze workflow op uw systeem te testen:
* [DevelopingWithServiceUserBundle downloaden en implementeren](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [De SetValue Custom OSGI-bundel downloaden en implementeren](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [&#x200B; voer de activa met betrekking tot dit artikel in AEM &#x200B;](assets/helpxworkflow.zip) in
* Open de [&#x200B; Tijd van de Vorm van het Verzoek &#x200B;](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Vul de details in en verzend
* Open [&#x200B; inbox &#x200B;](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). Er moet een nieuwe taak worden toegewezen. Open het formulier. De gegevens van de indiener moeten alleen-lezen zijn en er moet een nieuwe fiatteur-sectie zichtbaar zijn.
* Onderzoek het [&#x200B; werkschemamodel &#x200B;](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* Verken de processtap. Dit is de stap die de waarde van eerste stap op Nee instelt.
