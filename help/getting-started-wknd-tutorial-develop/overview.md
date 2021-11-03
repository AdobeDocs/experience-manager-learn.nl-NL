---
title: Getting Started with AEM Sites - WKND Tutorial
description: Aan de slag met AEM Sites - WKND-zelfstudie. De WKND-zelfstudie is een meerdelige zelfstudie die is ontworpen voor ontwikkelaars die nog geen ervaring hebben met Adobe Experience Manager. De zelfstudie doorloopt de implementatie van een AEM site voor een fictieve levensstijl, de WKND. De zelfstudie behandelt fundamentele onderwerpen zoals projectopstelling, gemaakte archetypes, de Componenten van de Kern, Bewerkbare Malplaatjes, cliëntbibliotheken, en componentenontwikkeling.
sub-product: sites
topics: development
version: Cloud Service
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: 08146f57235f3de7fd5ab73754166cc85e1f7dda
workflow-type: tm+mt
source-wordcount: '619'
ht-degree: 0%

---

# Aan de slag met AEM Sites - WKND-zelfstudie {#introduction}

Welkom bij een meerdelige zelfstudie die is ontworpen voor nieuwe ontwikkelaars in Adobe Experience Manager (AEM). This tutorial walks through the implementation of an AEM site for a fictitious lifestyle brand the WKND. De zelfstudie behandelt fundamentele onderwerpen zoals projectopstelling, de Componenten van de Kern, Bewerkbare Malplaatjes, cliënt-zijbibliotheken, en componentenontwikkeling met Adobe Experience Manager Sites.

## Overzicht {#wknd-tutorial-overview}

Het doel van deze meerdelige zelfstudie is om een ontwikkelaar te leren hoe hij een website implementeert met de nieuwste standaarden en technologieën in Adobe Experience Manager (AEM). After completing this tutorial a developer should understand the basic foundation of the platform and with knowledge of common design patterns in AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Options for starting a Sites project

Er zijn twee basisbenaderingen voor het starten van een AEM Sites-project.

**AEM Project Archetype** - Traditional approach to AEM development by generating a minimal AEM project using a Maven template. Dit is de aanbevolen aanpak voor AEM 6.5/6.4-projecten en AEM as a Cloud Service projecten die grote aanpassingen verwachten. De zelfstudie biedt een dieper inzicht in AEM ontwikkeling.

[De zelfstudie starten met het AEM Project Archetype](./project-archetype/overview.md)

**Sitesjablonen AEM** - Een low-code benadering van het produceren van een AEM Plaats door een vooraf bepaald Malplaatje van de Plaats te gebruiken. Gebruik componenten en sjablonen uit de keuzelijst om een site snel in bedrijf te stellen. Gebruik een themaworkflow om merkspecifieke stijlen en aanpassingen toe te passen met alleen CSS en JavaScript. Aanbevolen voor nieuwe projecten en ontwikkelaars. Momenteel alleen beschikbaar voor AEM as a Cloud Service.

[Start the tutorial using a Site Template](./site-template/create-site.md)

## Adobe XD UI Kit

Om deze zelfstudie dichter bij een werkelijk scenario te maken Adobe de getalenteerde ontwerpers van UX creeerden de modellen voor de plaats gebruikend [Adobe XD](https://www.adobe.com/products/xd.html). In de loop van de zelfstudie worden verschillende onderdelen van de ontwerpen geïmplementeerd in een volledig voor de auteur geschikte AEM. Special thanks to **Lorenzo Buosi** and **Kilian Amendola** who created the beautiful design for the WKND site.

Download de XD UI-kits:

* [UI-kit AEM Core Component](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND-UI-kit](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## Referentiesite {#reference-site}

A finished version of the WKND Site is also available as a reference: [https://wknd.site/](https://wknd.site/)

The tutorial covers the major development skills needed for an AEM developer but will *not* build the entire site end-to-end. De voltooide verwijzingsplaats is een andere grote middel om meer van AEM uit de doosmogelijkheden te onderzoeken en te zien.

To test the latest code before jumping into the tutorial, download and install the **[latest release from GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Powered by Adobe Stock

Many of the images in the WKND Reference website are from [Adobe Stock](https://stock.adobe.com/) and are Third Party Material as defined in the Demo Asset Additional Terms at [https://www.adobe.com/legal/terms.html](https://www.adobe.com/legal/terms.html). If you want to use an Adobe Stock image for other purposes beyond viewing this demo website, such as featuring it on a website, or in marketing materials, you can purchase a license on Adobe Stock.

With Adobe Stock, you have access to more than 140 million high-quality, royalty-free images including photos, graphics, videos and templates to jumpstart your creative projects.

## Next Steps {#next-steps}

What are you waiting for?! Learn how to [generate a new Adobe Experience Manager project using the AEM Project Archetype](./project-archetype/overview.md) or [create a site using a Site Template](./site-template/create-site.md).
