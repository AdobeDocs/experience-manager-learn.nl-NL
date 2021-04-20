---
title: Aan de slag met AEM en Adobe Target
seo-title: Aan de slag met AEM en Adobe Target
description: Een end-to-end zelfstudie waarin wordt getoond hoe u persoonlijke ervaringen kunt creëren en leveren met Adobe Experience Manager en Adobe Target. In deze zelfstudie leert u ook over verschillende personen die betrokken zijn bij het proces van het begin tot het einde en hoe zij met elkaar samenwerken
seo-description: Een end-to-end zelfstudie waarin wordt getoond hoe u persoonlijke ervaringen kunt creëren en leveren met Adobe Experience Manager en Adobe Target. In deze zelfstudie leert u ook over verschillende personen die betrokken zijn bij het proces van het begin tot het einde en hoe zij met elkaar samenwerken
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '894'
ht-degree: 0%

---


# Aan de slag met AEM en Adobe Target {#getting-started-with-aem-target}

AEM en Target zijn beide krachtige oplossingen met schijnbaar overlappende mogelijkheden. Klanten hebben soms moeite om te begrijpen hoe en wanneer ze deze producten samen moeten gebruiken om persoonlijke ervaring te bieden. Om optimale ervaring voor elke eindgebruiker te bieden, zouden de verschillende teams binnen uw organisatie nauw moeten samenwerken en bepalen wie wat doet.

In deze zelfstudie behandelen wij drie verschillende scenario&#39;s voor AEM en Doel, die u helpen begrijpen wat het beste voor uw organisatie werkt en hoe de verschillende teams samenwerken.

* Scenario 1 : Personalisatie met AEM Experience Fragments
* Scenario 2: Personalisatie met behulp van Visual Experience Composer
* Scenario 3: Aanpassing van de volledige webpaginamogelijkheden

## Personalisatie met AEM ervaringsfragmenten {#personalization-using-aem-experience-fragment}

Voor dit scenario zullen wij AEM en Doel gebruiken. Het is duidelijk dat beide producten hun eigen sterke punten hebben en wanneer het gaat om het bieden van gepersonaliseerde ervaringen aan de gebruikers van uw site, hebt u **gepersonaliseerde inhoud (inhoud van AEM)** en een **intelligente manier (Doel)** nodig om deze inhoud te dienen op basis van een specifieke gebruiker.

AEM helpt u persoonlijke inhoud te maken, waarbij al uw inhoud en middelen op een centrale locatie worden samengebracht om uw strategie voor personalisatie te voeden. Met AEM kunt u eenvoudig inhoud voor desktops, tablets en mobiele apparaten op één locatie maken zonder dat u code hoeft te schrijven. Het is niet nodig om pagina&#39;s te maken voor elk apparaat. AEM past automatisch elke ervaring aan met uw inhoud. U kunt de inhoud ook van AEM naar Adobe Target exporteren als aanbiedingen met een druk op een knop.

We hebben nu persoonlijke inhoud in de vorm van voorstellen van AEM in Target. Met Doel kunt u deze aanbiedingen op schaal aanbieden op basis van een combinatie van op regels gebaseerde en door AI gestuurde methoden voor het leren van machines die gedrags-, context- en offlinevariabelen bevatten.  Met Doel kunt u eenvoudig A/B- en MVT-activiteiten (Multivariate) instellen en uitvoeren om de beste aanbiedingen, inhoud en ervaringen te bepalen.

**De** fragmenten van de ervaring vormen een enorme stap voorwaarts om de inhoud/ervaring creators aan de verpersoonlijkingsberoeps te verbinden die bedrijfsresultaten gebruikend Doel drijven.

* AEM auteurs van inhoudeditors gepersonaliseerde inhoud als Fragmenten van de Ervaring en zijn variaties
* AEM exporteert Experience Fragment HTML naar Doel &#x200B;
* Het doel &#x200B; gebruikt AEM de prijsverhoging van het Fragment van de Ervaring als Aanbiedingen in Activiteiten
* Doel levert Experience Fragment HTML, AEM levert afbeeldingen waarnaar wordt verwezen

   ![Personalisatie met behulp van het diagram Fragmenten voor ervaring](assets/personalization-use-case-1/use-case-1-diagram.png)

