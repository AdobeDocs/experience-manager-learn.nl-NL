---
title: Stijlsysteem gebruiken in AEM Forms
description: Het stijlbeleid voor de knopcomponent definiëren
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
exl-id: 52205a93-d03c-430c-a707-b351ab333939
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# De stijl definiëren in het beleid voor de component

* Meld u aan bij uw lokale AEM-exemplaar voor cloud en navigeer naar Tools | Algemeen | Sjablonen | uw projectnaam.

* Selecteer en open **Leeg met het malplaatje van de Componenten van de Kern** op geef wijze uit.
* Klik op het beleidspictogram van de knoopcomponent om de beleidsredacteur te openen.

* ![ knoop-beleid ](assets/button-policy.png)

Bepaal het beleid zoals hieronder getoond

![ knoop-beleid-details ](assets/styling-policy.png)

We hebben twee stijlen/variaties gedefinieerd: Marketing en Corporate. Deze variaties zijn gekoppeld aan corresponderende CSS-klassen.**gelieve ervoor te zorgen is er geen ruimte vóór en na de CSS klassennamen**.
Sla uw wijzigingen op.

| Stijl | CSS-klasse |
|-----------|------------------------------------|
| Marketing | cmp-adaptiveform-button-marketing |
| Bedrijf | cmp-adaptiveform-button-corporate |

Deze CSS-klassen worden gedefinieerd in het CSS-bestand (_button.scss) van de component.

## Volgende stappen

[CSS-klassen definiëren](./create-variations.md)
