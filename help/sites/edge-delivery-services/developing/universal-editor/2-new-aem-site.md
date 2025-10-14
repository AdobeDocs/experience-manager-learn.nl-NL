---
title: Een AEM-site maken
description: Maak een site in AEM Sites for Edge Delivery Services die u kunt bewerken met de Universal Editor.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
exl-id: d1ebcaf4-cea6-4820-8b05-3a0c71749d33
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 0%

---

# Een AEM-site maken

Op de AEM-site wordt de inhoud van de website bewerkt, beheerd en gepubliceerd. Om een plaats tot stand te brengen van AEM die via Edge Delivery Services wordt geleverd en gebruikend Universele Redacteur wordt geschreven, gebruik [&#x200B; Edge Delivery Services met het auteursplaatsmalplaatje van AEM &#x200B;](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases) om een nieuwe plaats op de Auteur van AEM tot stand te brengen.

Op de AEM-site wordt de inhoud van de website opgeslagen en geschreven. De definitieve ervaring is een combinatie de plaatsinhoud van AEM met de [&#x200B; code van de website &#x200B;](./1-new-code-project.md).

![&#x200B; Nieuwe Plaats van AEM voor Edge Delivery Services en Universele Redacteur &#x200B;](./assets/2-new-aem-site/new-site.png)

Volg de [&#x200B; gedetailleerde stappen die in documentatie &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-aem-site) worden geschetst om een nieuwe plaats van AEM tot stand te brengen.  Hieronder vindt u een overzicht van de stappen, inclusief de waarden die in deze zelfstudie worden gebruikt.
1. **creeer een nieuwe plaats** in de Auteur van AEM. In deze zelfstudie wordt de volgende sitenaam gebruikt:
   * Titel van site: `WKND (Universal Editor)`
   * Sitenaam: `aem-wknd-eds-ue`

      * De waarde van de plaatsnaam moet de naam van de plaatsweg [&#x200B; aanpassen die aan `paths.json` &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/path-mapping) wordt toegevoegd.

2. **de Invoer het recentste malplaatje** van [&#x200B; Edge Delivery Services met AEM creërend plaatsmalplaatje &#x200B;](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases).
3. **Naam de plaats** om de naam van de bewaarplaats aan te passen GitHub en plaats GitHub URL als URL van de bewaarplaats.

## Nieuwe site publiceren voor voorvertoning

Na het creëren van de plaats in de Auteur van AEM, publiceer het aan de voorproef die van Edge Delivery Services de inhoud ter beschikking stelt van de [&#x200B; lokale ontwikkelomgeving &#x200B;](./3-local-development-environment.md).

1. Login aan **Auteur van AEM** en navigeer aan **Plaatsen**.
2. Selecteer de **nieuwe plaats** (`WKND (Universal Editor)`) en klik **leidt Publicaties**.
3. Kies **Voorproef** onder **Doelen** en klik daarna **.**
4. Onder **omvat de Montages van Kinderen**, uitgezochte **omvat kinderen**, schrap andere opties, en klik **O.K.**.
5. Klik **publiceren** om de inhoud van de plaats aan voorproef te publiceren.
6. Als de pagina&#39;s eenmaal zijn gepubliceerd om een voorvertoning weer te geven, zijn ze beschikbaar in de Edge Delivery Services-voorvertoningsomgeving (de pagina&#39;s worden niet weergegeven op de AEM Preview-service).
