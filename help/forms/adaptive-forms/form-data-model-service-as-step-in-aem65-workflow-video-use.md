---
title: De service Formuliergegevensmodel gebruiken als stap in de AEM 6.5-workflow
description: AEM Forms 6.5 introduceerde de mogelijkheid om variabelen te maken in de AEM Workflow. Met deze nieuwe mogelijkheid met de "Invoke Form Data Model Service" in de AEM-workflow is het heel eenvoudig geworden. In de volgende video worden de stappen beschreven die nodig zijn voor het gebruik van de Invoke Form Data Model Service in de AEM-workflow.
feature: Workflow
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
last-substantial-update: 2021-02-09T00:00:00Z
duration: 239
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---

# De service Formuliergegevensmodel gebruiken als stap in de AEM 6.5-workflow {#using-form-data-model-service-as-step-in-workflow}

Vanaf AEM Forms 6.4 kunnen we nu Form Data Model Service gebruiken als onderdeel van AEM Workflow. In de volgende video worden de stappen doorlopen die nodig zijn om de stap Formuliergegevensmodel in de AEM-workflow te configureren

> het vermogen dat in deze video wordt getoond vereist AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=12&learn=on)

Volg onderstaande instructies om deze mogelijkheid op uw server te testen

* Opstelling tomcat met het dossier SampleRest.war zoals die [&#x200B; hier &#x200B;](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html) wordt beschreven.Het oorlogsdossier in Tomcat wordt opgesteld heeft de code om de het creditscore van de aanvrager terug te keren.De creditscore is willekeurig aantal tussen 200 en 800 die

* [Elementen importeren naar AEM met pakketbeheer](assets/aem65-loanapplication.zip)
* Het pakket bevat het volgende:

   * Workflowmodel dat gebruikmaakt van FDM-stap.
   * Formuliergegevensmodel dat wordt gebruikt in de FDM-stap.
   * Aangepast formulier om de workflow bij verzending te activeren.
* Open [&#x200B; MortgaugeApplicationForm &#x200B;](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Vul de gegevens in en verzend deze. Op de vormvoorlegging wordt het [&#x200B; de werkschema van de leningentoepassing &#x200B;](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) teweeggebracht.

![&#x200B; workflow &#x200B;](assets/invokefdm651.PNG) .
De workflow gebruikt de component Splitsen of Splitsen om de toepassing naar de beheerder te leiden als de creditscore hoger is dan 500. Als de creditscore lager is dan 500, wordt de toepassing naar cavery gerouteerd.
