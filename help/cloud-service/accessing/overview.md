---
title: Toegang tot AEM configureren als Cloud Service
description: AEM als Cloud Service is de cloud-native manier om de AEM toepassingen te benutten en maakt als zodanig gebruik van Adobe IMS (Identity Management System) om het aanmelden van gebruikers, zowel beheerders als gewone gebruikers, bij de AEM Author-service te vergemakkelijken. Leer hoe u Adobe IMS-gebruikers, gebruikersgroepen en productprofielen gebruikt in combinatie met AEM groepen en machtigingen om specifieke toegang tot AEM-auteur te bieden.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
kt: 5882
thumbnail: KT-5882.jpg
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 0%

---

# Toegang tot AEM configureren als Cloud Service

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Inleiding tot Adobe IMS"
>abstract="AEM als Cloud Service gebruikt Adobe IMS (Identity Management System) om het aanmelden van zijn gebruikers, zowel beheerders als gewone gebruikers, naar de AEM Author-service te vergemakkelijken. Leer hoe Adobe IMS-gebruikers, -groepen en -productprofielen samen met AEM groepen en machtigingen worden gebruikt om fijngeoriënteerde toegang tot de AEM Author-service te bieden."

AEM als Cloud Service is de cloud-native manier om de AEM toepassingen te benutten, en maakt als zodanig gebruik van Adobe IMS (Identity Management System) om het aanmelden van gebruikers, zowel beheerders als gewone gebruikers, bij AEM Author Service te vergemakkelijken.

![Adobe Admin Console](./assets/hero.png)

Leer hoe Adobe IMS-gebruikers, -groepen en -productprofielen samen met AEM groepen en machtigingen worden gebruikt om fijngeoriënteerde toegang tot de AEM Author-service te bieden.

## Adobe IMS-gebruikers

Gebruikers die toegang tot de AEM-auteurservice nodig hebben, worden beheerd als [Adobe IMS-gebruikers](https://helpx.adobe.com/nl/enterprise/using/set-up-identity.html) in [AdminConsole](https://adminconsole.adobe.com). Leer wat Adobe IMS-gebruikers zijn en hoe ze in de Admin Console worden benaderd en beheerd.

[Meer informatie over Adobe IMS-gebruikers](./adobe-ims-users.md)

## Adobe IMS-gebruikersgroepen

Gebruikers die toegang krijgen tot de AEM-auteurservice moeten in logische groepen worden ingedeeld met [Adobe IMS-gebruikersgroepen](https://helpx.adobe.com/enterprise/using/user-groups.html) in [Adobe AdminConsole](https://adminconsole.adobe.com). Adobe IMS-gebruikersgroepen bieden geen directe machtigingen of toegang tot AEM (dit is de taak van [Adobe IMS-productprofielen](#adobe-ims-product-profiles)), maar zijn een goede manier om logische groepen gebruikers te definiëren die op hun beurt kunnen worden vertaald naar specifieke toegangsniveaus in de AEM-auteurservice, met behulp van AEM groepen en machtigingen.

[Meer informatie over Adobe IMS-gebruikersgroepen](./adobe-ims-user-groups.md)

## Adobe IMS-productprofielen

[Adobe IMS-productprofielen](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html), beheerd in  [AdminConsole](https://adminconsole.adobe.com), zijn het mechanisme dat  [Adobe IMS-](#adobe-ims-users) gebruikers toegang biedt tot aanmelding bij de AEM Author-service met een basistoegangsniveau.

+ Met het productprofiel __AEM Users__ kunnen gebruikers alleen-lezen toegang krijgen tot AEM via lidmaatschap van AEM groep Medewerkers.
+ Met het productprofiel __AEM Beheerders__ hebben gebruikers volledig beheerbare toegang tot AEM.

[Meer informatie over Adobe IMS-productprofielen](./adobe-ims-product-profiles.md)

## Gebruikersgroepen en machtigingen AEM

Adobe Experience Manager bouwt verder op Adobe IMS-gebruikers, gebruikersgroepen en productprofielen om gebruikers aanpasbare toegang tot AEM te bieden. Leer hoe u AEM groepen en machtigingen kunt samenstellen en hoe ze samenwerken met de abstracties van Adobe IMS om naadloze en aanpasbare toegang tot AEM te bieden.

[Meer informatie over AEM gebruiker, groepen en machtigingen](./aem-users-groups-and-permissions.md)

## Doorloop voor toegang en machtigingen

Een verkorte procedure waarbij u Adobe IMS-gebruikers, gebruikersgroepen en productprofielen configureert in Adobe AdminConsole en hoe u deze Adobe IMS-abstracties in AEM Author kunt gebruiken om specifieke op groepen gebaseerde machtigingen te definiëren en beheren.

[AEM toegang en toestemmingen lopen door](./walk-through.md)

## Aanvullende Adobe Admin Console-bronnen

De volgende documentatie behandelt [Adobe Admin Console](https://adminconsole.adobe.com)-specifieke details en zorgen die kunnen helpen bij een beter inzicht in de Adobe Admin Console en het gebruiken van het om gebruikers en toegang over de producten van Experience Cloud te beheren.

+ [Adobe Admin Console-identiteitsoverzicht](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console Admin-rollen](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console Developer-rollen](https://helpx.adobe.com/enterprise/using/manage-developers.html)
