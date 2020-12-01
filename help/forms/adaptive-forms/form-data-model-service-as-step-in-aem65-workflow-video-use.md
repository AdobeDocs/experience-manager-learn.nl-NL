---
title: De service Formuliergegevensmodel gebruiken als stap in AEM 6.5-workflow
seo-title: De service Formuliergegevensmodel gebruiken als stap in AEM 6.5-workflow
description: AEM Forms 6.5 introduceerde de mogelijkheid om variabelen te maken in de AEM Workflow. Met deze nieuwe mogelijkheid die de "Invoke Form Data Model Service" in AEM workflow gebruikt, is heel eenvoudig geworden. De volgende video zal u door de stappen lopen betrokken bij het gebruiken van de Invoke Dienst van het ModelGegevens van de Vorm in AEM Werkstroom.
seo-description: AEM Forms 6.5 introduceerde de mogelijkheid om variabelen te maken in de AEM Workflow. Met deze nieuwe mogelijkheid die de "Invoke Form Data Model Service" in AEM workflow gebruikt, is heel eenvoudig geworden. De volgende video zal u door de stappen lopen betrokken bij het gebruiken van de Invoke Dienst van het ModelGegevens van de Vorm in AEM Werkstroom.
feature: workflow.
topics: workflow.
audience: developer.
doc-type: technical video.
activity: setup.
version: 6.5.
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---


# De service Formuliergegevensmodel gebruiken als stap in AEM 6.5-workflow {#using-form-data-model-service-as-step-in-workflow}

Vanaf AEM Forms 6.4 kunnen we nu Form Data Model Service gebruiken als onderdeel van AEM Workflow. De volgende video loopt door de stappen nodig om de stap van het Model van de Gegevens van de Vorm in AEM Werkschema te vormen

>!![NOTE]Voor de in deze video gedemonstreerde functionaliteit is AEM Forms 6.5.1 vereist


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

Volg onderstaande instructies om deze mogelijkheid op uw server te testen

* Setup tomcat met SampleRest.war-bestand zoals beschreven [hier](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html).Het oorlogsbestand dat in Tomcat wordt geïmplementeerd, heeft de code om de creditscore van de aanvrager te retourneren.De creditscore is een willekeurig getal tussen 200 en 800

* [ Elementen in AEM importeren met pakketbeheer](assets/aem65-loanapplication.zip)
* Het pakket bevat het volgende:

   * Workflowmodel dat gebruikmaakt van FDM-stap.
   * Formuliergegevensmodel dat wordt gebruikt in de FDM-stap.
   * Aangepast formulier om de workflow bij verzending te activeren.
* Open [MortgaugeApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Vul de gegevens in en verzend deze. Bij het verzenden van het formulier wordt de [werkstroom van de leningtoepassing](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) geactiveerd.

![ workflow  ](assets/invokefdm651.PNG).
De workflow gebruikt de component Splitsen of Splitsen om de toepassing naar de beheerder te leiden als de creditscore hoger is dan 500. Als de creditscore lager is dan 500, wordt de toepassing naar cavery gerouteerd.
