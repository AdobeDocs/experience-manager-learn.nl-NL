---
title: Stel de arbeiders van de Asset compute aan Adobe I/O Runtime voor gebruik met AEM as a Cloud Service op
description: De projecten van de asset compute, en de werknemers die zij bevatten, moeten in Adobe I/O Runtime worden ingezet om door AEM as a Cloud Service te worden gebruikt.
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

De projecten van de asset compute, en de arbeiders die zij bevatten, moeten aan Adobe I/O Runtime via Adobe I/O CLI worden opgesteld om door AEM as a Cloud Service te worden gebruikt.

Bij de implementatie naar Adobe I/O Runtime voor gebruik door AEM as a Cloud Service auteur zijn slechts twee omgevingsvariabelen vereist:

+ `AIO_runtime_namespace` wijst de Werkruimte van de Bouwer van de App om op te stellen
+ `AIO_runtime_auth` zijn de verificatiegegevens van de App Builder-werkruimte

De andere standaardvariabelen die in het `.env` Het bestand wordt impliciet door AEM as a Cloud Service verschaft wanneer de Asset compute-worker wordt aangeroepen.

## Werkruimte Ontwikkeling

Omdat dit project is gegenereerd met `aio app init` met de `Development` werkruimte, `AIO_runtime_namespace` wordt automatisch ingesteld op `81368-wkndaemassetcompute-development` met de overeenkomst `AIO_runtime_auth` in onze lokale `.env` bestand.  Als een `.env` Het bestand bestaat in de map die wordt gebruikt om de implementatieopdracht uit te voeren, en de waarden ervan worden gebruikt, tenzij deze worden vervangen door een variabele export op besturingssysteemniveau. Zo ziet u [stadium en productie](#stage-and-production) de werkruimten zijn bedoeld.

![Implementatie van AIR-apps met behulp van .env-variabelen](./assets/runtime/development__aio.png)

Om aan de werkruimte op te stellen die in de projecten wordt bepaald `.env` bestand:

1. Open de bevellijn in de wortel van het project van de Asset compute
1. De opdracht uitvoeren `aio app deploy`
1. De opdracht uitvoeren `aio app get-url` om de worker-URL op te halen die in het AEM as a Cloud Service verwerkingsprofiel kan worden gebruikt om naar deze aangepaste Asset compute-worker te verwijzen. Als het project meerdere workers bevat, worden afzonderlijke URL&#39;s voor elke worker weergegeven.

Als de lokale ontwikkeling en AEM as a Cloud Service Ontwikkelomgevingen afzonderlijke plaatsingen van de Asset compute gebruiken, kunnen de plaatsingen aan AEM as a Cloud Service Dev op de zelfde manier worden beheerd zoals [Implementatie van werkgebied en productie](#stage-and-production).

## Werkruimten voor werkruimten Werkgebied en Productie{#stage-and-production}

De werkruimten Werkgebied en Productie worden typisch opgesteld door uw systeem van CI/CD van keus. Het project van de Asset compute moet aan elke Werkruimte (Stadium en toen Productie) afzonderlijk worden opgesteld.

Als u werkelijke omgevingsvariabelen instelt, worden de waarden voor dezelfde variabelen overschreven in `.env`.

![Implementatie van een AIR-toepassing met behulp van exportvariabelen](./assets/runtime/stage__export-and-aio.png)

De algemene aanpak, die doorgaans door een CI/CD-systeem wordt geautomatiseerd, voor de implementatie in werkgebied- en productieomgevingen is:

1. Zorg ervoor dat [Adobe I/O CLI npm module en Asset compute plug-in](../set-up/development-environment.md#aio) zijn geïnstalleerd
1. Controle uit het project van de Asset compute om van Git op te stellen
1. De omgevingsvariabelen instellen met de waarden die overeenkomen met de doelwerkruimte (werkgebied of productie)
   + De twee vereiste variabelen zijn `AIO_runtime_namespace` en `AIO_runtime_auth` en worden per werkruimte in de Adobe I/O Developer Console verkregen via de Workspace __Alles downloaden__ gebruiken.

![Adobe Developer Console - AIO Runtime Namespace en Auth](./assets/runtime/stage-auth-namespace.png)

De waarden van deze toetsen kunnen worden ingesteld door exportopdrachten uit te voeren via de opdrachtregel:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Als uw medewerkers van de Asset compute andere variabelen nodig hebben, zoals cloudopslag, moeten deze ook als omgevingsvariabelen worden geëxporteerd.

1. Zodra alle milieuvariabelen voor de doelwerkruimte worden geplaatst om op te stellen, voer het opstellen bevel uit:
   + `aio app deploy`
1. De URL(s) van de worker waarnaar wordt verwezen door het AEM as a Cloud Service verwerkingsprofiel is ook beschikbaar via:
   + `aio app get-url`.

Als de projectversie van de Asset compute verandert, veranderen de worker-URL&#39;s ook om de nieuwe versie weer te geven en moet de URL worden bijgewerkt in de verwerkingsprofielen.

## Workspace API-provisioning{#workspace-api-provisioning}

Wanneer [vestiging App Builder project in Adobe I/O](../set-up/app-builder.md) ter ondersteuning van lokale ontwikkeling is een nieuwe werkruimte voor ontwikkeling gecreëerd en __Asset compute, I/O-gebeurtenissen__ en __API&#39;s voor I/O Events Management__ zijn toegevoegd.

De __Asset compute, I/O-gebeurtenissen__ en __API&#39;s voor I/O Events Management__ APIS wordt alleen expliciet toegevoegd aan de werkruimten die worden gebruikt voor lokale ontwikkeling. Werkruimten die (uitsluitend) integreren met AEM as a Cloud Service omgevingen doen __niet__ Deze API&#39;s moeten expliciet worden toegevoegd omdat de API&#39;s van nature beschikbaar worden gemaakt voor AEM as a Cloud Service.
