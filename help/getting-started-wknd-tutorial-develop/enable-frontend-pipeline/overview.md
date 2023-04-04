---
title: De front-end pijpleiding voor standaardAEM project Archetype toestaan
description: Leer hoe u een front-end pijpleiding voor standaard AEM project voor snellere plaatsing van statische middelen zoals CSS, JavaScript, Doopvonten, Pictogrammen toelaat. Ook scheiding van front-end ontwikkeling van full-stack back-end ontwikkeling op AEM.
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 0%

---


# De front-end pijpleiding voor standaardAEM project Archetype toestaan{#enable-front-end-pipeline-standard-aem-project}

Leer hoe u de [AEM WKND-siteproject](https://github.com/adobe/aem-guides-wknd) (ook bekend als Standard AEM Project) gemaakt met [Projectarchetype AEM](https://github.com/adobe/aem-project-archetype) om front-end middelen zoals CSS, JavaScript, Doopvonten, en Pictogrammen op te stellen gebruikend een front-end pijpleiding voor een snellere ontwikkeling-aan-plaatsingscyclus. De scheiding van front-end ontwikkeling van full-stack back-end ontwikkeling op AEM. U leert ook hoe deze front-end bronnen zijn __niet__ gediend van de AEM bewaarplaats maar van CDN, een verandering in leveringsparadigm.


Een nieuwe front-end pijpleiding wordt gecreeerd in de Manager van de Wolk van Adobe die slechts bouwt en opstelt `ui.frontend` artefacten aan ingebouwde CDN en informeert AEM over zijn plaats. Bij AEM tijdens het genereren van de HTML van de webpagina wordt de `<link>` en `<script>` -tags, naar deze artefactlocatie verwijzen in de `href` kenmerkwaarde.

Nochtans, na de WKND Plaatsen AEM projectomzetting, kunnen de front-end ontwikkelaars van en parallel aan om het even welke volledig-stapel achterste-eindontwikkeling op AEM werken, die zijn eigen plaatsingspijpleidingen heeft.

>[!IMPORTANT]
>
>Over het algemeen, wordt de front-end pijpleiding typisch gebruikt met [Snel site maken AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en), er is een verwante zelfstudie [Aan de slag met AEM Sites - Snel site maken](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) voor meer informatie. In deze zelfstudie en bijbehorende video&#39;s komen verwijzingen ernaar aan de orde, om ervoor te zorgen dat subtiele verschillen worden uitgeroepen en er een directe of indirecte vergelijking is om cruciale concepten uit te leggen.


Verwante [zelfstudie met meerdere stappen](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) doorloopt de implementatie van een AEM site voor een fictieve levensstijl op de WKND met behulp van de functie Snel site maken. De [Thematische workflow](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) om de werking van de pijpleiding aan de voorzijde te begrijpen is ook nuttig.

## Overzicht, voordelen, en overwegingen voor front-end pijpleiding

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>Dit geldt alleen voor AEM as a Cloud Service en niet voor op AMS gebaseerde Adobe Cloud Manager-implementaties.

## Vereisten

De implementatiestap in deze zelfstudie vindt plaats in een Adobe Cloud Manager, zorg ervoor dat u een __Implementatiebeheer__ rol, zie Cloud Manage [Roldefinities](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions).

Zorg ervoor dat u de [Sandbox-programma](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) en [Ontwikkelingsomgeving](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) als u deze zelfstudie voltooit.

## Volgende stappen {#next-steps}

Een geleidelijke zelfstudie doorloopt de [AEM WKND-siteproject](https://github.com/adobe/aem-guides-wknd) conversie om de pijpleiding aan de voorzijde mogelijk te maken.

Waar wacht u op? De zelfstudie starten door naar de [Project in volledige stapel bekijken](review-uifrontend-module.md) hoofdstuk en hervat de levenscyclus aan de voorkant van de ontwikkelingscyclus in het kader van het standaard AEM Sites Project.

