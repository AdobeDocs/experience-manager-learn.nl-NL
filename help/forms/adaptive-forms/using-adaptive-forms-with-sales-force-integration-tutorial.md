---
title: Gegevensbron configureren met Salesforce in AEM Forms 6.3 en 6.4
seo-title: Gegevensbron configureren met Salesforce in AEM Forms 6.3 en 6.4
description: AEM Forms integreren met Salesforce met behulp van formuliergegevensmodel
seo-description: AEM Forms integreren met Salesforce met behulp van formuliergegevensmodel
uuid: 0124526d-f1a3-4f57-b090-a418a595632e
feature: Adaptive Forms, Form Data Model
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 8e314fc3-62d0-4c42-b1ff-49ee34255e83
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '936'
ht-degree: 0%

---


# Gegevensbron configureren met Salesforce in AEM Forms 6.3 en 6.4{#configuring-datasource-with-salesforce-in-aem-forms-and}

## Vereisten {#prerequisites}

In dit artikel zullen wij door het proces lopen om Gegevensbron met Salesforce tot stand te brengen

Vereisten voor deze zelfstudie:

* Blader naar de onderkant van deze pagina en download het wagerbestand en sla het op de vaste schijf op.
* AEM Forms met SSL ingeschakeld

   * [Officiële documentatie voor het inschakelen van SSL op AEM 6.3](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [Officiële documentatie voor het inschakelen van SSL op AEM 6.4](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Je hebt een Salesforce-account nodig
* U moet een verbonden app maken. Het officiële documentatieformulier Salesforce voor het maken van de app wordt [hier](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0) weergegeven.
* Geef de juiste OAuth-oppervlakken op voor de app (ik heb alle beschikbare OAuth-oppervlakken geselecteerd voor testdoeleinden)
* Geef de URL voor de callback op. De callback-URL in mijn geval was

   * Als u **AEM Forms 6.3** gebruikt, zal callback URL https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html zijn. In dit URL-createlead is de naam van mijn formuliergegevensmodel.

   * Als u** AEM Forms 6.4** gebruikt, is de callback-URL [https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html](https://gbedekar-w7-1:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html)

In dit voorbeeld is gbedekar -w7-1:6443 de naam van mijn server en de poort waarop AEM wordt uitgevoerd.

Zodra u de Verbonden nota van de App **Consumentensleutel en Geheime Sleutel** hebt gecreeerd. U hebt deze nodig bij het maken van de gegevensbron in AEM Forms.

Nu u uw verbonden app hebt gemaakt, moet u een wagerbestand maken voor de bewerkingen die u in koopkracht moet uitvoeren. Een voorbeeldwagerbestand wordt opgenomen als onderdeel van de downloadbare middelen. Met dit kwikbestand kunt u het object Lead maken bij het verzenden van een adaptief formulier. Verken dit wagerbestand.

De volgende stap bestaat uit het maken van een gegevensbron in AEM Forms. Voer de volgende stappen uit volgens uw AEM Forms-versie

## AEM Forms 6.3 {#aem-forms}

* Aanmelden bij AEM Forms via het HTTPS-protocol
* Navigeer naar de cloudservices door https://&lt;servername>:&lt;serverport> /etc/cloudservices.html, bijvoorbeeld https://gbedekar-w7-1:6443/etc/cloudservices.html
* Blader omlaag naar &quot;Formuliergegevensmodel&quot;.
* Klik op Configuraties tonen.
* Klik op ‘+’ om een nieuwe configuratie toe te voegen
* Selecteer Volledige service opnieuw instellen. Geef de configuratie een betekenisvolle titel en naam. Bijvoorbeeld:

   * Naam: CreateLeadInSalesForce
   * Titel: CreateLeadInSalesForce

* Klik op &quot;Maken&quot;

**In het volgende scherm **

* Selecteer &quot;Bestand&quot; als optie voor het bronbestand van de computer. Bladeren naar het bestand dat u eerder hebt gedownload
* Verificatietype selecteren als OAuth2.0
* Geef de waarden voor ClientID en Client Secret op
* OAuth URL is - **https://login.salesforce.com/services/oauth2/authorize**
* Token-URL vernieuwen - **https://na5.salesforce.com/services/oauth2/token**
* **URL voor toegangstoek - https://na5.salesforce.com/services/oauth2/token**
* Bereik vergunning: ** api   chatter_api full id   openhartig   verfrissen_token visuale web*
* Verificatiehandler: Vergunninghouder
* Klik op &quot;Verbinding maken met OAUTH&quot;. Als alles goed gaat, worden geen fouten weergegeven

Nadat u het formuliergegevensmodel hebt gemaakt met Salesforce, kunt u vervolgens de formuliergegevensintegratie maken met de gegevensbron die u zojuist hebt gemaakt. De officiële documentatie voor het creëren van de Integratie van de Gegevens van de Vorm is [hier](https://helpx.adobe.com/aem-forms/6-3/data-integration.html).

Zorg ervoor u het Model van de Gegevens van de Vorm vormt om de dienst van de POST te omvatten om een Lood voorwerp in SFDC tot stand te brengen.

U zult ook Lees en schrijven de Dienst voor het voorwerp van de Lood moeten vormen. Raadpleeg de schermafbeeldingen onder aan deze pagina.

Nadat u het formuliergegevensmodel hebt gemaakt, kunt u op dit model gebaseerde Adaptive Forms maken en de verzendmethoden van het formuliergegevensmodel gebruiken om lead in SFDC te maken.

## AEM Forms 6.4 {#aem-forms-1}

* Gegevensbron maken

   * [Navigeren naar gegevensbronnen](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * Klik op de knop Maken
   * Geef enkele betekenisvolle waarden op

      * Naam: CreateLeadInSalesForce
      * Titel: CreateLeadInSalesForce
      * Servicetype: RESTful-service
   * Klik op Next
   * Bron wagen: Bestand
   * Blader naar en selecteer het gummetje dat u in de vorige stap hebt gedownload
   * Type verificatie: OAuth 2.0. Geef de volgende waarden op
   * Geef de waarden voor ClientID en Client Secret op
   * OAuth URL is - **https://login.salesforce.com/services/oauth2/authorize**
   * Token-URL vernieuwen - **https://na5.salesforce.com/services/oauth2/token**
   * Toegangstoken Ur **l - https://na5.salesforce.com/services/oauth2/token**
   * Bereik vergunning: ** api chatter_api full id openid refresh_token visualforce web**
   * Verificatiehandler: Vergunninghouder
   * Klik op de knop &quot;Verbinding maken met OAuth&quot;. Als er fouten optreden, controleert u de voorgaande stappen om ervoor te zorgen dat alle gegevens correct zijn ingevoerd.


Zodra u uw Gegevensbron gebruikend SalesForce hebt gecreeerd, kunt u de Integratie van de Gegevens van de Vorm dan tot stand brengen gebruikend de Gegevensbron die u net hebt gecreeerd. De documentatieverbinding voor dat is [hier](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

Zorg ervoor u het Model van de Gegevens van de Vorm vormt om de dienst van de POST te omvatten om een Lood voorwerp in SFDC tot stand te brengen.

U zult ook Lees en schrijven de Dienst voor het voorwerp van de Lood moeten vormen. Raadpleeg de schermafbeeldingen onder aan deze pagina.

Nadat u het formuliergegevensmodel hebt gemaakt, kunt u op dit model gebaseerde Adaptive Forms maken en de verzendmethoden van het formuliergegevensmodel gebruiken om lead in SFDC te maken.

>[!NOTE]
>
>Controleer of de URL in het wagerbestand overeenkomt met uw regio. De URL in het voorbeeldwagerbestand is bijvoorbeeld &quot;na46.salesforce.com&quot;, omdat het account in Noord-Amerika is gemaakt. De eenvoudigste manier is om u aan te melden bij uw Salesforce-account en de URL te controleren.

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
