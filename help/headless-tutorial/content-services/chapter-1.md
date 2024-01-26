---
title: Hoofdstuk 1 - Lesbestanden instellen en downloaden - Inhoudsservices
description: Hoofdstuk 1 van de AEM zelfstudie zonder kop stelt de basislijninstelling voor de AEM voor de zelfstudie.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
duration: 110
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---

# Zelfstudie instellen

De nieuwste versie van AEM en AEM WCM Core Components wordt altijd aanbevolen.

* AEM 6.5 of hoger
* AEM WCM Core Components 2.4.0 of hoger
   * Opgenomen in de [WKND Mobile AEM Application Content Package hieronder](#wknd-mobile-application-packages)

Voordat u deze zelfstudie start, moet u controleren of de volgende AEM exemplaren zijn [geïnstalleerd en uitgevoerd op uw lokale computer](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **AEM auteur** op **poort 4502**
* **AEM publiceren** op **poort 4503**

## WKND Mobile-toepassingspakketten{#wknd-mobile-application-packages}

Installeer de volgende AEM Inhoudspakketten op **beide** Auteur AEM en AEM publiceren, gebruiken [!DNL AEM Package Manager].

* [ui.apps: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Proxycomponent voor AEM WCM Core-componenten
   * [!DNL WKND Mobile] CSS van pagina&#39;s voor inhoudsservices AEM (voor kleine opmaak)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Sitestructuur
   * [!DNL WKND Mobile] DAM-mapstructuur
   * [!DNL WKND Mobile] afbeeldingselementen

In [Hoofdstuk 7](./chapter-7.md) wij zullen de [!DNL WKND Mobile] Android Mobile-toepassing gebruiken [Android Studio](https://developer.android.com/studio) en de geleverde APK (Android Application Package):

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Hoofdstuk AEM inhoudspakketten

Deze reeks inhoudspakketten leidt tot inhoud en configuratie die in het bijbehorende hoofdstuk, en alle voorafgaande hoofdstukken worden beschreven. Deze pakketten zijn optioneel, maar kunnen het maken van inhoud versnellen.

* [Hoofdstuk 2 Inhoud: GitHub > Middelen > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Hoofdstuk 3 Inhoud: GitHub > Middelen > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Hoofdstuk 4 Inhoud: GitHub > Middelen > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Hoofdstuk 5 Inhoud: GitHub > Middelen > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Broncode

De broncode voor zowel het AEM als het [!DNL Android Mobile App] zijn beschikbaar op [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). De broncode hoeft niet te worden samengesteld of gewijzigd voor deze zelfstudie, maar is bedoeld om volledige transparantie mogelijk te maken in de manier waarop alle aspecten van de zelfstudie worden opgebouwd.

Als u een probleem hebt met de zelfstudie of de code, kunt u een [GitHub-probleem](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Overslaan naar het einde

Als u het einde van de zelfstudie wilt bereiken, kunt u het volgende doen: [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) inhoudspakket kan worden geïnstalleerd op **beide** AEM Auteur en AEM publiceren. Merk op dat de inhoud en de configuratie niet zullen tonen zoals die in AEM Auteur wordt gepubliceerd, echter wegens de handplaatsing, al vereiste inhoud en configuratie op AEM publiceren beschikbaar zijn die toestaat [!DNL WKND Mobile App] om de inhoud te openen.


## Volgende stap

* [Hoofdstuk 2 - Fragmentmodellen voor gebeurtenisinhoud definiëren](./chapter-2.md)
