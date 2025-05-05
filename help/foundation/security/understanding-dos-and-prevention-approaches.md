---
title: Inzicht in DoS/DDoS-preventie
description: Leer hoe u DoS- en DDoS-aanvallen op AEM kunt voorkomen en beperken.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Security
topic: Security, Development
role: Admin, Architect, Developer
level: Beginner
doc-type: Article
duration: 75
last-substantial-update: 2024-03-30T00:00:00Z
jira: KT-15219
exl-id: 1d7dd829-e235-4884-a13f-b6ea8f6b4b0b
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---

# Inzicht in DoS/DDoS-preventie in AEM

Leer over de opties beschikbaar om Dos en aanvallen van Dos en van DDoS op uw milieu van AEM te verhinderen en te verlichten. Alvorens in de preventiemechanismen te duiken, een kort overzicht van [ DoS ](https://developer.mozilla.org/en-US/docs/Glossary/DOS_attack) en [ DDoS ](https://developer.mozilla.org/en-US/docs/Glossary/Distributed_Denial_of_Service).

- DoS (Ontkenning van Dienst) en DDoS (Verspreid Ontkenning van de Dienst) aanvallen zijn beide kwaadwillige pogingen om het normale functioneren van een gerichte server, de dienst, of het netwerk te verstoren, die het ontoegankelijk maken voor zijn voorgenomen gebruikers maken.
- De aanvallen van Dos komen typisch uit één enkele bron voort, terwijl de aanvallen DDoS uit veelvoudige bronnen komen.
- De aanvallen van DDoS zijn vaak groter in schaal in vergelijking met de aanvallen van Dos toe te schrijven aan de gecombineerde middelen van veelvoudige aanvallen apparaten.
- Deze aanvallen worden uitgevoerd door het doel met bovenmatig verkeer te overstromen, en kwetsbaarheid in netwerkprotocollen te exploiteren.

In de volgende tabel wordt beschreven hoe u DoS- en DDoS-aanvallen voorkomt en beperkt:

<table>
    <tbody>
        <tr>
            <td><strong>Preventiemechanisme</strong></td>
            <td><strong>Beschrijving</strong></td>
            <td><strong>AEM as a Cloud Service</strong></td>
            <td><strong>AEM 6.5 (AMS)</strong></td>
            <td><strong>AEM 6.5 (on-prem)</strong></td>
        </tr>
        <tr>
            <td>Web Application Firewall (WAF)</td>
            <td>Een veiligheidsoplossing die wordt ontworpen om Webtoepassingen tegen diverse soorten aanvallen te beschermen.</td>
            <td>
            <a href="https://experienceleague.adobe.com/nl/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis#waf-rules" target="_blank"> WAF-DDoS de vergunning van de Bescherming </a></td>
            <td><a href="https://docs.aws.amazon.com/waf/" target="_blank"> AWS </a> of <a href="https://azure.microsoft.com/en-us/products/web-application-firewall" target="_blank"> Azure </a> WAF via contract van AMS.</td>
            <td>WAF van uw voorkeur</td>
        </tr>
        <tr>
            <td>ModSecurity</td>
            <td>ModSecurity (ook bekend als de Apache-module 'mod_security') is een open-source, platformonafhankelijke oplossing die bescherming biedt tegen een reeks aanvallen op webtoepassingen.<br/> In AEM as a Cloud Service is dit alleen van toepassing op de AEM Publish-service, omdat er geen Apache-webserver en AEM Dispatcher aanwezig zijn vóór de AEM Author-service.</td>
            <td colspan="3"><a href="https://experienceleague.adobe.com/nl/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection" target="_blank">ModSecurity inschakelen </a></td>
        </tr>
        <tr>
            <td>Verkeersfilterregels</td>
            <td>De filterregels van het verkeer kunnen worden gebruikt om verzoeken bij de laag te blokkeren of toe te staan CDN.</td>
            <td><a href="https://experienceleague.adobe.com/nl/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis" target="_blank">Voorbeelden van filterregels voor het verkeer</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html" target="_blank"> AWS </a> of <a href="https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/rate-limiting-overview" target="_blank"> Azure </a> regel beperkende eigenschappen.</td>
            <td>Uw voorkeursoplossing</td>
        </tr>
    </tbody>
</table>

## Analyse na incidenten en voortdurende verbetering

Terwijl er geen one-size-past-al standaardstroom voor het identificeren van en het verhinderen van aanvallen DoS/DDoS is en het van het veiligheidsproces van uw organisatie afhangt. De **post-inherente analyse en de ononderbroken verbetering** is een cruciale stap in het proces. Hier volgen enkele aanbevolen procedures:

- Identificeer de worteloorzaak van de aanval DoS/DDoS door een post-inherente analyse, met inbegrip van het herzien van logboeken, netwerkverkeer, en systeemconfiguraties uit te voeren.
- Verbeteren van de preventiemechanismen op basis van de bevindingen van de analyse na het incident.

