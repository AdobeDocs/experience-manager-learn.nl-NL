---
title: AEM Sites met tag-eigenschap verbinden met IMS
description: Leer hoe u AEM Sites in AEM verbindt met eigenschap Tag met behulp van IMS-configuratie. Deze instelling verifieert AEM met de API voor starten en stelt AEM in staat te communiceren via de API's voor starten voor toegang tot de eigenschappen van tags.
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
duration: 63
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# AEM Sites met tag-eigenschap verbinden met IMS{#connect-aem-and-tag-property-using-ims}

>[!NOTE]
>
>Het proces om Adobe Experience Platform Launch als reeks technologieën van de gegevensinzameling anders te noemen wordt uitgevoerd in AEM product UI, inhoud, en documentatie, zodat wordt de term Lanceren nog gebruikt hier.

Leer hoe u in AEM verbinding kunt maken met AEM met eigenschap Tag met behulp van de IMS-configuratie (Identity Management System). Deze instelling verifieert AEM met de API voor starten en stelt AEM in staat te communiceren via de API&#39;s voor starten voor toegang tot de eigenschappen van tags.

## IMS-configuratie maken of opnieuw gebruiken

De IMS-configuratie die gebruikmaakt van het Adobe Developer Console-project is vereist om AEM te integreren met de nieuwe tageigenschap. Met deze configuratie kunnen AEM communiceren met de toepassing Tags met behulp van de API&#39;s voor starten en IMS wordt het beveiligingsaspect van deze integratie afgehandeld.

Wanneer een AEM als Cloud Service wordt verstrekt worden een paar configuraties IMS zoals Asset compute, Adobe Analytics, en de Lancering van de Adobe automatisch gecreeerd. De automatisch gemaakte **Adobe starten** De configuratie IMS kan worden gebruikt of een nieuwe configuratie IMS zou moeten worden gecreeerd als u AEM milieu 6.X gebruikt.

Automatisch gemaakte revisie **Adobe starten** IMS-configuratie met de volgende stappen.

1. In AEM opent u de **Gereedschappen** menu

1. Selecteer Adobe IMS Configurations in de sectie Beveiliging.

1. Selecteer de **Adobe starten** kaart en klik op **Eigenschappen**, bekijk de details van **Certificaat** en **Account** tabs. Klik vervolgens op **Annuleren** om terug te keren zonder automatisch gemaakte details te wijzigen.

1. Selecteer de **Adobe starten** kaart en deze keer klikken **Health controleren** moet u de **Succes** bericht zoals hieronder.

   ![Adobe Start gezonde IMS-configuratie](assets/adobe-launch-healthy-ims-config.png)


## Volgende stappen

[Een configuratie van de Cloud Service Launch maken in AEM](create-aem-launch-cloud-service.md)
