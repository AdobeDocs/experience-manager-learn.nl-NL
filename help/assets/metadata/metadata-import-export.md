---
title: Metagegevens importeren en exporteren in AEM Assets gebruiken
description: Leer hoe u de metagegevensfuncties voor importeren en exporteren van Adobe Experience Manager Assets kunt gebruiken. Met de import- en exportmogelijkheden kunnen auteurs van inhoud de metagegevens van updates voor bestaande elementen bulksgewijs verzenden.
version: 6.4, 6.5, Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
last-substantial-update: 2022-06-13T00:00:00Z
doc-type: Feature Video
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
duration: 419
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---

# Metagegevens importeren en exporteren in AEM Assets gebruiken {#metadata-import-and-export}

Leer hoe u de metagegevensfuncties voor importeren en exporteren van Adobe Experience Manager Assets kunt gebruiken. Met de import- en exportmogelijkheden kunnen auteurs van inhoud de metagegevens van updates voor bestaande elementen bulksgewijs verzenden.

## Metagegevens exporteren {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132?quality=12&learn=on)

## Metagegevens importeren {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374?quality=12&learn=on)

>[!NOTE]
>
> Wanneer u een CSV-bestand voorbereidt om te importeren, is het eenvoudiger om met de functie Metagegevens exporteren een CSV te genereren met de lijst met elementen. Vervolgens kunt u het gegenereerde CSV-bestand wijzigen en importeren met de functie Importeren.

## CSV-bestandsindeling metagegevens {#metadata-file-format}

### Eerste rij

* De eerste rij van het CSV-bestand definieert het metagegevensschema.
* De standaardinstellingen van de kolom Eerste `assetPath`, die het absolute JCR-pad voor een element bevat.

* De volgende kolommen in de eerste rij wijzen naar andere meta-gegevenseigenschappen van een element.
   * Bijvoorbeeld: `dc:title, dc:description, jcr:title`

* Single Value, indeling voor eigenschappen

   * `<metadata property name> {{<property type}}`
   * Als het type eigenschap niet is opgegeven, wordt standaard String gebruikt.
   * Bijvoorbeeld: `dc:title {{String}}`

* Eigenschapnaam is hoofdlettergevoelig
   * Juist: `dc:title {{String}}`
   * Onjuist: `Dc:Title {{String}}`

* Type eigenschap is niet hoofdlettergevoelig
* Alles geldig [Typen JCR-eigenschappen](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) worden ondersteund

* Opmaak Multi Value Property - `<metadata property name> {{<property type : MULTI }}`

### Tweede rij naar N rijen

* De eerste kolom bevat het absolute JCR-pad voor een element. Bijvoorbeeld: /content/dam/asset1.jpg
* De eigenschap metagegevens voor een element kan ontbrekende waarden bevatten in het CSV-bestand. Ontbrekende eigenschappen van metagegevens voor dat specifieke element worden niet bijgewerkt.
