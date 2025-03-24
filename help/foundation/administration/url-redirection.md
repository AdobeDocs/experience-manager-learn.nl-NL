---
title: URL-omleidingen
description: Meer informatie over de verschillende opties voor het uitvoeren van URL-omleiding in AEM.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
jira: KT-11466
last-substantial-update: 2024-10-22T00:00:00Z
index: y
doc-type: Article
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
duration: 164
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '961'
ht-degree: 0%

---

# URL-omleidingen

URL omleiden is een algemeen aspect als onderdeel van websitebewerking. Architecten en beheerders worden uitgedaagd om de beste oplossing te vinden op hoe en waar te om de omleiding te beheren URL die flexibiliteit en snelle omleidingstijd verstrekt.

Zorg ervoor u met [ AEM (6.x) alias de Klassieke infrastructuur van AEM ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/dispatcher-tutorial/chapter-2) en [ AEM as a Cloud Service ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/architecture) vertrouwd bent. De belangrijkste verschillen zijn:

1. AEM as a Cloud Service heeft [ ingebouwd CDN ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn), echter, kunnen de klanten een CDN (BYOCDN) vóór AEM-Beheerde CDN verstrekken.
1. AEM 6.x of on-premise of Adobe Managed Services (AMS) geen door AEM beheerde CDN omvat, en de klanten moeten hun eigen brengen.

De overige AEM-services (AEM Author/Publish en Dispatcher) zijn overigens conceptueel vergelijkbaar tussen AEM 6.x en AEM as a Cloud Service.

AEM-oplossingen voor URL-omleiding zijn als volgt:

