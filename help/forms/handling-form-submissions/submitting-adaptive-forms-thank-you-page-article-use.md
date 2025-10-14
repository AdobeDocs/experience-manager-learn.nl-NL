---
title: Verzenden naar pagina Hartelijk dank
description: Een pagina voor bedankt weergeven bij het verzenden van een adaptief formulier
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
last-substantial-update: 2020-07-07T00:00:00Z
duration: 51
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 0%

---

# Verzenden naar pagina Hartelijk dank {#submitting-to-thank-you-page}

Verzenden naar de eindpuntoptie REST geeft de gegevens die in het formulier zijn ingevuld door aan een geconfigureerde bevestigingspagina als onderdeel van de HTTP GET-aanvraag. U kunt de naam toevoegen van de velden die u wilt aanvragen. De indeling van het verzoek is:

\{fieldName\} = \{parameterName\}. Bijvoorbeeld, subitterName is de naam van een adaptief vormgebied en de verzender is de naam van de parameter. In dank kunt u tot de voorlegger parameter toegang hebben gebruikend request.getParameter (&quot;voorlegger&quot;) om greep van de waarde van het gebied van de voorleggernaam te krijgen.

`submitterName=submitter`

In de onderstaande schermafbeelding sturen we het adaptieve formulier om u te bedanken op de pagina /content/thankyou. Voor deze dankbetuigingen geven we 3 aanvraagkenmerken door die de waarden van de formuliervelden bevatten.

![&#x200B; Dank u pagina &#x200B;](assets/thankyoupage.gif)

U kunt ook naar het externe eindpunt verzenden via POST. Om dat te verwezenlijken, moet u enkel &quot;toelaten postverzoek&quot;checkbox en URL voor het externe eindpunt verstrekken. Wanneer u het formulier verzendt, wordt de pagina Hartelijk dank weergegeven en wordt het POST-eindpunt tegelijkertijd aangeroepen.

![&#x200B; Vastlegconfiguratie &#x200B;](assets/capture.gif)

Volg de onderstaande instructies om deze mogelijkheid op uw server te testen:

* Invoer het [&#x200B; activa dossier verbonden aan dit artikel in AEM gebruikend pakketmanager &#x200B;](assets/submittingtorestendpoint.zip)
* Punt uw browser aan de [&#x200B; Tijd van de Vorm van het Verzoek &#x200B;](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Vul het vereiste veld in en verzend het formulier
* Je moet een pagina voor bedankt openen waarop je gegevens op de pagina zijn ingevuld
