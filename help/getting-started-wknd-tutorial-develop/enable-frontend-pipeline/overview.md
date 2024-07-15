---
title: De front-end pijpleiding voor standaardAEM project Archetype toestaan
description: Leer hoe u een front-end pijpleiding voor standaard AEM project voor snellere plaatsing van statische middelen zoals CSS, JavaScript, Doopvonten, Pictogrammen toelaat. Ook scheiding van front-end ontwikkeling van full-stack back-end ontwikkeling op AEM.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: disable
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
doc-type: Tutorial
exl-id: b795e7e8-f611-4fc3-9846-1d3f1a28ccbc
duration: 206
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---

# De front-end pijpleiding voor standaardAEM project Archetype toestaan{#enable-front-end-pipeline-standard-aem-project}

Leer hoe te om het [ AEM WebND Project van Plaatsen ](https://github.com/adobe/aem-guides-wknd) (alias Standaard AEM Project) toe te laten gecreeerd gebruikend [ AEM Archetype van het Project ](https://github.com/adobe/aem-project-archetype) om front-end middelen zoals CSS, JavaScript, Doopvonten, en Pictogrammen op te stellen gebruikend een front-end pijpleiding voor een snellere ontwikkeling-aan-plaatsing cyclus. De scheiding van front-end ontwikkeling van full-stack back-end ontwikkeling op AEM. U leert ook hoe deze front-end middelen __niet__ van de AEM bewaarplaats maar van CDN, een verandering in leveringsparadigm worden gediend.


Een nieuwe front-end pijpleiding wordt gecreeerd in Adobe Cloud Manager die slechts `ui.frontend` artefacten bouwt en opstelt aan ingebouwde CDN en AEM over zijn plaats informeert. Bij AEM tijdens het genereren van de HTML van de webpagina verwijzen de tags `<link>` en `<script>` naar deze artefactlocatie in de waarde van het kenmerk `href` .

Nochtans, na de WKND Plaatsen AEM projectomzetting, kunnen de front-end ontwikkelaars van en parallel aan om het even welke volledig-stapel achterste-eindontwikkeling op AEM werken, die zijn eigen plaatsingspijpleidingen heeft.

>[!IMPORTANT]
>
>Over het algemeen, wordt de front-end pijpleiding typisch gebruikt met [ AEM Snelle Verwezenlijking van de Plaats ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en), is er een verwant leerprogramma [ Begonnen het worden met AEM Sites - Snelle Verwezenlijking van de Plaats ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) om meer over het te leren. In deze zelfstudie en bijbehorende video&#39;s komen verwijzingen ernaar aan de orde, om ervoor te zorgen dat subtiele verschillen worden uitgeroepen en er een directe of indirecte vergelijking is om cruciale concepten uit te leggen.


Een verwant [ multi-step leerprogramma ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) loopt door het uitvoeren van een AEM plaats voor een fictieve levensstijlmerk WKND gebruikend de Snelle eigenschap van de Aanmaak van de Plaats. Het herzien van het [ Theming werkschema ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) om de voorste-eind werkingen van de Pijpleiding te begrijpen is ook nuttig.

## Overzicht, voordelen, en overwegingen voor front-end pijpleiding

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>Dit geldt alleen voor AEM as a Cloud Service en niet voor Cloud Manager-implementaties op basis van AMS.

## Vereisten

De plaatsingsstap in dit leerprogramma vindt in een Adobe Cloud Manager plaats, zorg ervoor dat u de rol van de Manager van de Plaatsing van de a ____ hebt, ziet de Wolk [ Definities van de Rol ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions) beheert.

Zorg ervoor om het [ programma Sandbox ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) en [ milieu van de Ontwikkeling ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) te gebruiken wanneer de voltooiing van dit leerprogramma.

## Volgende stappen {#next-steps}

Een stap-voor-stap leerprogramma loopt door de [ AEM 1} omzetting van het Project van Plaatsen WKND {om het voor de front-end pijpleiding toe te laten.](https://github.com/adobe/aem-guides-wknd)

Waar wacht u op? Begin het leerprogramma door aan het [ Volledige hoofdstuk van het Project van het Overzicht te navigeren ](review-uifrontend-module.md) en de cyclus van het voorwaartse ontwikkelingsleven in de context van het standaardProject van AEM Sites opnieuw te vatten.
