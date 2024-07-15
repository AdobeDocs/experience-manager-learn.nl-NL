---
title: Rekeningen en diensten voor uitbreidbaarheid van de Asset compute instellen
description: Ontwikkelaars van Asset computen hebben toegang nodig tot accounts en services, waaronder AEM as a Cloud Service, App Builder en cloudopslag die door Microsoft of Amazon worden geleverd.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
duration: 212
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 1%

---

# Accounts en services instellen

Deze zelfstudie vereist dat de volgende services worden geleverd en toegankelijk zijn via de Adobe ID van de leerling.

Alle Adobe-services moeten toegankelijk zijn via dezelfde Adobe, met dezelfde Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [ App Builder ](#app-builder)
   + De levering kan tussen 2 - 10 dagen vergen
+ Cloud-opslag
   + [ Azure Blob Storage ](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + of [ Amazon S3 ](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Zorg ervoor dat u toegang hebt tot alle bovenstaande services voordat u doorgaat met deze zelfstudie.
> 
> Lees hieronder secties over het instellen en leveren van de vereiste services.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

Toegang tot een AEM as a Cloud Service-omgeving is vereist om AEM Assets-verwerkingsprofielen te configureren voor het aanroepen van de aangepaste Asset compute-worker.

In het ideale geval is een sandboxprogramma of een ontwikkelomgeving zonder sandbox beschikbaar voor gebruik.

Merk op dat een lokale AEM SDK ontoereikend is om deze zelfstudie te voltooien, aangezien de lokale AEM SDK niet kan communiceren met Asset compute microservices, is in plaats daarvan een echte AEM as a Cloud Service-omgeving vereist.

## App Builder{#app-builder}

Het [ App Builder ](https://developer.adobe.com/app-builder/) kader wordt gebruikt voor de bouw van en het opstellen van douaneacties aan Adobe I/O Runtime, Adobe serverless platform. AEM Asset compute projecten zijn speciaal gebouwde App Builder-projecten die met AEM Assets integreren via Process Profiles, en die de mogelijkheid bieden om toegang te krijgen tot en gebruik te maken van asset binaries.

Meld u aan voor de voorvertoning om toegang te krijgen tot App Builder.

1. [ Teken omhoog voor de proef van App Builder ](https://developer.adobe.com/app-builder/trial/).
1. Wacht ongeveer 2 - 10 dagen tot u via e-mail op de hoogte wordt gebracht dat u provisioned bent alvorens met het leerprogramma verder te gaan.
   + Als u onzeker bent als u provisioned bent geweest, ga met de volgende stappen verder en als u niet a __App Builder__ project in [ Adobe Developer Console ](https://developer.adobe.com/console/) kunt tot stand brengen u nog niet provisioned bent.

## Cloud-opslag

Opslag in de cloud is vereist voor lokale ontwikkeling van Asset compute-projecten.

Wanneer medewerkers van de Asset compute naar de Adobe I/O Runtime worden geïmplementeerd voor rechtstreeks gebruik door AEM as a Cloud Service, is deze cloudopslag niet strikt vereist, aangezien AEM de cloudopslag biedt van waaruit het middel wordt gelezen en waarnaar wordt geschreven.

### Microsoft Azure Blob-opslag{#azure-blob-storage}

Als u nog geen toegang tot de Opslag van Microsoft Azure Blob hebt, onderteken omhoog voor a [ vrije 12 maandrekening ](https://azure.microsoft.com/en-us/free/).

Dit leerprogramma zal de opslag van Azure Blob gebruiken, nochtans [ Amazon S3 ](#amazon-s3) kan evenals slechts minder belangrijke variatie aan het leerprogramma worden gebruikt.

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_klik-door van levering Azure BlobOpslag (Geen audio)_

1. Login aan uw [ Microsoft Azure rekening ](https://azure.microsoft.com/en-us/account/).
1. Navigeer aan de __Rekeningen van de Opslag__ Azure de dienstsectie
1. Tik op __+ Toevoegen__ om een nieuwe Blob Storage-account te maken
1. Creeer een nieuwe __groep van het Middel__ zoals nodig, bijvoorbeeld: `aem-as-a-cloud-service`
1. Verstrek de naam van de a __rekening van de Opslag__, bijvoorbeeld: `aemguideswkndassetcomput`
   + De __naam van de de rekeningsrekening van de Opslag__ die voor [ wordt gebruikt vormend wolkenopslag ](../develop/environment-variables.md) in het lokale Hulpmiddel van de Ontwikkeling van de Asset compute
   + De __sleutels van de Toegang__ verbonden aan de opslagrekening worden ook vereist wanneer [ vormend wolkenopslag ](../develop/environment-variables.md).
1. Laat alles anders als gebrek, en tik __Overzicht + creeer__ knoop
   + Naar keuze, selecteer de __plaats__ dicht bij u.
1. Herzie het inrichtingsverzoek voor correctheid, en tik __creeer__ knoop indien tevreden

### Amazon S3{#amazon-s3}

Het gebruiken van [ Microsoft Azure BlobOpslag ](#azure-blob-storage) wordt geadviseerd voor de voltooiing van dit leerprogramma, nochtans [ Amazon S3 ](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) kan ook worden gebruikt.

Als het gebruiken van de opslag van Amazon S3, specificeer de geloofsbrieven van de de wolkenopslag van Amazon S3 wanneer [ het vormen van de het omgevingsvariabelen van het project ](../develop/environment-variables.md#amazon-s3).

Als u wolkenopslag speciaal voor dit leerprogramma moet verstrekken, adviseren wij gebruikend [ Azure BlobOpslag ](#azure-blob-storage).
