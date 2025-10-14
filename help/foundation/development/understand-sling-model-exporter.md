---
title: Uitgever van verkoopmodel in AEM begrijpen
description: Apache Sling Models 1.3.0 introduceert Sling Model Exporter, een elegante manier om Sling Model voorwerpen in douaneabstracties uit te voeren of in series te vervaardigen. In dit artikel wordt naast het traditionele gebruik van Sling Models de HTML-scripts gevuld met behulp van het Sling Model Exporter-framework om een Sling Model in JSON te serialiseren.
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
duration: 133
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Begrijpen [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 introduceert [!DNL Sling Model Exporter] , een elegante manier om [!DNL Sling Model] -objecten te exporteren of van serienummering te voorzien in aangepaste abstracties. In dit artikel wordt naast het traditionele gebruik van [!DNL Sling Models] voor het vullen van HTML-scripts ook gebruikgemaakt van het [!DNL Sling Model Exporter] -framework om een [!DNL Sling Model] -script te serialiseren in JSON.

## Traditioneel verkoopmodel HTTP-aanvraagstroom

Het traditionele gebruik-geval voor [!DNL Sling Models] is een bedrijfsabstractie voor een middel of een verzoek te verstrekken, dat de manuscripten van HTML (of, eerder JSPs) een interface voor de toegang tot van bedrijfsfuncties verstrekt.

Er worden algemene patronen ontwikkeld in [!DNL Sling Models] die staan voor AEM-componenten of -pagina&#39;s. Met de [!DNL Sling Model] -objecten kunt u de HTML-scripts voorzien van gegevens, met als eindresultaat HTML dat in de browser wordt weergegeven.

### Verzendmodel HTTP-aanvraagstroom

![&#x200B; het Verkopen ModelStroom van het Verzoek &#x200B;](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] Aanvraag wordt gedaan voor een resource in AEM.

   Voorbeeld: `HTTP GET /content/my-resource.html`

1. Op basis van de `sling:resourceType` -bron van de aanvraag wordt het juiste script opgelost.

1. Het script past de aanvraag of bron aan de gewenste [!DNL Sling Model] aan.

1. Het script gebruikt het [!DNL Sling Model] -object om de HTML-uitvoering te genereren.

1. De HTML die door het script wordt gegenereerd, wordt geretourneerd in de HTTP Response.

Dit traditionele patroon werkt goed in de context van het genereren van HTML, aangezien de [!DNL Sling Model] gemakkelijk kan worden gebruikt via HTML. Het maken van meer gestructureerde gegevens, zoals JSON of XML, is een veel lastiger zaak, omdat HTML zich niet automatisch leent aan de definitie van deze indelingen.

## [!DNL Sling Model Exporter] HTTP-aanvraagstroom

Apache [!DNL Sling Model Exporter] wordt geleverd met een bij Sling geleverde Jackson Exporter die een &quot;gewoon&quot; [!DNL Sling Model] -object automatisch serialiseert in JSON. De Jackson Exporter, die behoorlijk configureerbaar is, inspecteert bij zijn kern het [!DNL Sling Model] voorwerp, en produceert JSON gebruikend om het even welke &quot;getter&quot;methodes als sleutels JSON, en de getter terugkeerwaarden als waarden JSON.

Door de directe serialisatie van [!DNL Sling Models] kunnen ze zowel normale webverzoeken uitvoeren met hun HTML-antwoorden die zijn gemaakt met de traditionele [!DNL Sling Model] request-flow (zie boven), als JSON-uitvoeringen die kunnen worden gebruikt door webservices of JavaScript-toepassingen, beschikbaar maken.

![&#x200B; het Verdelen van de stroom van het Verzoek van HTTP van de ModelExporter &#x200B;](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Deze stroom beschrijft de stroom gebruikend verstrekt Jackson Exporter om output te produceren JSON. Het gebruik van douaneExporteurs volgt de zelfde stroom maar met hun outputformaat.*

1. HTTP GET Request is gemaakt voor een resource in AEM met de kiezer en de extensie die zijn geregistreerd bij de Exporter van [!DNL Sling Model] .

   Voorbeeld: `HTTP GET /content/my-resource.model.json`

1. Bij Sling worden de `sling:resourceType` , de kiezer en de extensie van de aangevraagde bron omgezet in een dynamisch gegenereerde Sling Exporter Servlet, die aan de [!DNL Sling Model] wordt toegewezen met Exporter.
1. De opgeloste Sling Exporter Servlet roept [!DNL Sling Model Exporter] tegen het [!DNL Sling Model] voorwerp aan dat van het verzoek of de middel wordt aangepast (zoals die door de Verschuivende Modellen aanpassingstabellen wordt bepaald).
1. De exportfunctie serialiseert de [!DNL Sling Model] op basis van de opmerkingen Exporter Options en Exporter-specific Sling Model en retourneert het resultaat naar Sling Exporter Servlet.
1. De Sling Exporter Servlet keert de JSON vertoning van [!DNL Sling Model] in de Reactie van HTTP terug.

>[!NOTE]
>
>Hoewel het Apache Sling-project de Jackson Exporter biedt die [!DNL Sling Models] serialiseert naar JSON, ondersteunt het Exporter-framework ook aangepaste Exporters. Een project kan bijvoorbeeld een aangepaste Exporter implementeren die een [!DNL Sling Model] in XML serialiseert.

>[!NOTE]
>
>Niet alleen [!DNL Sling Model Exporter] *rangschikt* [!DNL Sling Models], kan het hen als voorwerpen van Java ook uitvoeren. Het exporteren naar andere Java-objecten speelt geen rol in de HTTP-aanvraagstroom en wordt dus niet weergegeven in het bovenstaande diagram.

## Ondersteunende materialen

* [&#x200B; Apache  [!DNL Sling Model Exporter]  documentatie van het Kader &#x200B;](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
