---
title: Exporteurs van verkoopmodellen ontwikkelen in AEM
description: Deze technische wandeling door het instellen van AEM voor gebruik met Sling Model Exporter, het verbeteren van een bestaand Sling Model met behulp van het Exporter-framework voor uitvoering als JSON, en hoe u Exporter-opties en Jackson-annotaties gebruikt om de uitvoer verder aan te passen.
version: 6.3, 6.4, 6.5
sub-product: stichting, inhouddiensten
feature: APIs
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 0%

---


# Exporteurs van verkoopmodellen ontwikkelen

Deze technische wandeling door het instellen van AEM voor gebruik met Sling Model Exporter, het verbeteren van een bestaand Sling Model met behulp van het Exporter-framework voor uitvoering als JSON, en hoe u Exporter-opties en Jackson-annotaties gebruikt om de uitvoer verder aan te passen.

Sling Model Exporter werd geïntroduceerd in Sling Models v1.3.0. Met deze nieuwe functie kunnen nieuwe annotaties worden toegevoegd aan Sling Models die definiëren hoe het model kan worden geëxporteerd als een ander Java-object, of meer algemeen, in een andere indeling, zoals JSON.

Apache Sling biedt een Jackson JSON-exporter die het meest voorkomende geval van het exporteren van Sling Models als JSON-objecten voor gebruik door programmatische webgebruikers, zoals andere webservices en JavaScript-toepassingen, behandelt.

## AEM configureren voor Verkoopmodel-exportfunctie

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] is een kenmerk van het  [!DNL Apache Sling] project en niet rechtstreeks gebonden aan de AEM productreleasecyclus. [!DNL Sling Model Exporter] is compatibel met AEM 6.3 en hoger.

## Het use-case voor [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] is ideaal voor het leveraging van Verschuivende Modellen die reeds bedrijfslogica bevatten die de vertoningen van HTML via HTML (of vroeger JSP) steunen, en de zelfde bedrijfsvertegenwoordiging zoals JSON voor consumptie door de programmatic diensten van het Web of toepassingen van JavaScript blootstellen.

## Een Verkoopmodel-exportfunctie maken

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

Het inschakelen van [!DNL Exporter]-ondersteuning op een [!DNL Sling Model] is net zo eenvoudig als het toevoegen van de `@Exporter`-annotatie aan de Java-klasse.

## Exportopties voor verkoopmodel toepassen

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] ondersteunt het doorgeven van exportopties per model naar de Exporter-implementatie om te bepalen hoe de exporter uiteindelijk  [!DNL Sling Model] wordt geëxporteerd. Deze opties zijn over het algemeen &quot;globaal&quot; van toepassing op de manier waarop [!DNL Sling Model] wordt geëxporteerd, in tegenstelling tot per gegevenspunt dat kan worden uitgevoerd via hieronder beschreven inline-annotaties.

[!DNL Jackson Exporter] opties zijn:

* [Opties voor Mapper-functies](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Opties voor serialisatiefunctie](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## [!DNL Jackson]-annotaties toepassen

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

Exporters kunnen implementaties ook annotaties ondersteunen die inline kunnen worden toegepast op de klasse [!DNL Sling Model], die een fijnere controle kunnen bieden over de manier waarop de gegevens worden geëxporteerd.

* [[!DNL Jackson Exporter] annotaties](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## De code {#view-the-code} weergeven

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Ondersteunende materialen {#supporting-materials}

* [[!DNL Jackson Mapper] Feature Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Feature Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Docs](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
