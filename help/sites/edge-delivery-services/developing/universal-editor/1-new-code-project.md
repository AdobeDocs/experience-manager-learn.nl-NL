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
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 0%

---


# Een codeproject voor Edge Delivery Services maken

Om AEM websites voor Edge Delivery Services en Universele Redacteur te bouwen, gebruik Adobe [ AEM Boilerplate XWalk projectmalplaatje ](https://github.com/adobe-rnd/aem-boilerplate-xwalk). Met deze sjabloon maakt u een nieuw codeproject dat de CSS en JavaScript bevat die worden gebruikt voor het maken van de ervaring met websites. Deze malplaatje leidt tot een nieuwe bewaarplaats GitHub, en laadt het met boilerplate van de Adobe code en configuratie, die een stevige stichting voor uw AEM websiteproject verstrekken.

Herinner me, [ AEM websites die door Edge Delivery Services ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/overview) worden geleverd slechts cliënt-kant (browser) code hebben. De code van de website wordt niet uitgevoerd in de AEM Auteur of de diensten van Publish.

![ Nieuw project van Edge Delivery Services ](./assets/1-new-project/new-project.png)

Voer de onderstaande stappen uit om een Edge Delivery Services-codeproject te maken waarvan de inhoud kan worden bewerkt in de Universal Editor:

1. **opstelling een rekening GitHub.** Als u een project voor uw organisatie creeert, zorg ervoor dat de organisatie een rekening GitHub heeft, en u bent een lid.
2. **creeer een nieuw codeproject** gebruikend het [ AEM Boilerplate XWalk projectmalplaatje ](https://github.com/adobe-rnd/aem-boilerplate-xwalk).
3. **installeer de AEM toepassing GitHub van de Synchronisatie van de Code** en verleent toegang tot de bewaarplaats. U kunt [ hier vinden app ](https://github.com/apps/aem-code-sync).
4. **vorm uw nieuw project`fstab.yaml`** om aan de correcte AEM dienst van de Auteur te richten.

   * Als u wilt experimenteren, kunt u lagere AEM as a Cloud Service-omgevingen gebruiken (Stage, Dev of RDE), maar echte websites-implementaties moeten worden geconfigureerd om een productie AEM Auteur-service te gebruiken.

5. **geef uw nieuw project`paths.json`** uit om de de dienstweg van de AEMAuteur aan de wortel van uw website in kaart te brengen.

Voor meer gedetailleerde instructies, controleer uit [ creeer uw het projectsectie van GitHub ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) in de begonnen gids.
