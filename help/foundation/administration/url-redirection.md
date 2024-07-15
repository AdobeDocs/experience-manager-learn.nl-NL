---
title: URL-omleidingen
description: Leer meer over de verschillende opties om URL omleiding in AEM uit te voeren.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
jira: KT-11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
doc-type: Article
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
duration: 164
source-git-commit: 3cc9b4fa0a30d36638a8c28a73663ffa455ba4a3
workflow-type: tm+mt
source-wordcount: '877'
ht-degree: 0%

---

# URL-omleidingen

URL omleiden is een algemeen aspect als onderdeel van websitebewerking. Architecten en beheerders worden uitgedaagd om de beste oplossing te vinden op hoe en waar te om de omleiding te beheren URL die flexibiliteit en snelle omleidingstijd verstrekt.

Zorg ervoor u met [ AEM (6.x) alias AEM Klassieke ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/dispatcher-tutorial/chapter-2) en [ AEM as a Cloud Service ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/architecture) infrastructuur vertrouwd bent. De belangrijkste verschillen zijn:

1. AEM as a Cloud Service heeft [ ingebouwde CDN ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn), echter, kunnen de klanten een CDN (BYOCDN) vóór AEM-beheerde CDN verstrekken.
1. AEM 6.x of op-gebouw of Adobe Managed Services (AMS) geen AEM-beheerde CDN omvat, en de klanten moeten hun brengen.

De andere AEM diensten (AEM Auteur/Publish, en Dispatcher) zijn anders conceptueel vergelijkbaar tussen AEM 6.x en AEM as a Cloud Service.

AEM URL-omleidingsoplossingen zijn als volgt:

