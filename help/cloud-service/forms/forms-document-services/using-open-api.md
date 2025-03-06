---
title: AEM Forms-communicatie-API configureren
description: AEM Forms Communication-API's op basis van OpenAPI's configureren voor server-naar-server verificatie
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# AEM Forms Communication-API&#39;s op basis van OpenAPI configureren op AEM Forms as a Cloud Service

## Vereisten

* Laatste exemplaar van AEM Forms as a Cloud Service.
* Alle noodzakelijke [ productprofielen worden toegevoegd aan het milieu.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)

* AEM API-toegang tot het productprofiel inschakelen, zoals hieronder wordt weergegeven
  ![ product_profile1 ](assets/product-profiles1.png)
  ![ product_profile ](assets/product-profiles.png)

## Adobe Developer Console-project maken

Login aan [ Adobe Developer Console ](https://developer.adobe.com/console/) gebruikend uw Adobe ID.
Een nieuw project maken door op het juiste pictogram te klikken
![ nieuw-project ](assets/new-project.png)

Geef een betekenisvolle naam aan het project en klik op het pictogram API toevoegen
![ nieuw-project ](assets/new-project2.png)

Experience Cloud selecteren
![ new-project3 ](assets/new-project3.png)
Selecteer AEM Forms Communications API en klik op Next
![ new-project4 ](assets/new-project4.png)

Controleer of u server-naar-server verificatie hebt geselecteerd en klik op Volgende
![ new-project5 ](assets/new-project5.png)
Selecteer de profielen en klik op de knop geconfigureerde API opslaan om uw instellingen op te slaan
![ new-project6 ](assets/new-project6.png)
Klik in OAuth server-aan-server
![ new-project7 ](assets/new-project7.png)
Kopieer de client-id, clientgeheim en segmenten
![ nieuw-project8 ](assets/new-project8.png)

## AEM-instantie configureren om ADC-projectcommunicatie in te schakelen

Als u reeds een project van AEM Forms hebt, [ gelieve deze instructies ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis) te volgen om OAuth van het Project van Adobe Developer Console Server-aan-Server referentie ClientID toe te laten om met de instantie van AEM te communiceren

Als u geen project van AEM Forms hebt, te creëren gelieve een [ Project van AEM Forms door deze documentatie te volgen.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/getting-started) en laat dan OAuth van het Project van Adobe Developer Console Server-aan-Server referentie ClientID toe om met de instantie van AEM [ te communiceren gebruikend deze documentatie.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)


## Volgende stappen

[Toegangstoken genereren](./generate-access-token.md)