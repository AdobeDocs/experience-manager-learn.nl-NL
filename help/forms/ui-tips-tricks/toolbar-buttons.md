---
title: Plaats de knoppen Volgende en Vorige op de werkbalk
description: De werkbalkknoppen verdelen
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9270
source-git-commit: 96b78ff5056bd9c2be39fb2cf21b4f92863af089
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# De werkbalkknop verdelen

Wanneer u de knoppen Volgende en Vorige aan de werkbalk in AEM Forms toevoegt, worden de knoppen standaard naast elkaar geplaatst. U kunt de knop Volgende helemaal rechts op de werkbalk drukken terwijl u de knop Vorige/Vorige links houdt

![werkbalkafstand](assets/toolbar-spacing.png)


## De werkbalk opmaken

Het bovenstaande gebruiksgeval kan eenvoudig worden verwezenlijkt door de stijlredacteur te gebruiken. Nadat u de knop Vorige/Volgende aan de werkbalk hebt toegevoegd, selecteert u de laag Stijl in het menu Bewerken. Selecteer de stijlmodus en selecteer de werkbalk om het stijleigenschappenblad te openen. Vouw de sectie Dimension en Positie uit en controleer of u alle eigenschappen ziet. De volgende eigenschappen instellen
* Dimension en positie
   * Breedte: 100%
   * Positie: relatief

Uw wijzigingen opslaan

## De knop Volgende opmaken

Selecteer de knop Volgende en zorg ervoor dat u het stijleigenschappenblad van de volgende knop (niet de volgende knoptekst) opent. De volgende eigenschappen instellen
* Dimension en positie
   * positie: absolute top 1 px rechts 1 px
* Rand
   * Straal rand: 4px(Boven,Rechts,Onder,Links)

Uw wijzigingen opslaan
