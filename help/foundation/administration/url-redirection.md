---
title: URL-omleidingen
description: Leer meer over de verschillende opties om URL omleiding in AEM uit te voeren.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
kt: 11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
source-git-commit: d5645e975aa290392348cc69d078b24921a7d13a
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---


# URL-omleidingen

URL omleiden is een algemeen aspect als onderdeel van websitebewerking. Architecten en beheerders worden uitgedaagd om de beste oplossing te vinden op hoe en waar te om de omleiding te beheren URL die flexibiliteit en snelle omleidingstijd verstrekt.

Zorg ervoor dat u vertrouwd bent met de [AEM 6,x)](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-2.html#the-%E2%80%9Clegacy%E2%80%9D-setup) en [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/architecture.html#runtime-architecture) infrastructuur. De belangrijkste verschillen zijn:

1.  AEM as a Cloud Service heeft [ingebouwde CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html)Maar klanten kunnen een CDN (BYOCDN) vóór een door AEM beheerde CDN opgeven.
1.  AEM 6.x of on-premise of de Beheerde Diensten van Adobe (AMS) geen AEM-beheerde CDN omvat, en de klanten moeten hun brengen.

De andere AEM services (AEM Author/Publish en Dispatcher) zijn anders conceptueel vergelijkbaar tussen AEM 6.x en AEM as a Cloud Service.

AEM URL-omleidingsoplossingen zijn als volgt:

|  | Beheerd en geïmplementeerd als AEM projectcode | Capaciteit om door marketing/inhoudsteam te veranderen | AEM als compatibel met Cloud Service | Waar uitvoering in omleiding plaatsvindt |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [Aan Edge via een introductie van uw eigen CDN](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Rand/CDN |
| [Apache `mod_rewrite` regels als Dispatcher config ](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS-opdrachten - Kaartbeheer omleiden](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ACS-opdrachten - Omleidingsbeheer](#redirect-manager) | ✘ | ✔ | ✔ | AEM |


## Oplossingsopties

Hieronder volgen opties voor de oplossing in de volgorde waarin deze dichter bij de browser van de websitebezoeker staan.

### Aan de rand via een eigen CDN

Sommige CDN-services bieden omleidingsoplossingen op Edge-niveau, waardoor ronde reizen tot de oorsprong worden gereduceerd. Zie [Akamai Edge Redirector](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [AWS CloudFront-functies](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Raadpleeg uw CDN-serviceprovider voor de mogelijkheid voor omleiding op Edge-niveau.

Het beheren van omleidingen op Edge- of CDN-niveau heeft prestatievoordelen, maar deze worden niet als onderdeel van AEM beheerd, maar eerder als afzonderlijke projecten. Een goed doordacht proces om omleidingsregels te beheren en in te voeren is essentieel om problemen te vermijden.


### Apache `mod_rewrite` module

Een gemeenschappelijke oplossing gebruikt [Mod Apache Module_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). De [Projectarchetype AEM](https://github.com/adobe/aem-project-archetype) verstrekt een verzender projectstructuur voor allebei [AEM 6,x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) en [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) projecten. De standaardinstellingen (onveranderlijk) en aangepaste herschrijfregels worden gedefinieerd in het dialoogvenster `conf.d/rewrites` map en de herschrijfengine is ingeschakeld voor `virtualhosts` die luistert naar de poort `80` via `conf.d/dispatcher_vhost.conf` bestand. Een voorbeeldimplementatie is beschikbaar in het dialoogvenster [AEM WKND-siteproject](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

In AEM as a Cloud Service worden deze omleidingsregels beheerd als onderdeel van AEM code en geïmplementeerd via Cloud Manager [WebTier config-pijpleiding](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#web-tier-config-pipelines) of [Volledige pijplijn](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#full-stack-pipeline). Aldus is uw AEM project-specifiek proces in spel om, de omleidingsregels te beheren op te stellen en te vinden.

De meeste CDN-services plaatsen de HTTP 301- en 302-omleidingen wel in het cachegeheugen op, afhankelijk van hun `Cache-Control` of `Expires` kopteksten. Dit helpt te voorkomen dat de retourvlucht na de eerste omleiding van Apache/Dispatcher ontstaat.


### ACS AEM Commons

Er zijn twee functies beschikbaar binnen [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) om URL-omleidingen te beheren. Opmerking: ACS AEM Commons is een door de gemeenschap beheerd, open-source project dat niet door Adobe wordt ondersteund.

#### Omleiden Map Manager

[Omleiden Map Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) staat AEM 6.x beheerders toe om gemakkelijk te handhaven en te publiceren [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) bestanden zonder rechtstreeks toegang te krijgen tot de Apache-webserver of zonder dat de Apache-webserver opnieuw moet worden opgestart. Deze eigenschap staat toestemmingengebruikers toe om, omleidingsregels van een console in AEM tot stand te brengen bij te werken en te schrappen, zonder de hulp van het ontwikkelingsteam of een AEM plaatsing. Omleiden Map Manager is **NIET AEM as a Cloud Service compatibel**.

#### Omleidingsbeheer

[Omleidingsbeheer](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) Hiermee kunnen gebruikers in AEM eenvoudig omleidingen van AEM onderhouden en publiceren. De implementatie is gebaseerd op het Java™ servlet-filter, wat een typisch JVM-bronnengebruik is. Deze eigenschap elimineert ook de afhankelijkheid van het AEM ontwikkelingsteam en de AEM plaatsingen. Redirect Manager is beide **AEM as a Cloud Service** en **AEM 6,x** compatibel. Merk op dat, terwijl het aanvankelijke opnieuw gerichte verzoek zijn dienst van de Publicatie AEM moet zijn om 301/302 te produceren, (de meeste) CDNs geheime voorgeheugen 301/302 door gebrek, toestaand verdere verzoeken om bij edge/CDN worden opnieuw gericht.


## Welke oplossing is geschikt voor de implementatie

Hieronder staan enkele criteria om de juiste oplossing te bepalen. Ook, zou het de IT en Marketing proces van uw organisatie moeten helpen om de juiste oplossing te kiezen.

1. Toelatend het marketing team of super gebruikers om omleidingsregels zonder het AEM ontwikkelingsteam en AEM versie, plaatsingscycli te beheren.
1. Het proces om, de veranderingen of risicovermindering te verifiëren te volgen en terug te keren.
1. Beschikbaarheid van _Expertise in de materie_ for **Bij Edge via CDN-service** oplossing.

