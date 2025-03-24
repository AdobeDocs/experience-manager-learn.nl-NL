---
title: Een codeproject maken
description: Maak een codeproject voor Edge Delivery Services dat u kunt bewerken met de Universal Editor.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: e1fb7a58-2bba-4952-ad53-53ecf80836db
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 0%

---

# Een Edge Delivery Services-codeproject maken

Om AEM websites voor Edge Delivery Services en Universele Redacteur te bouwen, gebruik Adobe [ AEM Boilerplate XWalk projectmalplaatje ](https://github.com/adobe-rnd/aem-boilerplate-xwalk). Met deze sjabloon maakt u een nieuw codeproject dat de CSS en JavaScript bevat die worden gebruikt voor het maken van de ervaring met websites. Deze sjabloon maakt een nieuwe GitHub-opslagplaats en laadt deze met Adobe boilerplate-code en -configuratie, die een solide basis voor uw AEM-websiteproject bieden.

Herinner me, [ AEM websites die door Edge Delivery Services ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/overview) worden geleverd hebben slechts cliënt-kant (browser) code. Websitecode wordt niet uitgevoerd in de AEM-auteur- of -publicatieservices.

![ Nieuw project van Edge Delivery Services ](./assets/1-new-project/new-project.png)

Volg de [ gedetailleerde stappen die in documentatie ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) worden geschetst voor het creëren van een de codeproject van Edge Delivery Services de waarvan inhoud in Universele Redacteur editable is.  Hieronder vindt u een overzicht van de stappen, inclusief de waarden die in deze zelfstudie worden gebruikt.

1. **opstelling een rekening GitHub.** Als u een project voor uw organisatie creeert, zorg ervoor dat de organisatie een rekening GitHub heeft, en u bent een lid.
2. **creeer een nieuw codeproject** gebruikend het [ AEM Boilerplate XWalk projectmalplaatje ](https://github.com/adobe-rnd/aem-boilerplate-xwalk).
3. **installeer de app van GitHub van de Synchronisatie van de Code van AEM** en verleent toegang tot de bewaarplaats. U kunt [ hier vinden app ](https://github.com/apps/aem-code-sync).
4. **vorm uw nieuw project`fstab.yaml`** om aan de correcte dienst van de Auteur van AEM te richten.

   * Als u wilt experimenteren, kunt u lagere AEM as a Cloud Service-omgevingen gebruiken (Stage of Dev), maar echte websites-implementaties moeten worden geconfigureerd om een AEM-service voor productie te gebruiken.

5. **geef het nieuwe project`paths.json`** uit om de de dienstweg van de Auteur van AEM aan de wortel van uw website in kaart te brengen.

Deze bewaarplaats van Git wordt gekloond in het [ lokale hoofdstuk van de ontwikkelomgeving ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/developing/universal-editor/3-local-development-environment), en waar de code wordt ontwikkeld.
