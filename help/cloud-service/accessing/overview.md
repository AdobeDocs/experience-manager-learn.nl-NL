---
title: Toegang tot AEM configureren als Cloud Service
description: 'AEM als Cloud Service is de cloud-native manier om de AEM toepassingen te benutten en maakt als zodanig gebruik van Adobe IMS (Identity Management System) om het aanmelden van gebruikers, zowel beheerders als gewone gebruikers, bij de AEM Author-service te vergemakkelijken. Leer hoe Adobe IMS-gebruikers, gebruikersgroepen en productprofielen allemaal worden gebruikt in combinatie met AEM groepen en machtigingen om specifieke toegang tot AEM-auteur te bieden.  '
feature: users-and-groups
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 0%

---


# Toegang tot AEM configureren als Cloud Service

AEM als Cloud Service is de cloud-native manier om de AEM toepassingen te benutten, en maakt als zodanig gebruik van Adobe IMS (Identity Management System) om het aanmelden van gebruikers, zowel beheerders als gewone gebruikers, bij AEM Author Service te vergemakkelijken. Leer hoe Adobe IMS-gebruikers, -groepen en -productprofielen samen met AEM groepen en machtigingen worden gebruikt om fijngeoriënteerde toegang tot de AEM Author-service te bieden.

## Adobe IMS-gebruikers

Gebruikers die toegang tot de AEM-auteurservice nodig hebben, worden als [Adobe IMS-gebruikers](https://helpx.adobe.com/nl/enterprise/using/set-up-identity.html) beheerd in [AdminConsole](https://adminconsole.adobe.com)van AEM-Adobe. Leer over welke gebruikers van Adobe IMS zijn, en hoe zij in Admin Console worden betreden en worden beheerd.

[Meer informatie over Adobe IMS-gebruikers](./adobe-ims-users.md)

## Adobe IMS-gebruikersgroepen

Gebruikers die toegang krijgen tot de AEM-auteurservice, moeten in logische groepen worden ingedeeld met gebruik van [Adobe IMS-gebruikersgroepen](https://helpx.adobe.com/enterprise/using/user-groups.html) in [Adobe AdminConsole](https://adminconsole.adobe.com). Adobe IMS-gebruikersgroepen bieden geen directe machtigingen of toegang tot AEM (dit is de taak van [Adobe IMS-productprofielen](#adobe-ims-product-profiles)), maar ze vormen een goede manier om logische gebruikersgroepen te definiëren die op hun beurt kunnen worden vertaald naar specifieke toegangsniveaus in de AEM-auteurservice, met behulp van AEM groepen en machtigingen.

[Meer informatie over Adobe IMS-gebruikersgroepen](./adobe-ims-user-groups.md)

## Adobe IMS-productprofielen

[Adobe IMS-productprofielen](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html), beheerd in [Adobe AdminConsole](https://adminconsole.adobe.com), zijn het mechanisme dat gebruikers [van](#adobe-ims-users) Adobe IMS toegang biedt tot aanmelding bij AEM Author-service met een basistoegangsniveau.

+ Het __AEM gebruikersprofiel__ biedt gebruikers alleen-lezen toegang tot AEM via lidmaatschap in AEM groep Medewerkers.
+ Het __AEM Beheerders__ -productprofiel biedt gebruikers volledige, beheerbare toegang tot AEM.

[Meer informatie over Adobe IMS-productprofielen](./adobe-ims-product-profiles.md)

## Gebruikersgroepen en machtigingen AEM

Adobe Experience Manager bouwt verder op Adobe IMS-gebruikers, gebruikersgroepen en productprofielen om gebruikers aanpasbare toegang tot AEM te bieden. Leer hoe u AEM groepen en machtigingen kunt samenstellen en hoe ze samenwerken met de abstracties van Adobe IMS om naadloze en aanpasbare toegang tot AEM te bieden.

[Meer informatie over AEM gebruiker, groepen en machtigingen](./aem-users-groups-and-permissions.md)

## Doorloop voor toegang en machtigingen

Een verkorte analyse die Adobe IMS gebruikers, gebruikersgroepen en productprofielen in Adobe AdminConsole vormt, en hoe te hefboomwerking deze Adobe IMS abstracties in Auteur AEM om specifieke op groep gebaseerde toestemmingen te bepalen en te beheren.

[AEM toegang en toestemmingen lopen door](./walk-through.md)

## Aanvullende Adobe Admin Console-bronnen

De volgende documentatie heeft betrekking op [Adobe Admin Console](https://adminconsole.adobe.com)-specifieke details en problemen die kunnen helpen bij een beter inzicht in de Adobe Admin Console en het gebruik ervan voor het beheer van gebruikers en toegang tot verschillende Experience Cloud-producten.

+ [Adobe Admin Console-identiteitsoverzicht](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console Admin-rollen](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console Developer-rollen](https://helpx.adobe.com/enterprise/using/manage-developers.html)