---
title: Metagegevens importeren en exporteren in AEM Assets gebruiken
description: Leer hoe u de metagegevensfuncties voor importeren en exporteren van Adobe Experience Manager Assets kunt gebruiken. Met de import- en exportmogelijkheden kunnen auteurs van inhoud de metagegevens van updates voor bestaande elementen bulksgewijs verzenden.
version: 6.3, 6.4, 6.5, cloud-service
topics: Content Management
feature: Metagegevens
role: Beheer
level: Intermediair
kt: 647, 917
thumbnail: 22132.jpg
translation-type: tm+mt
source-git-commit: 0d012d61b7740e461e641dddd6c5255a022305ea
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 2%

---


# Metagegevens importeren en exporteren gebruiken in AEM Assets {#metadata-import-and-export}

Leer hoe u de metagegevensfuncties voor importeren en exporteren van Adobe Experience Manager Assets kunt gebruiken. Met de import- en exportmogelijkheden kunnen auteurs van inhoud de metagegevens van updates voor bestaande elementen bulksgewijs verzenden.

## Metagegevens exporteren {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=12&learn=on)

## Metagegevens importeren {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=12&learn=on)

>[!NOTE]
>
> Wanneer u een CSV-bestand voorbereidt om te importeren, is het eenvoudiger om met de functie Metagegevens exporteren een CSV te genereren met de lijst met elementen. Vervolgens kunt u het gegenereerde CSV-bestand wijzigen en importeren met de functie Importeren.

## CSV-bestandsindeling metagegevens {#metadata-file-format}

### Eerste rij

* De eerste rij van het CSV-bestand definieert het metagegevensschema.
* De eerste kolom heeft standaard de waarde `assetPath`, die het absolute JCR-pad voor een element bevat.

* De volgende kolommen in de eerste rij wijzen naar andere meta-gegevenseigenschappen van een element.
   * Bijvoorbeeld : `dc:title, dc:description, jcr:title`

* Single Value, indeling voor eigenschappen

   * `<metadata property name> {{<property type}}`
   * Als het type eigenschap niet wordt opgegeven, wordt standaard String gebruikt.
   * Bijvoorbeeld: `dc:title {{String}}`

* Eigenschapnaam is hoofdlettergevoelig
   * Correct : `dc:title {{String}}`
   * Incorrect: `Dc:Title {{String}}`

* Type eigenschap is niet hoofdlettergevoelig
* Alle geldige [JCR-eigenschapstypen](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) worden ondersteund

* Opmaak eigenschap van meerdere waarden - `<metadata property name> {{<property type : MULTI }}`

### Tweede rij naar N rijen

* De eerste kolom bevat het absolute JCR-pad voor een element. Bijvoorbeeld: /content/dam/asset1.jpg
* De eigenschap metagegevens voor een element kan ontbrekende waarden bevatten in het CSV-bestand. Ontbrekende eigenschappen van metagegevens voor dat specifieke element worden niet bijgewerkt.
