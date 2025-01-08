---
title: Overzicht van AEM API's
description: Meer informatie over de verschillende typen API's in Adobe Experience Manager (AEM) en een overzicht van op OpenAPI-specificaties gebaseerde API's, beter bekend als op OpenAPI gebaseerde AEM.
version: Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2024-11-20T00:00:00Z
duration: 0
exl-id: 23b2be0d-a8d4-4521-96ba-78b70f4e9cba
source-git-commit: d5745a17af6b72b1871925dd7c50cbbb152012fe
workflow-type: tm+mt
source-wordcount: '1024'
ht-degree: 0%

---

# Overzicht van AEM API&#39;s{#aem-apis-overview}

Leer over de verschillende types van APIs in Adobe Experience Manager (AEM) as a Cloud Service en krijg een overzicht van [ OpenAPI Specificatie (OAS) ](https://swagger.io/specification/) gebaseerde AEM APIs, algemeen gekend als OpenAPI-Gebaseerde AEM APIs.

AEM as a Cloud Service biedt een groot aantal API&#39;s voor het maken, lezen, bijwerken en verwijderen van inhoud, elementen en formulieren. Met deze API&#39;s kunnen ontwikkelaars aangepaste toepassingen maken die met AEM werken.

Laten we de verschillende typen API&#39;s in AEM onderzoeken en de belangrijkste concepten begrijpen voor het benaderen van Adobe-API&#39;s.

## Typen AEM API&#39;s{#types-of-aem-apis}

AEM biedt zowel verouderde als moderne API&#39;s voor interactie met de auteur en het publiceren van servicetypen.

- **Verouderde APIs**: Ingebracht in vroegere AEM versies, erfenis APIs wordt nog gesteund voor achterwaartse verenigbaarheid.

- **Moderne APIs**: Gebaseerd op REST, de Specificatie OpenAPI, volgen deze APIs huidige API ontwerpopbest praktijken en worden geadviseerd voor nieuwe integratie.


| AEM API-type | Specificaties | Beschikbaarheid | Hoofdletters gebruiken | Voorbeeld |
| --- | --- | --- | --- | --- |
| Traditionele (niet-RESTful) API&#39;s | Sling Servlets | AEM 6.X, AEM as a Cloud Service | Verouderde integratie, achterwaartse compatibiliteit | [ de Bouwer API van de Vraag ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api) en anderen |
| RESTful-API&#39;s | HTTP, JSON | AEM 6.X, AEM as a Cloud Service | CRUD-bewerkingen, moderne toepassingen | [ HTTP API van Assets ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), [ het REST API van het Werkschema ](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api), [ Exporter JSON voor de Diensten van de Inhoud ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter) en anderen |
| GraphQL API&#39;s | GraphQL | AEM 6.X, AEM as a Cloud Service | CMS, SPA, mobiele apps zonder koptelefoon | [ GraphQL API ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| Op OpenAPI gebaseerde AEM API&#39;s | REST, OpenAPI | **slechts AEM as a Cloud Service** | API-eerste ontwikkeling, moderne toepassingen | [ de Auteur API van Assets ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/), [ Omslagen API ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/), [ AEM Sites API ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/), [ Forms Acrobat Services ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) en anderen |

>[!IMPORTANT]
>
>De op OpenAPI gebaseerde AEM-API&#39;s zijn alleen beschikbaar in AEM as a Cloud Service en zijn niet compatibel met AEM 6.X.

Voor meer details over AEM APIs, zie [ Adobe Experience Manager as a Cloud Service APIs ](https://developer.adobe.com/experience-cloud/experience-manager-apis/).

Laten we eens nader kijken naar de op OpenAPI gebaseerde AEM API&#39;s en de belangrijke concepten voor toegang tot Adobe API&#39;s.

## Op OpenAPI gebaseerde AEM API&#39;s{#openapi-based-aem-apis}

>[!AVAILABILITY]
>
>API&#39;s die zijn gebaseerd op OpenAPI zijn beschikbaar als onderdeel van een programma voor vroege toegang. Als u in de toegang tot van hen geinteresseerd bent, moedigen wij u aan om [ aem-apis@adobe.com ](mailto:aem-apis@adobe.com) met een beschrijving van uw gebruiksgeval te e-mailen.

De [ Specificatie OpenAPI ](https://swagger.io/specification/) (die vroeger als Swagger wordt bekend) is een wijd gebruikte norm voor het bepalen van RESTful APIs. AEM as a Cloud Service biedt verschillende API&#39;s die zijn gebaseerd op OpenAPI-specificaties (of gewoon op OpenAPI gebaseerde AEM API&#39;s), waardoor het eenvoudiger wordt om aangepaste toepassingen te maken die interageren met AEM auteur- of publicatieservice. Hieronder volgen enkele voorbeelden:

**Plaatsen**

- [ Plaatsen API ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/): APIs voor het werken met de Fragmenten van de Inhoud.

**Assets**

- [ Mappen API ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/): APIs voor het werken met omslagen zoals creeer, lijst en schrap omslagen.

- [ de Auteur API van Assets ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/): APIs voor het werken met activa en zijn meta-gegevens.

**Forms**

- [ Communicatie APIs van Forms ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/): APIs voor het werken met vormen en documenten.

In toekomstige versies worden meer op OpenAPI gebaseerde AEM-API&#39;s toegevoegd ter ondersteuning van extra gebruiksgevallen.

## Verificatieondersteuning{#authentication-support}

De op OpenAPI gebaseerde AEM API&#39;s ondersteunen de volgende verificatiemethoden:

- **OAuth Server-aan-Server credential**: Ideaal voor backend diensten die API toegang zonder gebruikersinteractie vereisen. Het gebruikt _client_credentials_ giftype, toelatend veilig toegangsbeheer op het serverniveau. Voor meer informatie, zie [ Server-aan-Server referentie ](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential).

- **OAuth App credential van het Web**: Geschikt voor Webtoepassingen met front-end en _achterste_ componenten die tot APIs namens gebruikers toegang hebben. Het gebruikt het _authentication_code_ subsidietype, waar de backendserver veilig geheimen en tokens beheert. Voor meer informatie, zie {de referentie van de App van 0} OAuth Web ](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential).[

- **OAuth de Enige referentie van de App van de Pagina**: Ontworpen voor SPA die in browser lopen, die tot APIs namens een gebruiker zonder een achtergrondserver moet toegang hebben. Het gebruikt _authentication_code_ verlenen type en baseert zich op cliënt-zijveiligheidsmechanismen gebruikend PKCE (Sleutel van het Bewijs voor de Uitwisseling van de Code) om de stroom van de vergunningscode te beveiligen. Voor meer informatie, zie [ OAuth Enige de credentie van de Pagina App ](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential).

### Verschil tussen OAuth Server-aan-Server en OAuth Web App/Single Page App geloofsbrieven{#difference-between-oauth-server-to-server-and-oauth-web-app-single-page-app-credentials}

| | OAuth server-aan-server | OAuth-gebruikersverificatie (web-app) |
| --- | --- | --- |
| Verificatiedoel | Ontworpen voor machine-aan-machine interactie. | Ontworpen voor gebruikersgestuurde interacties. |
| Gedrag token | Geeft toegangstokens uit die de cliënttoepassing zelf vertegenwoordigen. | Geeft toegangstokens uit namens een geverifieerde gebruiker. |
| Gevallen gebruiken | Ondersteuningsservices die API-toegang zonder gebruikersinteractie nodig hebben. | Webtoepassingen met front-end en backendcomponenten die API&#39;s benaderen namens gebruikers. |
| Beveiligingsoverwegingen | Sla gevoelige gegevens (`client_id`, `client_secret` ) veilig op in back-endsystemen. | De gebruiker verklaart voor authentiek en wordt verleend hun eigen tijdelijk toegangstoken. Sla gevoelige gegevens (`client_id`, `client_secret` ) veilig op in back-endsystemen. |
| Type subsidie | _client_credentials_ | _authentication_code_ |

## Toegang tot Adobe-API&#39;s en verwante concepten{#accessing-adobe-apis-and-related-concepts}

Voordat u Adobe-API&#39;s opent, is het van essentieel belang dat u deze belangrijke concepten begrijpt:

- **[Adobe Developer Console ](https://developer.adobe.com/)**: De ontwikkelaarshub voor de toegang tot van Adobe APIs, SDKs, gebeurtenissen in real time, serverless functies, en meer. Merk op dat het van _AEM_ Developer Console verschillend is, die voor het zuiveren AEM toepassingen wordt gebruikt.

- **[Project van Adobe Developer Console ](https://developer.adobe.com/developer-console/docs/guides/projects/)**: Centrale plaats voor het beheren van API integratie, gebeurtenissen, en runtime functies. Hier, vormt u APIs, plaatst authentificatie, en produceert vereiste geloofsbrieven.

- **[Profielen van het Product ](https://helpx.adobe.com/enterprise/using/manage-product-profiles.html)**: De Profielen van het product verstrekken een toestemmingsvooraf ingesteld die u toestaat om gebruiker of toepassingstoegang tot de producten van de Adobe zoals AEM, Adobe Target, Adobe Analytics, en anderen te controleren. Elk product van de Adobe heeft vooraf bepaalde productprofielen verbonden aan het.

- **de Diensten**: De diensten bepalen de daadwerkelijke toestemmingen en worden geassocieerd met het Profiel van het Product. Als u de voorinstelling voor machtigingen wilt beperken of vergroten, kunt u de services die aan het productprofiel zijn gekoppeld, deselecteren of selecteren. Zo kunt u het toegangsniveau voor het product en de bijbehorende API&#39;s bepalen. In AEM as a Cloud Service, vertegenwoordigen de diensten gebruikersgroepen met vooraf bepaalde Lijsten van het Toegangsbeheer (ACLs) voor bewaargegevensopslagknopen, die korrelig toestemmingsbeheer toestaan.

## Volgende stappen{#next-steps}

Met inzicht in de verschillende AEM API-typen, waaronder
AEM API&#39;s die zijn gebaseerd op OpenAPI&#39;s en de belangrijkste concepten voor toegang tot Adobe-API&#39;s zijn nu klaar om aangepaste toepassingen te maken die met AEM werken.

Laten we beginnen met:

- [ roept op OpenAPI-Gebaseerde AEM APIs voor server aan serverauthentificatie ](invoke-openapi-based-aem-apis.md) leerprogramma aan, dat aantoont hoe te om tot op OpenAPI-Gebaseerde AEM toegang te hebben _gebruikend OAuth Server-aan-Server geloofsbrieven_.
- [ roept op OpenAPI-Gebaseerde AEM APIs met gebruikersauthentificatie van een Web app ](invoke-openapi-based-aem-apis-from-web-app.md) leerprogramma aan, dat aantoont hoe te om tot op OpenAPI-Gebaseerde AEM APIs van a _Webtoepassing toegang te hebben gebruikend de geloofsbrieven van de Toepassing van het Web OAuth_.
