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
duration: 165
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Websites beschermen met verkeersfilterregels (inclusief WAF-regels)

Leer over **regels van de verkeersfilter**, met inbegrip van zijn subcategorie van **de regels van de Firewall van de Toepassing van het Web (WAF)** in AEM as a Cloud Service (AEMCS). Lees over hoe te om, de regels tot stand te brengen op te stellen en te testen. Analyseer ook de resultaten om uw AEM te beschermen.

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## Overzicht

Het verminderen van het risico van inbreuken op de beveiliging is een topprioriteit voor elke organisatie. AEMCS biedt de eigenschap van de het verkeersfilterregels, met inbegrip van de regels van WAF, aan om websites en toepassingen te beschermen.

De filterregels van het verkeer worden opgesteld aan [ ingebouwde CDN ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html) en worden geëvalueerd alvorens het verzoek de AEM infrastructuur bereikt. Met deze functie kunt u de beveiliging van uw website aanzienlijk verbeteren, zodat alleen legitieme verzoeken toegang hebben tot de AEM-infrastructuur.

Deze zelfstudie begeleidt u door het proces om de resultaten van de regels van de verkeersfilter, met inbegrip van de regels van WAF te creëren, op te stellen, te testen en te analyseren.

U kunt meer over de regels van de verkeersfilter in [ dit artikel ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en) lezen.

>[!IMPORTANT]
>
> Een subcategorie van de regels van het verkeersfilter genoemd &quot;de regels van WAF&quot;vereisen een bescherming WAF-DDoS of de Uitgebreide vergunning van de Veiligheid.

Wij nodigen u uit om terugkoppelen of vragen over de regels van de verkeersfilter te stellen door **aemcs-waf-adopter@adobe.com** te e-mailen.

## Volgende stap

Leer [ hoe te opstelling ](./how-to-setup.md) de eigenschap zodat kunt u, de regels van de verkeersfilter creëren opstellen en testen. Lees over vestiging de **Elasticsearch, Logstash, en Kibana (ELK)** stackdashboardtooling om de resultaten van uw Logboeken te analyseren AEMCS CDN.


