---
title: Hoofdstuk 1 - Lesbestanden instellen en downloaden
seo-title: Aan de slag met AEM Content Services - Hoofdstuk 1 - Lesbestanden instellen
description: Hoofdstuk 1 van de AEM zelfstudie zonder kop stelt de basislijninstelling voor de AEM voor de zelfstudie.
seo-description: Hoofdstuk 1 van de AEM zelfstudie zonder kop stelt de basislijninstelling voor de AEM voor de zelfstudie.
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 0%

---


# Zelfstudie instellen

De nieuwste versie van AEM en AEM WCM Core Components wordt altijd aanbevolen.

* AEM 6.5 of hoger
* AEM WCM Core Components 2.4.0 of hoger
   * Opgenomen in het [WKND Mobile AEM Application Content Package hieronder](#wknd-mobile-application-packages)

Voordat u deze zelfstudie start, moet u controleren of de volgende AEM op uw lokale computer [zijn](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install)geïnstalleerd en uitgevoerd:

* **AEM-auteur** op **poort 4502**
* **AEM-publicatie** op **poort 4503**

## WKND Mobile-toepassingspakketten{#wknd-mobile-application-packages}

Installeer de volgende AEM Inhoudspakketten op **zowel** AEM-auteur als AEM-publicatie met behulp van [!DNL AEM Package Manager].

* [ui.apps: GitHub > Middelen > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Proxycomponent voor AEM WCM Core-componenten
   * [!DNL WKND Mobile] CSS van pagina&#39;s voor inhoudsservices AEM (voor kleine opmaak)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Sitestructuur
   * [!DNL WKND Mobile] DAM-mapstructuur
   * [!DNL WKND Mobile] afbeeldingselementen

In [Hoofdstuk 7](./chapter-7.md) voeren we de mobiele [!DNL WKND Mobile] Android-app uit met [Android Studio](https://developer.android.com/studio) en de meegeleverde APK (Android Application Package):

* [[!DNL Android Mobile-app: GitHub > Middelen > wint-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Hoofdstuk AEM inhoudspakketten

Deze reeks inhoudspakketten leidt tot inhoud en configuratie die in het bijbehorende hoofdstuk, en alle voorafgaande hoofdstukken worden beschreven. Deze pakketten zijn optioneel, maar kunnen het maken van inhoud versnellen.

* [Hoofdstuk 2 Inhoud: GitHub > Middelen > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Hoofdstuk 3 Inhoud: GitHub > Middelen > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Hoofdstuk 4 Inhoud: GitHub > Middelen > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Hoofdstuk 5 Inhoud: GitHub > Middelen > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Broncode

De broncode voor zowel het AEM project als [!DNL Android Mobile App] is beschikbaar op [[!DNL AEMGidsen - het Project van GitHub van WKND Mobile]](https://github.com/adobe/aem-guides-wknd-mobile). De broncode hoeft niet te worden samengesteld of gewijzigd voor deze zelfstudie, maar is bedoeld om volledige transparantie mogelijk te maken in de manier waarop alle aspecten van de zelfstudie worden opgebouwd.

Als u een kwestie met het leerprogramma of de code vindt, gelieve een kwestie [te verlaten](https://github.com/adobe/aem-guides-wknd-mobile/issues)GitHub.

## Overslaan naar het einde

Als u het einde van de zelfstudie wilt overslaan, kunt u het inhoudspakket [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) installeren op **zowel** AEM-auteur als AEM-publicatie. Merk op dat de inhoud en de configuratie niet zullen tonen zoals die in Auteur AEM wordt gepubliceerd, nochtans wegens de handplaatsing, alle vereiste inhoud en de configuratie beschikbaar zullen zijn op Publiceren AEM toestaand de [!DNL WKND Mobile App] om tot de inhoud toegang te hebben.


## Volgende stap

* [Hoofdstuk 2 - Fragmentmodellen voor gebeurtenisinhoud definiëren](./chapter-2.md)
