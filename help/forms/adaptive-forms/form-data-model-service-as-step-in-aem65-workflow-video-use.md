---
title: De service Formuliergegevensmodel gebruiken als stap in AEM 6.5-workflow
description: AEM Forms 6.5 introduceerde de mogelijkheid om variabelen te maken in de AEM Workflow. Met deze nieuwe mogelijkheid die de "Invoke Form Data Model Service" in AEM workflow gebruikt, is heel eenvoudig geworden. De volgende video zal u door de stappen lopen betrokken bij het gebruiken van de Invoke Dienst van het Model van Gegevens van de Vorm in AEM Werkstroom.
feature: Workflow
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# De service Formuliergegevensmodel gebruiken als stap in AEM 6.5-workflow {#using-form-data-model-service-as-step-in-workflow}

Vanaf AEM Forms 6.4 kunnen we nu Form Data Model Service gebruiken als onderdeel van AEM Workflow. De volgende video loopt door de stappen nodig om de stap van het Model van de Gegevens van de Vorm in AEM Werkschema te vormen

>!![NOTE]Voor de in deze video gedemonstreerde functionaliteit is AEM Forms 6.5.1 vereist


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

Volg onderstaande instructies om deze mogelijkheid op uw server te testen

* Setup tomcat met SampleRest.war-bestand zoals beschreven [hier](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html).Het oorlogsdossier in Tomcat heeft de code om de creditscore van de aanvrager terug te geven.De creditscore is een willekeurig getal tussen 200 en 800

* [ Elementen in AEM importeren met pakketbeheer](assets/aem65-loanapplication.zip)
* Het pakket bevat het volgende:

   * Workflowmodel dat gebruikmaakt van FDM-stap.
   * Formuliergegevensmodel dat wordt gebruikt in de FDM-stap.
   * Aangepast formulier om de workflow bij verzending te activeren.
* Open de [MortgaugeApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Vul de gegevens in en verzend deze. Bij de indiening van het formulier [werkstroom van de lade](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) wordt geactiveerd.

![ werkstroom ](assets/invokefdm651.PNG).
De workflow gebruikt de component Splitsen of Splitsen om de toepassing naar de beheerder te leiden als de creditscore hoger is dan 500. Als de creditscore lager is dan 500, wordt de toepassing naar cavery gerouteerd.
