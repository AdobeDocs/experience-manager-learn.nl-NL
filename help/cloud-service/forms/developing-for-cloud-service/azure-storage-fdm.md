---
title: Configuratie van cloudservices en formuliergegevensmodel naar cloudinstantie verplaatsen
description: Maak een adaptief formulier op basis van het Azure-opslagformuliergegevensmodel en duw dit naar de cloud-instantie.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Developer Tools
jira: KT-9006
exl-id: 77c00a35-43bf-485f-ac12-0fffb307dc16
duration: 45
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# Configuratie van cloudservices opnemen in uw project

Een configuratiecontainer met de naam &#39;FormTutorial&#39; maken voor de configuratie van uw cloudservices
Maak een configuratie voor cloudservices voor Azure Storage met de naam &#39;FormsCSAndAzureBlob&#39; in de container &#39;FormTutorial&#39; door de Azure-opslagaccountgegevens en de Azure-toegangssleutel op te geven.

Open uw AEM-project in IntelliJ. Zorg ervoor dat u de map FormTutorial toevoegt zoals hieronder in het project ui.content wordt weergegeven
![&#x200B; wolk-diensten-configuratie &#x200B;](assets/cloud-services-configuration.png)

Zorg ervoor u de volgende ingang in het ui.content- project filter.xml toevoegt

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![&#x200B; filter-xml &#x200B;](assets/ui-content-filter.png)

## Formuliergegevensmodel opnemen in uw project

Maak een formuliergegevensmodel op basis van de configuratie voor cloudservices die u in de vorige stap hebt gemaakt. Om het model van vormgegevens in uw project te omvatten creeer de aangewezen omslagstructuur in uw AEM project in intelliJ. Mijn formuliergegevensmodel bevindt zich bijvoorbeeld in een map die registraties wordt genoemd
![&#x200B; fdm-inhoud &#x200B;](assets/ui-content-fdm.png)

Neem het juiste item op in het bestand filter.xml van het project ui.content

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>Wanneer u nu uw project bouwt en implementeert met gebruik van cloudbeheer, moet u uw Azure-toegangssleutel opnieuw invoeren in de configuratie van cloudservices. Vermijd het opnieuw ingaan van de toegangstoets, wordt het geadviseerd om context te creëren bewuste configuratie gebruikend de milieuvariabelen zoals die in het [&#x200B; volgende artikel &#x200B;](./context-aware-fdm.md) worden verklaard

## Volgende stappen

[Contextbewuste configuratie maken](./context-aware-fdm.md)
