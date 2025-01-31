---
title: Een codeproject maken
description: Creeer een codeproject voor Edge Delivery Services, editable gebruikend de Universele Redacteur.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: e1fb7a58-2bba-4952-ad53-53ecf80836db
source-git-commit: 48b402642738abf512edab68b6074935cb7dd444
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 0%

---

# Een codeproject voor Edge Delivery Services maken

Om AEM websites voor Edge Delivery Services en Universele Redacteur te bouwen, gebruik Adobe [ AEM Boilerplate XWalk projectmalplaatje ](https://github.com/adobe-rnd/aem-boilerplate-xwalk). Met deze sjabloon maakt u een nieuw codeproject dat de CSS en JavaScript bevat die worden gebruikt voor het maken van de ervaring met websites. Deze malplaatje leidt tot een nieuwe bewaarplaats GitHub, en laadt het met boilerplate van de Adobe code en configuratie, die een stevige stichting voor uw AEM websiteproject verstrekken.

Herinner me, [ AEM websites die door Edge Delivery Services ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/overview) worden geleverd slechts cliënt-kant (browser) code hebben. De code van de website wordt niet uitgevoerd in de AEM Auteur of de diensten van Publish.

![ Nieuw project van Edge Delivery Services ](./assets/1-new-project/new-project.png)

Volg de [ gedetailleerde stappen die in documentatie ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) worden geschetst een project van de code van Edge Delivery Services de waarvan inhoud in Universele Redacteur editable is.  Hieronder vindt u een overzicht van de stappen, inclusief de waarden die in deze zelfstudie worden gebruikt.

1. **opstelling een rekening GitHub.** Als u een project voor uw organisatie creeert, zorg ervoor dat de organisatie een rekening GitHub heeft, en u bent een lid.
2. **creeer een nieuw codeproject** gebruikend het [ AEM Boilerplate XWalk projectmalplaatje ](https://github.com/adobe-rnd/aem-boilerplate-xwalk).
3. **installeer de AEM toepassing GitHub van de Synchronisatie van de Code** en verleent toegang tot de bewaarplaats. U kunt [ hier vinden app ](https://github.com/apps/aem-code-sync).
4. **vorm uw nieuw project`fstab.yaml`** om aan de correcte AEM dienst van de Auteur te richten.

   * Als u wilt experimenteren, kunt u lagere AEM as a Cloud Service-omgevingen gebruiken (Stage of Dev), maar echte implementaties van websites moeten worden geconfigureerd voor het gebruik van een productie AEM service.

5. **geef uw nieuw project`paths.json`** uit om de de dienstweg van de AEMAuteur aan de wortel van uw website in kaart te brengen.

Deze bewaarplaats van Git wordt gekloond in het [ lokale hoofdstuk van de ontwikkelomgeving ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/developing/universal-editor/3-local-development-environment), en waar de code wordt ontwikkeld.
