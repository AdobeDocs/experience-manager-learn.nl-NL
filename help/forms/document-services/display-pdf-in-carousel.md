---
title: Meerdere PDF-documenten weergeven
description: Meerdere PDF-documenten in een adaptieve vorm doorlopen.
version: 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
jira: KT-10292
exl-id: c1d248c3-8208-476e-b0ae-cab25575cd6a
last-substantial-update: 2021-10-12T00:00:00Z
duration: 66
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 0%

---

# Meerdere PDF-documenten weergeven in een carrousel

Een veelvoorkomend geval is dat meerdere PDF-documenten vóór verzending aan de gebruiker worden getoond om te worden gecontroleerd.

Om dit gebruiksgeval te verwezenlijken hebben wij [ Adobe PDF ingebed API ](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) gebruikt.

[ A levende demo van deze steekproef kan hier worden ervaren.](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

De volgende stappen zijn uitgevoerd om de integratie te voltooien

## Een aangepaste component maken om meerdere PDF-documenten weer te geven

Er is een aangepaste component (pdf-carrousel) gemaakt om door PDF-documenten te bladeren

## Clientbibliotheek

Er is een clientbibliotheek gemaakt om de PDF weer te geven met de Adobe PDF Embed-API. De PDF die moeten worden weergegeven, worden opgegeven in de pdf-carrouselcomponenten.

## Adaptief formulier maken

Een adaptief formulier maken op basis van sommige tabbladen (dit voorbeeld heeft drie tabbladen)
Enkele adaptieve formuliercomponenten toevoegen op de eerste twee tabbladen
De PDF-carrouselcomponent toevoegen op het derde tabblad
De component pdf-carrousel configureren, zoals in de onderstaande schermafbeelding wordt getoond
![ pdf-carousel ](assets/pdf-carousel-af-component.png)

**bed PDF API Sleutel** in - dit is de sleutel die u kunt gebruiken om pdf in te bedden. Deze sleutel werkt alleen met localhost. U kunt [ uw eigen sleutel ](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) tot stand brengen en het associëren met ander domein.

**specificeer de Documenten van de PDF** - hier kunt u de pdf- documenten specificeren die u in de carrousel wilt worden getoond.


## Het voorbeeld op de server implementeren

Voer de volgende stappen uit om dit op uw lokale server te testen:

1. [ voer de cliëntbibliotheek ](assets/pdf-carousel-client-lib.zip) in uw lokale AEM instantie [ in gebruikend pakketmanager ](http://localhost:4502/crx/packmgr/index.jsp)
1. [ de Invoer van de pdf carrouselcomponent ](assets/pdf-carousel-component.zip) in uw lokale AEM instantie [ gebruikend de pakketmanager ](http://localhost:4502/crx/packmgr/index.jsp)
1. [ de Invoer de Aangepaste Vorm ](assets/adaptive-form-pdf-carousel.zip) in uw lokale AEM instantie [ gebruikend pakketmanager ](http://localhost:4502/crx/packmgr/index.jsp)
1. [ de Invoer van steekproef pdf&#39;s ](assets/pdf-carousel-sample-documents.zip) in uw lokale AEM instantie [ gebruikend de verbinding van het activadossier uploadt ](http://localhost:4502/assets.html/content/dam)
1. [ Voorproef Aangepaste Vorm ](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. Tab naar het tabblad Documenten dat moet worden gecontroleerd. In de carrouselcomponent moeten drie PDF-documenten worden weergegeven.
