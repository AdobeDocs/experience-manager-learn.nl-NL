---
title: Adobe Experience Manager integreren met Adobe Target
seo-title: Een artikel waarin verschillende manieren worden beschreven om Adobe Experience Manager(AEM) met Adobe Target te integreren voor het leveren van gepersonaliseerde inhoud.
description: Een artikel waarin wordt beschreven hoe u Adobe Experience Manager voor verschillende scenario's kunt instellen met Adobe Target.
seo-description: Een artikel waarin wordt beschreven hoe u Adobe Experience Manager voor verschillende scenario's kunt instellen met Adobe Target.
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 1%

---


# Adobe Experience Manager integreren met Adobe Target

In deze sectie zullen we bespreken hoe we Adobe Experience Manager voor verschillende scenario&#39;s kunnen instellen met Adobe Target. Gebaseerd op uw scenario en organisatorische vereisten.

* **Voeg Adobe Target JavaScript-bibliotheek toe (vereist voor alle scenario&#39;s)** Voor sites die worden gehost op AEM, kunt u doelbibliotheken aan uw site toevoegen met de opdracht [Starten](https://docs.adobe.com/content/help/en/launch/using/overview.html). De lancering verstrekt een eenvoudige manier om alle markeringen noodzakelijk om relevante klantenervaringen op te stellen en te beheren.
* **Voeg de Adobe Target-Cloud Services toe (vereist voor het scenario Experience Fragments)** Voor AEM klanten die de aanbiedingen van Experience Fragment willen gebruiken om een activiteit in Adobe Target te maken, moet u Adobe Target integreren met AEM met behulp van de Legacy-Cloud Services. Deze integratie is vereist om Experience Fragments van AEM naar Target te duwen als HTML/JSON-aanbiedingen en om de aanbiedingen gelijk te houden met AEM. 
*Deze integratie is vereist voor de uitvoering van scenario 1.*

## Vereisten

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5 (*het nieuwste Service Pack wordt aanbevolen*)
   * Download AEM WKND-referentiesite-pakketten
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [Kernonderdelen](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [Digitale gegevenslaag](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - <https://>`<yourcompany>`.experienceCloud.adobe.com
   * Experience Cloud voorzien van de volgende oplossingen
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O-console](https://console.adobe.io)

* **Omgeving**
   * Java 1.8 of Java 11 (alleen AEM 6.5+)
   * Apache Maven (3.3.9 of hoger)
   * Chroom

>[!NOTE]
>
> De klant moet van Experience Platform Launch en Adobe I/O van de steun [van de](https://helpx.adobe.com/nl/contact/enterprise-support.ec.html) Adobe voorzien of uw systeembeheerder bereiken

### AEM instellen{#set-up-aem}

AEM auteur- en publicatieexemplaar is nodig om deze zelfstudie te voltooien. De instantie van de auteur wordt uitgevoerd op `http://localhost:4502` en wordt gepubliceerd `http://localhost:4503`. Zie voor meer informatie: [Stel een lokale AEM ontwikkelomgeving](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html)in.

#### AEM-auteur- en publicatie-instanties instellen

1. Haal een kopie van de [AEM QuickStart Jar en een licentie op.](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. Maak als volgt een mapstructuur op uw computer:
   ![Mapstructuur](assets/implementation/aem-setup-1.png)
3. Wijzig de naam van de QuickStart-jar `aem-author-p4502.jar` en plaats deze onder de `/author` directory. Voeg het `license.properties` bestand onder de `/author` map toe.
   ![Instantie AEM-auteur](assets/implementation/aem-setup-author.png)
4. Maak een kopie van de Quickstart-jar, wijzig de naam van de jar `aem-publish-p4503.jar` en plaats deze onder de `/publish` directory. Voeg een kopie van het `license.properties` bestand toe onder de `/publish` map.
   ![AEM-publicatie-instantie](assets/implementation/aem-setup-publish.png)
5. Dubbelklik op het `aem-author-p4502.jar` bestand om de instantie Auteur te installeren. Hierdoor wordt de instantie van de auteur gestart en wordt poort 4502 op de lokale computer uitgevoerd.
6. Meld u aan met de onderstaande referenties. Wanneer de aanmelding is geslaagd, wordt u naar het scherm AEM startpagina geleid.
username : **beheerderswachtwoord**: **beheerder**
   ![AEM-publicatie-instantie](assets/implementation/aem-author-home-page.png)
7. Dubbelklik op het `aem-publish-p4503.jar` bestand om een publicatie-instantie te installeren. U kunt een nieuw lusje zien open in uw browser voor uw publicatieinstantie, die op haven 4503 loopt en de WebRetail homepage toont. We gebruiken de WKND-referentiesite voor deze zelfstudie en installeren de pakketten op de auteurinstantie.
8. Navigeer naar de AEM-auteur in uw webbrowser op `http://localhost:4502`. Navigeer in het scherm AEM Start naar *[Extra > Implementatie > Pakketten](http://localhost:4502/crx/packmgr/index.jsp)*.
9. De pakketten voor AEM downloaden en uploaden (hierboven vermeld onder *[Voorwaarden > AEM](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. Nadat u de pakketten op AEM-auteur hebt geïnstalleerd, selecteert u elk geüploade pakket in AEM Package Manager en selecteert u **Meer > Repliceren** om ervoor te zorgen dat de pakketten worden geïmplementeerd in AEM Publish.
11. U hebt nu de WKND-referentiesite en alle aanvullende pakketten die vereist zijn voor deze zelfstudie geïnstalleerd.

[VOLGENDE HOOFDSTUK](./using-launch-adobe-io.md): In het volgende hoofdstuk, zult u Lancering met AEM integreren.
