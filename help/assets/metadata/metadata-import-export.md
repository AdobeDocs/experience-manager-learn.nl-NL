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
> Wanneer het openen van meta-gegevens voer CSV- dossier in Excel, gebruik de [ importeur van Excel ](https://support.microsoft.com/en-us/office/import-data-from-a-csv-html-or-text-file-b62efe49-4d5b-4429-b788-e1211b5e90f6) eerder dan het tweemaal klikken van het dossier om kwesties met UTF-8 gecodeerde Csv- dossiers te vermijden.
>
> Voer de volgende stappen uit om het CSV-bestand voor het exporteren van metagegevens te openen in Excel:
> 
> 1. Microsoft Excel openen
> 1. Selecteer __Dossier > Nieuw__ om een leeg spreadsheet tot stand te brengen
> 1. Met het lege open spreadsheet, uitgezochte __Dossier > de Invoer__
> 1. Selecteer __dossier van de Tekst 0} {en klik__ Invoer ____
> 1. Selecteer het uitgevoerde CSV- dossier van het dossiersysteem en klik __krijgt Gegevens__
> 1. Op stap 1 van de de invoertovenaar, selecteer __Gescheiden__ en plaats __oorsprong van het Dossier__ aan __Unicode (UTF-8)__, en klik __daarna__
> 1. Op stap 2, plaats de __Scheidingstekens__ aan __komma__, en klik __daarna__
> 1. Op stap 3, verlaat het __gegevensformaat van de Kolom__ zoals is, en klik __Afwerking__
> 1. Selecteer __Invoer__ om de gegevens aan spreadsheet toe te voegen

## Metagegevens importeren {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374?quality=12&learn=on)

>[!NOTE]
>
> Wanneer u een CSV-bestand voorbereidt om te importeren, is het eenvoudiger om met de functie Metagegevens exporteren een CSV te genereren met de lijst met elementen. Vervolgens kunt u het gegenereerde CSV-bestand wijzigen en importeren met de functie Importeren.

## CSV-bestandsindeling metagegevens {#metadata-file-format}

### Eerste rij

* De eerste rij van het CSV-bestand definieert het metagegevensschema.
* De eerste kolom heeft standaard de waarde `assetPath` , die het absolute JCR-pad voor een element bevat.

* De volgende kolommen in de eerste rij wijzen naar andere meta-gegevenseigenschappen van een element.
   * Bijvoorbeeld : `dc:title, dc:description, jcr:title`

* Single Value, indeling voor eigenschappen

   * `<metadata property name> {{<property type}}`
   * Als het type eigenschap niet is opgegeven, wordt standaard String gebruikt.
   * Bijvoorbeeld: `dc:title {{String}}`

* Eigenschapnaam is hoofdlettergevoelig
   * Correct : `dc:title {{String}}`
   * Onjuist: `Dc:Title {{String}}`

* Type eigenschap is niet hoofdlettergevoelig
* Alle geldige [ types van Bezit JCR ](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) worden gesteund

* Opmaak eigenschap van meerdere waarden - `<metadata property name> {{<property type : MULTI }}`

### Tweede rij naar N rijen

* De eerste kolom bevat het absolute JCR-pad voor een element. Bijvoorbeeld: /content/dam/asset1.jpg
* De eigenschap metagegevens voor een element kan ontbrekende waarden bevatten in het CSV-bestand. Ontbrekende eigenschappen van metagegevens voor dat specifieke element worden niet bijgewerkt.
