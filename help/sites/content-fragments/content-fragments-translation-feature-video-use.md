---
title: Ondersteuning voor vertaling voor AEM Content Fragments
description: Leer hoe u Content Fragments kunt lokaliseren en vertalen met Adobe Experience Manager. Elementen met gemengde media die aan een inhoudsfragment zijn gekoppeld, kunnen ook worden uitgepakt en vertaald.
feature: Content Fragments, Multi Site Manager
topic: Localization
role: User
level: Intermediate
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
jira: KT-201
thumbnail: 18131.jpg
doc-type: Feature Video
exl-id: cc4ffbd0-207a-42e4-bfcb-d6c83fb97237
duration: 223
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 0%

---

# Ondersteuning voor vertaling voor AEM Content Fragments {#translation-support-content-fragments}

Leer hoe u Content Fragments kunt lokaliseren en vertalen met Adobe Experience Manager. Elementen met gemengde media die aan een inhoudsfragment zijn gekoppeld, kunnen ook worden uitgepakt en vertaald.

>[!VIDEO](https://video.tv.adobe.com/v/18131?quality=12&learn=on)

## Gebruik hoofdletters/kleine letters voor het omzetten van inhoudsfragmenten {#content-fragment-translation-use-cases}

Inhoudsfragmenten zijn een herkend inhoudstype dat door AEM wordt geëxtraheerd voor verzending naar een externe vertaalservice. Verschillende gebruiksgevallen worden vanuit het vak ondersteund:

1. Een tevreden Fragment kan [ direct in de console van Assets voor taalexemplaar en vertaling ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html?lang=nl-NL) worden geselecteerd.
2. Inhoudsfragmenten waarnaar op een sitepagina wordt verwezen, worden naar de juiste taalmap gekopieerd en voor vertaling uitgepakt wanneer de sitepagina is geselecteerd voor een taalkopie.
3. Inline media-elementen die zijn ingesloten in een inhoudsfragment, kunnen worden geëxtraheerd en vertaald.
4. Verzamelingen van middelen die aan een inhoudsfragment zijn gekoppeld, kunnen worden uitgepakt en vertaald.

## Editor voor omzettingsregels {#translation-rules-editor}

Het vertaalgedrag van Experience Manager kan worden bijgewerkt door de **Redacteur van de Regels van de Vertaling te gebruiken**. Om de vertaling bij te werken, navigeer aan **Hulpmiddelen** > **Algemeen** > **Vertaalconfiguratie** in [ http://localhost:4502/libs/cq/translation/translationrules/contexts.html ](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

Verwijs vanuit de box configuraties naar Content Fragments bij `fragmentPath` met een middeltype van `core/wcm/components/contentfragment/v1/contentfragment`. Alle componenten die van `v1/contentfragment` erven worden erkend door de standaardconfiguratie.

![ Redacteur van de Regels van de Vertaling ](assets/translation-configuration.png)
