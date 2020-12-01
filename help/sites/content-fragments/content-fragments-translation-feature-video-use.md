---
title: Vertaling gebruiken met AEM inhoudsfragmenten
description: AEM 6.3 introduceert de mogelijkheid om inhoudsfragmenten te vertalen. Elementen met gemengde media en verzamelingen van middelen die bij een inhoudsfragment horen, kunnen ook worden geëxtraheerd en vertaald.
sub-product: sites, activa, inhoudsdiensten
feature: content-fragments, multi-site-manager
topics: localization, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 0%

---


# Vertaling gebruiken met AEM inhoudsfragmenten{#using-translation-with-aem-content-fragments}

AEM 6.3 introduceert de mogelijkheid om inhoudsfragmenten te vertalen. Elementen met gemengde media en verzamelingen van middelen die bij een inhoudsfragment horen, kunnen ook worden geëxtraheerd en vertaald.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=9&learn=on)

## Gebruik Gevallen {#content-fragment-translation-use-cases} voor het omzetten van inhoudsfragmenten

Inhoudsfragmenten zijn herkende inhoudsfragmenten die AEM uitpakken voor verzending naar een externe vertaalservice. Verschillende gebruiksgevallen worden vanuit het vak ondersteund:

1. Een inhoudsfragment kan rechtstreeks in de middelenconsole worden geselecteerd voor het kopiëren en vertalen van talen
2. Inhoudsfragmenten waarnaar op een sitepagina wordt verwezen, worden gekopieerd naar de juiste taalmap en geëxtraheerd voor vertaling wanneer de sitepagina is geselecteerd voor een taalkopie
3. Inline media-elementen die zijn ingesloten in een inhoudsfragment, kunnen worden geëxtraheerd en vertaald.
4. Verzamelingen van middelen die aan een inhoudsfragment zijn gekoppeld, kunnen worden geëxtraheerd en vertaald

## Opties voor vertaalconfiguratie {#translation-config-options}

In de vertaalconfiguratie in het vak kunt u kiezen uit verschillende opties voor het vertalen van inhoudsfragmenten. Standaard worden inline media-elementen en bijbehorende elementen NIET vertaald. Als u de vertaalconfiguratie wilt bijwerken, navigeert u naar [http://localhost:4502/etc/cloudservices/translation/default_translation.html](http://localhost:4502/etc/cloudservices/translation/default_translation.html).

Er zijn vier opties voor het omzetten van Content Fragment-elementen:

1. **Niet vertalen (standaard)**
2. **Alleen inline-media-elementen**
3. **Alleen gekoppelde Asset Collections**
4. **Inline-media-elementen en bijbehorende verzamelingen**

![Config. omzetten](assets/classic-ui-dialog.png)
