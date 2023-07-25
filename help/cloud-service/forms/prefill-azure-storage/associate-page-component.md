---
title: Paginacomponent koppelen aan de nieuwe aangepaste formuliersjabloon
description: Een nieuw pagina-onderdeel maken
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---

# De paginacomponent aan de sjabloon koppelen

De volgende stap bestaat uit het koppelen van de pagina-component aan de nieuwe aangepaste formuliersjabloon. Zo zorgt u ervoor dat de code in de paginacomponent wordt uitgevoerd telkens wanneer een adaptief formulier wordt gegenereerd dat is gebaseerd op de nieuwe sjabloon. In deze zelfstudie wordt een nieuw, adaptief formuliersjabloon, **StoreAndRestoreFromAzure** is gemaakt in het dialoogvenster **AzurePortalStorage** map.
Navigeer naar /conf/AzurePortalStorage/settings/wcm/templates/storeandrestaurefromazure/initial/jcr:content node, voeg de volgende eigenschap toe en sla de wijzigingen op.

| **Eigenschapnaam** | **Type eigenschap** | **Waarde van eigenschap** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | String | azureportalpagecomponent/component/page/storeandfetch |

Navigeer naar /conf/AzurePortalStorage/settings/wcm/templates/storeandrestaurefromazure/structure/jcr:content node, voeg de volgende eigenschap toe en sla de wijzigingen op.
| **Eigenschapnaam**  | **Type eigenschap** | **Waarde van eigenschap**                                    | |—|—|—| | sling:resourceType | String | azureportalpagecomponent/component/page/storeandfetch |


## Volgende stappen

[Integratie met Azure Storage maken](./create-fdm.md)