|                                                   | Beheerd en geïmplementeerd als AEM projectcode | Capaciteit om door marketing/inhoudsteam te veranderen | AEM als compatibel met Cloud Service | Waar uitvoering in omleiding plaatsvindt |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [ bij Edge via AEM-geleide CDN ](#at-edge-via-aem-managed-cdn) | ✔ | ✘ | ✔ | Edge/CDN (ingebouwde) |
| [ bij Edge via breng uw eigen CDN (BYOCDN) ](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN (BYOCDN) |
| [ Apache `mod_rewrite` regels als Dispatcher config ](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ ACS Commons - richt de Manager van de Kaart ](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ ACS Commons - richt Manager ](#redirect-manager) opnieuw | ✘ | ✔ | ✔ | AEM |
| [ het `Redirect` paginabezit ](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## Oplossingsopties

Hieronder volgen opties voor de oplossing in de volgorde waarin deze dichter bij de browser van de websitebezoeker staan.

### Bij Edge via door AEM beheerde CDN {#at-edge-via-aem-managed-cdn}

Deze optie is alleen beschikbaar voor AEM as a Cloud Service-klanten.

[ AEM-geleide CDN ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn) verstrekt een omleidingsoplossing op het niveau van Edge waarbij ronde reizen aan de oorsprong worden verminderd. De [ cliënt-kant richt ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors) eigenschap opnieuw richt staat u toe om de omleidingsregels in de AEM projectcode te vormen en op te stellen gebruikend de [ Pijpleiding Config ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager). De grootte van het CDN-configuratiebestand (`cdn.yaml`) mag niet groter zijn dan 100KB.

Het beheren van omleidingen op Edge- of CDN-niveau heeft prestatievoordelen.

### Bij Edge via je eigen CDN

Sommige CDN-services bieden omleidingsoplossingen op Edge-niveau, waardoor ronde reizen tot de oorsprong worden gereduceerd. Zie [ Akamai Edge Redirector ](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [ de Functies van AWS CloudFront ](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Neem contact op met uw CDN-serviceprovider voor een omleidingsfunctie op Edge-niveau.

Het beheren van omleidingen op Edge- of CDN-niveau heeft prestatievoordelen, maar deze worden niet als onderdeel van AEM beheerd, maar eerder als afzonderlijke projecten. Een goed gedefinieerd proces om omleidingsregels te beheren en in te voeren is van cruciaal belang om problemen te voorkomen.


### Apache-module `mod_rewrite`

Een gemeenschappelijke oplossing gebruikt [ Apache Module mod_rewrite ](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). Het [ AEM Archetype van het Project ](https://github.com/adobe/aem-project-archetype) verstrekt een het projectstructuur van Dispatcher voor zowel [ AEM 6.x ](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) en [ AEM as a Cloud Service ](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) project. De standaardinstellingen (onveranderlijk) en de aangepaste herschrijfregels worden gedefinieerd in de map `conf.d/rewrites` en het herschrijfprogramma wordt ingeschakeld voor `virtualhosts` die poort `80` via `conf.d/dispatcher_vhost.conf` -bestand aanroept. Een voorbeeldimplementatie is beschikbaar in het [ AEM Project van Plaatsen WKND ](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

In AEM as a Cloud Service, worden deze omleidingsregels beheerd als deel van AEM code en opgesteld via de Cloud Manager [ de Rij config van het Web pijpleiding ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines) of [ volledig-stapelpijpleiding ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines). Aldus, is uw AEM project-specifiek proces in spel om, de omleidingsregels te beheren op te stellen en te vinden.

De meeste CDN-services plaatsen de HTTP 301- en 302-omleidingen wel in het cachegeheugen op, afhankelijk van de `Cache-Control` - of `Expires` -headers. Het helpt te voorkomen dat de retourvlucht na de eerste omleiding van Apache/Dispatcher plaatsvindt.


### ACS AEM Commons

Er zijn twee eigenschappen beschikbaar binnen [ ACS AEM Commons ](https://adobe-consulting-services.github.io/acs-aem-commons/) om URL te leiden richt. Opmerking: ACS AEM Commons is een door de gemeenschap beheerd, open-source project dat niet door Adobe wordt ondersteund.

#### Omleiden Map Manager

[ Redirect de Manager van de Kaart ](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) hulp AEM 6.x beheerders om [ Apache te handhaven en te publiceren RewriteMap ](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) dossiers zonder de server van het Web van Apache direct tot toegang te hebben of een Apache de servernieuw begin van het Web te vereisen. Deze eigenschap staat toestemmingengebruikers toe om, omleidingsregels van een console in AEM tot stand te brengen bij te werken en te schrappen, zonder de hulp van het ontwikkelingsteam of een AEM plaatsing. Redirect de Manager van de Kaart is **NIET compatibel met AEM as a Cloud Service**.

#### Omleidingsbeheer

[ Redirect Manager ](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) staat de gebruikers in AEM toe om redirects van AEM gemakkelijk te handhaven en te publiceren. De implementatie is gebaseerd op het Java™ servlet-filter, wat een typisch JVM-bronnengebruik is. Deze eigenschap elimineert ook de afhankelijkheid van het AEM ontwikkelingsteam en de AEM plaatsingen. Redirect Manager is zowel **AEM as a Cloud Service** als **AEM 6.x** compatibel. Terwijl het aanvankelijke opnieuw gerichte verzoek de dienst van AEMPublish moet raken om 301/302 (de meeste) geheime voorgeheugen 301/302 van CDNs door gebrek te produceren, toestaand verdere verzoeken om bij edge/CDN worden opnieuw gericht.

### De eigenschap `Redirect` page

Het uit-van-de-doos (OOTB) `Redirect` paginabezit van het [ Geavanceerde lusje ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties.html) staat inhoudsauteurs toe om de omleidingsplaats voor de huidige pagina te bepalen. Deze oplossing is het meest geschikt voor omleidingsscenario&#39;s per pagina en heeft geen centrale locatie om de pagina-omleidingen weer te geven en te beheren.

## Welke oplossing is geschikt voor de implementatie

Hieronder staan enkele criteria om de juiste oplossing te bepalen. Ook, zou het de IT en Marketing proces van uw organisatie moeten helpen om de juiste oplossing te kiezen.

1. Toelatend het marketing team of super gebruikers om omleidingsregels zonder het AEM ontwikkelingsteam en de AEM plaatsingen te beheren.
1. Het proces om, de veranderingen of risicobeperking te beheren te verifiëren, te volgen en terug te keren.
1. Beschikbaarheid van _Expertise van de Onderwerp_ voor **bij Edge via CDN de Dienst** oplossing.
