---
title: Meerdere PDF-documenten weergeven
description: Meerdere PDF-documenten in een adaptieve vorm doorlopen.
version: 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
kt: 10292
exl-id: c1d248c3-8208-476e-b0ae-cab25575cd6a
last-substantial-update: 2021-10-12T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Meerdere PDF-documenten weergeven in een carrousel

Een veelvoorkomend geval is dat meerdere PDF-documenten vóór verzending aan de gebruiker worden getoond om te worden gecontroleerd.

Voor dit gebruiksgeval hebben we de [Adobe PDF Embed-API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[Hier kunt u een live demo van dit voorbeeld bekijken.](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

De volgende stappen zijn uitgevoerd om de integratie te voltooien

## Een aangepaste component maken om meerdere PDF-documenten weer te geven

Er is een aangepaste component (pdf-carrousel) gemaakt om door PDF-documenten te bladeren

## Clientbibliotheek

Er is een clientbibliotheek gemaakt om de PDF weer te geven met de Adobe PDF Embed-API. De PDF die moeten worden weergegeven, worden opgegeven in de pdf-carrouselcomponenten.

## Adaptief formulier maken

Een adaptief formulier maken op basis van sommige tabbladen (Dit voorbeeld heeft 3 tabbladen) Voeg enkele adaptieve formuliercomponenten toe op de eerste twee tabbladen Voeg de pdf-carrouselcomponent toe op het derde tabblad Configureer de pdf-carrouselcomponent zoals in de onderstaande schermafbeelding wordt getoond
![pdf-carousel](assets/pdf-carousel-af-component.png)

**API-sleutel voor PDF insluiten** - Dit is de sleutel die u kunt gebruiken om pdf in te sluiten. Deze sleutel werkt alleen met localhost. U kunt [uw eigen sleutel](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) en deze aan een ander domein koppelen.

**PDF-documenten opgeven** - Hier kunt u de PDF-documenten opgeven die u in de carrousel wilt weergeven.


## Het voorbeeld op de server implementeren

Voer de volgende stappen uit om dit op uw lokale server te testen:

1. [De clientbibliotheek importeren](assets/pdf-carousel-client-lib.zip) in uw lokale AEM [pakketbeheer gebruiken](http://localhost:4502/crx/packmgr/index.jsp)
1. [De PDF-carrouselcomponent importeren](assets/pdf-carousel-component.zip) in uw lokale AEM [pakketbeheer gebruiken](http://localhost:4502/crx/packmgr/index.jsp)
1. [Het adaptieve formulier importeren ](assets/adaptive-form-pdf-carousel.zip) in uw lokale AEM [pakketbeheer gebruiken](http://localhost:4502/crx/packmgr/index.jsp)
1. [De voorbeeld-PDF&#39;s importeren om weer te geven](assets/pdf-carousel-sample-documents.zip) in uw lokale AEM [de koppeling voor het uploaden van het bestand met middelen gebruiken](http://localhost:4502/assets.html/content/dam)
1. [Voorbeeld van adaptief formulier](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. Tab naar het tabblad Documenten dat moet worden gecontroleerd. In de carrouselcomponent moeten drie PDF-documenten worden weergegeven.
