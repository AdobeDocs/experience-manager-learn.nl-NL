---
title: De service Formuliergegevensmodel gebruiken als stap in de workflow
description: Vanaf AEM Forms 6.4 kunnen we nu het formuliergegevensmodel gebruiken als onderdeel van AEM workflow. De volgende video loopt door de stappen nodig om de modelstap van de Gegevens van de Vorm in AEM Werkschema te vormen.
feature: Workflow
type: Tutorial
version: 6.4,6.5
topic: Ontwikkeling
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---


# De service Formuliergegevensmodel gebruiken als stap in de workflow {#using-form-data-model-service-as-step-in-workflow}

Vanaf AEM Forms 6.4 kunnen we nu het formuliergegevensmodel gebruiken als onderdeel van AEM workflow. De volgende video loopt door de stappen nodig om de stap van het Model van de Gegevens van de Vorm in AEM Werkschema te vormen


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

Volg onderstaande instructies om deze mogelijkheid op uw server te testen
* [Download en implementeer de setvalue-bundel](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dit is de aangepaste OSGI-bundel waarmee eigenschappen van metagegevens worden ingesteld.
>!![NOTE]In AEM Forms 6.5 en hoger is deze mogelijkheid beschikbaar buiten het vak, zoals hier  [beschreven](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* Setup tomcat met SampleRest.war-bestand zoals beschreven [hier](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html).Het oorlogsbestand dat in Tomcat wordt geïmplementeerd, heeft de code om de creditscore van de aanvrager te retourneren. De creditscore is een willekeurig getal tussen 200 en 800

* [Importeer de elementen in AEM met behulp van pakketbeheer](assets/invoke-fdm-as-service-step.zip). Het pakket bevat het volgende:

   * Workflowmodel dat gebruikmaakt van FDM-stap.
   * Formuliergegevensmodel dat wordt gebruikt in de FDM-stap.
   * Aangepast formulier om de workflow bij verzending te activeren.
* Open [MortgaugeApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Vul de gegevens in en verzend deze. Bij het verzenden van het formulier wordt de [werkstroom van de leningtoepassing](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) geactiveerd.

![ workflow  ](assets/fdm-as-service-step-workflow.PNG).
De workflow gebruikt de component Splitsen of Splitsen om de toepassing naar de beheerder te leiden als de creditscore hoger is dan 500. Als de creditscore lager is dan 500, wordt de toepassing gerouteerd naar cavery
