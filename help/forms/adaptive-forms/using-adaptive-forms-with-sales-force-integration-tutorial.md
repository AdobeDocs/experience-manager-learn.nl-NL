---
title: DataSource configureren met Salesforce in AEM Forms 6.3 en 6.4
description: AEM Forms integreren met Salesforce met behulp van formuliergegevensmodel
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7a4fd109-514a-41a8-a3fe-53c1de32cb6d
last-substantial-update: 2020-02-14T00:00:00Z
duration: 175
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---

# DataSource configureren met Salesforce in AEM Forms 6.3 en 6.4{#configuring-datasource-with-salesforce-in-aem-forms-and}

## Vereisten {#prerequisites}

In dit artikel doorlopen we het proces voor het maken van Data Source met Salesforce

Vereisten voor deze zelfstudie:

* Blader naar de onderkant van deze pagina en download het wagerbestand en sla het op de vaste schijf op.
* AEM Forms met SSL ingeschakeld

   * [ Officiële Documentatie voor het toelaten van SSL op AEM 6.3 ](https://helpx.adobe.com/nl/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [ Officiële Documentatie voor het toelaten van SSL op AEM 6.4 ](https://helpx.adobe.com/nl/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Je hebt een Salesforce-account nodig
* U moet een verbonden app maken. De officiële documentatie van Salesforce voor het creëren van app is vermeld [ hier ](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0).
* Geef de juiste OAuth-oppervlakken op voor de app (ik heb alle beschikbare OAuth-oppervlakken geselecteerd voor testdoeleinden)
* Geef de URL voor de callback op. De callback-URL in mijn geval was

   * Als u **AEM Forms 6.3** gebruikt, is callback URL https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html. In dit URL-createlead is de naam van mijn formuliergegevensmodel.

   * Als u **&#x200B; AEM Forms 6.4** gebruikt, is de callback-URL https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html

In dit voorbeeld is gbedekar -w7-1:6443 de naam van mijn server en de poort waarop AEM wordt uitgevoerd.

Zodra u de Verbonden nota van de App **Consumentensleutel en Geheime Sleutel** hebt gecreeerd. U hebt deze nodig bij het maken van de gegevensbron in AEM Forms.

Nu u uw verbonden app hebt gemaakt, moet u een wagerbestand maken voor de bewerkingen die u in koopkracht moet uitvoeren. Een voorbeeldwagerbestand wordt opgenomen als onderdeel van de downloadbare middelen. Met dit kwikbestand kunt u het object Lead maken bij het verzenden van een adaptief formulier. Verken dit wagerbestand.

De volgende stap is het maken van Data Source in AEM Forms. Voer de volgende stappen uit volgens uw AEM Forms-versie

## AEM Forms 6.3 {#aem-forms}

* Aanmelden bij AEM Forms via het HTTPS-protocol
* Navigeer naar de cloudservices door https://&lt;servername>:&lt;serverport> /etc/cloudservices.html, bijvoorbeeld https://gbedekar-w7-1:6443/etc/cloudservices.html
* Blader omlaag naar &quot;Formuliergegevensmodel&quot;.
* Klik op Configuraties tonen.
* Klik op ‘+’ om een nieuwe configuratie toe te voegen
* Selecteer Volledige service opnieuw instellen. Geef de configuratie een betekenisvolle titel en naam. Bijvoorbeeld:

   * Naam: CreateLeadInSalesForce
   * Titel: CreateLeadInSalesForce

* Klik op Maken

**In het volgende scherm &#x200B;**

* Selecteer &quot;Bestand&quot; als optie voor het bronbestand van de computer. Bladeren naar het bestand dat u eerder hebt gedownload
* Verificatietype selecteren als OAuth2.0
* De waarden ClientID en Client Secret opgeven
* OAuth Url is - **https://login.salesforce.com/services/oauth2/authorize**
* Vernieuw Symbolische Url - **https://na5.salesforce.com/services/oauth2/token**
* **Toke URL van de Toegang - https://na5.salesforce.com/services/oauth2/token**
* Reikwijdte vergunning: ** api   chatter_api full id   openhartig   verfrissen_token visuale web*
* Verificatiehandler: houder van autorisatie
* Klik op &quot;Verbinding maken met OAUTH&quot;. Als alles goed gaat, worden geen fouten weergegeven

Nadat u het formuliergegevensmodel hebt gemaakt met Salesforce, kunt u vervolgens de formuliergegevensintegratie maken met de Data Source die u zojuist hebt gemaakt. De officiële documentatie voor het creëren van de Integratie van de Gegevens van de Vorm is [ hier ](https://helpx.adobe.com/nl/aem-forms/6-3/data-integration.html).

Zorg ervoor dat u het formuliergegevensmodel zo configureert dat de POST-service wordt gebruikt om een Lead-object in SFDC te maken.

U zult ook Lees en schrijven de Dienst voor het voorwerp van de Lood moeten vormen. Raadpleeg de schermafbeeldingen onder aan deze pagina.

Nadat u het formuliergegevensmodel hebt gemaakt, kunt u op dit model gebaseerde Adaptive Forms maken en de verzendmethoden van het formuliergegevensmodel gebruiken om lead in SFDC te maken.

## AEM Forms 6.4 {#aem-forms-1}

* Source voor gegevens maken

   * [ ga aan Gegevensbronnen ](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * Klik op de knop Maken
   * Geef enkele betekenisvolle waarden op

      * Naam: CreateLeadInSalesForce
      * Titel: CreateLeadInSalesForce
      * Servicetype: RESTful-service

   * Klik op Volgende
   * Swagger Source: Bestand
   * Blader naar en selecteer het gummetje dat u in de vorige stap hebt gedownload
   * Type verificatie: OAuth 2.0. Geef de volgende waarden op
   * De waarden ClientID en Client Secret opgeven
   * OAuth Url is - **https://login.salesforce.com/services/oauth2/authorize**
   * Vernieuw Symbolische Url - **https://na5.salesforce.com/services/oauth2/token**
   * Token Ur van de toegang **l - https://na5.salesforce.com/services/oauth2/token**
   * Autorisatiebereik: **&#x200B; api chatter_api full id open id refresh_token visualforce web**
   * Verificatiehandler: houder van autorisatie
   * Klik op &quot;Verbinding maken met OAuth&quot;. Als er fouten optreden, controleert u de voorgaande stappen om ervoor te zorgen dat alle gegevens correct zijn ingevoerd.

Nadat u de Data Source met Salesforce hebt gemaakt, kunt u vervolgens de Form Data Integration maken met de Data Source die u zojuist hebt gemaakt. De documentatieverbinding voor dat is [ hier ](https://helpx.adobe.com/nl/experience-manager/6-4/forms/using/create-form-data-models.html)

Zorg ervoor dat u het formuliergegevensmodel zo configureert dat de POST-service wordt gebruikt om een Lead-object in SFDC te maken.

U zult ook Lees en schrijven de Dienst voor het voorwerp van de Lood moeten vormen. Raadpleeg de schermafbeeldingen onder aan deze pagina.

Nadat u het formuliergegevensmodel hebt gemaakt, kunt u op dit model gebaseerde Adaptive Forms maken en de verzendmethoden van het formuliergegevensmodel gebruiken om lead in SFDC te maken.

>[!NOTE]
>
>Controleer of de URL in het wagerbestand overeenkomt met uw regio. De URL in het voorbeeldwagerbestand is bijvoorbeeld &quot;na46.salesforce.com&quot;, omdat het account in Noord-Amerika is gemaakt. De eenvoudigste manier is om u aan te melden bij uw Salesforce-account en de URL te controleren.

![ sfdc1 ](assets/sfdc1.gif)

![ sfdc2 ](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
