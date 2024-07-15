---
title: De linkernavigatietabels opmaken met pictogrammen
description: Pictogrammen toevoegen om actieve en voltooide tabbladen aan te geven
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9359
exl-id: f7c1f991-0486-4355-8502-cd5b038537e3
last-substantial-update: 2019-07-07T00:00:00Z
duration: 68
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '328'
ht-degree: 0%

---

# Pictogrammen toevoegen om actieve en voltooide tabbladen aan te geven

Als u een adaptief formulier hebt met linkertabnavigatie, kunt u pictogrammen weergeven om de status van de tab aan te geven. U wilt bijvoorbeeld een pictogram weergeven om het actieve tabblad en het actieve pictogram aan te geven om het voltooide tabblad aan te geven, zoals in de onderstaande schermafbeelding wordt getoond.

![ toolbar-uit elkaar plaatsen ](assets/active-completed.png)

## Een adaptief formulier maken

Een eenvoudig adaptief formulier op basis van de sjabloon Standaard en het thema Canvas 3.0 is gebruikt om het voorbeeldformulier te maken.
De [ pictogrammen die in dit artikel ](assets/icons.zip) worden gebruikt kunnen van hier worden gedownload.


## De standaardstatus opmaken

Het formulier openen in de bewerkingsmodus
Zorg ervoor dat u zich in de stijllaag bevindt en selecteer een willekeurig tabblad (bijvoorbeeld het tabblad Algemeen).
U bevindt zich in de standaardstatus wanneer u de stijleditor voor de tab opent, zoals wordt weergegeven in de onderstaande schermafbeelding
![ navigatie-lusje ](assets/navigation-tab.png)

CSS-eigenschappen instellen voor de standaardstatus zoals hieronder wordt weergegeven
| Categorie | Eigenschapnaam  |  Waarde van eigenschap |
|:—|:—|:—|
| Dimensionen en positie | Breedte | 50 px |
| Tekst | Tekendikte| Vet |
| Tekst | Kleur | #FFF |
|Tekst | Lijnhoogte| 3 |
|Tekst  | Tekst uitlijnen | Links |
|Achtergrond| Kleur | #056dae |

Uw wijzigingen opslaan

## De actieve status opmaken

Zorg ervoor dat u de status Actief hebt en maak de volgende CSS-eigenschappen op

| Categorie | Eigenschapnaam | Waarde van eigenschap |
|:---|:---|:---|
| Dimensionen en positie | Breedte | 50 px |
| Tekst | Dikte lettertype | Vet |
| Tekst | Kleur | #FFF |
| Tekst | Lijnhoogte | 3 |
| Tekst | Tekst uitlijnen | Links |
| Achtergrond | Kleur | #056dae |

De achtergrondafbeelding opmaken zoals wordt weergegeven in de onderstaande schermafbeelding

Sla uw wijzigingen op.



![ actief-staat ](assets/active-state.png)

## De bezochte staat opmaken

Zorg ervoor dat u in de bezochte staat bent en stijl de volgende eigenschappen

| Categorie | Eigenschapnaam | Waarde van eigenschap |
|:---|:---|:---|
| Dimensionen en positie | Breedte | 50 px |
| Tekst | Dikte lettertype | Vet |
| Tekst | Kleur | #FFF |
| Tekst | Lijnhoogte | 3 |
| Tekst | Tekst uitlijnen | Links |
| Achtergrond | Kleur | #056dae |

De achtergrondafbeelding opmaken zoals wordt weergegeven in de onderstaande schermafbeelding


![ bezocht-staat ](assets/visited-state.png)

Uw wijzigingen opslaan

Bekijk een voorbeeld van het formulier en test of de pictogrammen werken zoals u had verwacht.
