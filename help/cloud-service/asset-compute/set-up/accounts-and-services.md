---
title: Rekeningen en diensten voor uitbreidbaarheid van de Asset compute instellen
description: Voor het ontwikkelen van Asset compute-workers hebt u toegang tot accounts en services nodig, waaronder AEM als Cloud Service, Adobe Project Firefly en cloudopslag die door Microsoft of Amazon wordt geleverd.
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '635'
ht-degree: 2%

---


# Accounts en services instellen

Deze zelfstudie vereist dat de volgende services worden geleverd en toegankelijk zijn via de Adobe ID van de leerling.

Alle services van de Adobe moeten toegankelijk zijn via dezelfde Adobe Org, die uw Adobe ID gebruikt.

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [Adobe Project FireFly](#adobe-project-firefly)
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

Toegang tot een AEM als een Cloud Service-omgeving is vereist om AEM Assets Processing Profiles te configureren om de aangepaste Asset compute-worker aan te roepen.

In het ideale geval is een sandboxprogramma of een ontwikkelomgeving zonder sandbox beschikbaar voor gebruik.

Merk op dat een lokale AEM SDK ontoereikend is om deze zelfstudie te voltooien, aangezien de lokale AEM SDK niet met de microservices van de Asset compute kan communiceren, in plaats daarvan een waar AEM als Cloud Service-omgeving is vereist.

## Adobe Project firefly{#adobe-project-firefly}

Het [Adobe Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html)-framework wordt gebruikt voor het bouwen en implementeren van aangepaste acties op Adobe I/O Runtime, een Adobe zonder server. AEM projecten van de Asset compute zijn speciaal gebouwde Vuurwerk projecten die met AEM Assets via het Profielen van de Verwerking integreren, en de capaciteit verstrekken om activa tot binaire getallen toegang te hebben en te verwerken.

Meld u aan voor de voorvertoning om probleemloos toegang te krijgen tot het project.

1. [Meld u aan voor een probleemloze voorvertoning](https://adobeio.typeform.com/to/obqgRm) van het project.
1. Wacht ongeveer 2 - 10 dagen tot u via e-mail op de hoogte wordt gebracht dat u provisioned bent alvorens met het leerprogramma verder te gaan.
   + Als u niet zeker bent of u provisioned bent, ga met de volgende stappen verder en als u niet kunt om een __Project Firefly__ project in [Adobe Console van de Ontwikkelaar ](https://console.adobe.io) te creëren u nog niet provisioned bent.

## Cloud-opslag

Opslag in de cloud is vereist voor lokale ontwikkeling van Asset compute-projecten.

Wanneer medewerkers van de Asset compute naar de Adobe I/O Runtime worden geïmplementeerd voor rechtstreeks gebruik door AEM als Cloud Service, is deze cloudopslag niet strikt vereist, omdat AEM de cloudopslag biedt van waaruit het middel wordt gelezen en waarnaar wordt geschreven.

### Microsoft Azure Blob Storage{#azure-blob-storage}

Als u nog geen toegang hebt tot Microsoft Azure Blob Storage, meldt u zich aan voor een [gratis account van 12 maanden](https://azure.microsoft.com/en-us/free/).

Deze zelfstudie maakt gebruik van Azure Blob Storage, maar [Amazon S3](#amazon-s3) kan ook worden gebruikt als slechts een kleine wijziging van de zelfstudie.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)

_Doorklikken van provisioning Azure Blob Storage (Geen audio)_


1. Meld u aan bij uw [Microsoft Azure-account](https://azure.microsoft.com/en-us/account/).
1. Navigeer naar de sectie __Opslagaccounts__ Azure-services
1. Tik __+ Add__ om een nieuwe Blob Storage-account te maken
1. Maak zo nodig een nieuwe __Brongroep__, bijvoorbeeld: `aem-as-a-cloud-service`
1. Geef een __Naam opslagaccount__ op, bijvoorbeeld: `aemguideswkndassetcomput`
   + De __Naam van opslagaccount__ wordt gebruikt voor [het configureren van cloudopslag](../develop/environment-variables.md) voor het ontwikkelingsprogramma voor lokale Asset compute
   + De __Toegangstoetsen__ verbonden aan de opslagrekening worden ook vereist wanneer [het vormen van wolkenopslag](../develop/environment-variables.md).
1. Laat alle andere opties standaard staan en tik op de knop __Revisie + create__
   + Selecteer desgewenst de __locatie__ dicht bij u.
1. Controleer het inrichtingsverzoek voor juistheid, en tik __Create__ knoop indien tevreden

### Amazon S3{#amazon-s3}

Het gebruik van [Microsoft Azure Blob Storage](#azure-blob-storage) wordt aanbevolen voor het voltooien van deze zelfstudie, maar [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) kan ook worden gebruikt.

Als u Amazon S3-opslag gebruikt, geeft u de gegevens voor de Amazon S3-cloudopslag op wanneer u de omgevingsvariabelen van het project [configureert.](../develop/environment-variables.md#amazon-s3)

Als u cloudopslag speciaal voor deze zelfstudie nodig hebt, raden we u aan om [Azure Blob Storage](#azure-blob-storage) te gebruiken.
