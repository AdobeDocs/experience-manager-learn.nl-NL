---
title: Workers voor middelenverwerking naar Adobe I/O Runtime implementeren voor gebruik met AEM als Cloud Service
description: 'De projecten van de Compute van activa, en de arbeiders die zij bevatten, moeten aan Adobe I/O Runtime worden opgesteld om door AEM als Cloud Service te worden gebruikt. '
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '648'
ht-degree: 0%

---


# Distribueren naar Adobe I/O Runtime

De projecten van de Compute van activa, en de arbeiders die zij bevatten, moeten aan Adobe I/O Runtime via Adobe I/O CLI worden opgesteld die door AEM als Cloud Service moet worden gebruikt.

Wanneer het opstellen aan Adobe I/O Runtime voor gebruik door AEM als de diensten van de Auteur van de Cloud Service slechts worden twee milieuvariabelen vereist:

+ `AIO_runtime_namespace` wijst de werkruimte van het Project van de Adobe om te opstellen
+ `AIO_runtime_auth` zijn de verificatiereferenties van de Adobe Project Firefly-werkruimte

De andere standaardvariabelen die in het `.env` bestand zijn gedefinieerd, worden impliciet door AEM als Cloud Service verstrekt wanneer de worker Asset Compute wordt aangeroepen.

## Werkruimte Ontwikkeling

Omdat dit project is gegenereerd `aio app init` met behulp van de `Development` werkruimte, `AIO_runtime_namespace` wordt dit automatisch ingesteld op `81368-wkndaemassetcompute-development` de overeenkomende `AIO_runtime_auth` code in ons lokale `.env` bestand.  Als een `.env` dossier in de folder bestaat die wordt gebruikt om het opstellen bevel uit te geven, worden zijn waarden gebruikt, tenzij zij via een OS niveau veranderlijke uitvoer worden vervangen, die is hoe het [stadium en de productiegerelateerde](#stage-and-production) werkruimten worden gericht.

![Implementatie van AIR-apps met behulp van .env-variabelen](./assets/runtime/development__aio.png)

Om op te stellen aan de werkruimte die in het `.env` projectdossier wordt bepaald:

1. Open de bevellijn in de wortel van het Element Compute project
1. De opdracht uitvoeren `aio app deploy`
1. Voer de opdracht uit `aio app get-url` om de URL van de worker te verkrijgen voor gebruik in de AEM als een Cloud Service-verwerkingsprofiel om naar deze aangepaste worker Asset Compute te verwijzen. Als het project meerdere workers bevat, worden afzonderlijke URL&#39;s voor elke worker weergegeven.

Als de lokale ontwikkeling en AEM als milieu&#39;s van de Ontwikkeling van de Cloud Service afzonderlijke plaatsingen van de Compute van Activa gebruiken, kunnen de plaatsingen aan AEM als Cloud Service Dev op de zelfde manier zoals [Stadium en de plaatsingen](#stage-and-production)van de Productie worden beheerd.

## Werkruimten voor werkruimten Werkgebied en Productie{#stage-and-production}

De werkruimten Werkgebied en Productie worden typisch opgesteld door uw systeem van CI/CD van keus. Het project van de Verwerking van Activa moet aan elke Werkruimte (Stadium en toen Productie) afzonderlijk worden opgesteld.

Als u echte omgevingsvariabelen instelt, worden de waarden voor dezelfde variabelen overschreven in `.env`.

![Implementatie van apps met behulp van exportvariabelen](./assets/runtime/stage__export-and-aio.png)

De algemene aanpak, die doorgaans door een CI/CD-systeem wordt geautomatiseerd, voor de implementatie in werkgebied- en productieomgevingen is:

1. Zorg ervoor dat de [Adobe I/O CLI npm-module en de Asset Compute-plug-in](../set-up/development-environment.md#aio) zijn geïnstalleerd
1. Ontdek het project Asset Compute dat vanaf Git moet worden geïmplementeerd
1. De omgevingsvariabelen instellen met de waarden die overeenkomen met de doelwerkruimte (werkgebied of productie)
   + De twee vereiste variabelen zijn `AIO_runtime_namespace` en worden per werkruimte verkregen in de Adobe I/O-ontwikkelaarsconsole via de functie Alles `AIO_runtime_auth` ____ downloaden van Workspace.

![Adobe Developer Console - AIO Runtime Namespace en Auth](./assets/runtime/stage-auth-namespace.png)

De waarden van deze toetsen kunnen worden ingesteld door exportopdrachten uit te voeren via de opdrachtregel:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Als uw workers voor Asset Compute andere variabelen nodig hebben, zoals cloudopslag, moeten deze ook als omgevingsvariabelen worden geëxporteerd.

1. Zodra alle milieuvariabelen voor de doelwerkruimte worden geplaatst om op te stellen, voer het opstellen bevel uit:
   + `aio app deploy`
1. De worker-URL(&#39;s) waarnaar de AEM verwijst als een Cloud Service-verwerkingsprofiel is ook beschikbaar via:
   + `aio app get-url`.

Als de projectversie van Asset Compute de URL&#39;s van de worker wijzigt en de nieuwe versie weerspiegelt, moet de URL worden bijgewerkt in de verwerkingsprofielen.

## Workspace API-provisioning{#workspace-api-provisioning}

Toen [vestiging het Project van de Adobe Vuurwerk in Adobe I/O](../set-up/firefly.md) om lokale ontwikkeling te steunen, werd een nieuwe werkruimte van de Ontwikkeling gecreeerd en de Compute van __Activa, I/O Gebeurtenissen__ en __I/O de APIs__ van het Gebeurtenisbeheer werden toegevoegd aan het.

De API&#39;s __APIS voor Asset Compute, I/O Events__ en ____ I/O Events Management worden alleen expliciet toegevoegd aan de werkruimten die worden gebruikt voor lokale ontwikkeling. Voor werkruimten die (uitsluitend) met AEM als een Cloud Service-omgeving integreren, is het __niet__ nodig deze API&#39;s expliciet toe te voegen, aangezien de API&#39;s van nature beschikbaar worden gemaakt voor AEM als Cloud Service.
