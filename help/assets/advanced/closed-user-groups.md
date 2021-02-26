---
title: Gesloten gebruikersgroepen in AEM Assets
description: Gesloten gebruikersgroepen (CUG's) is een functie die wordt gebruikt om de toegang tot inhoud te beperken tot een bepaalde groep gebruikers op een gepubliceerde site. In deze video ziet u hoe groepen van gebruikers met gesloten deuren kunnen worden gebruikt met Adobe Experience Manager Assets om de toegang tot een specifieke map met middelen te beperken.
version: 6.3, 6.4, 6.5, cloud-service
topic: Beheer, beveiliging
feature: Gebruiker en groepen
role: Beheer
level: Intermediair
kt: 649
thumbnail: 22155.jpg
translation-type: tm+mt
source-git-commit: 407840a0e0c90c4f004390a052d036f9b69fa8df
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 0%

---


# Gesloten gebruikersgroepen{#using-closed-user-groups-with-aem-assets}

Gesloten gebruikersgroepen (CUG&#39;s) is een functie die wordt gebruikt om de toegang tot inhoud te beperken tot een bepaalde groep gebruikers op een gepubliceerde site. In deze video ziet u hoe groepen van gebruikers met gesloten deuren kunnen worden gebruikt met Adobe Experience Manager Assets om de toegang tot een specifieke map met middelen te beperken. Ondersteuning voor gesloten gebruikersgroepen met AEM Assets werd voor het eerst geïntroduceerd in AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Gesloten gebruikersgroep (CUG) met AEM Assets

* Ontworpen om toegang tot elementen op een AEM-publicatie-instantie te beperken.
* Subsidies lezen toegang tot een set gebruikers/groepen.
* CUG kan alleen op mapniveau worden geconfigureerd. CUG kan niet worden ingesteld voor afzonderlijke elementen.
* Het CUG-beleid wordt automatisch overgeërfd door submappen en toegepaste elementen.
* Het beleid van de CUG kan door subfolders worden met voeten getreden door een nieuw beleid van de GECG te plaatsen. Dit moet spaarzaam worden toegepast en wordt niet als een goede praktijk beschouwd.

## Gesloten gebruikersgroepen vs. Toegangsbeheerlijsten {#closed-user-groups-vs-access-control-lists}

Zowel worden de Gesloten Groepen van de Gebruiker (KUG) als de Lijsten van het Toegangsbeheer (ACL) gebruikt om toegang tot inhoud in AEM te controleren en op AEM gebruikers en groepen van de Veiligheid gebaseerd. De toepassing en implementatie van deze functies is echter heel anders. De volgende tabel geeft een overzicht van het onderscheid tussen de twee functies.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Beoogd gebruik | Vorm en pas toestemmingen voor inhoud op de **huidige** AEM instantie toe. | Configureer CUG-beleid voor inhoud op AEM **auteur**-instantie. CUG-beleid toepassen op inhoud op AEM **publish**-instantie(s). |
| Machtigingsniveaus | Bepaalt verleende/ontkende toestemmingen voor gebruikers/groepen voor alle niveaus: Lees, wijzig, creeer, schrap, leest ACL, geef ACL uit, herhaal. | Subsidies lezen toegang tot een set gebruikers/groepen. Weigert leestoegang tot *alle andere* gebruikers/groepen. |
| Publicatie | ACLs is *not* gepubliceerd met inhoud. | CUG-beleid *zijn* gepubliceerd met inhoud. |

## Ondersteunende koppelingen {#supporting-links}

* [Middelen en gesloten gebruikersgroepen beheren](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [Een gesloten gebruikersgroep maken](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Documentatie voor gesloten gebruikersgroep afbreken](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
