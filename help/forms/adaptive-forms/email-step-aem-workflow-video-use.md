---
title: E-mailstap verzenden van Forms Workflow gebruiken
description: De stap E-mail verzenden is geïntroduceerd in AEM Forms 6.4. Met deze stap kunnen we bedrijfsprocessen of een workflow maken waarmee u e-mailberichten met of zonder bijlagen kunt verzenden. De volgende video doorloopt de stappen voor het vormen verzendt e-mailcomponent
feature: Workflow
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 21e58bbc-c1d6-4d41-a4d4-f522a3a5d4a7
last-substantial-update: 2020-06-09T00:00:00Z
duration: 314
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '418'
ht-degree: 0%

---

# E-mailstap verzenden van Forms Workflow gebruiken {#using-send-email-step-of-forms-workflow}

De stap E-mail verzenden is geïntroduceerd in AEM Forms 6.4. Met deze stap kunnen we bedrijfsprocessen of een workflow maken waarmee u e-mailberichten met of zonder bijlagen kunt verzenden. De volgende video doorloopt de stappen voor het vormen van verzendt e-mailcomponent.

>[!VIDEO](https://video.tv.adobe.com/v/21499?quality=12&learn=on)

Als onderdeel van dit artikel doorlopen we de volgende gebruikszaak:

1. Een gebruiker vult het aanvraagformulier in
1. Bij het verzenden van formulieren wordt de AEM Workflow geactiveerd
1. De AEM Workflow gebruikt de Send E-mailcomponent om een e-mail met het DoR als bijlage te verzenden

Alvorens u gebruikt verzend E-mail stap zorg ervoor u de Dienst van de Post van Dag CQ van [&#x200B; configMgr &#x200B;](http://localhost:4502/system/console/configMgr) vormt. Geef de waarden op die specifiek zijn voor uw omgeving

![&#x200B; vorm de Dienst van de Post van de Dag CQ &#x200B;](assets/mailservice.png)

Als onderdeel van de elementen die aan dit artikel zijn gekoppeld, krijgt u het volgende

1. Adaptief formulier dat de workflow bij verzending start
1. Voorbeeld van een workflow die een e-mail met DOR als bijlage verzendt
1. OSGi-bundel die de eigenschappen van metagegevens maakt

Ga als volgt te werk om het voorbeeld op uw systeem uit te voeren:

1. [De gebruikersbundel DevelopingWithService implementeren](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [&#x200B; Download en installeer setvalue bundel &#x200B;](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) Deze bundel bevat de code voor het creëren van de meta-gegevenseigenschappen als deel van de processtap van het werkschema.
1. [&#x200B; vorm de Dienst van de Post van de Dag CQ &#x200B;](https://helpx.adobe.com/nl/experience-manager/6-5/sites/administering/using/notification.html)
1. [De aan dit artikel gekoppelde elementen importeren en installeren met pakketbeheer in CRX](assets/emaildoraemformskt.zip)
1. Lanceer de [&#x200B; adaptieve vorm &#x200B;](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled). Vul de vereiste velden in en verzend deze.
1. U moet een e-mail ontvangen met DocumentOfRecord als bijlage

Onderzoek het [&#x200B; werkschemamodel &#x200B;](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

Bekijk de processtap van de workflow. De aangepaste code die aan de processtap is gekoppeld, maakt eigenschapnamen voor metagegevens en stelt de waarden ervan in op basis van de verzonden gegevens. Deze waarden worden vervolgens gebruikt door de component send email.

>[!NOTE]
>
>In AEM Forms 6.5 en hoger hebt u deze aangepaste code niet nodig om eigenschappen van metagegevens te maken. Gebruik de functie voor variabelen in de AEM-workflow

Controleer of het tabblad Bijlagen van de component E-mail verzenden is geconfigureerd volgens de onderstaande schermopname
![&#x200B; verzendt E-mail het Lusje van de Bijlage van de Bijlage &#x200B;](assets/sendemailcomponentconfigure.jpg) &quot;DOR.pdf&quot;waarde moet de waarde aanpassen die in het Document van de Weg van het Verslag wordt gespecificeerd in de voorleggingsopties van uw adaptieve vorm.
