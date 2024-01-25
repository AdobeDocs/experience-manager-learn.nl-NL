---
title: Uitgever van verkoopmodel begrijpen in AEM
description: Apache Sling Models 1.3.0 introduceert Sling Model Exporter, een elegante manier om Sling Model voorwerpen in douaneabstracties uit te voeren of in series te vervaardigen. In dit artikel wordt naast het traditionele gebruik van Sling Models de HTML-scripts gevuld met behulp van het Sling Model Exporter-framework om een Sling Model in JSON te serialiseren.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
duration: 150
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Begrijpen [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 introduceert [!DNL Sling Model Exporter], een elegante manier om te exporteren of serialiseren [!DNL Sling Model] objecten in aangepaste abstracties. In dit artikel wordt naast het traditionele gebruik ook het gebruik van [!DNL Sling Models] om HTML-scripts te vullen met de functie [!DNL Sling Model Exporter] kader om een [!DNL Sling Model] naar JSON.

## Traditioneel verkoopmodel HTTP-aanvraagstroom

Het traditionele gebruiksgeval voor [!DNL Sling Models] is een bedrijfsabstractie voor een middel of een verzoek te verstrekken, dat manuscripten HTML (of, eerder JSPs) een interface voor de toegang tot van bedrijfsfuncties verstrekt.

Er ontwikkelen zich gemeenschappelijke patronen [!DNL Sling Models] die AEM componenten of pagina&#39;s vertegenwoordigen, en het gebruiken van [!DNL Sling Model] objecten die de HTML-scripts voorzien van gegevens, met als eindresultaat HTML dat in de browser wordt weergegeven.

### Verzendmodel HTTP-aanvraagstroom

![Aanvraagstroom voor verkoopmodel](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] Er wordt een verzoek ingediend voor een resource in AEM.

   Voorbeeld: `HTTP GET /content/my-resource.html`

1. Gebaseerd op de verzoekmiddelen `sling:resourceType`, wordt het juiste script opgelost.

1. Het script past de aanvraag of bron aan de gewenste waarden aan [!DNL Sling Model].

1. In het script worden de [!DNL Sling Model] -object om de HTML-uitvoering te genereren.

1. De HTML die door het Manuscript wordt geproduceerd is teruggekeerd in de Reactie van HTTP.

Dit traditionele patroon werkt goed in de context van het genereren van HTML als [!DNL Sling Model] kan gemakkelijk worden gebruikt via HTL. Het maken van meer gestructureerde gegevens, zoals JSON of XML, is een veel lastiger zaak, omdat HTML zich niet automatisch leent aan de definitie van deze indelingen.

## [!DNL Sling Model Exporter] HTTP-aanvraagstroom

Apache [!DNL Sling Model Exporter] wordt geleverd met een bij Sling geleverde Jackson Exporter die automatisch een &quot;gewone&quot; [!DNL Sling Model] object naar JSON. De Jackson Exporter, hoewel behoorlijk configureerbaar, bij zijn kern inspecteert het [!DNL Sling Model] en genereert JSON met behulp van &#39;getter&#39;-methoden als JSON-toetsen en de retourwaarden van getter als de JSON-waarden.

De directe serialisatie van [!DNL Sling Models] staat hen toe om beide normale verzoeken van het Web met hun gemaakte HTML reacties te onderhouden gebruikend de traditionele [!DNL Sling Model] verzoek om stroom (zie hierboven), maar stelt ook JSON-uitvoeringen bloot die door Webdiensten of toepassingen JavaScript kunnen worden verbruikt.

![HTTP-aanvraagstroom Sling Model Exporter](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Deze stroom beschrijft de stroom gebruikend de verstrekte Jackson Exporter om output te produceren JSON. Het gebruik van aangepaste exporters verloopt volgens dezelfde stroom, maar met hun uitvoerindeling.*

1. HTTP-GET-aanvraag is ingediend voor een bron in AEM met de kiezer en de extensie die zijn geregistreerd bij de [!DNL Sling Model]Exporter.

   Voorbeeld: `HTTP GET /content/my-resource.model.json`

1. Het verkopen lost de gevraagde middelen op `sling:resourceType`, kiezer en uitbreiding naar een dynamisch gegenereerde Sling Exporter Servlet, die is toegewezen aan de [!DNL Sling Model] met Exporter.
1. De opgeloste Servlet van de Exporter van de Verkoop haalt het [!DNL Sling Model Exporter] tegen de [!DNL Sling Model] object dat is aangepast aan het verzoek of de bron (zoals bepaald in de aanpasbare tabellen voor Sling Models).
1. De exportfunctie serialiseert de [!DNL Sling Model] op basis van de opmerkingen Exporter Options en Exporter-specific Sling Model en retourneert het resultaat naar Sling Exporter Servlet.
1. De Sling Exporter Servlet retourneert de JSON-uitvoering van de [!DNL Sling Model] in de HTTP-respons.

>[!NOTE]
>
>Terwijl het Apache Sling-project de Jackson Exporter biedt die serialiseert [!DNL Sling Models] aan JSON, steunt het kader van de Exporter ook douaneExporters. Een project kan bijvoorbeeld een aangepaste Exporter implementeren die een [!DNL Sling Model] in XML.

>[!NOTE]
>
>Niet alleen [!DNL Sling Model Exporter] *serialiseren* [!DNL Sling Models], kunt u deze ook exporteren als Java-objecten. Het exporteren naar andere Java-objecten speelt geen rol in de HTTP-aanvraagstroom en wordt dus niet weergegeven in het bovenstaande diagram.

## Ondersteunende materialen

* [Apache [!DNL Sling Model Exporter] Framework-documentatie](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
