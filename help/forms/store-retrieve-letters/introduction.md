---
title: Kopteksten opslaan en hervatten
description: Leer hoe u concepten van letters kunt opslaan en ophalen
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: e032070b-7332-4c2f-97ee-7e887a61aa7a
last-substantial-update: 2022-01-07T00:00:00Z
duration: 160
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 0%

---

# Inleiding

Met interactieve communicatie kunnen de agenten ad-hoccorrespondentie voorbereiden om gedeeltelijk voltooide correspondentie op te slaan en deze op te halen om te blijven werken. AEM Forms verstrekt u de [&#x200B; Interface van de Dienstverlener &#x200B;](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html). Van de klant wordt verwacht dat hij deze interface implementeert om de functionaliteit Opslaan en Hervatten te verkrijgen.

Dit artikel gebruikt MySQL-database om de metagegevens van de letter op te slaan. De lettergegevens worden opgeslagen in het bestandssysteem.

In de volgende video ziet u hoe u het hoofdlettergebruik gebruikt:

>[!VIDEO](https://video.tv.adobe.com/v/3441446?quality=12&learn=on&captions=dut)

## Voorwaarden

U zult het volgende nodig hebben om de oplossing uit te voeren om aan uw behoeften te voldoen

* Werkervaring met AEM Forms
* AEM Server 6.5 met Forms Add op
* Moet vertrouwd zijn met de bouw van OSGI-pakketten
