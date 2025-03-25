---
title: De service Formuliergegevensmodel gebruiken als stap in de workflow
description: Vanaf AEM Forms 6.4 kunnen we nu het formuliergegevensmodel gebruiken als onderdeel van de AEM-workflow. In de volgende video worden de stappen doorlopen die nodig zijn om de stap Formuliergegevensmodel in de AEM-workflow te configureren.
feature: Workflow
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
duration: 205
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# De service Formuliergegevensmodel gebruiken als stap in de workflow {#using-form-data-model-service-as-step-in-workflow}

Vanaf AEM Forms 6.4 kunnen we nu het formuliergegevensmodel gebruiken als onderdeel van de AEM-workflow. In de volgende video worden de stappen doorlopen die nodig zijn om de stap Formuliergegevensmodel in de AEM-workflow te configureren


>[!VIDEO](https://video.tv.adobe.com/v/21719?quality=12&learn=on)

Volg onderstaande instructies om deze mogelijkheid op uw server te testen
* [ Download en stel de setvalue bundel ](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) op. Dit is de aangepaste OSGI-bundel waarmee eigenschappen van metagegevens worden ingesteld.
> in AEM Forms 6.5 en boven dit vermogen is beschikbaar uit de doos zoals [ hier ](form-data-model-service-as-step-in-aem65-workflow-video-use.md) beschrijft

* Opstelling tomcat met het dossier SampleRest.war zoals die [ hier ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html) wordt beschreven.Het oorlogsdossier in Tomcat wordt opgesteld heeft de code om de het creditscore van de aanvrager terug te keren. De creditscore is een willekeurig getal tussen 200 en 800

* [ de Invoer de activa in AEM gebruikend pakketmanager ](assets/invoke-fdm-as-service-step.zip).Het pakket bevat het volgende:

   * Workflowmodel dat gebruikmaakt van FDM-stap.
   * Formuliergegevensmodel dat wordt gebruikt in de FDM-stap.
   * Aangepast formulier om de workflow bij verzending te activeren.
* Open [ MortgaugeApplicationForm ](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Vul de gegevens in en verzend deze. Op de vormvoorlegging wordt het [ de werkschema van de leningentoepassing ](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) teweeggebracht.

![ workflow ](assets/fdm-as-service-step-workflow.PNG) .
De workflow gebruikt de component Splitsen of Splitsen om de toepassing naar de beheerder te leiden als de creditscore hoger is dan 500. Als de creditscore lager is dan 500, wordt de toepassing gerouteerd naar cavery
