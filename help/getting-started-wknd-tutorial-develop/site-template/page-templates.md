---
title: Paginasjablonen
seo-title: Aan de slag met AEM Sites - Paginasjablonen
description: Leer hoe u paginasjablonen maakt en wijzigt. Begrijp de verhouding tussen een Malplaatje van de Pagina en een Pagina. Leer hoe u beleid van een paginasjabloon configureert voor korrelig beheer en consistentie van merken voor inhoud.  Een goed gestructureerde sjabloon voor artikel van het tijdschrift wordt gemaakt op basis van een model van Adobe XD.
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Inhoudsbeheer
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 0%

---


# Paginasjablonen {#page-templates}

>[!CAUTION]
>
> De functies voor snel maken van sites die hier worden getoond, worden in de tweede helft van 2021 gepubliceerd. De bijbehorende documentatie is beschikbaar voor voorbeelddoeleinden.

In dit hoofdstuk onderzoeken we de relatie tussen een paginasjabloon en een pagina. We maken een niet-opgemaakte sjabloon voor een tijdschriftartikel op basis van bepaalde modellen van [AdobeXD](https://www.adobe.com/products/xd.html). In het proces om het malplaatje uit te bouwen, zijn de Componenten van de Kern en de geavanceerde beleidsconfiguraties behandeld.

## Vereisten {#prerequisites}

Dit is een meerdelige zelfstudie en er wordt van uitgegaan dat de stappen in het hoofdstuk [Inhoud schrijven en wijzigingen publiceren](./author-content-publish.md) zijn voltooid.

## Doelstelling

1. Inspect a page design created in Adobe XD and map it to Core Components.
1. Begrijp de details van de Malplaatjes van de Pagina en hoe het beleid kan worden gebruikt om korrelige controle van paginainhoud af te dwingen.
1. Leer hoe sjablonen en pagina&#39;s zijn gekoppeld.

## Wat u gaat maken {#what-you-will-build}

In dit gedeelte van de zelfstudie maakt u een nieuwe sjabloon voor de artikelpagina van tijdschriften waarmee u nieuwe artikelen voor tijdschriften kunt maken en uitlijnt op een algemene structuur. De sjabloon is gebaseerd op ontwerpen en een UI-kit die in AdobeXD wordt gemaakt. Dit hoofdstuk is alleen gericht op het opbouwen van de structuur of het skelet van de sjabloon. Er worden geen stijlen geïmplementeerd, maar de sjabloon en pagina&#39;s zijn functioneel.

## UI-planning met Adobe XD {#adobexd}

In de meeste gevallen begint de planning voor een nieuwe website met modellen en statische ontwerpen. [Adobe ](https://www.adobe.com/products/xd.html) XD is een ontwerpgereedschap waarmee gebruikers een ervaring kunnen opbouwen. Vervolgens inspecteren we een UI-kit en -modellen om de structuur van het sjabloon voor artikelpagina te helpen plannen.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**Download het  [WKND-artikelontwerpbestand](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> Een generische [AEM de Uitrusting van de Componenten UI van de Kern is ook beschikbaar](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) als uitgangspunt voor douaneprojecten.

## De sjabloon voor artikelpagina voor tijdschriften maken

Wanneer u een pagina maakt, moet u een sjabloon selecteren die wordt gebruikt als basis voor het maken van de nieuwe pagina. De sjabloon definieert de structuur van de resulterende pagina, de initiële inhoud en de toegestane componenten.

Er zijn drie hoofdgebieden van [Paginasjablonen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html):

1. **Structuur**  - definieert componenten die deel uitmaken van de sjabloon. Deze kunnen niet worden bewerkt door auteurs van inhoud.
1. **Initiële inhoud**  - definieert componenten waarmee de sjabloon begint. Deze kunnen worden bewerkt en/of verwijderd door makers van inhoud
1. **Het beleid**  - bepaalt configuraties op hoe de componenten zich zullen gedragen en welke optiesauteurs beschikbaar zullen hebben.

Maak vervolgens een nieuwe sjabloon in AEM die overeenkomt met de structuur van de modellen. Dit zal in een lokale instantie van AEM voorkomen. Voer de stappen in de onderstaande video uit:

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

### Oplossingspakket

Een voltooide [oplossing van het Tijdschriftmalplaatje](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip) kan via de Manager van het Pakket worden gedownload en worden geïnstalleerd.

## Koptekst en voettekst bijwerken met ervaringsfragmenten {#experience-fragments}

Bij het maken van algemene inhoud, zoals een kop- of voettekst, wordt vaak gebruikgemaakt van een [Experience Fragment](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Met de functie Fragmenten van ervaring kunnen gebruikers meerdere componenten combineren om één component te maken die geschikt is voor referentie. De Fragmenten van de ervaring hebben het voordeel om multi-site beheer en [localization](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure) te steunen.

De Sitesjabloon genereerde een kop- en voettekst. Werk vervolgens de Experience Fragments bij zodat deze overeenkomen met de modellen. Voer de stappen in de onderstaande video uit:

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

Stappen op hoog niveau voor de onderstaande video:

1. Download het pakket met voorbeeldinhoud **[WKND-Starter-Assets-Skate-Article-1.0.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.0.zip)**.
1. Upload en installeer het inhoudspakket met gebruik van Package Manager.
1. Werk de fragmenten voor kop- en voettekstervaring bij om het WKND-logo te gebruiken

## Een artikelpagina voor tijdschriften maken

Maak vervolgens een nieuwe pagina met de sjabloon Artikelpagina tijdschrift. Maak de inhoud van de pagina zodat deze overeenkomt met de sitemakken. Voer de stappen in de onderstaande video uit:

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

## Gefeliciteerd! {#congratulations}

Je hebt zojuist een nieuwe sjabloon en pagina met Adobe Experience Manager Sites gemaakt.

### Volgende stappen {#next-steps}

Op dit moment komen de pagina met artikelen in het tijdschrift en de site niet overeen met de merkstijlen voor WKND. Volg de zelfstudie [Thema](theming.md) om de beste werkwijzen te leren voor het bijwerken van CSS en JavaScript-frontendcode die wordt gebruikt om algemene stijlen toe te passen op de site.

### Oplossingspakket

U kunt een oplossingspakket voor dit hoofdstuk downloaden: [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