|                                                   | Beheerd en geïmplementeerd als AEM-projectcode | Capaciteit om door marketing/inhoudsteam te veranderen | Compatibel met AEM | Waar uitvoering in omleiding plaatsvindt |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [ bij Edge via AEM-geleide CDN ](#at-edge-via-aem-managed-cdn) | ✔ | ✘ | ✔ | Edge/CDN (ingebouwde) |
| [ bij Edge via breng uw eigen CDN (BYOCDN) ](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN (BYOCDN) |
| [ Apache `mod_rewrite` regels als Dispatcher config ](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ ACS Commons - richt de Manager van de Kaart ](#redirect-map-manager) | ✘ | ✔ | ✔ | Dispatcher |
| [ ACS Commons - richt Manager ](#redirect-manager) opnieuw | ✘ | ✔ | ✔ | AEM/Dispatcher |
| [ het `Redirect` paginabezit ](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## Oplossingsopties

Hieronder volgen opties voor de oplossing in de volgorde waarin deze dichter bij de browser van de websitebezoeker staan.

### Bij Edge via door AEM beheerde CDN {#at-edge-via-aem-managed-cdn}

Deze optie is alleen beschikbaar voor AEM as a Cloud Service-klanten.

[ AEM-Beheerde CDN ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn) verstrekt een omleidingsoplossing op het niveau van Edge waarbij ronde reizen aan de oorsprong worden verminderd. De [ cliënt-kant richt ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors) eigenschap opnieuw richt staat u toe om de omleidingsregels in de het projectcode van AEM te vormen en op te stellen gebruikend de [ Pijpleiding Config ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager). De grootte van het CDN-configuratiebestand (`cdn.yaml`) mag niet groter zijn dan 100KB.

Het beheren van omleidingen op Edge- of CDN-niveau heeft prestatievoordelen.

### Bij Edge via je eigen CDN

Sommige CDN-services bieden omleidingsoplossingen op Edge-niveau, waardoor ronde reizen tot de oorsprong worden gereduceerd. Zie [ Akamai Edge Redirector ](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [ de Functies van AWS CloudFront ](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Neem contact op met uw CDN-serviceprovider voor een omleidingsfunctie op Edge-niveau.

Het beheren van omleidingen op Edge- of CDN-niveau heeft prestatievoordelen, maar deze worden niet beheerd als onderdeel van AEM, maar als afzonderlijke projecten. Een goed gedefinieerd proces om omleidingsregels te beheren en in te voeren is van cruciaal belang om problemen te voorkomen.


### Apache-module `mod_rewrite`

Een gemeenschappelijke oplossing gebruikt [ Apache Module mod_rewrite ](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). Het [ Archetype van het Project van AEM ](https://github.com/adobe/aem-project-archetype) verstrekt een het projectstructuur van Dispatcher voor zowel [ AEM 6.x ](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) als [ AEM as a Cloud Service ](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) project. De standaardinstellingen (onveranderlijk) en de aangepaste herschrijfregels worden gedefinieerd in de map `conf.d/rewrites` en het herschrijfprogramma wordt ingeschakeld voor `virtualhosts` die poort `80` via `conf.d/dispatcher_vhost.conf` -bestand aanroept. Een voorbeeldimplementatie is beschikbaar in het [ Project van de Plaatsen van AEM WKND ](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

In AEM as a Cloud Service, worden deze omleidingsregels beheerd als deel van de code van AEM en via de Cloud Manager [ de Rij config van de Rij van het Web pijpleiding ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines) of [ volledig-stapelpijpleiding ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines) opgesteld. Zo, is uw AEM project-specifiek proces aan spel, om, de omleidingsregels te beheren op te stellen en te traceren.

De meeste CDN-services plaatsen de HTTP 301- en 302-omleidingen wel in het cachegeheugen op, afhankelijk van de `Cache-Control` - of `Expires` -headers. Het helpt te voorkomen dat de retourvlucht na de eerste omleiding van Apache/Dispatcher plaatsvindt.


### ACS AEM Commons

Er zijn twee eigenschappen beschikbaar binnen [ ACS AEM Commons ](https://adobe-consulting-services.github.io/acs-aem-commons/) om URL te leiden richt. Opmerking: ACS AEM Commons is een door de gemeenschap beheerd, open-source project dat niet door Adobe wordt ondersteund.

#### Omleiden Map Manager

[ Redirect de Manager van de Kaart ](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) helpt de beheerders van AEM om [ Apache te handhaven en te publiceren RewriteMap ](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) dossiers zonder tot de server van het Web van Apache direct toegang te hebben of een Apache de servernieuw begin van het Web te vereisen. Met deze functie kunnen gebruikers machtigingen maken, bijwerken en omleidingsregels verwijderen via een console in AEM, zonder hulp van het ontwikkelingsteam of een AEM-implementatie. Redirect de Manager van de Kaart is zowel **AEM as a Cloud Service** (zie [ Pijpleiding-vrije URL richt ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) strategie en verwant [ leerprogramma ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/administration/implementing-pipeline-free-url-redirects#acs-commons---redirect-map-manager)) en **AEM 6.x** compatibel.

#### Omleidingsbeheer

[ Redirect Manager ](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) staat de gebruikers in AEM toe om omleidingen van AEM gemakkelijk te handhaven en te publiceren. De implementatie is gebaseerd op het Java™ servlet-filter, wat een typisch JVM-bronnengebruik is. Met deze functie wordt ook de afhankelijkheid van het AEM-ontwikkelingsteam en de AEM-implementaties opgeheven. Redirect Manager is zowel **AEM as a Cloud Service** als **AEM 6.x** compatibel. Terwijl het aanvankelijke opnieuw gerichte verzoek de publicatieservice van AEM moet raken om 301/302 (meeste) CDNs geheime voorgeheugen 301/302 door gebrek te produceren, toestaand verdere verzoeken om bij edge/CDN worden opnieuw gericht.

[ Redirect Manager ](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) steunt ook [ Pijpleiding-vrije URL richt ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) strategie voor **AEM as a Cloud Service** door [ het compileren richt zich in een tekstdossier ](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/subpages/rewritemap.html) voor [ Apache RewriteMap ](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html), zodat staat het voor het bijwerken van redirects toe die in de server van het Web Apache worden gebruikt zonder het direct tot toegang te hebben of zijn nieuw begin te vereisen. Verwijs naar het [ leerprogramma ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/administration/implementing-pipeline-free-url-redirects#acs-commons---redirect-manager) voor meer details. In dit scenario raakt de eerste omleidingsaanvraag de Apache-webserver en niet de AEM-publicatieservice.

### De eigenschap `Redirect` page

Het uit-van-de-doos (OOTB) `Redirect` paginabezit van het [ Geavanceerde lusje ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties.html) staat inhoudsauteurs toe om de omleidingsplaats voor de huidige pagina te bepalen. Deze oplossing is het meest geschikt voor omleidingsscenario&#39;s per pagina en heeft geen centrale locatie om de pagina-omleidingen weer te geven en te beheren.

## Welke oplossing is geschikt voor de implementatie

Hieronder staan enkele criteria om de juiste oplossing te bepalen. Ook, zou het de IT en Marketing proces van uw organisatie moeten helpen om de juiste oplossing te kiezen.

1. Het marketingteam of supergebruikers in staat stellen omleidingsregels te beheren zonder het AEM-ontwikkelingsteam en de AEM-implementaties.
1. Het proces om, de veranderingen of risicobeperking te beheren te verifiëren, te volgen en terug te keren.
1. Beschikbaarheid van _Expertise van de Onderwerp_ voor **bij Edge via CDN de Dienst** oplossing.
