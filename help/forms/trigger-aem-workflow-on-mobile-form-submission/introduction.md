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
source-git-commit: 5f42678502a785ead29982044d1f3f5ecf023e0f
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# De AEM van de trekker op een Mobiele Verzending van het Vorm

Een veel voorkomend geval is dat de XDP kan worden weergegeven als HTML voor activiteiten voor het vastleggen van gegevens. Bij het verzenden van dit formulier kan het nodig zijn een AEM workflow te activeren. In de AEM kunnen we de gegevens samenvoegen met de xdp-sjabloon en de gegenereerde PDF ter controle en goedkeuring presenteren. Het formulier wordt weergegeven op een publicatie-exemplaar en de workflow wordt geactiveerd op een AEM verwerkingsexemplaar.
De volgende stappen zijn betrokken bij het gebruiksgeval

* De gebruiker vult en legt een HTML5 vorm (HTML5 vertoning van XDP) voor.
* De verzending wordt afgehandeld door een servlet in de publicatie-instantie.
* De servlet slaat de gegevens in een vooraf bepaalde omslag in de bewaarplaats van de AEM verwerkingsinstantie op.
* Workflowstartprogramma is geconfigureerd om elke keer dat een nieuw bestand onder een bepaalde map wordt toegevoegd, een AEM workflow te activeren.

Deze zelfstudie doorloopt de stappen die nodig zijn voor het uitvoeren van het bovenstaande gebruiksgeval. De code van de steekproef en de activa met betrekking tot dit leerprogramma zijn [ hier beschikbaar.](./deploy-assets.md)


## Volgende stappen

[Formulierverzending verwerken](./handle-form-submission.md)