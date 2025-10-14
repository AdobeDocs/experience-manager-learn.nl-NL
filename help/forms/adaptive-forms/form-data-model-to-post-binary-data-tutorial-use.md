---
title: Binaire gegevens verzenden met het formuliergegevensmodel
description: Binaire gegevens via het formuliergegevensmodel naar AEM DAM verzenden
feature: Workflow
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9c62a7d6-8846-424c-97b8-2e6e3c1501ec
last-substantial-update: 2021-01-09T00:00:00Z
duration: 95
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# Binaire gegevens verzenden met het formuliergegevensmodel{#using-form-data-model-to-post-binary-data}

Vanaf AEM Forms 6.4 kunnen we nu de service Form Data Model aanroepen als een stap in de AEM-workflow. In dit artikel wordt een voorbeeld van een gebruiksgeval getoond voor het posten van een document met gebruik van de service Formuliergegevensmodel.

Het gebruiksgeval is als volgt:

1. Een gebruiker vult het adaptieve formulier en verzendt het.
1. Het adaptieve formulier is geconfigureerd om een document met records te genereren.
1. Bij het indienen van deze adaptieve formulieren wordt de AEM-workflow geactiveerd, die de Invoke Form Data Model Service gebruikt om het Document of Record naar AEM DAM te verzenden.

![&#x200B; posttodam &#x200B;](assets/posttodamshot1.png)

Tabblad Formuliergegevensmodel - Eigenschappen

Op het tabblad Service Input geven we het volgende in kaart

* file(The Binary Object that need to be stored) with DOR.pdf property relative to payload. Dat betekent dat wanneer het adaptieve formulier wordt verzonden, het gegenereerde Document Of Record wordt opgeslagen in een bestand met de naam DOR.pdf ten opzichte van de payload van de workflow.**zorg ervoor dit DOR.pdf het zelfde is dat u wanneer het vormen van het Adaptieve de voorleggingsbezit van de Vorm verstrekt.**

* fileName - Dit is de naam waarmee het binaire object in DAM wordt opgeslagen. Zo wilt u dit bezit dynamisch worden geproduceerd, zodat elke fileName uniek per voorlegging zou zijn. Daarom hebben we de processtap in de workflow gebruikt om de eigenschap metadata genaamd bestandsnaam te maken en de waarde ervan in te stellen op de combinatie van Lidnaam en Rekeningnummer van de persoon die het formulier indient. Als de naam van het lid van de persoon bijvoorbeeld John Jacobs is en zijn rekeningnummer 9846, zou de bestandsnaam John Jacobs_9846.pdf zijn

![&#x200B; fdmserviceinput &#x200B;](assets/fdminputservice.png)

Service-invoer

>[!NOTE]
>
>Het oplossen van problemen Tips - als om een of andere reden DOR.pdf niet in DAM wordt gecreeerd, stel de montages van de gegevensbronauthentificatie terug door [&#x200B; hier &#x200B;](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam) te klikken. Dit zijn de AEM-verificatie-instellingen, die standaard admin/admin zijn.

Volg de onderstaande stappen om deze mogelijkheid op uw server te testen:

1.[&#x200B; stel de DevelopingWithService-gebruikersbundel &#x200B;](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar) op

1. [&#x200B; Download en stel de setvalue bundel &#x200B;](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) op. Deze bundel van douaneOSGI wordt gebruikt om meta-gegevensbezit tot stand te brengen en zijn waarde van de voorgelegde vormgegevens te plaatsen.

1. [&#x200B; voer de activa &#x200B;](assets/postdortodam.zip) verbonden aan dit artikel in AEM in gebruikend pakketmanager.U zult het volgende krijgen

   1. Workflowmodel
   1. Adaptief formulier geconfigureerd voor verzending naar de AEM-workflow
   1. Gegevensbron geconfigureerd voor gebruik van het bestand PostToDam.JSON
   1. Formuliergegevensmodel dat gebruikmaakt van de Data Source

1. Punt uw [&#x200B; browser om de Aangepaste Vorm &#x200B;](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled) te openen
1. Vul het formulier in en verzend het.
1. Controleer de Assets-toepassing of het Document of Record is gemaakt en opgeslagen.


[&#x200B; het Dossier van de Wagger &#x200B;](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) dat in het creëren van de gegevensbron wordt gebruikt is beschikbaar voor uw verwijzing
