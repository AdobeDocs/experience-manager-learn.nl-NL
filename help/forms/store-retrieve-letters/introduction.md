---
title: Kopteksten opslaan en hervatten
description: Leer hoe u concepten van letters kunt opslaan en ophalen
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: e032070b-7332-4c2f-97ee-7e887a61aa7a
last-substantial-update: 2022-01-07T00:00:00Z
duration: 176
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 0%

---

# Inleiding

Met interactieve communicatie kunnen de agenten ad-hoccorrespondentie voorbereiden om gedeeltelijk voltooide correspondentie op te slaan en deze op te halen om te blijven werken. AEM Forms biedt u de [Service Provider Interface](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html). Van de klant wordt verwacht dat hij deze interface implementeert om de functionaliteit Opslaan en Hervatten te verkrijgen.

Dit artikel gebruikt MySQL-database om de metagegevens van de letter op te slaan. De lettergegevens worden opgeslagen in het bestandssysteem.

In de volgende video ziet u hoe u het hoofdlettergebruik gebruikt:

>[!VIDEO](https://video.tv.adobe.com/v/342129?quality=12&learn=on)

## Voorwaarden

U zult het volgende nodig hebben om de oplossing uit te voeren om aan uw behoeften te voldoen

* Werkervaring met AEM Forms
* AEM Server 6.5 met Forms Add op
* Moet vertrouwd zijn met de bouw van OSGI-pakketten
