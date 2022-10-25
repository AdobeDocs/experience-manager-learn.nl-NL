---
title: Toegang tot AEM as a Cloud Service configureren
description: AEM as a Cloud Service is de 'cloud-native' manier om de AEM toepassingen te benutten en maakt als zodanig gebruik van Adobe IMS (Identity Management System) om het aanmelden bij AEM Author Service van gebruikers, zowel beheerders als gewone gebruikers, te vergemakkelijken. Leer hoe u Adobe IMS-gebruikers, gebruikersgroepen en productprofielen gebruikt in combinatie met AEM groepen en machtigingen om specifieke toegang tot AEM-auteur te bieden.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
kt: 5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 0%

---

# Toegang tot AEM as a Cloud Service configureren {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Inleiding tot Adobe IMS"
>abstract="AEM as a Cloud Service maakt gebruik van Adobe IMS (Identity Management System) om het aanmelden bij AEM Author Service van gebruikers, zowel beheerders als gewone gebruikers, te vergemakkelijken. Leer hoe Adobe IMS-gebruikers, -groepen en -productprofielen worden gebruikt in overleg met AEM groepen en machtigingen om fijngeoriënteerde toegang tot de AEM Author-service te bieden."

AEM as a Cloud Service is de cloud-native manier om de AEM toepassingen te benutten en maakt als zodanig gebruik van Adobe IMS (Identity Management System) om het aanmelden bij AEM Author Service van gebruikers, zowel beheerders als gewone gebruikers, te vergemakkelijken.

![Adobe Admin Console](./assets/hero.png)

Leer hoe Adobe IMS-gebruikers, -groepen en -productprofielen worden gebruikt in overleg met AEM groepen en machtigingen om fijngeoriënteerde toegang tot de AEM Author-service te bieden.

## Adobe IMS-gebruikers

Gebruikers die toegang tot de AEM-auteurservice nodig hebben, worden beheerd als [Adobe IMS-gebruikers](https://helpx.adobe.com/nl/enterprise/using/set-up-identity.html) in [Adobe](https://adminconsole.adobe.com). Leer wat Adobe IMS-gebruikers zijn en hoe ze in de Admin Console worden benaderd en beheerd.

[Meer informatie over Adobe IMS-gebruikers](./adobe-ims-users.md)

## Adobe IMS-gebruikersgroepen

Gebruikers die toegang krijgen tot de AEM-auteurservice moeten in logische groepen worden ingedeeld met [Adobe IMS-gebruikersgroepen](https://helpx.adobe.com/enterprise/using/user-groups.html) in [Adobe](https://adminconsole.adobe.com). Adobe IMS-gebruikersgroepen bieden geen directe machtigingen of toegang tot AEM (dit is de taak van [Adobe IMS-productprofielen](#adobe-ims-product-profiles)), echter, zijn zij een grote manier om logische groepen gebruikers te bepalen die beurtelings aan specifieke niveaus van toegang in de dienst van de Auteur AEM kunnen worden vertaald, gebruikend AEM groepen en toestemmingen.

[Meer informatie over Adobe IMS-gebruikersgroepen](./adobe-ims-user-groups.md)

## Adobe IMS-productprofielen

[Adobe IMS-productprofielen](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html), beheerd in [Adobe](https://adminconsole.adobe.com)zijn de mechanismen die [Adobe IMS-gebruikers](#adobe-ims-users) toegang tot aanmelding bij de AEM-auteurservice met een basistoegangsniveau.

+ De __AEM__ Met het productprofiel hebben gebruikers alleen-lezentoegang tot AEM via lidmaatschap in AEM Contribute-groep.
+ De __AEM__ het productprofiel biedt gebruikers volledige, administratieve toegang tot AEM.

[Meer informatie over Adobe IMS-productprofielen](./adobe-ims-product-profiles.md)

## Gebruikersgroepen en machtigingen AEM

Adobe Experience Manager bouwt verder op Adobe IMS-gebruikers, -gebruikersgroepen en -productprofielen om gebruikers aanpasbare toegang tot AEM te bieden. Leer hoe u AEM groepen en machtigingen kunt samenstellen en hoe u deze in samenwerking met Adobe IMS-abstracties kunt maken om een naadloze en aanpasbare toegang tot AEM te bieden.

[Meer informatie over AEM gebruiker, groepen en machtigingen](./aem-users-groups-and-permissions.md)

## Doorloop voor toegang en machtigingen

Een verkorte procedure waarbij u Adobe IMS-gebruikers, gebruikersgroepen en productprofielen configureert in Adobe AdminConsole en hoe u deze Adobe IMS-abstracties in AEM Author kunt gebruiken om specifieke op groepen gebaseerde machtigingen te definiëren en beheren.

[AEM toegang en toestemmingen lopen door](./walk-through.md)

## Aanvullende Adobe Admin Console-bronnen

De volgende documentatie heeft betrekking op [Adobe Admin Console](https://adminconsole.adobe.com)-specifieke details en zorgen die kunnen helpen om de Adobe Admin Console beter te begrijpen en te gebruiken om gebruikers te beheren en toegang te krijgen tot verschillende Experience Cloud-producten.

+ [Adobe Admin Console-identiteitsoverzicht](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console Admin-rollen](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console Developer-rollen](https://helpx.adobe.com/enterprise/using/manage-developers.html)
