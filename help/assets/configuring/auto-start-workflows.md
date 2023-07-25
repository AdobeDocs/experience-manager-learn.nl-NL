---
title: Workflows automatisch starten
description: Workflows voor automatisch opstarten breiden de verwerking van bedrijfsmiddelen uit door automatisch een aangepaste workflow aan te roepen bij het uploaden of opnieuw verwerken.
feature: Asset Compute Microservices, Workflow
version: Cloud Service
kt: 4994
thumbnail: 37323.jpg
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-05-14T00:00:00Z
exl-id: 5e423f2c-90d2-474f-8bdc-fa15ae976f18
source-git-commit: 861b171b8ebbcf9565bdc94fb84a043ecb99c00a
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# Workflows automatisch starten

Workflows voor automatisch opstarten breiden de verwerking van bedrijfsmiddelen uit in AEM as a Cloud Service door automatisch een aangepaste workflow aan te roepen bij het uploaden of opnieuw verwerken zodra de verwerking van het element is voltooid.

>[!VIDEO](https://video.tv.adobe.com/v/37323?quality=12&learn=on)
> `Notice`: Gebruik Workflows automatisch starten voor het aanpassen van elementen na verwerking in plaats van Workflowopstartsters te gebruiken. Workflows voor automatisch opstarten zijn _alleen_ Wordt aangeroepen wanneer een element volledige verwerking is in plaats van draagraketten die tijdens de verwerking van elementen meerdere keren kunnen worden aangeroepen.

## De naverwerkingsworkflow aanpassen

Als u de naverwerkingsworkflow wilt aanpassen, kopieert u de standaardMiddelen in de cloud naverwerken [workflowmodel](../../foundation/workflow/use-the-workflow-editor.md).

1. Begin bij het scherm van de Modellen van het Werkschema door aan te navigeren _Gereedschappen_ > _Workflow_ > _Modellen_
2. Zoek en selecteer de _Middelen naverwerken via de cloud_ workflowmodel<br/>
   ![Het workflowmodel voor naverwerking vanaf de cloud selecteren](assets/auto-start-workflow-select-workflow.png)
3. Selecteer _Kopiëren_ knop om uw aangepaste workflow te maken
4. Selecteer uw huidige workflowmodel (dat nu wordt genoemd) _Middelen na verwerking door cloud1_) en klik op de knop _Bewerken_ knop om de workflow te bewerken
5. Geef uw aangepaste naverwerkingsworkflow een betekenisvolle naam in de Workfloweigenschappen<br/>
   ![De naam wijzigen](assets/auto-start-workflow-change-name.png)
6. Voeg de stappen toe om aan uw bedrijfsvereisten te voldoen. Voeg in dit geval een taak toe wanneer de activa volledige verwerking zijn. Zorg ervoor dat de laatste stap van de workflow altijd de _Workflow voltooid_ stap<br/>
   ![Workflowstappen toevoegen](assets/auto-start-workflow-customize-steps.png)
   > `Note`: Workflows voor automatisch opstarten worden uitgevoerd met elk element dat wordt geüpload of opnieuw wordt verwerkt. Denk hierbij zorgvuldig na over de gevolgen die het schalen van workflowstappen heeft voor met name bulkbewerkingen, zoals [Bulkinvoer](../../cloud-service/migration/bulk-import.md) of migraties.
7. Selecteer _Synchroniseren_ om uw wijzigingen op te slaan en het workflowmodel te synchroniseren

## Een aangepaste naverwerkingsworkflow gebruiken

Aangepaste naverwerking wordt geconfigureerd in mappen. Een aangepaste naverwerkingsworkflow voor een map configureren:

1. Selecteer de map waarvoor u de workflow wilt configureren en bewerk de eigenschappen van de map
2. Naar de _Middelverwerking_ tab
3. Selecteer de aangepaste naverwerkingsworkflow in het dialoogvenster _Workflow automatisch starten_ selectievak<br/>
   ![De naverwerkingsworkflow instellen](assets/auto-start-workflow-set-workflow.png)
4. Uw wijzigingen opslaan

De aangepaste naverwerkingsworkflow wordt nu uitgevoerd voor alle elementen die in die map zijn geüpload of opnieuw verwerkt.