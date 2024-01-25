---
title: Toegang tot AEM as a Cloud Service configureren
description: AEM as a Cloud Service is de cloud-native manier om de AEM toepassingen te benutten en maakt als zodanig gebruik van Adobe IMS (Identity Management System) om het aanmelden van gebruikers, zowel beheerders als gewone gebruikers, voor AEM Auteur-service te vergemakkelijken. Leer hoe u Adobe IMS-gebruikers, gebruikersgroepen en productprofielen gebruikt in combinatie met AEM groepen en machtigingen om specifieke toegang te verlenen aan AEM auteur.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
jira: KT-5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
duration: 126
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 0%

---

# Toegang tot AEM as a Cloud Service configureren {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Inleiding tot Adobe IMS"
>abstract="AEM as a Cloud Service maakt gebruik van Adobe IMS (Identity Management System) om het aanmelden van gebruikers, zowel beheerders als gewone gebruikers, voor AEM Auteursservice te vergemakkelijken. Leer hoe Adobe IMS-gebruikers, -groepen en -productprofielen worden gebruikt in overleg met AEM groepen en machtigingen om fijngeoriënteerde toegang tot AEM Auteur-service te bieden."

AEM as a Cloud Service is de cloud-native manier om de AEM toepassingen te benutten en maakt als zodanig gebruik van Adobe IMS (Identity Management System) om het aanmelden van gebruikers, zowel beheerders als gewone gebruikers, voor AEM Auteur-service te vergemakkelijken.

![Adobe Admin Console](./assets/hero.png)

Leer hoe Adobe IMS-gebruikers, -groepen en -productprofielen worden gebruikt in overleg met AEM groepen en machtigingen om fijngeoriënteerde toegang tot AEM Auteur-service te bieden.

## Adobe IMS-gebruikers

Gebruikers die toegang tot AEM Auteur-service nodig hebben, worden beheerd als [Adobe IMS-gebruikers](https://helpx.adobe.com/nl/enterprise/using/set-up-identity.html) in [AdminConsole van Adobe](https://adminconsole.adobe.com). Leer wat Adobe IMS-gebruikers zijn en hoe ze in de Admin Console worden benaderd en beheerd.

>[!NOTE]
>
>Wanneer een IMS-gebruiker uit AdminConsole wordt verwijderd, wordt deze niet automatisch verwijderd uit AEM, maar wanneer AEM sessie (token) is verlopen, kan deze zich NIET aanmelden bij AEM.


[Meer informatie over Adobe IMS-gebruikers](./adobe-ims-users.md)

## Adobe IMS-gebruikersgroepen

Gebruikers die toegang krijgen tot AEM Auteur-service moeten in logische groepen worden ingedeeld met [Adobe IMS-gebruikersgroepen](https://helpx.adobe.com/enterprise/using/user-groups.html) in [AdminConsole van Adobe](https://adminconsole.adobe.com). Adobe IMS-gebruikersgroepen bieden geen directe machtigingen of toegang tot AEM (dit is de taak van [Adobe IMS-productprofielen](#adobe-ims-product-profiles)), echter, zijn zij een grote manier om logische groeperingen van gebruikers te bepalen die beurtelings aan specifieke niveaus van toegang in AEM dienst van de Auteur kunnen worden vertaald, gebruikend AEM groepen en toestemmingen.

[Meer informatie over Adobe IMS-gebruikersgroepen](./adobe-ims-user-groups.md)

## Adobe IMS-productprofielen

[Adobe IMS-productprofielen](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html), beheerd in [AdminConsole van Adobe](https://adminconsole.adobe.com)zijn de mechanismen die [Adobe IMS-gebruikers](#adobe-ims-users) toegang tot login aan AEM dienst van de Auteur met een basisniveau van toegang.

+ De __AEM__ Met het productprofiel hebben gebruikers alleen-lezentoegang tot AEM via lidmaatschap in AEM Contribute-groep.
+ De __AEM__ het productprofiel biedt gebruikers volledige, administratieve toegang tot AEM.

[Meer informatie over Adobe IMS-productprofielen](./adobe-ims-product-profiles.md)

## Gebruikersgroepen en machtigingen AEM

Adobe Experience Manager bouwt verder op Adobe IMS-gebruikers, -gebruikersgroepen en -productprofielen om gebruikers aanpasbare toegang tot AEM te bieden. Leer hoe u AEM groepen en machtigingen kunt samenstellen en hoe u deze in samenwerking met Adobe IMS-abstracties kunt maken om een naadloze en aanpasbare toegang tot AEM te bieden.

[Meer informatie over AEM gebruiker, groepen en machtigingen](./aem-users-groups-and-permissions.md)

## Doorloop voor toegang en machtigingen

Een verkorte procedure door gebruikers, gebruikersgroepen en productprofielen van Adobe IMS in Adobe AdminConsole te vormen, en hoe te om deze abstracties van Adobe IMS in AEM Auteur te gebruiken om specifieke op groep gebaseerde toestemmingen te bepalen en te beheren.

[AEM toegang en toestemmingen lopen door](./walk-through.md)

## Aanvullende Adobe Admin Console-bronnen

De volgende documentatie heeft betrekking op [Adobe Admin Console](https://adminconsole.adobe.com)-specifieke details en zorgen die kunnen helpen om de Adobe Admin Console beter te begrijpen en te gebruiken om gebruikers te beheren en toegang te krijgen tot producten in de Experience Cloud.

+ [Adobe Admin Console-identiteitsoverzicht](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console Admin-rollen](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console Developer-rollen](https://helpx.adobe.com/enterprise/using/manage-developers.html)
