---
title: Verzenden naar pagina Hartelijk dank
seo-title: Verzenden naar pagina Hartelijk dank
description: Een pagina voor bedankt weergeven bij het verzenden van een adaptief formulier
seo-description: Een pagina voor bedankt weergeven bij het verzenden van een adaptief formulier
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---


# Verzenden naar pagina {#submitting-to-thank-you-page}

Verzenden naar de eindpuntoptie REST geeft de gegevens die in het formulier zijn ingevuld door aan een geconfigureerde bevestigingspagina als onderdeel van de HTTP-aanvraag. U kunt de naam toevoegen van de velden die u wilt aanvragen. De indeling van het verzoek is:

\{fieldName\} = \{parameterName\}. Bijvoorbeeld, subitterName is de naam van een adaptief vormgebied en de verzender is de naam van de parameter. In dank kunt u tot de voorlegger parameter toegang hebben gebruikend request.getParameter (&quot;voorlegger&quot;) om greep van de waarde van het gebied van de voorleggernaam te krijgen.

submitName=Subrespondter

In de onderstaande schermafbeelding sturen we het adaptieve formulier om u te bedanken op de pagina /content/thankyou. Voor deze dankbetuigingen geven we 3 aanvraagkenmerken door die de waarden van de formuliervelden bevatten.

![dank](assets/thankyoupage.gif)

U kunt ook naar het externe eindpunt verzenden via POST. Om dat te verwezenlijken, moet u enkel &quot;toelaten postverzoek&quot;checkbox en URL voor het externe eindpunt verstrekken. Wanneer u het formulier verzendt, wordt de pagina Bedankt en het eindpunt van de POST wordt tegelijkertijd aangeroepen.

![vastleggen](assets/capture.gif)


Volg de onderstaande instructies om deze mogelijkheid op uw server te testen:

* Importeer het [elementbestand dat aan dit artikel is gekoppeld in AEM met pakketbeheer](assets/submittingtorestendpoint.zip)
* Wijs uw browser aan [Time-off aanvraagformulier](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Vul het vereiste veld in en verzend het formulier
* Je moet een pagina voor bedankt openen waarop je gegevens op de pagina zijn ingevuld

