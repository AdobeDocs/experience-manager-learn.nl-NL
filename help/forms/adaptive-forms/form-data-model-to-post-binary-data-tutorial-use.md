---
title: Binaire gegevens verzenden met het formuliergegevensmodel
seo-title: Binaire gegevens verzenden met het formuliergegevensmodel
description: Binaire gegevens via het formuliergegevensmodel naar AEM DAM verzenden
seo-description: Binaire gegevens via het formuliergegevensmodel naar AEM DAM verzenden
uuid: dd344ed8-69f7-4d63-888a-3c96993fe99d
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 6e99df7d-c030-416b-83d2-24247f673b33
translation-type: tm+mt
source-git-commit: f07680e73316efb859a675f4b2212d8c3e03f6a0
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 0%

---


# Binaire gegevens verzenden met het formuliergegevensmodel{#using-form-data-model-to-post-binary-data}

Vanaf AEM Forms 6.4 kunnen we nu de service Formuliergegevensmodel aanroepen als een stap in AEM workflow. In dit artikel wordt een voorbeeld van een gebruiksgeval getoond voor het posten van een document met gebruik van de service Formuliergegevensmodel.

Het gebruiksgeval is als volgt:

1. Een gebruiker vult het adaptieve formulier en verzendt het.
1. Het adaptieve formulier is geconfigureerd om een document met records te genereren.
1. Bij het indienen van deze adaptieve formulieren wordt AEM workflow geactiveerd die de Invoke Form Data Model Service gebruikt om het Document of Record te POSTEN naar AEM DAM.

![posttodam](assets/posttodamshot1.png)

Tabblad Formuliergegevensmodel - Eigenschappen

Op het tabblad Service Input geven we het volgende in kaart

* file(The Binary Object that need to be stored) with DOR.pdf property relative to payload. Dat betekent dat wanneer het adaptieve formulier wordt verzonden, het gegenereerde Document Of Record wordt opgeslagen in een bestand met de naam DOR.pdf ten opzichte van de payload van de workflow.**Zorg ervoor dat dit bestand DOR.pdf hetzelfde is als het bestand dat u opgeeft bij het configureren van de eigenschap voor verzending van het adaptieve formulier.**

* fileName - Dit is de naam waarmee het binaire object in DAM wordt opgeslagen. Zo wilt u dit bezit dynamisch worden geproduceerd, zodat elke fileName uniek per voorlegging zou zijn. Daarom hebben we de processtap in de workflow gebruikt om de eigenschap metadata genaamd bestandsnaam te maken en de waarde ervan in te stellen op de combinatie van Lidnaam en Rekeningnummer van de persoon die het formulier indient. Als de naam van het lid van de persoon bijvoorbeeld John Jacobs is en zijn rekeningnummer 9846, zou de bestandsnaam John Jacobs_9846.pdf zijn

![fdmserviceinput](assets/fdminputservice.png)

Service-invoer

>[!NOTE]
>
>Tips voor het oplossen van problemen - Als het bestand DOR.pdf om een of andere reden niet in DAM is gemaakt, kunt u de instellingen voor gegevensbronverificatie opnieuw instellen door [hier](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam)te klikken. Dit zijn de instellingen voor AEM verificatie, die standaard admin/admin zijn.

Volg onderstaande stappen om deze mogelijkheid op uw server te testen:

1.[Implementeer de DevelopingWithService-gebruikersbundel](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Download en implementeer de setvalue-bundel](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Deze aangepaste OSGI-bundel wordt gebruikt om metagegevenseigenschap te maken en de waarde ervan in te stellen op basis van de verzonden formuliergegevens.

1. [Importeer de aan dit artikel gekoppelde elementen](assets/postdortodam.zip) in AEM met behulp van pakketbeheer. U krijgt het volgende

   1. Workflowmodel
   1. Adaptief formulier geconfigureerd voor verzending naar de AEM-workflow
   1. Gegevensbron geconfigureerd voor gebruik van het bestand PostToDam.JSON
   1. Formuliergegevensmodel dat gebruikmaakt van de gegevensbron

1. Het adaptieve formulier openen in de [browser](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. Vul het formulier in en verzend het.
1. Controleer de toepassing Middelen als het Document of Record is gemaakt en opgeslagen.


[Het wisselaarbestand](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) dat wordt gebruikt bij het maken van de gegevensbron is beschikbaar ter referentie
