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
source-git-commit: 726715890d997ba3bb85f4833e220ac2222b3a42
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---

# Metagegevens importeren en exporteren in AEM Assets gebruiken {#metadata-import-and-export}

Leer hoe u de metagegevensfuncties voor importeren en exporteren van Adobe Experience Manager Assets kunt gebruiken. Met de import- en exportmogelijkheden kunnen auteurs van inhoud de metagegevens van updates voor bestaande elementen bulksgewijs verzenden.

## Metagegevens exporteren {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132?quality=12&learn=on)

>[!TIP]
>
> Als u CSV-bestand voor metagegevens wilt openen in Excel, gebruikt u de opdracht [Excel-importer](https://support.microsoft.com/en-us/office/import-data-from-a-csv-html-or-text-file-b62efe49-4d5b-4429-b788-e1211b5e90f6) in plaats van te dubbelklikken op het bestand om problemen met UTF-8-gecodeerde CSV-bestanden te voorkomen.
>
> Voer de volgende stappen uit om het CSV-bestand voor het exporteren van metagegevens te openen in Excel:
> 
> 1. Microsoft Excel openen
> 1. Selecteren __Bestand > Nieuw__ een leeg spreadsheet maken
> 1. Open het lege werkblad en selecteer __Bestand > Importeren__
> 1. Selecteren __Tekst__ bestand en klik op __Importeren__
> 1. Selecteer het geëxporteerde CSV-bestand in het bestandssysteem en klik op __Gegevens ophalen__
> 1. Selecteer in stap 1 van de wizard Importeren de optie __Gescheiden__ en instellen __Oorsprong bestand__ tot __Unicode (UTF-8)__ en klik op __Volgende__
> 1. Bij stap 2 stelt u de __Scheidingstekens__ tot __Komma__ en klik op __Volgende__
> 1. Bij stap 3 laat u de __Kolomgegevensindeling__ ongewijzigd, en klik __Voltooien__
> 1. Selecteren __Importeren__ om de gegevens aan spreadsheet toe te voegen

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
