---
title: AEM-workflow activeren op HTML5 Formulierverzendintroductie
description: Een AEM-workflow activeren voor het verzenden van mobiele formulieren
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2024-09-17T00:00:00Z
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: ce51b25f-6069-4799-9a61-98c0a672e821
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 0%

---

# De AEM-workflow activeren op een mobiel formulier verzenden

Een veel voorkomend geval is het weergeven van de XDP als HTML voor activiteiten voor het vastleggen van gegevens. Bij verzending van dit formulier kan het nodig zijn een AEM-workflow te activeren. In de AEM-workflow kunt u de gegevens samenvoegen met de XDP-sjabloon en de gegenereerde PDF ter controle en goedkeuring presenteren. Het formulier wordt gegenereerd op een publicatie-exemplaar en de workflow wordt geactiveerd op een AEM-verwerkingsexemplaar.

De volgende stappen zijn betrokken bij het gebruiksgeval

* De gebruiker vult een HTML5-formulier (HTML5-uitvoering van XDP) en verzendt dit.
* De verzending wordt afgehandeld door een servlet in de publicatie-instantie.
* De server slaat de gegevens op in een vooraf gedefinieerde map in de opslagplaats van de AEM-verwerkingsinstantie.
* Workflowstartprogramma is geconfigureerd om elke keer dat een nieuw bestand onder een bepaalde map wordt toegevoegd, een AEM-workflow te activeren.

Deze zelfstudie doorloopt de stappen die nodig zijn voor het uitvoeren van het bovenstaande gebruiksgeval. De code van de steekproef en de activa met betrekking tot dit leerprogramma zijn [ hier beschikbaar.](./deploy-assets.md)


## Volgende stappen

[Formulierverzending verwerken](./handle-form-submission.md)
