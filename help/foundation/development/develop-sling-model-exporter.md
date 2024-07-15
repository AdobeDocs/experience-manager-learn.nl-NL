---
title: Exporteurs van verkoopmodellen ontwikkelen in AEM
description: Deze technische wandeling door het instellen van AEM voor gebruik met Sling Model Exporter, het verbeteren van een bestaand Sling Model met behulp van het Exporter-framework voor uitvoering als JSON, en hoe u Exporter-opties en Jackson-annotaties gebruikt om de uitvoer verder aan te passen.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
duration: 932
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---

# Exporteurs van verkoopmodellen ontwikkelen

Deze technische wandeling door het instellen van AEM voor gebruik met Sling Model Exporter, het verbeteren van een bestaand Sling Model met behulp van het Exporter-framework voor uitvoering als JSON, en hoe u Exporter-opties en Jackson-annotaties gebruikt om de uitvoer verder aan te passen.

Sling Model Exporter werd geïntroduceerd in Sling Models v1.3.0. Met deze nieuwe functie kunnen nieuwe annotaties worden toegevoegd aan Sling Models die definiëren hoe het model kan worden geëxporteerd als een ander Java-object, of meer algemeen, in een andere indeling, zoals JSON.

Apache Sling biedt een Jackson JSON-exporter die het meest voorkomende geval van het exporteren van Sling Models als JSON-objecten voor gebruik door programmatische webconsumenten, zoals andere webservices en JavaScript-toepassingen, behandelt.

## AEM configureren voor Verkoopmodel-exportfunctie

>[!VIDEO](https://video.tv.adobe.com/v/16862?quality=12&learn=on)

[!DNL Sling Model Exporter] is een functie van het [!DNL Apache Sling] -project en is niet rechtstreeks gebonden aan de AEM productreleasecyclus. [!DNL Sling Model Exporter] is compatibel met AEM 6.3 en hoger.

## De use-case voor [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863?quality=12&learn=on)

[!DNL Sling Model Exporter] is ideaal voor het leveraging van het Verkopen Modellen die reeds bedrijfslogica bevatten die HTML vertoningen via HTML (of vroeger JSP) steunen, en de zelfde bedrijfsvertegenwoordiging zoals JSON voor consumptie door de programmatic diensten van het Web of de toepassingen van JavaScript blootstellen.

## Een Verkoopmodel-exportfunctie maken

>[!VIDEO](https://video.tv.adobe.com/v/16864?quality=12&learn=on)

U kunt [!DNL Exporter] -ondersteuning inschakelen voor een [!DNL Sling Model] net zo eenvoudig als de `@Exporter` -annotatie toevoegen aan de Java-klasse.

## Exportopties voor verkoopmodel toepassen

>[!VIDEO](https://video.tv.adobe.com/v/16865?quality=12&learn=on)

[!DNL Sling Model Exporter] ondersteunt het doorgeven van exportopties per model naar de Exporter-implementatie om te bepalen hoe de [!DNL Sling Model] uiteindelijk wordt geëxporteerd. Deze opties zijn over het algemeen &quot;globaal&quot; van toepassing op de manier waarop [!DNL Sling Model] wordt geëxporteerd, in tegenstelling tot per gegevenspunt dat kan worden uitgevoerd via inline-annotaties die hieronder worden beschreven.

[!DNL Jackson Exporter] -opties zijn:

* [ de opties van de Eigenschap van de Mapper ](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [ de opties van de Eigenschap van de Rangschikking ](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## [!DNL Jackson] -annotaties toepassen

>[!VIDEO](https://video.tv.adobe.com/v/16866?quality=12&learn=on)

Exporters-implementaties kunnen ook annotaties ondersteunen die inline kunnen worden toegepast op de [!DNL Sling Model] -klasse, die een fijnere controle kunnen bieden over de manier waarop de gegevens worden geëxporteerd.

* [[!DNL Jackson Exporter] annotaties](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## De code weergeven {#view-the-code}

[ SampleSlingModelExporter.java ](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Ondersteunende materialen {#supporting-materials}

* [[!DNL Jackson Mapper]  Eigenschap Javadoc ](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization]  Eigenschap Javadoc ](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations]  Dokken ](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
