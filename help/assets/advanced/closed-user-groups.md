---
title: Gesloten gebruikersgroepen in AEM Assets
description: 'Gesloten gebruikersgroepen (CUG''s) is een functie die wordt gebruikt om de toegang tot inhoud te beperken tot een bepaalde groep gebruikers op een gepubliceerde site. In deze video ziet u hoe groepen van gebruikers met gesloten deuren kunnen worden gebruikt met Adobe Experience Manager Assets om de toegang tot een specifieke map met middelen te beperken. Ondersteuning voor gesloten gebruikersgroepen met AEM Assets werd voor het eerst geïntroduceerd in AEM 6.4. '
feature: asset-share
topics: authoring, collaboration, operations, sharing
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 0%

---


# Gesloten gebruikersgroepen{#using-closed-user-groups-with-aem-assets}

Gesloten gebruikersgroepen (CUG&#39;s) is een functie die wordt gebruikt om de toegang tot inhoud te beperken tot een bepaalde groep gebruikers op een gepubliceerde site. In deze video ziet u hoe groepen van gebruikers met gesloten deuren kunnen worden gebruikt met Adobe Experience Manager Assets om de toegang tot een specifieke map met middelen te beperken. Ondersteuning voor gesloten gebruikersgroepen met AEM Assets werd voor het eerst geïntroduceerd in AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=9&learn=on)

## Gesloten gebruikersgroep (CUG) met AEM Assets

* Ontworpen om toegang tot elementen op een AEM-publicatie-instantie te beperken.
* Subsidies lezen toegang tot een set gebruikers/groepen.
* CUG kan alleen op mapniveau worden geconfigureerd. CUG kan niet worden ingesteld voor afzonderlijke elementen.
* Het CUG-beleid wordt automatisch overgeërfd door submappen en toegepaste elementen.
* Het beleid van de CUG kan door subfolders worden met voeten getreden door een nieuw beleid van de GECG te plaatsen. Dit moet spaarzaam worden toegepast en wordt niet als een goede praktijk beschouwd.

## CUG-weergave in het JCR {#cug-representation-in-the-jcr}

![CUG-weergave in het JCR](assets/closed-user-groups/folder-properties-closed-user-groups.png)

We.Retail-leden Group toegevoegd als een gesloten gebruikersgroep aan de map: /content/dam/we-retail/nl/beta-products

Er wordt een mengsel van **rep:CugMixin** toegepast op de map **/content/dam/we-retail/en/beta-products**. Een knoop van **rep:cugPolicy** wordt toegevoegd onder de omslag en wij-kleinhandelsleden wordt gespecificeerd als hoofd. Een andere combinatie van **granite:AuthenticationRequired** wordt toegepast op de map beta-products en de eigenschap** granite:loginPath** geeft aan welke aanmeldingspagina moet worden gebruikt als een gebruiker niet is geverifieerd en probeert een element aan te vragen onder de map **bètaproducten**.

Beschrijving van JCR hieronder:

```xml
/beta-products
    - jcr:primaryType = sling:Folder
    - jcr:mixinTypes = rep:CugMixin, granite:AuthenticationRequired
    - granite:loginPath = /content/we-retail/us/en/community/signin
    + rep:cugPolicy
         - jcr:primaryType = rep:CugPolicy
         - rep:principalNames = we-retail-members
```

## Gesloten gebruikersgroepen vs. Toegangsbeheerlijsten {#closed-user-groups-vs-access-control-lists}

Zowel worden de Gesloten Groepen van de Gebruiker (KUG) als de Lijsten van het Toegangsbeheer (ACL) gebruikt om toegang tot inhoud in AEM te controleren en op AEM gebruikers en groepen van de Veiligheid gebaseerd. De toepassing en implementatie van deze functies is echter heel anders. De volgende tabel geeft een overzicht van het onderscheid tussen de twee functies.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Beoogd gebruik | Vorm en pas toestemmingen voor inhoud op de **huidige** AEM instantie toe. | Configureer CUG-beleid voor inhoud op AEM **auteur**-instantie. CUG-beleid toepassen op inhoud op AEM **publish**-instantie(s). |
| Machtigingsniveaus | Bepaalt verleende/ontkende toestemmingen voor gebruikers/groepen voor alle niveaus: Lees, wijzig, creeer, schrap, leest ACL, geef ACL uit, herhaal. | Subsidies lezen toegang tot een set gebruikers/groepen. Weigert leestoegang tot alle andere gebruikers/groepen. |
| Replicatie | ACLs wordt niet herhaald met inhoud. | CUG-beleid wordt gerepliceerd met inhoud. |

## Ondersteunende koppelingen {#supporting-links}

* [Middelen en gesloten gebruikersgroepen beheren](https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html#ClosedUserGroup)
* [Een gesloten gebruikersgroep maken](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/cug.html)
* [Documentatie voor gesloten gebruikersgroep afbreken](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
