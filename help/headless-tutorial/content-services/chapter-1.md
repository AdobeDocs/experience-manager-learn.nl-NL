---
title: Hoofdstuk 1 - Lesbestanden instellen en downloaden - Inhoudsservices
description: Hoofdstuk 1 van de AEM zelfstudie zonder kop stelt de basislijninstelling voor de AEM voor de zelfstudie.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
duration: 85
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---

# Zelfstudie instellen

De nieuwste versie van AEM en AEM WCM Core Components wordt altijd aanbevolen.

* AEM 6.5 of hoger
* AEM WCM Core Components 2.4.0 of hoger
   * Omvat in het [ Mobiele Pakket van de Inhoud van de AEM van de Toepassing hieronder ](#wknd-mobile-application-packages)

Voorafgaand aan de aanvang van dit leerprogramma zorgt ervoor dat de volgende AEM instanties [ geïnstalleerd en lopend op uw lokale machine ](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install) zijn:

* **AEM Auteur** op **haven 4502**
* **AEM Publish** op **haven 4503**

## WKND Mobile-toepassingspakketten{#wknd-mobile-application-packages}

Installeer de volgende AEM Pakketten van de Inhoud op **zowel** AEM Auteur als AEM Publish, gebruikend [!DNL AEM Package Manager].

* [ ui.apps: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip ](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Proxycomponent voor AEM WCM Core-componenten
   * [!DNL WKND Mobile] CSS AEM Content Services-pagina&#39;s (voor kleine opmaakmodellen)
* [ ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip ](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Sitestructuur
   * [!DNL WKND Mobile] DAM-mapstructuur
   * [!DNL WKND Mobile] afbeeldingselementen

In [ Hoofdstuk 7 ](./chapter-7.md) zullen wij [!DNL WKND Mobile] Android Mobile App in werking stellen gebruikend [ de Studio van Android ](https://developer.android.com/studio) en verstrekte APK (het Pakket van de Toepassing van Android):

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Hoofdstuk AEM inhoudspakketten

Deze reeks inhoudspakketten leidt tot inhoud en configuratie die in het bijbehorende hoofdstuk, en alle voorafgaande hoofdstukken worden beschreven. Deze pakketten zijn optioneel, maar kunnen het maken van inhoud versnellen.

* [ Hoofdstuk 2 Inhoud: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip ](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [ Hoofdstuk 3 Inhoud: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip ](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [ Hoofdstuk 4 Inhoud: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip ](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [ Hoofdstuk 5 Inhoud: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip ](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Source-code

De broncode voor zowel het AEM project als [!DNL Android Mobile App] is beschikbaar op [[!DNL AEM Guides - WKND Mobile GitHub Project] ](https://github.com/adobe/aem-guides-wknd-mobile). De broncode hoeft niet te worden samengesteld of gewijzigd voor deze zelfstudie, maar is bedoeld om volledige transparantie mogelijk te maken in de manier waarop alle aspecten van de zelfstudie worden opgebouwd.

Als u een kwestie met het leerprogramma of de code vindt, gelieve de kwestie van a [ GitHub ](https://github.com/adobe/aem-guides-wknd-mobile/issues) te verlaten.

## Overslaan naar het einde

Om aan het eind van het leerprogramma over te slaan, kan het {](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) inhoudspakket 0} com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip op **zowel** AEM Auteur als AEM Publish worden geïnstalleerd. [ Houd er rekening mee dat inhoud en configuratie niet worden weergegeven zoals is gepubliceerd in AEM auteur, maar vanwege de handmatige implementatie zijn alle vereiste inhoud en configuratie beschikbaar op AEM Publish, zodat de [!DNL WKND Mobile App] toegang heeft tot de inhoud.


## Volgende stap

* [Hoofdstuk 2 - Fragmentmodellen voor gebeurtenisinhoud definiëren](./chapter-2.md)
