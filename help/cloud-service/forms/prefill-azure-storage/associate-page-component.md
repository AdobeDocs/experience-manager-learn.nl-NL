---
title: Paginacomponent koppelen aan de nieuwe aangepaste formuliersjabloon
description: Een nieuw pagina-onderdeel maken
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
exl-id: 7b2b1e1c-820f-4387-a78b-5d889c31eec0
duration: 25
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---

# De paginacomponent aan de sjabloon koppelen

De volgende stap bestaat uit het koppelen van de pagina-component aan de nieuwe aangepaste formuliersjabloon. Zo zorgt u ervoor dat de code in de paginacomponent wordt uitgevoerd telkens wanneer een adaptief formulier wordt gegenereerd dat is gebaseerd op de nieuwe sjabloon. Voor dit leerprogramma werd een nieuw adaptief vormmalplaatje genoemd **StoreAndRestoreFromAzure** gecreeerd in de **AzurePortalStorage** omslag.
Navigeer naar /conf/AzurePortalStorage/settings/wcm/templates/storeandrestaurefromazure/initial/jcr:content node, voeg de volgende eigenschap toe en sla de wijzigingen op.

| **de Naam van het Bezit** | **Type van Bezit** | **Waarde van het Bezit** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | String | azureportalpagecomponent/component/page/storeandfetch |

Navigeer naar /conf/AzurePortalStorage/settings/wcm/templates/storeandrestaurefromazure/structure/jcr:content node, voeg de volgende eigenschap toe en sla de wijzigingen op.

| **de Naam van het Bezit** | **Type van Bezit** | **Waarde van het Bezit** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | String | azureportalpagecomponent/component/page/storeandfetch |


## Volgende stappen

[Integratie met Azure Storage maken](./create-fdm.md)
