---
title: E-mailstap verzenden van Forms Workflow gebruiken
description: De stap E-mail verzenden is geïntroduceerd in AEM Forms 6.4. Met deze stap kunnen we bedrijfsprocessen of een workflow maken waarmee u e-mailberichten met of zonder bijlagen kunt verzenden. De volgende video doorloopt de stappen voor het vormen verzendt e-mailcomponent
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 21e58bbc-c1d6-4d41-a4d4-f522a3a5d4a7
last-substantial-update: 2020-06-09T00:00:00Z
duration: 314
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '418'
ht-degree: 0%

---

# E-mailstap verzenden van Forms Workflow gebruiken {#using-send-email-step-of-forms-workflow}

De stap E-mail verzenden is geïntroduceerd in AEM Forms 6.4. Met deze stap kunnen we bedrijfsprocessen of een workflow maken waarmee u e-mailberichten met of zonder bijlagen kunt verzenden. De volgende video doorloopt de stappen voor het vormen van verzendt e-mailcomponent.

>[!VIDEO](https://video.tv.adobe.com/v/21499?quality=12&learn=on)

Als onderdeel van dit artikel doorlopen we de volgende gebruikszaak:

1. Een gebruiker vult het aanvraagformulier in
1. Bij het verzenden van formulieren wordt AEM workflow geactiveerd
1. De AEM-workflow gebruikt de component E-mail verzenden om een e-mail met de servicevoorwaarden als bijlage te verzenden

Alvorens u gebruikt verzend E-mail stap zorg ervoor u de Dienst van de Post van Dag CQ van [ configMgr ](http://localhost:4502/system/console/configMgr) vormt. Geef de waarden op die specifiek zijn voor uw omgeving

![ vorm de Dienst van de Post van de Dag CQ ](assets/mailservice.png)

Als onderdeel van de elementen die aan dit artikel zijn gekoppeld, krijgt u het volgende

1. Adaptief formulier dat de workflow bij verzending start
1. Voorbeeld van een workflow die een e-mail met DOR als bijlage verzendt
1. OSGi-bundel die de eigenschappen van metagegevens maakt

Ga als volgt te werk om het voorbeeld op uw systeem uit te voeren:

1. [De gebruikersbundel DevelopingWithService implementeren](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [ Download en installeer setvalue bundel ](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) Deze bundel bevat de code voor het creëren van de meta-gegevenseigenschappen als deel van de processtap van het werkschema.
1. [ vorm de Dienst van de Post van de Dag CQ ](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [De aan dit artikel gekoppelde elementen importeren en installeren met pakketbeheer in CRX](assets/emaildoraemformskt.zip)
1. Lanceer de [ adaptieve vorm ](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled). Vul de vereiste velden in en verzend deze.
1. U moet een e-mail ontvangen met DocumentOfRecord als bijlage

Onderzoek het [ werkschemamodel ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

Bekijk de processtap van de workflow. De aangepaste code die aan de processtap is gekoppeld, maakt eigenschapnamen voor metagegevens en stelt de waarden ervan in op basis van de verzonden gegevens. Deze waarden worden vervolgens gebruikt door de component send email.

>[!NOTE]
>
>In AEM Forms 6.5 en hoger hebt u deze aangepaste code niet nodig om eigenschappen van metagegevens te maken. Gebruik de functie voor variabelen in AEM workflow

Controleer of het tabblad Bijlagen van de component E-mail verzenden is geconfigureerd volgens de onderstaande schermopname
![ verzendt E-mail het Lusje van de Bijlage van de Bijlage ](assets/sendemailcomponentconfigure.jpg) &quot;DOR.pdf&quot;waarde moet de waarde aanpassen die in het Document van de Weg van het Verslag wordt gespecificeerd in de voorleggingsopties van uw adaptieve vorm.
