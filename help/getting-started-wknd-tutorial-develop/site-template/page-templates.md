---
title: Paginasjablonen
description: Leer hoe u paginasjablonen maakt en wijzigt. Begrijp de verhouding tussen een Malplaatje van de Pagina en een Pagina. Leer hoe u beleid van een paginasjabloon configureert voor korrelig beheer en consistentie van merken voor inhoud.  Een goed gestructureerde sjabloon voor artikel van het tijdschrift wordt gemaakt op basis van een model van Adobe XD.
version: Experience Manager as a Cloud Service
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
recommendations: noDisplay, noCatalog
duration: 1561
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '628'
ht-degree: 0%

---

# Paginasjablonen {#page-templates}

In dit hoofdstuk onderzoeken we de relatie tussen een paginasjabloon en een pagina. Wij zullen een niet-gestileerde malplaatje van het Artikel van het Tijdschrift bouwen dat op sommige modellen van [ wordt gebaseerd AdobeXD ](https://www.adobe.com/products/xd.html). In het proces om het malplaatje uit te bouwen, zijn de Componenten van de Kern en de geavanceerde beleidsconfiguraties behandeld.

## Vereisten {#prerequisites}

Dit is een meerdelig leerprogramma en men veronderstelt dat de stappen in de [ inhoud van de Auteur worden geschetst en veranderingen ](./author-content-publish.md) hoofdstuk publiceren zijn voltooid.

## Doelstelling

1. Begrijp de details van de Malplaatjes van de Pagina en hoe het beleid kan worden gebruikt om korrelige controle van paginainhoud af te dwingen.
1. Leer hoe sjablonen en pagina&#39;s zijn gekoppeld.
1. Een nieuwe sjabloon maken en een pagina ontwerpen.

## Wat u gaat maken {#what-you-will-build}

In dit gedeelte van de zelfstudie maakt u een nieuwe sjabloon voor de artikelpagina van tijdschriften waarmee u nieuwe artikelen voor tijdschriften kunt maken en uitlijnt op een algemene structuur. De sjabloon is gebaseerd op ontwerpen en een UI-kit die in AdobeXD is gemaakt. Dit hoofdstuk is alleen gericht op het opbouwen van de structuur of het skelet van de sjabloon. Er worden geen stijlen geïmplementeerd, maar de sjabloon en pagina&#39;s zijn functioneel.

## De sjabloon voor artikelpagina in het tijdschrift maken

Wanneer u een pagina maakt, moet u een sjabloon selecteren die wordt gebruikt als basis voor het maken van de nieuwe pagina. De sjabloon definieert de structuur van de resulterende pagina, de initiële inhoud en de toegestane componenten.

Er zijn 3 belangrijke gebieden van [ Malplaatjes van de Pagina ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html?lang=nl-NL):

1. **Structuur** - bepaalt componenten die een deel van het malplaatje zijn. Deze kunnen niet worden bewerkt door de auteurs van de inhoud.
1. **Aanvankelijke Inhoud** - bepaalt componenten die het malplaatje met begint, kunnen deze worden uitgegeven en/of door inhoudauteurs worden geschrapt
1. **Beleid** - bepaalt configuraties op hoe de componenten zich gedragen en welke optiesauteurs beschikbaar zullen hebben.

Maak vervolgens een nieuwe sjabloon in AEM die overeenkomt met de structuur van de modellen. Dit gebeurt in een lokale versie van AEM. Voer de stappen in de onderstaande video uit:

>[!VIDEO](https://video.tv.adobe.com/v/332915?quality=12&learn=on)

Met de volgende miniatuur kunt u uw sjabloon identificeren (of uw eigen sjabloon uploaden!)

![ de malplaatjeduimnagel van de Pagina van het Artikel ](./assets/page-templates/article-page-template-thumbnail.png)


### Oplossingspakket

Een gebeëindigde [ oplossing van het Malplaatje van het Tijdschrift ](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip) kan via de Manager van het Pakket worden gedownload en worden geïnstalleerd.

## Koptekst en voettekst bijwerken met ervaringsfragmenten {#experience-fragments}

Een gemeenschappelijke praktijk wanneer het creëren van globale inhoud, zoals een kopbal of footer, moet een [ Fragment van de Ervaring gebruiken ](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=nl-NL). Met de functie Fragmenten van ervaring kunnen gebruikers meerdere componenten combineren om één component te maken die geschikt is voor referentie. De Fragmenten van de ervaring hebben het voordeel om multi-site beheer en [ localisatie ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=nl-NL#localized-site-structure) te steunen.

De Sitesjabloon genereerde een kop- en voettekst. Werk vervolgens de Experience Fragments bij zodat deze overeenkomen met de modellen. Voer de stappen in de onderstaande video uit:

>[!VIDEO](https://video.tv.adobe.com/v/332916?quality=12&learn=on)

Stappen op hoog niveau voor de onderstaande video:

1. Download het pakket van de steekproefinhoud **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**.
1. Upload en installeer het inhoudspakket met gebruik van Package Manager.
1. Werk de fragmenten voor kop- en voettekstervaring bij om het WKND-logo te gebruiken

## Een artikelpagina voor tijdschriften maken

Maak vervolgens een nieuwe pagina met de sjabloon Artikelpagina tijdschrift. Maak de inhoud van de pagina zodat deze overeenkomt met de sitemakken. Voer de stappen in de onderstaande video uit:

>[!VIDEO](https://video.tv.adobe.com/v/332917?quality=12&learn=on)

Gebruik de [ verstrekte tekst ](./assets/page-templates/la-skateparks-copy.txt) om het artikellichaam te bevolken.

## Gefeliciteerd! {#congratulations}

Je hebt zojuist een nieuwe sjabloon en pagina met Adobe Experience Manager Sites gemaakt.

### Volgende stappen {#next-steps}

Op dit moment komen de pagina met artikelen in het tijdschrift en de site niet overeen met de merkstijlen voor WKND. Volg het [ Thema ](theming.md) leerprogramma om de beste praktijken voor het bijwerken van CSS en JavaScript frontend code te leren die wordt gebruikt om globale stijlen op de plaats toe te passen.

### Oplossingspakket

Een oplossingspakket voor dit hoofdstuk is beschikbaar om te downloaden: [ WKND-Tijdschrift-malplaatje-OPLOSSING-1.0.zip ](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
