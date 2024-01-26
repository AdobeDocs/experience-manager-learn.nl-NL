---
title: Websites beschermen met verkeersfilterregels (inclusief WAF-regels)
description: Leer over de regels van de Filter van het Verkeer, met inbegrip van zijn subcategorie van de regels van de Firewall van de Toepassing van het Web (WAF). Hoe te om, de regels tot stand te brengen op te stellen en te testen. Analyseer ook de resultaten om uw AEM te beschermen.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
duration: 177
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Websites beschermen met verkeersfilterregels (inclusief WAF-regels)

Meer informatie over **verkeersfilterregels**, inclusief de subcategorie **WAF-regels (Web Application Firewall)** in AEM as a Cloud Service (AEMCS). Lees over hoe te om, de regels tot stand te brengen op te stellen en te testen. Analyseer ook de resultaten om uw AEM te beschermen.

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## Overzicht

Het verminderen van het risico van inbreuken op de beveiliging is een topprioriteit voor elke organisatie. AEMCS biedt de eigenschap van de het verkeersfilterregels, met inbegrip van de regels van WAF, aan om websites en toepassingen te beschermen.

De filterregels van het verkeer worden opgesteld aan [ingebouwde CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html) en worden geëvalueerd voordat het verzoek de AEM infrastructuur bereikt. Met deze functie kunt u de beveiliging van uw website aanzienlijk verbeteren, zodat alleen legitieme verzoeken toegang hebben tot de AEM-infrastructuur.

Deze zelfstudie begeleidt u door het proces om de resultaten van de regels van de verkeersfilter, met inbegrip van de regels van WAF te creëren, op te stellen, te testen en te analyseren.

U kunt meer over de regels van de verkeersfilter in lezen [dit artikel](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en).

>[!IMPORTANT]
>
> Een subcategorie van de regels van het verkeersfilter genoemd &quot;de regels van WAF&quot;vereisen een bescherming WAF-DDoS of de Uitgebreide vergunning van de Veiligheid.

We nodigen u uit feedback te geven of vragen te stellen over regels voor het filter van het verkeer via e-mail **aemcs-waf-adopter@adobe.com**.

## Volgende stap

Meer informatie [instellen](./how-to-setup.md) de eigenschap zodat kunt u, de regels van de verkeersfilter creëren opstellen en testen. Lees meer over het instellen van de **Elasticsearch, Logstash en Kibana (ELK)** stackdashboardwerkset om de resultaten van uw AEMCS CDN-logboeken te analyseren.


