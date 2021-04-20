---
title: E-mailstap verzenden van Forms Workflow gebruiken
seo-title: E-mailstap verzenden van Forms Workflow gebruiken
description: De stap E-mail verzenden is geïntroduceerd in AEM Forms 6.4. Met deze stap kunnen we bedrijfsprocessen of een workflow maken waarmee u e-mailberichten met of zonder bijlagen kunt verzenden. De volgende video doorloopt de stappen voor het vormen van de send e-mailcomponent
seo-description: De stap E-mail verzenden is geïntroduceerd in AEM Forms 6.4. Met deze stap kunnen we bedrijfsprocessen of een workflow maken waarmee u e-mailberichten met of zonder bijlagen kunt verzenden. De volgende video doorloopt de stappen voor het vormen van de send e-mailcomponent
uuid: d054ebfb-3b9b-4ca4-8355-0eb0ee7febcb
feature: Workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: 3a11f602-2f4c-423a-baef-28824c0325a1
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 0%

---


# Het gebruiken verzendt E-mailStap van Forms Workflow {#using-send-email-step-of-forms-workflow}

De stap E-mail verzenden is geïntroduceerd in AEM Forms 6.4. Met deze stap kunnen we bedrijfsprocessen of een workflow maken waarmee u e-mailberichten met of zonder bijlagen kunt verzenden. De volgende video doorloopt de stappen voor het vormen van verzendt e-mailcomponent.

>[!VIDEO](https://video.tv.adobe.com/v/21499/?quality=9&learn=on)

Als onderdeel van dit artikel doorlopen we de volgende gebruikszaak:

1. Een gebruiker vult het aanvraagformulier in
1. Bij het verzenden van formulieren wordt AEM workflow geactiveerd
1. De AEM-workflow gebruikt de component E-mail verzenden om een e-mail met de servicevoorwaarden als bijlage te verzenden

Voordat u de stap E-mail verzenden gebruikt, moet u de Day CQ Mail Service configureren vanuit [configMgr](http://localhost:4502/system/console/configMgr). Geef de waarden op die specifiek zijn voor uw omgeving

![CQ-mailservice op dag configureren](assets/mailservice.png)

Als onderdeel van de elementen die aan dit artikel zijn gekoppeld, krijgt u het volgende

1. Adaptief formulier dat de workflow bij verzending activeert
1. Voorbeeld van een workflow die een e-mail met DOR als bijlage verzendt
1. OSGi-bundel die de eigenschappen van metagegevens maakt

Ga als volgt te werk om het voorbeeld op uw systeem uit te voeren:

1. [De gebruikersbundel DevelopingWithService implementeren](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [SetValue-](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)bundel downloaden en installeren Deze bundel bevat de code voor het maken van de eigenschappen van metagegevens als onderdeel van de processtap van de workflow.
1. [CQ-mailservice op dag configureren](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [De aan dit artikel gekoppelde elementen importeren en installeren met pakketbeheer in CRX](assets/emaildoraemformskt.zip)
1. Start het [adaptieve formulier](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled). Vul de vereiste velden in en verzend deze.
1. U moet een e-mail ontvangen met DocumentOfRecord als bijlage

Ontdek het [workflowmodel](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

Bekijk de processtap van de workflow. De aangepaste code die aan de processtap is gekoppeld, maakt eigenschapnamen voor metagegevens en stelt de waarden ervan in op basis van de verzonden gegevens. Deze waarden worden vervolgens gebruikt door de component send email.

>[!NOTE]
>
>In AEM Forms 6.5 en hoger hebt u deze aangepaste code niet nodig om eigenschappen van metagegevens te maken. Gebruik de functie voor variabelen in AEM workflow

Controleer of het tabblad Bijlagen van de component E-mail verzenden is geconfigureerd volgens de onderstaande schermafbeelding
![Tabblad E-mailbijlage verzenden](assets/sendemailcomponentconfigure.jpg)De waarde &quot;DOR.pdf&quot; moet overeenkomen met de waarde die is opgegeven in het pad Document of Record die is opgegeven in de verzendopties van het adaptieve formulier.

