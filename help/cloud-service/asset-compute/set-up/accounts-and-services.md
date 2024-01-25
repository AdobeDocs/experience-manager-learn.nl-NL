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
duration: 228
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 1%

---

# Accounts en services instellen

Deze zelfstudie vereist dat de volgende services worden geleverd en toegankelijk zijn via de Adobe ID van de leerling.

Alle Adobe-services moeten toegankelijk zijn via dezelfde Adobe, met dezelfde Adobe ID.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [App Builder](#app-builder)
   + De levering kan tussen 2 - 10 dagen vergen
+ Cloud-opslag
   + [Azure Blob Storage](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + of [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>Zorg ervoor dat u toegang hebt tot alle bovenstaande services voordat u doorgaat met deze zelfstudie.
> 
> Lees hieronder secties over het instellen en leveren van de vereiste services.

## AEM as a Cloud Service{#aem-as-a-cloud-service}

Toegang tot een AEM as a Cloud Service omgeving is vereist om AEM Assets Processing Profiles te configureren om de aangepaste Asset compute-worker aan te roepen.

In het ideale geval is een sandboxprogramma of een ontwikkelomgeving zonder sandbox beschikbaar voor gebruik.

Merk op dat een lokale AEM SDK ontoereikend is om deze zelfstudie te voltooien, aangezien de lokale AEM SDK niet kan communiceren met Asset compute microservices, in plaats daarvan is een echte AEM as a Cloud Service omgeving vereist.

## App Builder{#app-builder}

De [App Builder](https://developer.adobe.com/app-builder/) -framework wordt gebruikt voor het maken en implementeren van aangepaste acties voor Adobe I/O Runtime, het serverloze platform van Adobe. AEM projecten van de Asset compute zijn speciaal gebouwde projecten van de Bouwer van de App die met AEM Assets via de Profielen van de Verwerking integreren, en de capaciteit verstrekken om tot activa binaire getallen toegang te hebben en te verwerken.

Meld u aan voor de voorvertoning om toegang te krijgen tot App Builder.

1. [Aanmelden voor proefversie van App Builder](https://developer.adobe.com/app-builder/trial/).
1. Wacht ongeveer 2 - 10 dagen tot u via e-mail op de hoogte wordt gebracht dat u provisioned bent alvorens met het leerprogramma verder te gaan.
   + Als u niet zeker weet of u provisioned bent, ga met de volgende stappen verder en als u niet kunt tot stand brengen __App Builder__ project in [Adobe Developer Console](https://developer.adobe.com/console/) u bent nog niet provisioned.

## Cloud-opslag

Opslag in de cloud is vereist voor lokale ontwikkeling van Asset compute-projecten.

Wanneer medewerkers van de Asset compute naar de Adobe I/O Runtime worden geïmplementeerd voor rechtstreeks gebruik door AEM as a Cloud Service, is deze cloudopslag niet strikt vereist, omdat AEM de cloudopslag biedt van waaruit het middel wordt gelezen en waarnaar wordt geschreven.

### Microsoft Azure Blob-opslag{#azure-blob-storage}

Als u nog geen toegang hebt tot Microsoft Azure Blob Storage, meldt u zich aan voor een [gratis account voor 12 maanden](https://azure.microsoft.com/en-us/free/).

Deze zelfstudie maakt echter gebruik van Azure Blob Storage [Amazon S3](#amazon-s3) U kunt alleen kleine wijzigingen in de zelfstudie gebruiken.

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_Doorklikken van provisioning Azure Blob Storage (Geen audio)_

1. Aanmelden bij uw [Microsoft Azure-account](https://azure.microsoft.com/en-us/account/).
1. Ga naar de __Opslagaccounts__ Azure Services-sectie
1. Tikken __+ Toevoegen__ om een nieuwe Blob Storage-account te maken
1. Een nieuwe __Brongroep__ indien nodig, bijvoorbeeld: `aem-as-a-cloud-service`
1. Geef een __Naam opslagaccount__, bijvoorbeeld: `aemguideswkndassetcomput`
   + De __Naam opslagaccount__  gebruikt voor [configureren van cloudopslag](../develop/environment-variables.md) in het lokale ontwikkelingsinstrument voor Asset compute
   + De __Toegangstoetsen__ gekoppeld aan de opslagaccount ook vereist wanneer [configureren van cloudopslag](../develop/environment-variables.md).
1. Laat alle andere opties standaard staan en tik op de knop __Revisie + maken__ knop
   + Selecteer desgewenst de optie __locatie__ dicht bij u.
1. Controleer de inrichtingsaanvraag voor de juistheid en tik op __Maken__ knop indien verzadigd

### Amazon S3{#amazon-s3}

Gebruiken [Microsoft Azure Blob-opslag](#azure-blob-storage) wordt aanbevolen voor het voltooien van deze zelfstudie, maar [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) kan ook worden gebruikt.

Als u Amazon S3-opslag gebruikt, geeft u de gegevens voor de Amazon S3-cloudopslag op wanneer [configureren van de omgevingsvariabelen van het project](../develop/environment-variables.md#amazon-s3).

Als u cloudopslag speciaal voor deze zelfstudie nodig hebt, raden we u aan [Azure Blob Storage](#azure-blob-storage).
