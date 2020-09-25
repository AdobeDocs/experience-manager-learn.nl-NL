---
title: Begrijp het Verkopen ModelExporter in AEM
description: Apache Sling Models 1.3.0 introduceert Sling Model Exporter, een elegante manier om Sling Model voorwerpen in douaneabstracties uit te voeren of in series te vervaardigen. In dit artikel wordt naast het traditionele gebruik van Sling Models de HTML-scripts gevuld met behulp van het Sling Model Exporter-framework om een Sling Model in JSON te serialiseren.
version: 6.3, 6.4, 6.5
sub-product: stichting, inhouddiensten
feature: sling-models, sling-model-exporter
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
translation-type: tm+mt
source-git-commit: 63295cbc353a796959ba2e98e3e21c188f596372
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 0%

---


# Begrijpen [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 introduceert [!DNL Sling Model Exporter]een elegante manier om [!DNL Sling Model] objecten te exporteren of in serienummering op te nemen in aangepaste abstracties. In dit artikel wordt naast het traditionele gebruik van HTML-scripts ook gebruikgemaakt [!DNL Sling Models] van het [!DNL Sling Model Exporter] framework om een script [!DNL Sling Model] in JSON te serialiseren.

## Traditioneel verkoopmodel HTTP-aanvraagstroom

Het traditionele gebruik-geval voor [!DNL Sling Models] is een bedrijfsabstractie voor een middel of een verzoek te verstrekken, dat manuscripten HTML (of, eerder JSPs) een interface voor de toegang tot van bedrijfsfuncties verstrekt.

Er worden gemeenschappelijke patronen ontwikkeld [!DNL Sling Models] die AEM Componenten of Pagina&#39;s vertegenwoordigen en die de [!DNL Sling Model] objecten gebruiken om de HTML-scripts gegevens te geven, met als eindresultaat HTML die in de browser wordt weergegeven.

### Verzendmodel HTTP-aanvraagstroom

![Aanvraagstroom voor verkoopmodel](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] Er wordt een verzoek ingediend voor een resource in AEM.

   Voorbeeld: `HTTP GET /content/my-resource.html`

1. Gebaseerd op de verzoekmiddel `sling:resourceType`, wordt het aangewezen Manuscript opgelost.

1. Het script past de aanvraag of resource aan de gewenste waarden aan [!DNL Sling Model].

1. Het script gebruikt het [!DNL Sling Model] object om de HTML-uitvoering te genereren.

1. De HTML die door het script wordt gegenereerd, wordt geretourneerd in de HTTP-respons.

Dit traditionele patroon werkt goed in de context van het genereren van HTML, aangezien de HTML eenvoudig via HTML [!DNL Sling Model] kan worden gebruikt. Het maken van meer gestructureerde gegevens, zoals JSON of XML, is een veel lastiger zaak, omdat HTML zich niet automatisch leent aan de definitie van deze indelingen.

## [!DNL Sling Model Exporter] HTTP-aanvraagstroom

Apache [!DNL Sling Model Exporter] wordt geleverd met een Sling-functie die Jackson Exporter biedt en die een &quot;gewoon&quot; [!DNL Sling Model] object automatisch serialiseert in JSON. De Jackson Exporter, die behoorlijk configureerbaar is, inspecteert bij zijn kern het [!DNL Sling Model] voorwerp, en produceert JSON gebruikend om het even welke &quot;getter&quot;methodes als sleutels JSON, en de getterterugkeer waarden als waarden JSON.

De directe rangschikking van [!DNL Sling Models] staat hen toe om zowel normale verzoeken van het Web met hun reacties van HTML te onderhouden die gebruikend de traditionele [!DNL Sling Model] verzoekstroom (zie hierboven) worden gecreeerd, maar stelt ook JSON vertoningen bloot die door Webdiensten of toepassingen JavaScript kunnen worden verbruikt.

![HTTP-aanvraagstroom Sling Model Exporter](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Deze stroom beschrijft de stroom gebruikend de verstrekte Jackson Exporter om output te produceren JSON. Het gebruik van aangepaste exporters verloopt volgens dezelfde stroom, maar met hun uitvoerindeling.*

1. HTTP-GET-aanvraag wordt ingediend voor een bron in AEM met de kiezer en de extensie die bij de Exporter [!DNL Sling Model]van de gebruiker zijn geregistreerd.

   Voorbeeld: `HTTP GET /content/my-resource.model.json`

1. Sling lost het gevraagde middel, de selecteur `sling:resourceType`, en de uitbreiding op dynamisch geproduceerde Servlet van de Exporter van de Exporter Sling, die aan [!DNL Sling Model] met Exporter in kaart wordt gebracht.
1. De opgeloste Servlet van de Exporter van de Verspreiding haalt [!DNL Sling Model Exporter] tegen het [!DNL Sling Model] voorwerp aan dat van het verzoek of de middel (zoals bepaald door de Verschuivende Modellen aanpassingstabellen) wordt aangepast.
1. De exporter serialiseert het bestand [!DNL Sling Model] op basis van de opmerkingen Exporter Options en Exporter-specific Sling Model en retourneert het resultaat naar Sling Exporter Servlet.
1. De Sling Exporter Servlet keert de vertoning JSON van de [!DNL Sling Model] in de Reactie van HTTP terug.

>[!NOTE]
>
>Terwijl het project Apache Sling de Jackson Exporter verstrekt die [!DNL Sling Models] aan JSON serialiseert, steunt het kader van de Exporter ook douaneExporters. Bijvoorbeeld, kon een project een douaneExporter uitvoeren die een [!DNL Sling Model] in XML serializes.

>[!NOTE]
>
>Niet alleen wordt [!DNL Sling Model Exporter] serialiseren ** [!DNL Sling Models]uitgevoerd, maar ook kunnen deze objecten worden geëxporteerd als Java-objecten. Het exporteren naar andere Java-objecten speelt geen rol in de HTTP-aanvraagstroom en wordt dus niet weergegeven in het bovenstaande diagram.

## Ondersteunende materialen

* [ [!DNL Sling Model Exporter] ApacheFramework-documentatie](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
