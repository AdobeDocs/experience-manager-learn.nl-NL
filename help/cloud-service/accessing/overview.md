---
title: Toegang tot AEM as a Cloud Service configureren
description: AEM as a Cloud Service is de 'cloud-native' manier om de AEM-toepassingen te benutten en maakt als zodanig gebruik van Adobe IMS (Identity Management System) om het aanmelden van gebruikers, zowel beheerders als gewone gebruikers, bij de AEM Author-service te vergemakkelijken. Leer hoe Adobe IMS-gebruikers, -gebruikersgroepen en -productprofielen worden gebruikt in combinatie met AEM-groepen en -machtigingen om specifieke toegang tot AEM Author te bieden.
version: Experience Manager as a Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
jira: KT-5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
duration: 113
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 0%

---

# Toegang tot AEM as a Cloud Service configureren {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Inleiding tot Adobe IMS"
>abstract="AEM as a Cloud Service maakt gebruik van Adobe IMS (Identity Management System) om het aanmelden bij de AEM Author-service van gebruikers, zowel beheerders als gewone gebruikers, te vergemakkelijken. Leer hoe Adobe IMS-gebruikers, -groepen en -productprofielen worden gebruikt in overleg met AEM-groepen en -machtigingen om fijngeoriënteerde toegang tot de AEM Author-service te bieden."

AEM as a Cloud Service is de &#39;cloud-native&#39; manier om de AEM-toepassingen te benutten en maakt als zodanig gebruik van Adobe IMS (Identity Management System) om het aanmelden bij de AEM Author-service van gebruikers, zowel beheerders als gewone gebruikers, te vergemakkelijken.

![ Adobe Admin Console ](./assets/hero.png)

Leer hoe Adobe IMS-gebruikers, -groepen en -productprofielen worden gebruikt in overleg met AEM-groepen en -machtigingen om fijngeoriënteerde toegang tot de AEM Author-service te bieden.

## Adobe IMS-gebruikers

De gebruikers die toegang tot de dienst van de Auteur van AEM vereisen worden beheerd als [ gebruikers van Adobe IMS ](https://helpx.adobe.com/nl/enterprise/using/set-up-identity.html) in [ Adobe AdminConsole ](https://adminconsole.adobe.com). Leer wat Adobe IMS-gebruikers zijn en hoe ze in Admin Console worden benaderd en beheerd.

>[!NOTE]
>
>Wanneer een IMS-gebruiker uit AdminConsole wordt verwijderd, wordt deze niet automatisch verwijderd uit AEM, maar nadat de AEM-sessie(token) is verlopen, kan hij zich NIET aanmelden bij AEM.


[Meer informatie over Adobe IMS-gebruikers](./adobe-ims-users.md)

## Adobe IMS-gebruikersgroepen

De gebruikers die tot de dienst van de Auteur van AEM toegang hebben zouden in logische groepen moeten worden georganiseerd gebruikend [ de gebruikersgroepen van Adobe IMS ](https://helpx.adobe.com/nl/enterprise/using/user-groups.html) in [ Adobe AdminConsole ](https://adminconsole.adobe.com). De gebruikersgroepen van Adobe IMS verstrekken directe toestemmingen of geen toegang tot AEM (dit is de baan van [ het productprofielen van Adobe IMS ](#adobe-ims-product-profiles)), echter, zijn zij een grote manier om logische groepen gebruikers te bepalen die beurtelings aan specifieke niveaus van toegang in de dienst van de Auteur van AEM kunnen worden vertaald, gebruikend de groepen en de toestemmingen van AEM.

[Meer informatie over Adobe IMS-gebruikersgroepen](./adobe-ims-user-groups.md)

## Adobe IMS-productprofielen

[ de productprofielen van Adobe IMS ](https://helpx.adobe.com/nl/enterprise/using/manage-permissions-and-roles.html), die in [ Adobe AdminConsole ](https://adminconsole.adobe.com) worden beheerd, is het mechanisme dat [ gebruikers IMS van Adobe ](#adobe-ims-users) toegang tot login aan de dienst van de Auteur van AEM met een basisniveau van toegang verleent.

+ Het __gebruikers van AEM__ productprofiel verleent gebruikers read-only toegang tot AEM via lidmaatschap in de groep van Medewerkers van AEM.
+ Het __productprofiel van de Beheerders van AEM van 0&rbrace; &lbrace;verleent gebruikers volledige, administratieve toegang tot AEM.__

[Meer informatie over Adobe IMS-productprofielen](./adobe-ims-product-profiles.md)

## AEM-gebruikersgroepen en -machtigingen

Adobe Experience Manager bouwt verder op Adobe IMS-gebruikers, -gebruikersgroepen en -productprofielen om gebruikers aanpasbare toegang tot AEM te bieden. Leer hoe u AEM-groepen en -machtigingen kunt samenstellen en hoe u deze in samenwerking met Adobe IMS-abstracties kunt maken om AEM naadloos en aanpasbaar te maken.

[Meer informatie over AEM-gebruikers, -groepen en -machtigingen](./aem-users-groups-and-permissions.md)

## Doorloop voor toegang en machtigingen

Een verkorte procedure voor het configureren van Adobe IMS-gebruikers, gebruikersgroepen en productprofielen in Adobe AdminConsole en voor het benutten van deze Adobe IMS-abstracties in AEM Author om specifieke op groepen gebaseerde machtigingen te definiëren en te beheren.

[AEM-toegang en toegangsrechten doorlopen](./walk-through.md)

## Aanvullende Adobe Admin Console-bronnen

De volgende documentatie behandelt [&#128279;](https://adminconsole.adobe.com) - specifieke details van Adobe Admin Console  &lbrace;die in een beter inzicht in Adobe Admin Console kunnen helpen en het gebruiken om gebruikers en toegang over de producten van Experience Cloud te beheren.

+ [ Overzicht van de Identiteit van Adobe Admin Console ](https://helpx.adobe.com/nl/enterprise/using/identity.html)
+ [ Adobe Admin Console Admin rollen ](https://helpx.adobe.com/nl/enterprise/using/admin-roles.html)
+ [ de rollen van de Ontwikkelaar van Adobe Admin Console ](https://helpx.adobe.com/nl/enterprise/using/manage-developers.html)
