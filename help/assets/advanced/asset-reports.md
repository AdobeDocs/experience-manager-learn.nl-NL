---
title: Rapporten gebruiken in AEM Assets
description: 'AEM Assets biedt een rapportagekader op bedrijfsniveau dat schaalt voor grote opslagplaatsen door een intuïtieve gebruikerservaring. '
feature: reports
topics: authoring, operations, performance, metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
kt: 648
translation-type: tm+mt
source-git-commit: 3b5dd583a458393a41dbce1d8eeb0095a22db734
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 2%

---


# Rapporten gebruiken in AEM Assets{#using-reports-in-aem-assets}

AEM Assets biedt een rapportagekader op bedrijfsniveau dat schaalt voor grote opslagplaatsen door een intuïtieve gebruikerservaring.

>[!VIDEO](https://video.tv.adobe.com/v/22140/?quality=12&learn=on)

## Microsoft Excel-indelingen {#excel-formulas}

De volgende formules worden gebruikt in de video om de Middelen door de grafiek van de Grootte in Microsoft Excel te produceren.

### Middelgrootte normaliseren naar bytes {#asset-size-normalization-to-bytes}

```
=IF(RIGHT(D2,2)="KB",
      LEFT(D2,(LEN(D2)-2))*1024,
  IF(RIGHT(D2,2)="MB",
      LEFT(D2,(LEN(D2)-2))*1024*1024,
  IF(RIGHT(D2,2)="GB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024,
  IF(RIGHT(D2,2)="TB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024*1024, 0))))
```

### Aantal elementen op grootte {#asset-count-by-size}

#### Minder dan 200 kB {#less-than-kb}

```
=COUNTIFS(E2:E1000,"< 200000")
```

#### 200 kB tot 500 kB {#kb-to-kb}

```
=COUNTIFS(E2:E1000,">= 200000", E2:E1000,"<= 500000")
```

#### Meer dan 500 kB {#greater-than-kb}

```
=COUNTIFS(E2:E1000,"> 500000")
```

## Aanvullende bronnen{#additional-resources}

Alle [middelen Excel-bestand met diagram downloaden](./assets/asset-reports/all-assets.xlsx)
