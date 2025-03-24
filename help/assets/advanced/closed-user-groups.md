---
title: Gesloten gebruikersgroepen in AEM Assets
description: Gesloten gebruikersgroepen (CUG's) is een functie die wordt gebruikt om de toegang tot inhoud te beperken tot een bepaalde groep gebruikers op een gepubliceerde site. In deze video ziet u hoe gebruikersgroepen met gesloten deuren met Adobe Experience Manager Assets kunnen worden gebruikt om de toegang tot een specifieke map met elementen te beperken.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-649
thumbnail: 22155.jpg
last-substantial-update: 2022-06-06T00:00:00Z
doc-type: Feature Video
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
duration: 321
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# Gesloten gebruikersgroepen{#using-closed-user-groups-with-aem-assets}

Gesloten gebruikersgroepen (CUG&#39;s) is een functie die wordt gebruikt om de toegang tot inhoud te beperken tot een bepaalde groep gebruikers op een gepubliceerde site. In deze video ziet u hoe gebruikersgroepen met gesloten deuren met Adobe Experience Manager Assets kunnen worden gebruikt om de toegang tot een specifieke map met elementen te beperken. Ondersteuning voor gesloten gebruikersgroepen met AEM Assets werd voor het eerst geïntroduceerd in AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Gebruikersgroep (CUG) met AEM Assets

* Ontworpen om de toegang tot elementen op een AEM-publicatie-instantie te beperken.
* Subsidies lezen toegang tot een set gebruikers/groepen.
* CUG kan alleen op mapniveau worden geconfigureerd. CUG kan niet worden ingesteld voor afzonderlijke elementen.
* Het CUG-beleid wordt automatisch overgeërfd door submappen en toegepaste elementen.
* Het beleid van de CUG kan door subfolders worden met voeten getreden door een nieuw beleid van de GECG te plaatsen. Dit moet spaarzaam worden toegepast en wordt niet als een goede praktijk beschouwd.

## Gesloten gebruikersgroepen vs. Toegangsbeheerlijsten {#closed-user-groups-vs-access-control-lists}

Zowel worden de Gesloten Groepen van de Gebruiker (CUG) als de Lijsten van het Toegangsbeheer (ACL) gebruikt om toegang tot inhoud in AEM te controleren en die op de gebruikers en de groepen van de Veiligheid van AEM wordt gebaseerd. De toepassing en implementatie van deze functies is echter heel anders. De volgende tabel geeft een overzicht van het onderscheid tussen de twee functies.

|                   | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Beoogd gebruik | Vorm en pas toestemmingen voor inhoud op de **huidige** instantie van AEM toe. | Vorm het beleid van de CUG voor inhoud op AEM **auteur** instantie. Pas het beleid van de CUG voor inhoud op AEM **toe publiceert** instantie(s). |
| Machtigingsniveaus | Bepaalt verleende/ontkende toestemmingen voor gebruikers/groepen voor alle niveaus: Lees, wijzig, creeer, schrap, leest ACL, geef ACL uit, kopieer. | Subsidies lezen toegang tot een set gebruikers/groepen. Ontkent leestoegang tot *alle andere* gebruikers/groepen. |
| Publicatie | ACLs wordt *niet* gepubliceerd met inhoud. | Het beleid van de CUG *wordt* gepubliceerd met inhoud. |

## Ondersteunende koppelingen {#supporting-links}

* [ het Leiden Assets en Gesloten Gebruikersgroepen ](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [ Creërend een Gesloten Groep van de Gebruiker ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [ Oak Gesloten Documentatie van de Groep van de Gebruiker ](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
