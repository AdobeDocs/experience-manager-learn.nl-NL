---
title: Open front-end pijpleiding voor standaard AEM project Archetype
description: Leer hoe u een front-end pijpleiding voor standaard AEM project voor snellere plaatsing van statische middelen zoals CSS, JavaScript, Doopvonten, Pictogrammen toelaat. Ook het scheiden van front-end ontwikkeling van full-stack back-end ontwikkeling op AEM.
version: Experience Manager as a Cloud Service
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
source-git-commit: dbf63f30ccfd06e4f4d7883c2f7bc4ac78245364
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---

# Open front-end pijpleiding voor standaard AEM project Archetype{#enable-front-end-pipeline-standard-aem-project}

{{traditional-aem}}

Leer hoe te om het [&#x200B; WebND Project van de Plaatsen van AEM &#x200B;](https://github.com/adobe/aem-guides-wknd) (alias StandaardProject van AEM) toe te laten gecreeerd gebruikend [&#x200B; Archetype van het Project van AEM &#x200B;](https://github.com/adobe/aem-project-archetype) om front-end middelen zoals CSS, JavaScript, Doopvonten, en Pictogrammen op te stellen gebruikend een front-end pijpleiding voor een snellere ontwikkeling-aan-plaatsingscyclus. De scheiding van front-end ontwikkeling van full-stack back-end ontwikkeling op AEM. U leert ook hoe deze front-end middelen __niet__ van de bewaarplaats van AEM maar van CDN, een verandering in leveringsparadigm worden gediend.


In Adobe Cloud Manager wordt een nieuwe front-end pijplijn gemaakt die alleen `ui.frontend` artefacten bouwt en implementeert naar de ingebouwde CDN en AEM op de hoogte brengt van de locatie. In AEM verwijzen de tags `<link>` en `<script>` tijdens het genereren van de HTML-pagina naar deze artefactlocatie in de waarde van het kenmerk `href` .

Na de conversie van het WKND-project naar AEM kunnen de front-end ontwikkelaars echter afzonderlijk van en parallel aan elke full-stack back-end ontwikkeling werken op AEM, dat zijn eigen distributiepijpleidingen heeft.

>[!IMPORTANT]
>
>Over het algemeen, wordt de front-end pijpleiding typisch gebruikt met het [&#x200B; Snelle Gemaakt van de Plaats van AEM &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=nl-NL), is er een verwant leerprogramma [&#x200B; Begonnen het worden met AEM Sites - Snelle Verwezenlijking van de Plaats &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=nl-NL) om meer over het te leren. In deze zelfstudie en bijbehorende video&#39;s komen verwijzingen ernaar aan de orde, om ervoor te zorgen dat subtiele verschillen worden uitgeroepen en er een directe of indirecte vergelijking is om cruciale concepten uit te leggen.


Een verwant [&#x200B; multi-step leerprogramma &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=nl-NL) loopt door het uitvoeren van een plaats van AEM voor een fictieve levensstijlmerk WKND gebruikend de Snelle eigenschap van de Opstelling van de Plaats. Het herzien van het [&#x200B; Theming werkschema &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=nl-NL) om de voorste-eind werkingen van de Pijpleiding te begrijpen is ook nuttig.

## Overzicht, voordelen, en overwegingen voor front-end pijpleiding

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>Dit geldt alleen voor AEM as a Cloud Service en niet voor Adobe Cloud Manager-implementaties op basis van AMS.

## Vereisten

De plaatsingsstap in dit leerprogramma vindt in Adobe Cloud Manager plaats, zorg ervoor dat u de rol van de Manager van de a __Plaatsing__ hebt, ziet de Wolk [&#x200B; Definities van de Rol &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=nl-NL#role-definitions) beheert.

Zorg ervoor om het [&#x200B; programma Sandbox &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=nl-NL) en [&#x200B; milieu van de Ontwikkeling &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=nl-NL) te gebruiken wanneer de voltooiing van dit leerprogramma.

## Volgende stappen {#next-steps}

Een geleidelijke leerprogramma loopt door de [&#x200B; conversie van het Project van de Plaatsen van AEM WKND &#x200B;](https://github.com/adobe/aem-guides-wknd) om het voor de front-end pijpleiding toe te laten.

Waar wacht u op? Begin het leerprogramma door aan het [&#x200B; Volledige hoofdstuk van het Project van het Overzicht te navigeren &#x200B;](review-uifrontend-module.md) en de cyclus van het voorwaartse ontwikkelingsleven in de context van het standaardProject van AEM Sites opnieuw te vatten.
