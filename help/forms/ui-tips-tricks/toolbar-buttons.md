---
title: Plaats de knoppen Volgende en Vorige op de werkbalk
description: De werkbalkknoppen verdelen
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9291
exl-id: 1b55b6d2-3bab-4907-af89-c81a3b1a44cb
last-substantial-update: 2019-07-07T00:00:00Z
duration: 39
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 0%

---

# De werkbalkknop verdelen

Wanneer u de knoppen Volgende en Vorige aan de werkbalk in AEM Forms toevoegt, worden de knoppen standaard naast elkaar geplaatst. U kunt de knop Volgende helemaal naar rechts op de werkbalk drukken terwijl u de knop Vorige/Vorige links houdt

![ toolbar-uit elkaar plaatsen ](assets/toolbar-spacing.png)


## De werkbalk opmaken

Het bovenstaande gebruiksgeval kan eenvoudig worden verwezenlijkt door de stijlredacteur te gebruiken. Nadat u de knop Vorige/Volgende aan de werkbalk hebt toegevoegd, selecteert u de laag Stijl in het menu Bewerken. Selecteer de stijlmodus en selecteer de werkbalk om de stijlpagina te openen. Vouw de sectie Dimensies en Positie uit en controleer of u alle eigenschappen ziet. De volgende eigenschappen instellen
* Afmetingen en positie
   * Breedte: 100%
   * Positie: relatief

Uw wijzigingen opslaan

## De knop Volgende opmaken

Selecteer de knop Volgende en zorg ervoor dat u het stijleigenschappenblad van de volgende knop (niet de volgende knoptekst) opent. De volgende eigenschappen instellen
* Afmetingen en positie
   * positie: absoluut 1 px rechtsboven, 1 px
* Rand
   * Randstraal: 4 px (boven, rechts, onder, links)

Uw wijzigingen opslaan
