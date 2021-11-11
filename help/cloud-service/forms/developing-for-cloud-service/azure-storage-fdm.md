---
title: Configuratie van cloudservices en formuliergegevensmodel naar cloudinstantie verplaatsen
description: Maak een adaptief formulier op basis van het Azure-opslagformuliergegevensmodel en duw dit naar de cloud-instantie.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 9006
source-git-commit: 8484897297940ab28619c4b1af5362a5937eadfa
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%

---


# Configuratie van cloudservices opnemen in uw project

Maak een configuratiecontainer met de naam &#39;FormsTutorial&#39; voor de configuratie van uw cloudservices. Maak een configuratie voor cloudservices voor Azure Storage met de naam &#39;Formulierverzendingen opslaan in Azure&#39; in de container &#39;FormsTutorial&#39;. Geef de gegevens van de Azure-opslagaccount en de accountsleutel op

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
>Wanneer u nu uw project maakt en implementeert, beschikt het project over het formuliergegevensmodel op basis van de configuratie van de cloudservices die beschikbaar is in uw cloudinstantie
