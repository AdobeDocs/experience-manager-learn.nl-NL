---
title: Ondersteuning voor vertaling van AEM Content Fragments
description: Leer hoe u Content Fragments kunt lokaliseren en vertalen met Adobe Experience Manager. Elementen met gemengde media die aan een inhoudsfragment zijn gekoppeld, kunnen ook worden uitgepakt en vertaald.
feature: Content Fragments, Multi Site Manager
topic: Localization
role: User
level: Intermediate
version: 6.4, 6.5, Cloud Service
jira: KT-201
thumbnail: 18131.jpg
doc-type: Feature Video
exl-id: cc4ffbd0-207a-42e4-bfcb-d6c83fb97237
duration: 245
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 0%

---

# Ondersteuning voor vertaling van AEM Content Fragments {#translation-support-content-fragments}

Leer hoe u Content Fragments kunt lokaliseren en vertalen met Adobe Experience Manager. Elementen met gemengde media die aan een inhoudsfragment zijn gekoppeld, kunnen ook worden uitgepakt en vertaald.

>[!VIDEO](https://video.tv.adobe.com/v/18131?quality=12&learn=on)

## Gebruik hoofdletters/kleine letters voor het omzetten van inhoudsfragmenten {#content-fragment-translation-use-cases}

Inhoudsfragmenten zijn een herkend inhoudstype dat extracten AEM die naar een externe vertaalservice moeten worden verzonden. Verschillende gebruiksgevallen worden vanuit het vak ondersteund:

1. Een inhoudsfragment kan [rechtstreeks geselecteerd in de middelenconsole voor het kopiëren en vertalen van talen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html).
2. Inhoudsfragmenten waarnaar op een sitepagina wordt verwezen, worden naar de juiste taalmap gekopieerd en voor vertaling uitgepakt wanneer de sitepagina is geselecteerd voor een taalkopie.
3. Inline media-elementen die zijn ingesloten in een inhoudsfragment, kunnen worden geëxtraheerd en vertaald.
4. Verzamelingen van middelen die aan een inhoudsfragment zijn gekoppeld, kunnen worden uitgepakt en vertaald.

## Editor voor omzettingsregels {#translation-rules-editor}

Het vertaalgedrag van de Experience Manager kan worden bijgewerkt door **Editor voor omzettingsregels**. Als u de vertaling wilt bijwerken, navigeert u naar **Gereedschappen** > **Algemeen** > **Omzetconfiguratie** om [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

Verwijs uit de doos configuraties de Fragmenten van de verwijzingsInhoud bij `fragmentPath` met een type resource `core/wcm/components/contentfragment/v1/contentfragment`. Alle componenten die van het `v1/contentfragment` worden herkend door de standaardconfiguratie.

![Editor voor omzettingsregels](assets/translation-configuration.png)
