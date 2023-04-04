---
title: Exporteurs van verkoopmodellen ontwikkelen in AEM
description: Deze technische wandeling door het instellen van AEM voor gebruik met Sling Model Exporter, het verbeteren van een bestaand Sling Model met behulp van het Exporter-framework voor uitvoering als JSON, en hoe u Exporter-opties en Jackson-annotaties gebruikt om de uitvoer verder aan te passen.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 0%

---

# Exporteurs van verkoopmodellen ontwikkelen

Deze technische wandeling door het instellen van AEM voor gebruik met Sling Model Exporter, het verbeteren van een bestaand Sling Model met behulp van het Exporter-framework voor uitvoering als JSON, en hoe u Exporter-opties en Jackson-annotaties gebruikt om de uitvoer verder aan te passen.

Sling Model Exporter werd geïntroduceerd in Sling Models v1.3.0. Met deze nieuwe functie kunnen nieuwe annotaties worden toegevoegd aan Sling Models die definiëren hoe het model kan worden geëxporteerd als een ander Java-object, of meer algemeen, in een andere indeling, zoals JSON.

Apache Sling biedt een Jackson JSON-exporter die het meest voorkomende geval van het exporteren van Sling Models als JSON-objecten voor gebruik door programmatische webgebruikers, zoals andere webservices en JavaScript-toepassingen, behandelt.

## AEM configureren voor Verkoopmodel-exportfunctie

>[!VIDEO](https://video.tv.adobe.com/v/16862?quality=12&learn=on)

[!DNL Sling Model Exporter] is een kenmerk van de [!DNL Apache Sling] en niet rechtstreeks gebonden aan de AEM productreleasecyclus. [!DNL Sling Model Exporter] is compatibel met AEM 6.3 en hoger.

## Het gebruik-hoofdlettergebruik voor [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863?quality=12&learn=on)

[!DNL Sling Model Exporter] is ideaal voor het leveraging van Verschuivende Modellen die reeds bedrijfslogica bevatten die HTML vertoningen via HTL (of vroeger JSP) steunen, en de zelfde bedrijfsvertegenwoordiging zoals JSON voor consumptie door de programmatic diensten van het Web of toepassingen van JavaScript blootstellen.

## Een Verkoopmodel-exportfunctie maken

>[!VIDEO](https://video.tv.adobe.com/v/16864?quality=12&learn=on)

Inschakelen [!DNL Exporter] steun voor een [!DNL Sling Model] is net zo eenvoudig als het toevoegen van de `@Exporter` aantekening bij de klasse Java.

## Exportopties voor verkoopmodel toepassen

>[!VIDEO](https://video.tv.adobe.com/v/16865?quality=12&learn=on)

[!DNL Sling Model Exporter] ondersteunt het doorgeven van exportopties per model aan de Exporter-implementatie om te bepalen hoe de [!DNL Sling Model] wordt definitief geëxporteerd. Deze opties zijn doorgaans &quot;globaal&quot; van toepassing op de manier waarop de [!DNL Sling Model] wordt geëxporteerd, versus per gegevenspunt dat kan worden uitgevoerd via inline-annotaties die hieronder worden beschreven.

[!DNL Jackson Exporter] opties zijn:

* [Opties voor Mapper-functies](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Opties voor serialisatiefunctie](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Toepassen [!DNL Jackson] annotaties

>[!VIDEO](https://video.tv.adobe.com/v/16866?quality=12&learn=on)

Implementaties van exportfuncties kunnen ook annotaties ondersteunen die inline kunnen worden toegepast op de [!DNL Sling Model] klasse, die een fijner niveau van controle kan verstrekken hoe de gegevens worden uitgevoerd.

* [[!DNL Jackson Exporter] annotaties](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## De code weergeven {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Ondersteunende materialen {#supporting-materials}

* [[!DNL Jackson Mapper] Feature Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Feature Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Docs](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
