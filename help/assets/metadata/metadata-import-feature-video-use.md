---
title: Metagegevens importeren en exporteren in AEM Assets gebruiken
seo-title: Metagegevens importeren en exporteren in AEM Assets gebruiken
description: Met de AEM Assets-mogelijkheden voor het importeren en exporteren van metagegevens voor metagegevens van inhoud kunnen auteurs van inhoud metagegevens van elementen eenvoudig in en uit AEM verplaatsen en de mogelijkheden van Microsoft Excel benutten om metagegevens op schaal te manipuleren, waardoor de metagegevens van bulkupdates voor bestaande elementen in AEM worden vereenvoudigd.
seo-description: Met de AEM Assets-mogelijkheden voor het importeren en exporteren van metagegevens voor metagegevens van inhoud kunnen auteurs van inhoud metagegevens van elementen eenvoudig in en uit AEM verplaatsen en de mogelijkheden van Microsoft Excel benutten om metagegevens op schaal te manipuleren, waardoor de metagegevens van bulkupdates voor bestaande elementen in AEM worden vereenvoudigd.
uuid: db7e57a4-b0c1-4a48-906d-802c19964313
discoiquuid: 72dd9230-73e1-454e-a3e0-9281e621d901
topics: metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 1%

---


# Metagegevens importeren en exporteren in AEM Assets gebruiken{#using-metadata-import-and-export-in-aem-assets}

Met de AEM Assets-mogelijkheden voor het importeren en exporteren van metagegevens voor metagegevens van inhoud kunnen auteurs van inhoud metagegevens van elementen eenvoudig in en uit AEM verplaatsen en de mogelijkheden van Microsoft Excel benutten om metagegevens op schaal te manipuleren, waardoor de metagegevens van bulkupdates voor bestaande elementen in AEM worden vereenvoudigd.

## Metagegevens exporteren {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=9&learn=on)

## Metagegevens importeren {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=9&learn=on)

Winkelsportmap [downloaden](assets/we-retail-sports.zip)

Metagegevenspakket [voor element downloaden](assets/we-retail-sports-asset-metadata.zip)

## Bestandsindeling metagegevens {#metadata-file-format}

### CSV-bestandsindeling

#### Eerste rij

* De eerste rij van het CSV-bestand definieert het metagegevensschema.
* De eerste kolom heeft standaard de waarde `assetPath`, die het absolute JCR Path voor een element bevat.

* De volgende kolommen in de eerste rij wijzen naar andere meta-gegevenseigenschappen van een element.

   * Bijvoorbeeld : `dc:title, dc:description, jcr:title`

* Single Value, indeling voor eigenschappen

   * `<metadata property name> {{<property type}}`
   * Als het type eigenschap niet wordt opgegeven, wordt standaard String gebruikt.
   * Bijvoorbeeld: `dc:title {{String}}`

* Eigenschapnaam is hoofdlettergevoelig
   * Correct : `dc:title {{String}}`
   * Incorrect: `Dc:Ttle {{String}}`

* Type eigenschap is niet hoofdlettergevoelig
* Alle geldige [JCR-eigenschapstypen](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) worden ondersteund

* Opmaak Multi-Value-eigenschap - `<metadata property name> {{<property type : MULTI }}`

#### Tweede rij naar N rijen

* De eerste kolom bevat het absolute JCR-pad voor een element. Bijvoorbeeld: /content/dam/asset1.jpg
* De eigenschap metagegevens voor een element kan ontbrekende waarden bevatten in het CSV-bestand. Ontbrekende eigenschap metadata voor dat specifieke element wordt niet bijgewerkt.