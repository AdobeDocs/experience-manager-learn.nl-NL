---
title: Zelfstudie voor AEM Headless OpenAPI | Aflevering van inhoudsfragment
description: Een end-to-end zelfstudie waarin wordt geïllustreerd hoe u inhoud kunt samenstellen en beschikbaar maken met behulp van op AEM op OpenAPI gebaseerde API's voor het leveren van inhoudsfragmenten.
short-description: Een zelfstudie waarin wordt uitgelegd hoe u AEM-inhoud kunt ontwikkelen en beschikbaar maken met Content Fragment Delivery met OpenAPI's en deze kunt gebruiken in een externe app voor CMS-scenario's zonder kop.
doc-type: Tutorial
mini-toc-levels: 1
jira: KT-6678
thumbnail: KT-6678.jpg
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
duration: 54
exl-id: 1bb7c415-58f8-4f6c-a0bc-38bdbdb521cf
source-git-commit: f0b1b906e1ef04b53eca940f191e65d62a2e0bab
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 1%

---

# Aan de slag met AEM Content Fragment Delivery met OpenAPI&#39;s

Bekijk deze zelfstudie waarin u leert hoe u AEM-inhoud kunt ontwikkelen en beschikbaar maken met AEM Content Fragment Delivery with OpenAPIs en die door een externe app wordt gebruikt, in een CMS-scenario zonder kop. Dit verkent deze concepten door te lopen denken de verwezenlijking van een React app die teams WKND en bijbehorende liddetails toont. Teams en leden zijn gemodelleerd met AEM Content Fragment Models en worden gebruikt door de React-app met AEM Content Fragment Delivery met OpenAPI&#39;s.

![ app van de Teams van WKND ](./assets/overview/main.png)

Deze zelfstudie behandelt de volgende onderwerpen:

* Een projectconfiguratie maken
* Modellen van inhoudsfragmenten maken om gegevens te modelleren
* Inhoudsfragmenten maken op basis van de eerder gemaakte modellen
* Ontdek hoe u Content Fragments in AEM kunt opvragen met de AEM Content Fragment Delivery met de Try It-functie van de OpenAPI-documentatie
* Gegevens van inhoudsfragmenten opnemen via AEM Content Fragment Delivery met OpenAPI-aanroepen vanuit een voorbeeldtoepassing React
* De React-app verbeteren om te kunnen worden bewerkt in de Universal Editor

## Vereisten {#prerequisites}

U hebt het volgende nodig om deze zelfstudie te volgen:

* AEM Sites as a Cloud Service
* Basisvaardigheden voor HTML en JavaScript
* De volgende gereedschappen moeten lokaal zijn geïnstalleerd:
   * [ Node.js v22+ ](https://nodejs.org/)
   * [ Git ](https://git-scm.com/)
   * IDE (bijvoorbeeld, [ Microsoft® Visual Studio Code ](https://code.visualstudio.com/))

### AEM as a Cloud Service omgeving

Om dit leerprogramma te voltooien, adviseert men dat u **toegang van de Beheerder van 0} AEM tot een milieu van AEM as a Cloud Service hebt.** A **het milieu van de Ontwikkeling**, **Snelle Milieu van de Ontwikkeling**, of een milieu in het Programma van de zandbak van de a **** kan ook worden gebruikt.

## Laten we beginnen!

Begin het leerprogramma met [ het Bepalen Modellen van het Fragment van de Inhoud ](1-content-fragment-models.md).

## GitHub-project

De broncode, en inhoudspakketten zijn beschikbaar in de [ Hoofdloze leerprogramma&#39;s van AEM ](https://github.com/adobe/aem-tutorials) bewaarplaats GitHub.

De [`main` tak bevat de definitieve broncode ](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic) voor dit leerprogramma.
Momentopnamen van de code aan het einde van elke stap zijn beschikbaar als Git-tags.

* Begin van hoofdstuk 4 - Reactie-app: [`headless_open-api_basic` ](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic//headless/open-api/basic)
* Einde van hoofdstuk 4 - Reactie-app: [`headless_open-api_basic_4-end` ](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end//headless/open-api/basic)
* Einde van hoofdstuk 5 - Universele editor: [`headless_open-api_basic_5-end` ](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_5-end//headless/open-api/basic)

Als u een kwestie met het leerprogramma of de code vindt, verlaat de kwestie van a [ GitHub ](https://github.com/adobe/aem-tutorials/issues).
