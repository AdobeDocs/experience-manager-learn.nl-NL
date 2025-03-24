---
title: Opties voor aangepaste domeinnamen
description: Leer hoe u aangepaste domeinnamen voor uw door AEM as a Cloud Service gehoste website beheert en implementeert.
version: Experience Manager as a Cloud Service
feature: Cloud Manager, Custom Domain Names
topic: Architecture, Migration
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 130
last-substantial-update: 2024-08-09T00:00:00Z
jira: KT-15946
thumbnail: KT-15946.jpeg
exl-id: e11ff38c-e823-4631-a5b0-976c2d11353e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 0%

---

# Opties voor aangepaste domeinnamen

Leer hoe u domeinnamen voor uw door AEM as a Cloud Service gehoste website beheert en implementeert.

>[!VIDEO](https://video.tv.adobe.com/v/3432632?quality=12&learn=on)

## Voordat u begint

Voordat u begint met het implementeren van aangepaste domeinnamen, moet u de volgende concepten begrijpen:

### Wat is een domeinnaam

Een domeinnaam is de websitenaam met een mensvriendelijke naam, bijvoorbeeld adobe.com, die verwijst naar een specifieke locatie (IP-adres zoals 170.2.14.16 ) op internet.

### Standaarddomeinnamen in AEM as a Cloud Service

AEM as a Cloud Service wordt standaard voorzien van een standaarddomeinnaam, die eindigt in `*.adobeaemcloud.com` . Het SSL-certificaat van de wilde kaart dat is uitgegeven tegen `*.adobeaemcloud.com` wordt automatisch toegepast op alle omgevingen en dit jokertekencertificaat is de verantwoordelijkheid van Adobe.

Standaarddomeinnamen hebben de `https://<SERVICE-TYPE>-p<PROGRAM-ID>-e<ENVIRONMENT-ID>.adobeaemcloud.com` -indeling.

- `<SERVICE-TYPE>` kon **auteur** zijn, **publiceren** of **voorproef**.
- `<PROGRAM-ID>` is de unieke id voor het programma. Een organisatie kan meerdere programma&#39;s hebben.
- `<ENVIRONMENT-ID>` is het unieke herkenningsteken voor het milieu en elk programma bevat vier milieu&#39;s: **Snelle Ontwikkeling (RDE)**, **dev**, **stadium**, en **prod**. Elk milieu bevat bovengenoemde drie de diensttypes, behalve **RDE** die geen voorproefmilieu heeft.

Samengevat, zodra alle milieu&#39;s van AEM as a Cloud Service provisioned zijn, hebt u **11** (RDE heeft geen voorproefmilieu) unieke URLs die met de standaarddomeinnaam wordt gecombineerd.

### Door Adobe beheerde CDN versus door klant beheerde CDN

AEM as a Cloud Service is geïntegreerd met een door Adobe beheerde Content Delivery Network (CDN) om de latentie te verminderen en de prestaties van de website te verbeteren. CDN met Adobe-beheer wordt automatisch ingeschakeld voor alle omgevingen. Zie [ AEM as a Cloud Service caching ](../caching/overview.md) voor meer details.

Nochtans, kunnen de klanten hun eigen CDN ook gebruiken, die als **wordt bedoeld klant-geleide CDN**. Het is niet noodzakelijk maar weinig klanten gebruiken het voor collectieve beleid of andere redenen. In dit geval is de klant verantwoordelijk voor het beheer van de CDN-configuraties en -instellingen.

### Aangepaste domeinnamen

Voor branding, authenticiteit en bedrijfsontwikkelingsdoeleinden wordt altijd de voorkeur gegeven aan aangepaste domeinnamen boven standaarddomeinnaam. Nochtans, kunnen zij slechts op **worden toegepast publiceren** en **voorproef** de diensttypes, en niet **auteur**.

Wanneer u aangepaste domeinnamen toevoegt, moet u een geldig SSL-certificaat opgeven voor het opgegeven aangepaste domein. Het SSL-certificaat moet een geldig certificaat zijn dat is ondertekend door een vertrouwde certificeringsinstantie (CA).

Typisch, gebruiken de klanten een naam van het douanedomein voor de milieu&#39;s van Prod (de Website van AEM as a Cloud Service) en soms voor lagere milieu&#39;s zoals **stadium** of **dev**.

| AEM-servicetype | Aangepast domein ondersteund? |
|---------------------|:-----------------------:|
| Auteur | ✘ |
| Voorvertoning | ✔ |
| Publiceren | ✔ |

## Domeinnamen implementeren

Om domeinnamen uit te voeren gebruikend Adobe-geleide CDN of klant-beheerde CDN de volgende stroomdiagramgidsen u door het proces:

![ Stroomschema van het Beheer van de Naam van het Domein ](./assets/domain-name-management-flowchart.png){width="800" zoomable="yes"}

In de volgende tabel vindt u ook een overzicht van de locaties waarop u de specifieke configuraties kunt beheren:

| Aangepaste domeinnaam met | SSL-certificaat toevoegen aan | Domeinnaam toevoegen aan | DNS-records configureren op | Hebt u CDN-regel voor HTTP-headervalidatie nodig? |
|---------------------|:-----------------------:|-----------------------:|-----------------------:|-----------------------:|
| CDN met Adobe-beheer | Adobe Cloud Manager | Adobe Cloud Manager | DNS-hostingservice | ✘ |
| Door de klant beheerde CDN | CDN-leverancier | CDN-leverancier | DNS-hostingservice | ✔ |

### Stapsgewijze zelfstudies

Nu u het beheerproces voor domeinnamen begrijpt, kunt u aangepaste domeinnamen voor uw AEM as a Cloud Service-website implementeren door de onderstaande zelfstudies te volgen:

**[Namen van het Domein van de Douane met Adobe-Beheerde CDN](./custom-domain-name-with-adobe-managed-cdn.md)**: In dit leerprogramma, leert u hoe te om een naam van het douanedomein aan een **website van AEM as a Cloud Service met Adobe-Beheerde CDN** toe te voegen.
**[Namen van het Domein van de Douane met klant-geleide CDN](./custom-domain-names-with-customer-managed-cdn.md)**: In dit leerprogramma, leert u hoe te om een naam van het douanedomein aan een **website van AEM as a Cloud Service met klant-beheerde CDN** toe te voegen.
