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
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# De service Formuliergegevensmodel gebruiken als stap in de workflow {#using-form-data-model-service-as-step-in-workflow}

Vanaf AEM Forms 6.4 kunnen we nu het formuliergegevensmodel gebruiken als onderdeel van de AEM-workflow. In de volgende video worden de stappen doorlopen die nodig zijn om de stap Formuliergegevensmodel in de AEM-workflow te configureren


>[!VIDEO](https://video.tv.adobe.com/v/21719?quality=12&learn=on)

Volg onderstaande instructies om deze mogelijkheid op uw server te testen

* [&#x200B; Download en stel de setvalue bundel &#x200B;](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) op. Dit is de aangepaste OSGI-bundel waarmee eigenschappen van metagegevens worden ingesteld.

  >[!NOTE]
  >
  >In AEM Forms 6.5 en hierboven is dit vermogen beschikbaar uit de doos zoals [&#x200B; hier &#x200B;](form-data-model-service-as-step-in-aem65-workflow-video-use.md) beschrijven

* Opstelling tomcat met het dossier SampleRest.war zoals die [&#x200B; hier &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html?lang=nl-NL) wordt beschreven.Het oorlogsdossier in Tomcat wordt opgesteld heeft de code om de het creditscore van de aanvrager terug te keren. De creditscore is een willekeurig getal tussen 200 en 800

* [&#x200B; de Invoer de activa in AEM gebruikend pakketmanager &#x200B;](assets/invoke-fdm-as-service-step.zip).Het pakket bevat het volgende:

   * Workflowmodel dat gebruikmaakt van FDM-stap.
   * Formuliergegevensmodel dat wordt gebruikt in de FDM-stap.
   * Aangepast formulier om de workflow bij verzending te activeren.
* Open [&#x200B; MortgaugeApplicationForm &#x200B;](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Vul de gegevens in en verzend deze. Op de vormvoorlegging wordt het [&#x200B; de werkschema van de leningentoepassing &#x200B;](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) teweeggebracht.

![&#x200B; werkschema &#x200B;](assets/fdm-as-service-step-workflow.PNG).

De workflow gebruikt de component Splitsen of Splitsen om de toepassing naar de beheerder te leiden als de creditscore hoger is dan 500. Als de creditscore lager is dan 500, wordt de toepassing gerouteerd naar cavery