**Om dit scenario uit te voeren, moet u:**

* [AEM en Adobe Target integreren met Launch en Adobe I/O](./implementation.md#integrating-aem-target-options)
* [AEM en Adobe Target met behulp van verouderde Cloud Services](./implementation.md#integrating-aem-target-options)

***Na het uitvoeren van de bovengenoemde integraties, laat het  [scenario in detail](./personalization-use-case-1.md) onderzoeken.***

## Personalisatie met behulp van Visual Experience Composer

Marketers kunnen snel wijzigingen aanbrengen op hun website zonder code te wijzigen voor het uitvoeren van een test met Adobe Target Visual Experience Composer (VEC). VEC is WYSIWYG (wat u ziet is wat u krijgt) gebruikersinterface die u gemakkelijk gepersonaliseerde ervaringen en Aanbiedingen in de plaatsomgeving creeert en test. U kunt ervaringen en aanbiedingen voor doelactiviteiten maken door de lay-out en inhoud van een webpagina (of aanbieding) of mobiele webpagina te slepen en neer te zetten, te wisselen en te wijzigen.

VEC is een van de belangrijkste kenmerken van Adobe Target. Met VEC kunnen marketers en ontwerpers inhoud maken en wijzigen via een visuele interface. Veel ontwerpkeuzes kunnen worden gemaakt zonder dat de code rechtstreeks moet worden bewerkt. Het bewerken van HTML en JavaScript is ook mogelijk met behulp van de bewerkingsopties die beschikbaar zijn in de composer.

* Inhoud bevindt zich in AEM en inhoudeditors maken en beheren de sitepagina&#39;s
* Doel gebruikt AEM gehoste sitepagina&#39;s om tests en personalisatie uit te voeren
* Doel levert gepersonaliseerde inhoud
* Nieuwe inhoud maken met Adobe Target VEC
* Is van toepassing op zowel AEM gehoste sites als niet-AEM gehoste sites

   ![Personalisatie met het diagram Visual Experience Composer](assets/personalization-use-case-3/use-case-diagram-3.png)

**Om dit scenario uit te voeren, moet u:**

* [AEM en Adobe Target integreren met Launch en Adobe I/O](./implementation.md#integrating-aem-target-options)

***Na het uitvoeren van de bovengenoemde integratie, laat het  [scenario in detail onderzoeken.](./personalization-use-case-3.md)***

## Aanpassing van de volledige webpaginamogelijkheden

Door Adobe Experience Manager te integreren met Adobe Target kunt u uw sitegebruikers een persoonlijke ervaring bieden. Bovendien kunt u hiermee beter begrijpen welke versies van uw website-inhoud uw conversies het beste verbeteren tijdens een bepaalde testperiode. Met een A/B-test worden bijvoorbeeld twee of meer versies van uw website-inhoud vergeleken om te zien welke omzettingen, verkopen of andere maatstaven u het beste kunt vervangen. Een markeerteken kan activiteiten binnen Adobe Target maken om te begrijpen hoe gebruikers met de inhoud van uw site werken en hoe deze invloed heeft op de maatstaven van uw site.

* Inhoud bevindt zich in AEM en inhoudeditors maken en beheren de sitepagina&#39;s
* Doel gebruikt AEM gehoste sitepagina&#39;s om tests en personalisatie uit te voeren
* Doel levert gepersonaliseerde inhoud
* Er wordt hier geen nieuwe inhoud gemaakt
* Van toepassing op zowel AEM- als niet-AEM sites

   ![diagram](assets/personalization-use-case-2/use-case-2-diagram.png)

**Om dit scenario uit te voeren, moet u:**

* [AEM en Adobe Target integreren met Launch en Adobe I/O](./implementation.md#integrating-aem-target-options)

***Na het uitvoeren van de bovengenoemde integratie, laat het  [scenario in detail onderzoeken.](./personalization-use-case-2.md)***
