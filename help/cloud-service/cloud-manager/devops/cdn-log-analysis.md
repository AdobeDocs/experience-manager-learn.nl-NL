---
title: Gereedschap voor CDN-loganalyse
description: Meer informatie over de CDN-logboekanalyse van de AEM Cloud Service die Adobe biedt en over de manier waarop u inzicht krijgt in zowel de CDN-prestaties als de AEM-implementatie.
version: Experience Manager as a Cloud Service
feature: Developer Tools
topic: Development
role: Developer, Admin
level: Beginner
doc-type: Tutorial
duration: 219
last-substantial-update: 2024-05-17T00:00:00Z
jira: KT-15505
thumbnail: KT-15505.jpeg
exl-id: 830c2486-099b-454f-bc07-6bf36e81ac8d
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# Gereedschap voor CDN-loganalyse

Leer over de _Tooling van de Analyse van het Logboek van de Dienst CDN van de Wolk AEM Cloud die Adobe verstrekt en hoe het helpt om inzicht in zowel uw CDN prestaties als implementatie van AEM te krijgen.
 _
>[!VIDEO](https://video.tv.adobe.com/v/3429177?quality=12&learn=on)

## Overzicht

De [ Tooling van de Analyse van het Logboek van AEM as a Cloud Service CDN ](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) biedt pre-gebouwde dashboards aan die u met [ Splunk ](https://www.splunk.com/en_us/products/observability-cloud.html) of de [ ELK stapel ](https://www.elastic.co/elastic-stack) voor controle en analyse in real time van uw CDN logboeken kunt integreren.

Met dit gereedschap kunt u real-time controle en proactieve probleemdetectie uitvoeren. Aldus, het verzekeren van geoptimaliseerde inhoudslevering en juiste veiligheidsmaatregelen tegen Ontkenning van de (Dos) en Verdeelde Ontkenning van de (DDoS) aanvallen van de Dienst.

## Belangrijkste kenmerken

- Gestroomlijnde analyse van logbestanden
- Real-time controle
- Naadloze integratie
- Dashboards voor
   - Identificeer potentiële veiligheidsbedreigingen
   - Snellere ervaring voor eindgebruikers

## Overzicht van dashboard

Om de logboekanalyse snel te beginnen, verstrekt Adobe prebuilt dashboards voor zowel Splunk als stapel ELK.

- **CDN de Verhouding van het Actief van het Geheime voorgeheugen CDN**: verstrekt inzichten in de totale verhouding van de geheim voorgeheugenslag en het totale aantal verzoeken door HIT, PASS, en status MISS. Het verstrekt ook hoogste HIT, PASS, en MISS URLs.

  ![ CDN de Verhouding van de Actief van het Geheime voorgeheugen ](assets/CHR-dashboard.png)

- **Dashboard van het Verkeer CDN**: verstrekt inzichten in het verkeer via CDN en het verzoektarief van de Oorsprong, 4xx en 5xx foutentarieven, en niet in cache geplaatste verzoeken. Het verstrekt ook maximum CND en de verzoeken van de Oorsprong per seconde per cliëntIP adres en meer inzichten om de configuraties te optimaliseren CDN.

  ![ Dashboard van het Verkeer CDN ](assets/Traffic-dashboard.png)

- **WAF Dashboard**: verstrekt inzichten via geanalyseerde, gemarkeerde, en geblokkeerde verzoeken. Het verstrekt ook hoogste aanvallen door identiteitskaart van de Vlag van WAF, hoogste 100 aanvallers door cliënt IP, land, en gebruikersagent en meer inzichten om de configuraties van WAF te optimaliseren.

  ![ WAF Dashboard ](assets/WAF-Dashboard.png)

## Splunk-integratie

Voor organisaties die [ Splunk ](https://www.splunk.com/en_us/products/observability-cloud.html) leveraging en die het logboek van AEMCS door:sturen aan hun instanties van de Splunk hebben toegelaten kunnen prebuilt dashboards snel invoeren. Deze opstelling vergemakkelijkt versnelde logboekanalyse, die actionable inzichten verstrekt om de implementaties van AEM te optimaliseren en veiligheidsbedreigingen zoals DOS aanvallen te verlichten.

U kunt begonnen worden gebruikend [ Splunk dashboards voor de Analyse van het Logboek AEMCS CDN ](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis) gids.


## ELK-integratie

De [ stapel ELK ](https://www.elastic.co/elastic-stack), die Elasticsearch, Logstash, en Kibana omvat, is een andere krachtige optie voor logboekanalyse. Het is nuttig voor organisaties die geen toegang tot een opstelling van de Splunk of logboek het door:sturen mogelijkheden hebben. Het lokaal instellen van de ELK-stapel is eenvoudig, dankzij de gereedschapset kunt u snel aan de slag met het bestand Docker Compose. Vervolgens kunt u de vooraf gebouwde dashboards importeren en de CDN-logboeken invoeren die worden gedownload met de Adobe Cloud Manager.

U kunt begonnen worden gebruikend de [ ELK container van het Dok voor de Analyse van het Logboek AEMCS CDN ](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#elk-docker-container-for-aemcs-cdn-log-analysis) gids.
