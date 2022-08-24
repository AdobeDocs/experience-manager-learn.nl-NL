---
title: Configuratie van cloudservices en formuliergegevensmodel naar cloudinstantie verplaatsen
description: Maak een adaptief formulier op basis van het Azure-opslagformuliergegevensmodel en duw dit naar de cloud-instantie.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 9006
exl-id: 77c00a35-43bf-485f-ac12-0fffb307dc16
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Configuratie van cloudservices opnemen in uw project

Maak een configuratiecontainer met de naam &#39;FormTutorial&#39; om uw configuratie voor cloudservices in te stellen. Maak een configuratie voor cloudservices voor Azure Storage met de naam &#39;FormsCSAndAzureBlob&#39; in de &#39;FormTutorial&#39;-container door de gegevens van de Azure-opslagaccount en de Azure-toegangssleutel op te geven.

Open uw AEM project in IntelliJ. Zorg ervoor dat u de map FormTutorial toevoegt zoals hieronder in het project ui.content wordt weergegeven
![cloud-services-configuratie](assets/cloud-services-configuration.png)

Zorg ervoor u de volgende ingang in het ui.content- project filter.xml toevoegt

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## Formuliergegevensmodel opnemen in uw project

Maak een formuliergegevensmodel op basis van de configuratie voor cloudservices die u in de vorige stap hebt gemaakt. Om het model van vormgegevens in uw project te omvatten creeer de aangewezen omslagstructuur in uw AEM project in intelliJ. Mijn formuliergegevensmodel bevindt zich bijvoorbeeld in een map die registraties wordt genoemd
![fdm-inhoud](assets/ui-content-fdm.png)

Neem het juiste item op in het bestand filter.xml van het project ui.content

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>Wanneer u nu uw project bouwt en implementeert met gebruik van cloudbeheer, moet u uw Azure-toegangssleutel opnieuw invoeren in de configuratie van cloudservices. Als u wilt voorkomen dat de toegangstoets opnieuw wordt ingevoerd, kunt u het beste contextbewuste configuratie maken met behulp van de omgevingsvariabelen die in het dialoogvenster [volgend artikel](./context-aware-fdm.md)
