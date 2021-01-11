---
title: Metagegevens importeren en exporteren in AEM Assets gebruiken
description: Met de AEM Assets-mogelijkheden voor het importeren en exporteren van metagegevens voor metagegevens van inhoud kunnen auteurs van inhoud metagegevens van elementen eenvoudig in en uit AEM verplaatsen en de mogelijkheden van Microsoft Excel benutten om metagegevens op schaal te manipuleren, waardoor de metagegevens van bulkupdates voor bestaande elementen in AEM worden vereenvoudigd.
topics: metadata
audience: all
doc-type: feature video
activity: use
kt: 647
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 2c0818d0223a3db55e6407068f4802b9e7f7dd83
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---


# Metagegevens importeren en exporteren gebruiken in AEM Assets{#using-metadata-import-and-export-in-aem-assets}

Met de AEM Assets-mogelijkheden voor het importeren en exporteren van metagegevens voor metagegevens van inhoud kunnen auteurs van inhoud metagegevens van elementen eenvoudig in en uit AEM verplaatsen en de mogelijkheden van Microsoft Excel benutten om metagegevens op schaal te manipuleren, waardoor de metagegevens van bulkupdates voor bestaande elementen in AEM worden vereenvoudigd.

## Metagegevens exporteren {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=9&learn=on)

## Metagegevens importeren {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=9&learn=on)

Download [WeRetail sportmap](assets/we-retail-sports.zip)

[Metagegevenspakket van element](assets/we-retail-sports-asset-metadata.zip) downloaden

## Bestandsindeling metagegevens {#metadata-file-format}

### CSV-bestandsindeling

#### Eerste rij

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

#### Tweede rij naar N rijen

* De eerste kolom bevat het absolute JCR-pad voor een element. Bijvoorbeeld: /content/dam/asset1.jpg
* De eigenschap metagegevens voor een element kan ontbrekende waarden bevatten in het CSV-bestand. Ontbrekende eigenschap metadata voor dat specifieke element wordt niet bijgewerkt.
