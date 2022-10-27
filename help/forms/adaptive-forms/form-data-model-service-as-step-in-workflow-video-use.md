---
title: De service Formuliergegevensmodel gebruiken als stap in de workflow
description: Vanaf AEM Forms 6.4 kunnen we nu het formuliergegevensmodel gebruiken als onderdeel van AEM workflow. De volgende video loopt door de stappen nodig om de modelstap van de Gegevens van de Vorm in AEM Werkschema te vormen.
feature: Workflow
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# De service Formuliergegevensmodel gebruiken als stap in de workflow {#using-form-data-model-service-as-step-in-workflow}

Vanaf AEM Forms 6.4 kunnen we nu het formuliergegevensmodel gebruiken als onderdeel van AEM workflow. De volgende video loopt door de stappen nodig om de stap van het Model van de Gegevens van de Vorm in AEM Werkschema te vormen


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

Volg onderstaande instructies om deze mogelijkheid op uw server te testen
* [De setvalue-bundel downloaden en implementeren](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dit is de aangepaste OSGI-bundel waarmee eigenschappen van metagegevens worden ingesteld.
>!![NOTE]In AEM Forms 6.5 en hoger is deze mogelijkheid beschikbaar vanuit de verpakking als [hier beschrijven](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* Setup tomcat met SampleRest.war-bestand zoals beschreven [hier](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html).Het oorlogsdossier dat in Tomcat wordt opgesteld heeft de code om de kredietscore van de aanvrager terug te geven. De creditscore is een willekeurig getal tussen 200 en 800

* [Elementen in AEM importeren met pakketbeheer](assets/invoke-fdm-as-service-step.zip).Het pakket bevat het volgende:

   * Workflowmodel dat gebruikmaakt van FDM-stap.
   * Formuliergegevensmodel dat wordt gebruikt in de FDM-stap.
   * Aangepast formulier om de workflow bij verzending te activeren.
* Open de [MortgaugeApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Vul de gegevens in en verzend deze. Bij de indiening van het formulier [werkstroom van de lade](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) wordt geactiveerd.

![ werkstroom ](assets/fdm-as-service-step-workflow.PNG).
De workflow gebruikt de component Splitsen of Splitsen om de toepassing naar de beheerder te leiden als de creditscore hoger is dan 500. Als de creditscore lager is dan 500, wordt de toepassing gerouteerd naar cavery
