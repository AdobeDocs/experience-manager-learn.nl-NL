---
title: Trigger-AEM-workflow voor introductie van HTML5-formulier
description: Een AEM workflow activeren voor het verzenden van mobiele formulieren
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2024-09-17T00:00:00Z
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: c6ffa8f7a398b01fc12e1e2efe4382c941900496
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 0%

---

# De AEM van de trekker op een Mobiele Verzending van het Vorm

Een veel voorkomend geval is het weergeven van de XDP als HTML voor activiteiten voor het vastleggen van gegevens. Bij verzending van dit formulier kan het nodig zijn een AEM workflow te activeren. In de AEM workflow kunt u de gegevens samenvoegen met de XDP-sjabloon en de gegenereerde PDF ter controle en goedkeuring presenteren. Het formulier wordt gegenereerd op een publicatie-exemplaar en de workflow wordt geactiveerd op een AEM verwerkingsexemplaar.

De volgende stappen zijn betrokken bij het gebruiksgeval

* De gebruiker vult een HTML5-formulier (HTML-uitvoering van XDP) en verzendt dit.
* De verzending wordt afgehandeld door een servlet in de publicatie-instantie.
* De servlet slaat de gegevens in een vooraf bepaalde omslag in de bewaarplaats van de AEM verwerkingsinstantie op.
* Workflowstartprogramma is geconfigureerd om elke keer dat een nieuw bestand onder een bepaalde map wordt toegevoegd, een AEM workflow te activeren.

Deze zelfstudie doorloopt de stappen die nodig zijn voor het uitvoeren van het bovenstaande gebruiksgeval. De code van de steekproef en de activa met betrekking tot dit leerprogramma zijn [ hier beschikbaar.](./deploy-assets.md)


## Volgende stappen

[Formulierverzending verwerken](./handle-form-submission.md)
