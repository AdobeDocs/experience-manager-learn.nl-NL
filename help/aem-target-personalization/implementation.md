---
title: AEM Sites integreren met Adobe Target
description: Een artikel waarin wordt beschreven hoe u Adobe Experience Manager voor verschillende scenario's kunt instellen met Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 54a30cd9-d94a-4de5-82a1-69ab2263980d
duration: 130
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---

# AEM Sites integreren met Adobe Target

In deze sectie zullen we bespreken hoe we Adobe Experience Manager Sites voor verschillende scenario&#39;s kunnen instellen met Adobe Target. Gebaseerd op uw scenario en organisatorische vereisten.

* **voeg de Bibliotheek van Adobe Target JavaScript (die voor alle scenario&#39;s wordt vereist) toe**
Voor plaatsen die op AEM worden ontvangen, kunt u de bibliotheken van het Doel aan uw plaats toevoegen gebruikend, [ markeringen in Adobe Experience Platform ](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html). Tags bieden een eenvoudige manier om alle tags te implementeren en te beheren die nodig zijn om relevante ervaringen van klanten te benutten.
* **voeg de Cloud Servicen van Adobe Target (die voor het scenario van de Fragmenten van de Ervaring worden vereist) toe**
Voor AEM klanten, die de aanbiedingen van het Fragment van de Ervaring willen gebruiken om een activiteit binnen Adobe Target tot stand te brengen, zult u Adobe Target met AEM moeten integreren gebruikend de Verouderde Cloud Servicen. Deze integratie is vereist om Experience Fragments van AEM naar Target te duwen als HTML/JSON aanbiedingen en om de aanbiedingen gelijk te houden met AEM. *Deze integratie wordt vereist voor het uitvoeren van scenario 1.*

## Vereisten

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5 (*het recentste Pak van de Dienst wordt geadviseerd*)
   * Download AEM WKND-referentiesite-pakketten
      * [ aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip ](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [ aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip ](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [ Componenten van de Kern ](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [Digitale gegevenslaag](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud voorzien van de volgende oplossingen
      * [ de Inzameling van Gegevens ](https://experiencecloud.adobe.com)
      * [ Adobe Target ](https://experiencecloud.adobe.com)
      * [ Console van Adobe I/O ](https://console.adobe.io)

* **Milieu**
   * Java 1.8 of Java 11 (alleen AEM 6.5+)
   * Apache Maven (3.3.9 of hoger)
   * Chrome

>[!NOTE]
>
> De klant moet met de Inzameling en Adobe I/O van Gegevens van [ steun van de Adobe ](https://helpx.adobe.com/nl/contact/enterprise-support.ec.html) worden voorzien of uit reiken aan uw systeembeheerder

### AEM instellen{#set-up-aem}

AEM auteur- en publicatieexemplaar is nodig om deze zelfstudie te voltooien. De auteurinstantie loopt op `http://localhost:4502` en publiceert instantie lopend op `http://localhost:4503`. Voor meer informatie zie: [ Opstelling een Lokale Milieu van de Ontwikkeling van de AEM ](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html).

#### Auteur- en Publish-instanties instellen

1. Krijg een exemplaar van [ AEM Quickstart Jar en een vergunning.](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. Maak als volgt een mapstructuur op uw computer:
   ![ de Structuur van de Omslag ](assets/implementation/aem-setup-1.png)
3. Wijzig de naam van de QuickStart-jar in `aem-author-p4502.jar` en plaats deze onder de `/author` -map. Voeg het bestand `license.properties` toe onder de map `/author` .
   ![ AEM Instantie van de Auteur ](assets/implementation/aem-setup-author.png)
4. Maak een kopie van de QuickStart-jar, wijzig de naam in `aem-publish-p4503.jar` en plaats deze onder de `/publish` -map. Voeg een kopie van het `license.properties` -bestand toe onder de map `/publish` .
   ![ AEM de Instantie van Publish ](assets/implementation/aem-setup-publish.png)
5. Dubbelklik op het bestand `aem-author-p4502.jar` om de instantie Auteur te installeren. Hierdoor wordt de instantie van de auteur gestart en wordt poort 4502 op de lokale computer uitgevoerd.
6. Meld u aan met de onderstaande referenties. Wanneer de aanmelding met succes is voltooid, gaat u naar het scherm AEM startpagina.
username : **admin**
wachtwoord: **admin**
   ![ AEM de Instantie van Publish ](assets/implementation/aem-author-home-page.png)
7. Dubbelklik op het `aem-publish-p4503.jar` -bestand om een publicatie-instantie te installeren. U kunt een nieuw lusje zien open in uw browser voor uw publicatieinstantie, die op haven 4503 loopt en de WebRetail homepage toont. Wij gebruiken de WKND verwijzingsplaats voor dit leerprogramma en laten de pakketten op auteursinstantie installeren.
8. Navigeer naar AEM auteur in uw webbrowser op `http://localhost:4502` . Op het AEM scherm van het Begin, navigeer aan *[Hulpmiddelen > Plaatsing > Pakketten ](http://localhost:4502/crx/packmgr/index.jsp)*.
9. Download en upload de pakketten voor AEM (die hierboven onder *[Vereisten > AEM](#aem)* worden vermeld)
   * [ aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip ](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [ aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip ](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [ core.wcm.components.all-2.5.0.zip ](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. Na het installeren van de pakketten op AEM Auteur, selecteer elk geupload pakket in AEM Manager van het Pakket, en selecteer **Meer > Repliceer** om ervoor te zorgen de pakketten aan AEM Publish worden opgesteld.
11. U hebt nu de WKND-referentiesite en alle aanvullende pakketten die vereist zijn voor deze zelfstudie geïnstalleerd.

[ VOLGENDE HOOFDSTUK ](./using-launch-adobe-io.md): In het volgende hoofdstuk, zult u markeringen met AEM integreren.
