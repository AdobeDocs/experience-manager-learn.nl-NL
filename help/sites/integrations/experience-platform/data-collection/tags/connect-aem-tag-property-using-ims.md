---
title: AEM Sites met tag-eigenschap verbinden met IMS
description: Leer hoe u AEM Sites in AEM verbindt met eigenschap Tag met behulp van IMS-configuratie.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5981
thumbnail: 38555.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
duration: 50
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# AEM Sites met tag-eigenschap verbinden met IMS{#connect-aem-and-tag-property-using-ims}

Leer hoe u AEM met eigenschap tags verbindt met behulp van de IMS-configuratie (Identity Management System) in AEM. Deze instelling verifieert AEM met de API voor tags en stelt AEM in staat te communiceren via de API&#39;s voor tags voor toegang tot de eigenschappen van tags.

## IMS-configuratie maken of opnieuw gebruiken

De IMS-configuratie die gebruikmaakt van het Adobe Developer Console-project is vereist om AEM te integreren met de nieuwe tageigenschap. Dankzij deze configuratie kunnen AEM communiceren met de toepassing Tags met behulp van tags-API&#39;s en IMS wordt het beveiligingsaspect van deze integratie verwerkt.

Wanneer een AEM als Cloud Service-omgeving wordt ingericht, worden een aantal IMS-configuraties zoals Asset compute, Adobe Analytics en tags automatisch gemaakt. De automatisch gemaakte **tags in Adobe Experience Platform** De configuratie IMS kan worden gebruikt of een nieuwe configuratie IMS zou moeten worden gecreeerd als u AEM milieu 6.X gebruikt.

Automatisch gemaakte revisie **tags in Adobe Experience Platform** IMS-configuratie met de volgende stappen.

1. In AEM Auteur opent u de knop **Gereedschappen** menu
1. Selecteer Adobe IMS Configurations in de sectie Beveiliging.
1. Selecteer de **Adobe starten** kaart en klik op **Eigenschappen**, bekijk de details van **Certificaat** en **Account** tabs. Klik vervolgens op **Annuleren** om terug te keren zonder automatisch gemaakte details te wijzigen.
1. Selecteer de **Adobe starten** kaart en deze keer klikken **Health controleren** moet u de **Succes** bericht zoals hieronder.

   ![Tags Gezonde IMS-configuratie](assets/adobe-launch-healthy-ims-config.png)

## Volgende stappen

[Een configuratie voor de Cloud Service van tags maken in AEM](create-aem-launch-cloud-service.md)
