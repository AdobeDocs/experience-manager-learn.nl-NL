---
title: Workers voor Asset computen distribueren naar Adobe I/O Runtime voor gebruik met AEM as a Cloud Service
description: Asset compute-projecten, en de werknemers die ze bevatten, moeten in Adobe I/O Runtime worden ingezet om door AEM as a Cloud Service te worden gebruikt.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
duration: 128
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# Distribueren naar Adobe I/O Runtime

De projecten van de asset compute, en de werknemers die zij bevatten, moeten via de Adobe I/O CLI worden opgesteld aan Adobe I/O Runtime om door AEM as a Cloud Service te worden gebruikt.

Bij implementatie naar Adobe I/O Runtime voor gebruik door AEM as a Cloud Service Author-services zijn slechts twee omgevingsvariabelen vereist:

+ `AIO_runtime_namespace` wijst de App Builder Workspace aan implementatie toe
+ `AIO_runtime_auth` zijn de verificatiegegevens van de App Builder-werkruimte

De andere standaardvariabelen die in het `.env` -bestand zijn gedefinieerd, worden impliciet door AEM as a Cloud Service verschaft wanneer de Asset compute worker wordt aangeroepen.

## Werkruimte Ontwikkeling

Omdat dit project is gegenereerd met `aio app init` in de `Development` -werkruimte, wordt `AIO_runtime_namespace` automatisch ingesteld op `81368-wkndaemassetcompute-development` met de overeenkomende `AIO_runtime_auth` in ons lokale `.env` -bestand.  Als een `.env` dossier in de folder bestaat die wordt gebruikt om het opstellen bevel uit te geven, worden zijn waarden gebruikt, tenzij zij via een OS niveau veranderlijke uitvoer worden vervangen, die is hoe [ stadium en productie ](#stage-and-production) werkruimten worden gericht.

![ de implementatie van de audio-app gebruikend variabelen .env ](./assets/runtime/development__aio.png)

Distribueren naar de werkruimte die is gedefinieerd in het projectbestand `.env` :

1. Open de bevellijn in de wortel van het project van de Asset compute
1. De opdracht uitvoeren `aio app deploy`
1. Voer de opdracht `aio app get-url` uit om de URL van de worker te verkrijgen voor gebruik in het AEM as a Cloud Service-verwerkingsprofiel om naar deze aangepaste Asset compute-worker te verwijzen. Als het project meerdere workers bevat, worden afzonderlijke URL&#39;s voor elke worker weergegeven.

Als de lokale ontwikkeling en de milieu&#39;s van de Ontwikkeling van AEM as a Cloud Service afzonderlijke plaatsingen van de Asset compute gebruiken, kunnen de plaatsingen aan AEM as a Cloud Service Dev op de zelfde manier zoals [ het Stadium en de plaatsingen van de Productie ](#stage-and-production) worden beheerd.

## Werkruimten voor werkruimten Werkgebied en Productie{#stage-and-production}

De werkruimten Werkgebied en Productie worden typisch opgesteld door uw systeem van CI/CD van keus. Het project van de Asset compute moet aan elke Workspace (Stadium en toen Productie) afzonderlijk worden opgesteld.

Als u echte omgevingsvariabelen instelt, overschrijft u de waarden voor variabelen met dezelfde naam in `.env` .

![ de implementatie van de audio-app gebruikend de uitvoervariabelen ](./assets/runtime/stage__export-and-aio.png)

De algemene aanpak, die doorgaans door een CI/CD-systeem wordt geautomatiseerd, voor de implementatie in werkgebied- en productieomgevingen is:

1. Verzeker de [ Adobe I/O CLI npm module en de insteekmodule van de Asset compute ](../set-up/development-environment.md#aio) geïnstalleerd zijn
1. Controle uit het project van de Asset compute om van Git op te stellen
1. De omgevingsvariabelen instellen met de waarden die overeenkomen met de doelwerkruimte (werkgebied of productie)
   + De twee vereiste variabelen zijn `AIO_runtime_namespace` en `AIO_runtime_auth` en worden verkregen per werkruimte in Adobe I/O Developer Console via de Workspace __Download Al__ eigenschap.

![ Adobe Developer Console - AIO Runtime Namespace en Auth ](./assets/runtime/stage-auth-namespace.png)

De waarden van deze toetsen kunnen worden ingesteld door exportopdrachten uit te voeren via de opdrachtregel:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Als uw medewerkers van de Asset compute andere variabelen nodig hebben, zoals cloudopslag, moeten deze ook als omgevingsvariabelen worden geëxporteerd.

1. Zodra alle milieuvariabelen voor de doelwerkruimte worden geplaatst om op te stellen, voer het opstellen bevel uit:
   + `aio app deploy`
1. De worker-URL waarnaar wordt verwezen door het AEM as a Cloud Service-verwerkingsprofiel is ook beschikbaar via:
   + `aio app get-url`.

Als de projectversie van de Asset compute verandert, veranderen de worker-URL&#39;s ook om de nieuwe versie weer te geven en moet de URL worden bijgewerkt in de verwerkingsprofielen.

## Workspace API-provisioning{#workspace-api-provisioning}

Toen [ vestiging het project van App Builder in Adobe I/O ](../set-up/app-builder.md) om lokale ontwikkeling te steunen, werd een nieuwe werkruimte van de Ontwikkeling gecreeerd en __Asset compute, werden de Gebeurtenissen I/O__ en __I/O het Beheer APIs van Gebeurtenissen__ toegevoegd aan het.

De __Asset compute, I/O Gebeurtenissen__ en __APIs van het Beheer van Gebeurtenissen I/O__ APIS worden slechts uitdrukkelijk toegevoegd aan de werkruimten die voor lokale ontwikkeling worden gebruikt. De werkruimten die (exclusief) met de milieu&#39;s van AEM as a Cloud Service integreren ____ hebben deze uitdrukkelijk toegevoegde APIs niet nodig aangezien APIs van nature ter beschikking van AEM as a Cloud Service wordt gesteld.
