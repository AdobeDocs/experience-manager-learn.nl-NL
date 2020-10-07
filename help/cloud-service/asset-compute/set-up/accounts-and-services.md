---
title: Accounts en services instellen voor de rekbaarheid van Asset Compute
description: Voor het ontwikkelen van workers voor Asset Compute is toegang vereist tot accounts en services, waaronder AEM als Cloud Service, Adobe Project Firefly en cloudopslag van Microsoft of Amazon.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 1%

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

Toegang tot een AEM als een Cloud Service-omgeving is vereist om AEM Assets Processing Profiles te configureren voor het aanroepen van de aangepaste Asset Compute-worker.

In het ideale geval is een sandboxprogramma of een ontwikkelomgeving zonder sandbox beschikbaar voor gebruik.

Merk op dat een lokale AEM SDK ontoereikend is om deze zelfstudie te voltooien, aangezien de lokale AEM SDK niet kan communiceren met de microservices van Asset Compute, in plaats daarvan een waar AEM als Cloud Service-omgeving is vereist.

## Adobe Project Firefly{#adobe-project-firefly}

Het [Adobe Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly.html) -framework wordt gebruikt voor het bouwen en implementeren van aangepaste acties op Adobe I/O Runtime, een platform zonder Adobe. AEM Asset Compute-projecten zijn speciaal gebouwde Firefly-projecten die met AEM Assets integreren via Process Profiles, en die de mogelijkheid bieden om toegang te krijgen tot en gebruik te maken van binaire bestanden met middelen.

Meld u aan voor de voorvertoning om probleemloos toegang te krijgen tot het project.

1. [Meld u aan voor een probleemloze voorvertoning](https://adobeio.typeform.com/to/obqgRm)van het project.
1. Wacht ongeveer 2 - 10 dagen tot u via e-mail op de hoogte wordt gebracht dat u provisioned bent alvorens met het leerprogramma verder te gaan.
   + Als u niet zeker bent of u provisioned bent geweest, ga met de volgende stappen verder en als u niet een project van het __Project kunt tot stand brengen Firefly__ in de Console [van de Ontwikkelaar van](https://console.adobe.io) Adobe hebt u nog niet provisioned.

## Cloud-opslag

De opslag van de wolk wordt vereist voor lokale ontwikkeling van Activa Compute projecten.

Wanneer workers voor Asset Compute naar de Adobe I/O Runtime worden geïmplementeerd voor direct gebruik door AEM als Cloud Service, is deze cloudopslag niet strikt vereist, omdat AEM de cloudopslag biedt van waaruit het middel wordt gelezen en waarnaar wordt geschreven.

### Microsoft Azure Blob-opslag{#azure-blob-storage}

Als u nog geen toegang hebt tot Microsoft Azure Blob Storage, meldt u zich aan voor een [gratis account](https://azure.microsoft.com/en-us/free/)van 12 maanden.

Deze zelfstudie maakt gebruik van Azure Blob Storage, maar [Amazon S3](#amazon-s3) kan ook worden gebruikt voor kleine wijzigingen in de zelfstudie.

>[!VIDEO](https://video.tv.adobe.com/v/40377/?quality=12&learn=on)
_Doorklikken van provisioning Azure Blob Storage (Geen audio)_


1. Meld u aan bij uw [Microsoft Azure-account](https://azure.microsoft.com/en-us/account/).
1. Navigeer naar de sectie __Storage Accounts__ Azure Services
1. Tik __+ Toevoegen__ om een nieuwe Blob Storage-account te maken
1. Maak zo nodig een nieuwe __bronnengroep__ , bijvoorbeeld: `aem-as-a-cloud-service`
1. Geef bijvoorbeeld de naam __van een__ opslagaccount op: `aemguideswkndassetcomput`
   + De naam __van de__ opslagaccount wordt gebruikt voor het [configureren van cloudopslag](../develop/environment-variables.md) voor het lokale hulpprogramma voor het berekenen van bedrijfsmiddelen
   + De __toegangstoetsen__ die aan de opslagaccount zijn gekoppeld, zijn ook vereist bij het [configureren van cloudopslag](../develop/environment-variables.md).
1. Laat alle andere opties standaard staan en tik op de knop __Revisie + maken__
   + Selecteer desgewenst de __locatie__ die u dicht bij u ligt.
1. Controleer de inrichtingsaanvraag voor de juistheid en tik op de knop __Maken__ als u tevreden bent

### Amazon S3{#amazon-s3}

Het gebruik van [Microsoft Azure Blob Storage](#azure-blob-storage) wordt aanbevolen voor het voltooien van deze zelfstudie, maar [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) kan ook worden gebruikt.

Als u Amazon S3-opslag gebruikt, geeft u de gegevens voor de Amazon S3-cloudopslag op bij het [configureren van de omgevingsvariabelen](../develop/environment-variables.md#amazon-s3)van het project.

Als u cloudopslag speciaal voor deze zelfstudie nodig hebt, raden we u aan [Azure Blob Storage](#azure-blob-storage)te gebruiken.
