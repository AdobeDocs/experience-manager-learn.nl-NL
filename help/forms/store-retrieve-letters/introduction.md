---
title: Kopteksten opslaan en hervatten
seo-title: Save and resume letters
description: Leer hoe u concepten van letters kunt opslaan en ophalen
seo-description: Learn how to save and retrieve draft letters
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# Inleiding

Met interactieve communicatie kunnen de agenten ad-hoccorrespondentie voorbereiden om gedeeltelijk voltooide correspondentie op te slaan en deze op te halen om te blijven werken. AEM Forms biedt u de [Service Provider Interface](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html). Van de klant wordt verwacht dat hij deze interface implementeert om de functionaliteit Opslaan en Hervatten te verkrijgen.

Dit artikel gebruikt MySQL-database om de metagegevens van de letter op te slaan. De lettergegevens worden opgeslagen in het bestandssysteem.

In de volgende video ziet u hoe u het hoofdlettergebruik gebruikt:

>[!VIDEO](https://video.tv.adobe.com/v/342129/quality=9)

## Voorwaarden

U zult het volgende nodig hebben om de oplossing uit te voeren om aan uw behoeften te voldoen

* Werkervaring met AEM Forms
* AEM Server 6.5 met Forms Add op
* Moet vertrouwd zijn met de bouw van OSGI-bundels