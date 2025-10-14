---
title: Accounts en services voor Asset Compute-uitbreidbaarheid instellen
description: Voor het ontwikkelen van Asset Compute-workers hebt u toegang nodig tot accounts en services, waaronder AEM as a Cloud Service, App Builder en cloudopslag die door Microsoft of Amazon worden geleverd.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
duration: 212
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 1%

---

# Accounts en services instellen

Deze zelfstudie vereist dat de volgende services worden geleverd en toegankelijk zijn via de Adobe ID van de leerling.

Alle Adobe-services moeten toegankelijk zijn via dezelfde Adobe Org, die uw Adobe ID gebruikt.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [&#x200B; App Builder &#x200B;](#app-builder)
   + De levering kan tussen 2 - 10 dagen vergen
+ Cloud-opslag
   + [&#x200B; Azure Blob Storage &#x200B;](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + of [&#x200B; Amazon S3 &#x200B;](https://aws.amazon.com/s3/?did=ft_card&trk=ft_card)

>[!WARNING]
>
>Zorg ervoor dat u toegang hebt tot alle bovenstaande services voordat u doorgaat met deze zelfstudie.
> 
> Lees hieronder secties over het instellen en leveren van de vereiste services.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

Toegang tot een AEM as a Cloud Service-omgeving is vereist om AEM Assets-verwerkingsprofielen te configureren voor het aanroepen van de aangepaste Asset Compute-worker.

In het ideale geval is een sandboxprogramma of een ontwikkelomgeving zonder sandbox beschikbaar voor gebruik.

Een lokale AEM SDK is onvoldoende om deze zelfstudie te voltooien, omdat de lokale AEM SDK niet kan communiceren met Asset Compute-microservices, maar een echte AEM as a Cloud Service-omgeving is vereist.

## App Builder{#app-builder}

Het [&#x200B; App Builder &#x200B;](https://developer.adobe.com/app-builder/) kader wordt gebruikt voor de bouw van en het opstellen van douaneacties aan Adobe I/O Runtime, Adobe serverless platform. AEM Asset Compute-projecten zijn speciaal gebouwde App Builder-projecten die met AEM Assets integreren via Process Profiles, en die toegang bieden tot en verwerking van asset binaries.

Meld u aan voor de voorvertoning om toegang te krijgen tot App Builder.

1. [&#x200B; Teken omhoog voor de proef van App Builder &#x200B;](https://developer.adobe.com/app-builder/trial/).
1. Wacht ongeveer 2 - 10 dagen tot u via e-mail op de hoogte wordt gebracht dat u provisioned bent alvorens met het leerprogramma verder te gaan.
   + Als u onzeker bent als u provisioned bent geweest, ga met de volgende stappen verder en als u niet a __App Builder__ project in [&#x200B; Adobe Developer Console &#x200B;](https://developer.adobe.com/console/) kunt tot stand brengen u nog niet provisioned bent.

## Cloud-opslag

Opslag in de cloud is vereist voor lokale ontwikkeling van Asset Compute-projecten.

Wanneer Asset Compute-workers naar de Adobe I/O Runtime worden geïmplementeerd voor rechtstreeks gebruik door AEM as a Cloud Service, is deze cloudopslag niet strikt vereist, aangezien AEM de cloudopslag biedt van waaruit het middel wordt gelezen en waarnaar wordt geschreven.

### Microsoft Azure Blob-opslag{#azure-blob-storage}

Als u nog geen toegang tot de Opslag van Microsoft Azure Blob hebt, onderteken omhoog voor a [&#x200B; vrije 12 maandrekening &#x200B;](https://azure.microsoft.com/en-us/free/).

Dit leerprogramma zal de opslag van Azure Blob gebruiken, nochtans [&#x200B; Amazon S3 &#x200B;](#amazon-s3) kan evenals slechts minder belangrijke variatie aan het leerprogramma worden gebruikt.

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_klik-door van levering Azure BlobOpslag (Geen audio)_

1. Login aan uw [&#x200B; Microsoft Azure rekening &#x200B;](https://azure.microsoft.com/en-us/account/).
1. Navigeer aan de __Rekeningen van de Opslag__ Azure de dienstsectie
1. Tik op __+ Toevoegen__ om een nieuwe Blob Storage-account te maken
1. Creeer een nieuwe __groep van het Middel__ zoals nodig, bijvoorbeeld: `aem-as-a-cloud-service`
1. Verstrek de naam van de a __rekening van de Opslag__, bijvoorbeeld: `aemguideswkndassetcomput`
   + De __naam van de de rekeningsrekening van de Opslag__ die voor [&#x200B; wordt gebruikt vormend wolkenopslag &#x200B;](../develop/environment-variables.md) in het lokale Hulpmiddel van de Ontwikkeling van Asset Compute
   + De __sleutels van de Toegang__ verbonden aan de opslagrekening worden ook vereist wanneer [&#x200B; vormend wolkenopslag &#x200B;](../develop/environment-variables.md).
1. Laat alles anders als gebrek, en tik __Overzicht + creeer__ knoop
   + Naar keuze, selecteer de __plaats__ dicht bij u.
1. Herzie het inrichtingsverzoek voor correctheid, en tik __creeer__ knoop indien tevreden

### Amazon S3{#amazon-s3}

Het gebruiken van [&#x200B; Microsoft Azure BlobOpslag &#x200B;](#azure-blob-storage) wordt geadviseerd voor de voltooiing van dit leerprogramma, nochtans [&#x200B; Amazon S3 &#x200B;](https://aws.amazon.com/s3/?did=ft_card&trk=ft_card) kan ook worden gebruikt.

Als het gebruiken van de opslag van Amazon S3, specificeer de geloofsbrieven van de de wolkenopslag van Amazon S3 wanneer [&#x200B; het vormen van de het omgevingsvariabelen van het project &#x200B;](../develop/environment-variables.md#amazon-s3).

Als u wolkenopslag speciaal voor dit leerprogramma moet verstrekken, adviseren wij gebruikend [&#x200B; Azure BlobOpslag &#x200B;](#azure-blob-storage).
