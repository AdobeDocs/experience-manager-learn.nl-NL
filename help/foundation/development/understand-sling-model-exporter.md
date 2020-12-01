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


# [!DNL Sling Model Exporter] begrijpen

Apache [!DNL Sling Models] 1.3.0 introduceert [!DNL Sling Model Exporter], een elegante manier om [!DNL Sling Model] voorwerpen in douaneabstracties uit te voeren of in series te vervaardigen. In dit artikel wordt naast het traditionele gebruik van [!DNL Sling Models] voor het vullen van HTML-scripts ook het [!DNL Sling Model Exporter]-framework gebruikt om een [!DNL Sling Model] in JSON te serialiseren.

## Traditioneel verkoopmodel HTTP-aanvraagstroom

Het traditionele gebruik-geval voor [!DNL Sling Models] is een bedrijfsabstractie voor een middel of een verzoek te verstrekken, die manuscripten HTML (of, eerder JSPs) een interface voor de toegang tot van bedrijfsfuncties verstrekt.

Er worden gemeenschappelijke patronen ontwikkeld [!DNL Sling Models] die AEM Componenten of Pagina&#39;s vertegenwoordigen, en het gebruik van de [!DNL Sling Model] voorwerpen om de manuscripten van HTML van gegevens te voorzien, met een eindresultaat van HTML die in browser wordt getoond.

### Verzendmodel HTTP-aanvraagstroom

![Aanvraagstroom voor verkoopmodel](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] Er wordt een verzoek ingediend voor een resource in AEM.

   Voorbeeld: `HTTP GET /content/my-resource.html`

1. Gebaseerd op `sling:resourceType` van het verzoekmiddel, wordt het aangewezen Manuscript opgelost.

1. Het manuscript past het Verzoek of Middel aan het gewenste [!DNL Sling Model] aan.

1. Het script gebruikt het object [!DNL Sling Model] om de HTML-uitvoering te genereren.

1. De HTML die door het script wordt gegenereerd, wordt geretourneerd in de HTTP-respons.

Dit traditionele patroon werkt goed in de context van het genereren van HTML, aangezien [!DNL Sling Model] gemakkelijk kan worden gebruikt via HTML. Het maken van meer gestructureerde gegevens, zoals JSON of XML, is een veel lastiger zaak, omdat HTML zich niet automatisch leent aan de definitie van deze indelingen.

## [!DNL Sling Model Exporter] HTTP-aanvraagstroom

Apache [!DNL Sling Model Exporter] wordt geleverd met een bij Sling geleverde Jackson Exporter die een &quot;gewoon&quot; [!DNL Sling Model]-object automatisch serialiseert in JSON. De Jackson Exporter, die behoorlijk configureerbaar is, inspecteert bij zijn kern het [!DNL Sling Model] voorwerp, en produceert JSON gebruikend om het even welke &quot;getter&quot;methodes als sleutels JSON, en de getter terugkeerwaarden als waarden JSON.

Door de directe serialisatie van [!DNL Sling Models] kunnen ze zowel normale webverzoeken uitvoeren met hun HTML-reacties die zijn gemaakt met de traditionele [!DNL Sling Model]-aanvraagstroom (zie boven), als kunnen JSON-uitvoeringen die kunnen worden gebruikt door webservices of JavaScript-toepassingen, ook beschikbaar worden gemaakt.

![HTTP-aanvraagstroom Sling Model Exporter](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Deze stroom beschrijft de stroom gebruikend de verstrekte Jackson Exporter om output te produceren JSON. Het gebruik van aangepaste exporters volgt dezelfde stroom, maar met hun uitvoerindeling.*

1. HTTP-GET-aanvraag wordt ingediend voor een bron in AEM met de kiezer en de extensie die bij de Exporter van [!DNL Sling Model] zijn geregistreerd.

   Voorbeeld: `HTTP GET /content/my-resource.model.json`

1. Bij Sling worden de `sling:resourceType`, de kiezer en de extensie van de aangevraagde bron omgezet in een dynamisch gegenereerde Sling Exporter Servlet, die samen met Exporter aan [!DNL Sling Model] wordt toegewezen.
1. De opgeloste Servlet van de Exporter van de Verspreiding haalt [!DNL Sling Model Exporter] tegen het [!DNL Sling Model] voorwerp aan dat van het verzoek of de middel wordt aangepast (zoals bepaald door de Verschuivende Modellen aanpassingstabellen).
1. De exporteur serialiseert [!DNL Sling Model] die op de Opties van de Exporter en Exporter-specifieke het Schuiven Model annotaties wordt gebaseerd en keert het resultaat aan Servlet van de Exporter Sling terug.
1. De Sling Exporter Servlet keert de JSON vertoning van [!DNL Sling Model] in de Reactie van HTTP terug.

>[!NOTE]
>
>Terwijl het Apache Sling-project de Jackson Exporter verstrekt die [!DNL Sling Models] aan JSON serializes, steunt het kader van de Exporter ook douaneExporters. Een project kan bijvoorbeeld een aangepaste Exporter implementeren die een [!DNL Sling Model] serialiseert in XML.

>[!NOTE]
>
>Niet alleen voert [!DNL Sling Model Exporter] *serialize* [!DNL Sling Models] uit, het kan hen ook als voorwerpen van Java uitvoeren. Het exporteren naar andere Java-objecten speelt geen rol in de HTTP-aanvraagstroom en wordt dus niet weergegeven in het bovenstaande diagram.

## Ondersteunende materialen

* [ [!DNL Sling Model Exporter] ApacheFramework-documentatie](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
