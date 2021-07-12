---
title: Ondersteuning voor vertaling van AEM Content Fragments
description: Leer hoe u Content Fragments kunt lokaliseren en vertalen met Adobe Experience Manager. Elementen met gemengde media die aan een inhoudsfragment zijn gekoppeld, kunnen ook worden geëxtraheerd en vertaald.
feature: Inhoudsfragmenten, beheer voor meerdere sites
topic: Lokalisatie
role: User
level: Intermediate
version: 6.3, 6.4, 6.5, cloud-service
kt: 201
thumbnail: 18131.jpg
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---


# Ondersteuning voor vertaling van AEM Content Fragments {#translation-support-content-fragments}

Leer hoe u Content Fragments kunt lokaliseren en vertalen met Adobe Experience Manager. Elementen met gemengde media die aan een inhoudsfragment zijn gekoppeld, kunnen ook worden geëxtraheerd en vertaald.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## Gebruik hoofdletters/kleine letters voor het omzetten van inhoudsfragmenten {#content-fragment-translation-use-cases}

Inhoudsfragmenten zijn een herkend inhoudstype dat extracten AEM die naar een externe vertaalservice moeten worden verzonden. Verschillende gebruiksgevallen worden vanuit het vak ondersteund:

1. Een inhoudsfragment kan rechtstreeks in de middelenconsole worden geselecteerd voor het kopiëren en vertalen van talen
2. Inhoudsfragmenten waarnaar op een sitepagina wordt verwezen, worden gekopieerd naar de juiste taalmap en geëxtraheerd voor vertaling wanneer de sitepagina is geselecteerd voor een taalkopie
3. Inline media-elementen die zijn ingesloten in een inhoudsfragment, kunnen worden geëxtraheerd en vertaald.
4. Verzamelingen van middelen die aan een inhoudsfragment zijn gekoppeld, kunnen worden geëxtraheerd en vertaald

## Editor voor omzettingsregels {#translation-rules-editor}

Het vertaalgedrag van de Experience Manager kan worden bijgewerkt door **Redacteur van de Regels van de Vertaling** te gebruiken. Als u de vertaling wilt bijwerken, navigeert u naar **Tools** > **General** > **Translation Configuration** op [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

Uit de doos van de configuraties verwijzingsInhoudsfragmenten bij `fragmentPath` met een middeltype van `core/wcm/components/contentfragment/v1/contentfragment`. Alle componenten die van `v1/contentfragment` erven worden erkend door de standaardconfiguratie.

![Editor voor omzettingsregels](assets/translation-configuration.png)
