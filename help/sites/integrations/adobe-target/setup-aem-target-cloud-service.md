---
title: Adobe Target Cloud Service-account maken
description: Stap voor stap doorloopt u hoe u Adobe Experience Manager als Cloud Service met Adobe Target kunt integreren met Cloud Service- en Adobe IMS-verificatie
feature: cloud-services
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6044
thumbnail: 41244.jpg
translation-type: tm+mt
source-git-commit: 25ca90f641aaeb93fc9319692f3b099d6b528dd1
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# Adobe Target Cloud Service-account maken {#adobe-target-cloud-service}

Stap voor stap doorloopt op hoe te om Adobe Experience Manager als Cloud Service met Adobe Target te integreren gebruikend Cloud Service en Adobe IMS authentificatie.

>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>Er is een bekend probleem met de configuratie van Adobe Target Cloud Services die in de video wordt weergegeven. Het wordt aangeraden in plaats daarvan dezelfde stappen in de video uit te voeren, maar de configuratie [Adobe Target-Cloud Services voor](https://docs.adobe.com/content/help/en/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html)oudere versies te gebruiken.

## Vaak voorkomende problemen

Wanneer het uitvoeren van het Fragment van de Ervaring naar het doel van Adobe, als uw Integratie van het Doel in admin console niet de juiste toestemming heeft, zou u een fout kunnen ontvangen zoals hieronder getoond.

**UI-fout**![doel-API](assets/error-target-offer.png)

**Logfout**![doel-API-consolefout](assets/target-console-error.png)


**Oplossing**

1. Naar [Admin Console navigeren](https://adminconsole.adobe.com/)
2. Selecteer Producten > Adobe Target > Productprofiel
3. Selecteer op het tabblad Integratie uw Adobe I/O-project (Integratie)
4. Wijs de rol van Editor of fiatteur toe.

![DoelAPI-fout](assets/target-permissions.png)

Als u de juiste machtigingen toevoegt aan uw doelintegratie, kunt u het bovenstaande probleem beter oplossen.