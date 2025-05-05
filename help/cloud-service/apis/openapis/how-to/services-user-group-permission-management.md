---
title: Beheer van machtigingen voor gebruikersgroepen voor productprofiel en services
description: Leer hoe u machtigingen voor de gebruikersgroepen Productprofiel en Services in AEM as a Cloud Service beheert.
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-17429
thumbnail: KT-17429.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 3230a8e7-6342-4497-9163-1898700f29a4
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---

# Beheer van machtigingen voor gebruikersgroepen voor productprofiel en services

Leer hoe u machtigingen voor de gebruikersgroepen Productprofiel en Services in AEM as a Cloud Service beheert.

In deze zelfstudie leert u:

- Het Profiel van het product en zijn vereniging met de Diensten.
- Het bijwerken van de toestemmingen van de gebruikersgroep van de Diensten.

## Achtergrond

Wanneer u AEM API gebruikt, moet u het _Profiel van het Product_ aan _Geloofsbrieven_ in het project van Adobe Developer Console (of ADC) toewijzen. Het _Profiel van het Product_ (en de bijbehorende Dienst) verstrekt de _toestemmingen of de vergunning_ aan de geloofsbrieven om tot de middelen van AEM toegang te hebben. In het volgende screenshot, kunt u _Geloofsbrieven_ en _Profiel van het Product_ voor een Auteur API van AEM Assets zien:

![ Geloofsbrieven en het Profiel van het Product ](../assets/how-to/API-Credentials-Product-Profile.png)

Een Profiel van het Product wordt geassocieerd met één of meerdere _Diensten_. In AEM as a Cloud Service, vertegenwoordigt de a _Dienst_ gebruikersgroepen met vooraf bepaalde Lijsten van het Toegangsbeheer (ACLs) voor gegevensopslagplaatsen, die korrelig toestemmingsbeheer toestaan.

![ het Profiel van het Product van de Gebruiker van de Technische Rekening ](../assets/s2s/technical-account-user-product-profile.png)

Na succesvolle API-aanroeping wordt een gebruiker die de referentie van het ADC-project vertegenwoordigt, in de AEM Auteur-service gemaakt, samen met de gebruikersgroepen die overeenkomen met de configuratie Productprofiel en Services.

![ Technisch Lidmaatschap van de Gebruiker van de Rekening ](../assets/s2s/technical-account-user-membership.png)

In het bovenstaande scenario wordt de gebruiker `1323d2...` gemaakt in de AEM Author-service en is deze een lid van de gebruikersgroepen `AEM Assets Collaborator Users - Service` en `AEM Assets Collaborator Users - author - Program XXX - Environment XXX` .

## Machtigingen voor gebruikersgroepen bijwerken

Het grootste deel van de _Diensten_ verstrekt _LEZEN_ toestemming aan de middelen van AEM, via de gebruikersgroepen in de instantie van AEM die de zelfde naam zoals de _Dienst_ hebben.

Er zijn tijden wanneer de geloofsbrieven (ook bekend als technische rekeningsgebruiker) extra toestemmingen zoals _creeer, Update, schrap_ (CUD) van de middelen van AEM nodig hebben. In dergelijke gevallen, kunt u de toestemmingen van de _gebruikersgroepen van de Diensten_ &lbrace;in de instantie van AEM bijwerken.

Bijvoorbeeld, wanneer de aanroeping van de Auteur van AEM Assets API a [ 403 fout voor niet-GET verzoeken ](../use-cases/invoke-api-using-oauth-s2s.md#403-error-for-non-get-requests) ontvangt, kunt u de toestemmingen van de _Gebruikers van de Medewerker van AEM Assets - de gebruikersgroep van de Dienst_ in de instantie van AEM bijwerken.

Gebruikend het interface van de toestemmingengebruiker of [ Sling het manuscript van de Initialisatie van de Bewaarplaats van de Bewaarplaats ](https://sling.apache.org/documentation/bundles/repository-initialization.html), kunt u de toestemmingen van de uit-van-de-doos gebruikersgroepen in de instantie van AEM bijwerken.

### Machtigingen bijwerken via de gebruikersinterface voor machtigingen

Voer de volgende stappen uit om de machtigingen van de gebruikersgroep services (bijvoorbeeld `AEM Assets Collaborator Users - Service` ) bij te werken met behulp van de gebruikersinterface voor machtigingen:

- Navigeer aan de **Hulpmiddelen** > **Veiligheid** > **Toestemmingen** in de instantie van AEM.

- Zoek naar de groep van de dienstengebruiker (bijvoorbeeld `AEM Assets Collaborator Users - Service`).

  ![ de gebruikersgroep van het Onderzoek ](../assets/how-to/search-user-group.png)

- Klik **ACE** toevoegen om een nieuw Ingang van het Toegangsbeheer (ACE) voor de gebruikersgroep toe te voegen.

  ![ voegt ACE ](../assets/how-to/add-ace.png) toe

### Machtigingen bijwerken met behulp van het Initialisatiescript in de repository

Voer de volgende stappen uit om de machtigingen van de gebruikersgroep services (bijvoorbeeld `AEM Assets Collaborator Users - Service` ) bij te werken met behulp van het initialisatiescript voor de gegevensopslagruimte:

- Open het AEM-project in uw favoriete IDE.

- Navigeren naar de module `ui.config`

- Maak een bestand in `ui.config/src/main/content/jcr_root/apps/<PROJECT-NAME>/osgiconfig/config.author` genaamd `org.apache.sling.jcr.repoinit.RepositoryInitializer-services-group-acl-update.cfg.json` met de volgende inhoud:

  ```json
  {
      "scripts": [
          "set ACL for \"AEM Assets Collaborator Users - Service\" (ACLOptions=ignoreMissingPrincipal)",
          "    allow jcr:read,jcr:versionManagement,crx:replicate,rep:write on /content/dam",
          "end"
      ]
  }
  ```

- Leg de wijzigingen vast en duw deze naar de opslagplaats.

- Stel de veranderingen in de instantie van AEM op gebruikend de [ volledig-stapelpijpleiding van Cloud Manager ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#full-stack-pipeline).

- U kunt de toestemmingen van de gebruikersgroep ook verifiëren gebruikend de **mening van Toestemmingen**. Navigeer aan de **Hulpmiddelen** > **Veiligheid** > **Toestemmingen** in de instantie van AEM.

  ![ mening van Toestemmingen ](../assets/how-to/permissions-view.png)

### Machtigingen verifiëren

Nadat u de machtigingen met een van de bovenstaande methoden hebt bijgewerkt, werkt de PATCH-aanvraag om de metagegevens van de elementen bij te werken nu zonder problemen.

![ PATCH Verzoek ](../assets/how-to/patch-request.png)

## Samenvatting

U hebt geleerd hoe u machtigingen voor de gebruikersgroepen Productprofiel en Services in AEM as a Cloud Service kunt beheren. U kunt de machtigingen van de gebruikersgroepen voor services in de AEM-instantie bijwerken met behulp van de gebruikersinterface voor machtigingen of het initialisatiescript in de repository.
