---
title: Exporteurs van verkoopmodellen in AEM ontwikkelen
description: Deze technische wandeling door het opzetten van AEM voor gebruik met Sling Model Exporter, het verbeteren van een bestaand Sling Model gebruikend het kader van de Exporter aan vertoning als JSON, en hoe te om de opties van de Exporter en de aantekeningen van Jackson te gebruiken om de output verder aan te passen.
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
duration: 932
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---

# Exporteurs van verkoopmodellen ontwikkelen

Deze technische wandeling door het opzetten van AEM voor gebruik met Sling Model Exporter, het verbeteren van een bestaand Sling Model gebruikend het kader van de Exporter aan vertoning als JSON, en hoe te om de opties van de Exporter en de aantekeningen van Jackson te gebruiken om de output verder aan te passen.

Sling Model Exporter werd geïntroduceerd in Sling Models v1.3.0. Met deze nieuwe functie kunnen nieuwe annotaties worden toegevoegd aan Sling Models die definiëren hoe het model kan worden geëxporteerd als een ander Java-object, of meer algemeen, in een andere indeling, zoals JSON.

Apache Sling biedt een Jackson JSON-exporter die het meest voorkomende geval van het exporteren van Sling Models als JSON-objecten voor gebruik door programmatische webconsumenten, zoals andere webservices en JavaScript-toepassingen, behandelt.

## AEM for Sling Model Exporter configureren

>[!VIDEO](https://video.tv.adobe.com/v/16862?quality=12&learn=on)

[!DNL Sling Model Exporter] is een functie van het [!DNL Apache Sling] -project en is niet rechtstreeks gekoppeld aan de AEM-productreleasecyclus. [!DNL Sling Model Exporter] is compatibel met AEM 6.3 en hoger.

## De use-case voor [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863?quality=12&learn=on)

[!DNL Sling Model Exporter] is ideaal voor het leveraging Sling Models die reeds bedrijfslogica bevatten die HTML vertoningen via HTML (of vroeger JSP) steunen, en de zelfde bedrijfsvertegenwoordiging zoals JSON voor consumptie door de programmatic diensten van het Web of de toepassingen van JavaScript blootstellen.

## Een Verkoopmodel-exportfunctie maken

>[!VIDEO](https://video.tv.adobe.com/v/16864?quality=12&learn=on)

U kunt [!DNL Exporter] -ondersteuning inschakelen voor een [!DNL Sling Model] net zo eenvoudig als de `@Exporter` -annotatie toevoegen aan de Java-klasse.

## Exportopties voor verkoopmodel toepassen

>[!VIDEO](https://video.tv.adobe.com/v/16865?quality=12&learn=on)

[!DNL Sling Model Exporter] ondersteunt het doorgeven van exportopties per model naar de Exporter-implementatie om te bepalen hoe de [!DNL Sling Model] uiteindelijk wordt geëxporteerd. Deze opties zijn over het algemeen &quot;globaal&quot; van toepassing op de manier waarop [!DNL Sling Model] wordt geëxporteerd, in tegenstelling tot per gegevenspunt dat kan worden uitgevoerd via inline-annotaties die hieronder worden beschreven.

[!DNL Jackson Exporter] -opties zijn:

* [&#x200B; de opties van de Eigenschap van de Mapper &#x200B;](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [&#x200B; de opties van de Eigenschap van de Rangschikking &#x200B;](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## [!DNL Jackson] -annotaties toepassen

>[!VIDEO](https://video.tv.adobe.com/v/16866?quality=12&learn=on)

Exporters-implementaties kunnen ook annotaties ondersteunen die inline kunnen worden toegepast op de [!DNL Sling Model] -klasse, die een fijnere controle kunnen bieden over de manier waarop de gegevens worden geëxporteerd.

* [[!DNL Jackson Exporter] annotaties](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## De code weergeven {#view-the-code}

[&#x200B; SampleSlingModelExporter.java &#x200B;](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Ondersteunende materialen {#supporting-materials}

* [[!DNL Jackson Mapper]  Eigenschap Javadoc &#x200B;](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization]  Eigenschap Javadoc &#x200B;](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations]  Dokken &#x200B;](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
