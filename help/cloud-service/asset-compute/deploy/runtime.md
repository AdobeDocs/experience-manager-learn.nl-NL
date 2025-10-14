---
title: Asset Compute-workers distribueren naar Adobe I/O Runtime voor gebruik met AEM as a Cloud Service
description: Asset Compute-projecten, en de werknemers die ze bevatten, moeten worden ingezet in Adobe I/O Runtime om door AEM as a Cloud Service te worden gebruikt.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
duration: 128
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# Distribueren naar Adobe I/O Runtime

Asset Compute-projecten, en de werknemers die ze bevatten, moeten via de Adobe I/O CLI naar Adobe I/O Runtime worden gestuurd om door AEM as a Cloud Service te worden gebruikt.

Bij implementatie naar Adobe I/O Runtime voor gebruik door AEM as a Cloud Service Author-services zijn slechts twee omgevingsvariabelen vereist:

+ `AIO_runtime_namespace` wijst de App Builder Workspace aan implementatie toe
+ `AIO_runtime_auth` zijn de verificatiegegevens van de App Builder-werkruimte

De andere standaardvariabelen die in het `.env` -bestand zijn gedefinieerd, worden impliciet door AEM as a Cloud Service verschaft wanneer de Asset Compute-worker wordt aangeroepen.

## Werkruimte Ontwikkeling

Omdat dit project is gegenereerd met `aio app init` in de `Development` -werkruimte, wordt `AIO_runtime_namespace` automatisch ingesteld op `81368-wkndaemassetcompute-development` met de overeenkomende `AIO_runtime_auth` in ons lokale `.env` -bestand.  Als een `.env` dossier in de folder bestaat die wordt gebruikt om het opstellen bevel uit te geven, worden zijn waarden gebruikt, tenzij zij via een OS niveau veranderlijke uitvoer worden vervangen, die is hoe [&#x200B; stadium en productie &#x200B;](#stage-and-production) werkruimten worden gericht.

![&#x200B; de implementatie van de audio-app gebruikend variabelen .env &#x200B;](./assets/runtime/development__aio.png)

Distribueren naar de werkruimte die is gedefinieerd in het projectbestand `.env` :

1. De opdrachtregel openen in de hoofdmap van het Asset Compute-project
1. De opdracht uitvoeren `aio app deploy`
1. Voer de opdracht `aio app get-url` uit om de URL van de worker te verkrijgen voor gebruik in het AEM as a Cloud Service-verwerkingsprofiel om naar deze aangepaste Asset Compute-worker te verwijzen. Als het project meerdere workers bevat, worden afzonderlijke URL&#39;s voor elke worker weergegeven.

Als de lokale ontwikkeling en de milieu&#39;s van de Ontwikkeling van AEM as a Cloud Service afzonderlijke plaatsingen van Asset Compute gebruiken, kunnen de plaatsingen aan AEM as a Cloud Service Dev op de zelfde manier worden beheerd zoals [&#x200B; het Stadium en de plaatsingen van de Productie &#x200B;](#stage-and-production).

## Werkruimten voor werkruimten Werkgebied en Productie{#stage-and-production}

De werkruimten Werkgebied en Productie worden typisch opgesteld door uw systeem van CI/CD van keus. Het Asset Compute-project moet op discrete wijze worden geïmplementeerd op elke Workspace (werkgebied en vervolgens productie).

Als u echte omgevingsvariabelen instelt, overschrijft u de waarden voor variabelen met dezelfde naam in `.env` .

![&#x200B; de implementatie van de audio-app gebruikend de uitvoervariabelen &#x200B;](./assets/runtime/stage__export-and-aio.png)

De algemene aanpak, die doorgaans door een CI/CD-systeem wordt geautomatiseerd, voor de implementatie in werkgebied- en productieomgevingen is:

1. Verzeker [&#x200B; Adobe I/O CLI npm module en de stop-in van Asset Compute &#x200B;](../set-up/development-environment.md#aio) geïnstalleerd zijn
1. Ontdek het Asset Compute-project dat u wilt implementeren vanuit Git
1. De omgevingsvariabelen instellen met de waarden die overeenkomen met de doelwerkruimte (werkgebied of productie)
   + De twee vereiste variabelen zijn `AIO_runtime_namespace` en `AIO_runtime_auth` en worden verkregen per werkruimte in Adobe I/O Developer Console via de Workspace __downloadt allen__ eigenschap.

![&#x200B; Adobe Developer Console - AIO Runtime Namespace en Auth &#x200B;](./assets/runtime/stage-auth-namespace.png)

De waarden van deze toetsen kunnen worden ingesteld door exportopdrachten uit te voeren via de opdrachtregel:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Als uw Asset Compute-workers andere variabelen nodig hebben, zoals cloudopslag, moeten deze ook als omgevingsvariabelen worden geëxporteerd.

1. Zodra alle milieuvariabelen voor de doelwerkruimte worden geplaatst om op te stellen, voer het opstellen bevel uit:
   + `aio app deploy`
1. De worker-URL waarnaar wordt verwezen door het AEM as a Cloud Service-verwerkingsprofiel is ook beschikbaar via:
   + `aio app get-url`.

Als de Asset Compute-projectversie verandert, veranderen de URL&#39;s van de worker ook in de nieuwe versie en moet de URL worden bijgewerkt in de verwerkingsprofielen.

## Workspace API-provisioning{#workspace-api-provisioning}

Toen [&#x200B; vestiging het project van App Builder in Adobe I/O &#x200B;](../set-up/app-builder.md) om lokale ontwikkeling te steunen, werd een nieuwe werkruimte van de Ontwikkeling gecreeerd en __Asset Compute, I/O Gebeurtenissen__ en __I/O het Beheer APIs van Gebeurtenissen__ werden toegevoegd aan het.

De __Asset Compute, I/O Gebeurtenissen__ en __APIs van het Beheer van Gebeurtenissen I/O__ APIS worden slechts uitdrukkelijk toegevoegd aan de werkruimten die voor lokale ontwikkeling worden gebruikt. De werkruimten die (exclusief) met de milieu&#39;s van AEM as a Cloud Service integreren ____ hebben deze uitdrukkelijk toegevoegde APIs niet nodig aangezien APIs van nature ter beschikking van AEM as a Cloud Service wordt gesteld.
