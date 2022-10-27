---
title: Verzenden naar pagina Hartelijk dank
seo-title: Submitting To Thank You Page
description: Een pagina voor bedankt weergeven bij het verzenden van een adaptief formulier
seo-description: Display a thank you page on submitting Adaptive Form
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%

---

# Verzenden naar pagina Hartelijk dank {#submitting-to-thank-you-page}

Verzenden naar de eindpuntoptie REST geeft de gegevens die in het formulier zijn ingevuld door aan een geconfigureerde bevestigingspagina als onderdeel van de HTTP-aanvraag. U kunt de naam toevoegen van de velden die u wilt aanvragen. De indeling van het verzoek is:

\{fieldName\} = \{parameterName\}. Bijvoorbeeld, subitterName is de naam van een adaptief vormgebied en de verzender is de naam van de parameter. In dank kunt u tot de voorlegger parameter toegang hebben gebruikend request.getParameter (&quot;voorlegger&quot;) om greep van de waarde van het gebied van de voorleggernaam te krijgen.

`submitterName=submitter`

In de onderstaande schermafbeelding sturen we het adaptieve formulier om u te bedanken op de pagina /content/thankyou. Voor deze dankbetuigingen geven we 3 aanvraagkenmerken door die de waarden van de formuliervelden bevatten.

![Bedankt, pagina](assets/thankyoupage.gif)

U kunt ook naar het externe eindpunt verzenden via POST. Om dat te verwezenlijken, moet u enkel &quot;toelaten postverzoek&quot;checkbox en URL voor het externe eindpunt verstrekken. Wanneer u het formulier verzendt, wordt de pagina &quot;Bedankt&quot; weergegeven en wordt het eindpunt van de POST tegelijkertijd aangeroepen.

![Vastlegconfiguratie](assets/capture.gif)

Volg de onderstaande instructies om deze mogelijkheid op uw server te testen:

* Het dialoogvenster Importeren [bestand met elementen dat is gekoppeld aan dit artikel in AEM met pakketbeheer](assets/submittingtorestendpoint.zip)
* Wijs uw browser aan [Formulier voor time-out](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Vul het vereiste veld in en verzend het formulier
* Je moet een pagina voor bedankt openen waarop je gegevens op de pagina zijn ingevuld
