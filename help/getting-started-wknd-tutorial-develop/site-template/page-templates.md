---
title: Paginasjablonen
description: Leer hoe u paginasjablonen maakt en wijzigt. Begrijp de verhouding tussen een Malplaatje van de Pagina en een Pagina. Leer hoe u beleid van een paginasjabloon configureert voor korrelig beheer en consistentie van merken voor inhoud.  Een goed gestructureerde sjabloon voor artikel van het tijdschrift wordt gemaakt op basis van een model van Adobe XD.
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
source-git-commit: 04096fe3c99cdcce2d43b2b29899c2bbe37ac056
workflow-type: tm+mt
source-wordcount: '691'
ht-degree: 0%

---

# Paginasjablonen {#page-templates}

>[!CAUTION]
>
> Het gereedschap Snel site maken is momenteel een technische voorvertoning. Het wordt ter beschikking gesteld voor test- en evaluatiedoeleinden en is niet bestemd voor gebruik in de productie, tenzij overeengekomen met Adobe Support.

In dit hoofdstuk onderzoeken we de relatie tussen een paginasjabloon en een pagina. Wij zullen een niet-gestileerde sjabloon van het Artikel van het Tijdschrift opstellen die op sommige modellen van wordt gebaseerd [AdobeXD](https://www.adobe.com/products/xd.html). In het proces om het malplaatje uit te bouwen, zijn de Componenten van de Kern en de geavanceerde beleidsconfiguraties behandeld.

## Vereisten {#prerequisites}

Dit is een meerdelige zelfstudie en er wordt aangenomen dat de stappen die worden beschreven in het dialoogvenster [Inhoud ontwerpen en wijzigingen publiceren](./author-content-publish.md) is voltooid.

## Doelstelling

1. Begrijp de details van de Malplaatjes van de Pagina en hoe het beleid kan worden gebruikt om korrelige controle van paginainhoud af te dwingen.
1. Leer hoe sjablonen en pagina&#39;s zijn gekoppeld.
1. Een nieuwe sjabloon maken en een pagina ontwerpen.

## Wat u gaat maken {#what-you-will-build}

In dit gedeelte van de zelfstudie maakt u een nieuwe sjabloon voor de artikelpagina van tijdschriften waarmee u nieuwe artikelen voor tijdschriften kunt maken en uitlijnt op een algemene structuur. De sjabloon is gebaseerd op ontwerpen en een UI-kit die in AdobeXD wordt gemaakt. Dit hoofdstuk is alleen gericht op het opbouwen van de structuur of het skelet van de sjabloon. Er worden geen stijlen geïmplementeerd, maar de sjabloon en pagina&#39;s zijn functioneel.

## De sjabloon voor artikelpagina voor tijdschriften maken

Wanneer u een pagina maakt, moet u een sjabloon selecteren die wordt gebruikt als basis voor het maken van de nieuwe pagina. De sjabloon definieert de structuur van de resulterende pagina, de initiële inhoud en de toegestane componenten.

Er zijn drie hoofdgebieden [Paginasjablonen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html):

1. **Structuur** - definieert componenten die deel uitmaken van de sjabloon. Deze kunnen niet worden bewerkt door auteurs van inhoud.
1. **Oorspronkelijke inhoud** - definieert componenten waarmee de sjabloon begint. Deze kunnen door makers van inhoud worden bewerkt en/of verwijderd.
1. **Beleid** - definieert configuraties op basis van het gedrag van componenten en de opties waarover auteurs beschikken.

Maak vervolgens een nieuwe sjabloon in AEM die overeenkomt met de structuur van de modellen. Dit zal in een lokale instantie van AEM voorkomen. Voer de stappen in de onderstaande video uit:

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

U kunt de volgende miniatuur gebruiken om uw sjabloon te identificeren (of uw eigen sjabloon te uploaden!)

![Miniatuur van artikelpaginasjabloon](./assets/page-templates/article-page-template-thumbnail.png)


### Oplossingspakket

Een voltooide bewerking [oplossing van het Tijdschriftsjabloon](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip) kan worden gedownload en geïnstalleerd via Package Manager.

## Koptekst en voettekst bijwerken met ervaringsfragmenten {#experience-fragments}

Bij het maken van algemene inhoud, zoals een kop- of voettekst, is het gebruikelijk om een [Ervaar fragment](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Met de functie Fragmenten van ervaring kunnen gebruikers meerdere componenten combineren om één component te maken die geschikt is voor referentie. De Fragmenten van de ervaring hebben het voordeel om multi-site beheer te steunen en [lokalisatie](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

De Sitesjabloon genereerde een kop- en voettekst. Werk vervolgens de Experience Fragments bij zodat deze overeenkomen met de modellen. Voer de stappen in de onderstaande video uit:

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

Stappen op hoog niveau voor de onderstaande video:

1. Download het pakket met voorbeeldinhoud **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**.
1. Upload en installeer het inhoudspakket met gebruik van Package Manager.
1. Werk de fragmenten voor kop- en voettekstervaring bij om het WKND-logo te gebruiken

## Een artikelpagina voor tijdschriften maken

Maak vervolgens een nieuwe pagina met de sjabloon Artikelpagina tijdschrift. Maak de inhoud van de pagina zodat deze overeenkomt met de sitemakken. Voer de stappen in de onderstaande video uit:

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

Gebruik de [opgegeven tekst](./assets/page-templates/la-skateparks-copy.txt) om de inhoud van het artikel te vullen.

## Gefeliciteerd! {#congratulations}

Je hebt zojuist een nieuwe sjabloon en pagina met Adobe Experience Manager Sites gemaakt.

### Volgende stappen {#next-steps}

Op dit moment komen de pagina met artikelen in het tijdschrift en de site niet overeen met de merkstijlen voor WKND. Volg de [Thema](theming.md) zelfstudie voor het leren van de beste werkwijzen voor het bijwerken van CSS en JavaScript-frontendcode die wordt gebruikt om algemene stijlen op de site toe te passen.

### Oplossingspakket

U kunt een oplossingspakket voor dit hoofdstuk downloaden: [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
