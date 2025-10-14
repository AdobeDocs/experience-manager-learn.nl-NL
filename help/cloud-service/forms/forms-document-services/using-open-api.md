---
title: AEM Forms-communicatie-API configureren
description: AEM Forms Communication-API's op basis van OpenAPI's configureren voor server-naar-server verificatie
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: b9c0b04b-6131-4752-b2f0-58e1fb5f40aa
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# AEM Forms Communication-API&#39;s op basis van OpenAPI configureren op AEM Forms as a Cloud Service

## Vereisten

* Laatste exemplaar van AEM Forms as a Cloud Service.
* Alle noodzakelijke [&#x200B; productprofielen worden toegevoegd aan het milieu.](https://experienceleague.adobe.com/nl/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)

* AEM API-toegang tot het productprofiel inschakelen, zoals hieronder wordt weergegeven
  ![&#x200B; product_profile1 &#x200B;](assets/product-profiles1.png)
  ![&#x200B; product_profile &#x200B;](assets/product-profiles.png)

## Adobe Developer Console-project maken

Login aan [&#x200B; Adobe Developer Console &#x200B;](https://developer.adobe.com/console/) gebruikend uw Adobe ID.
Een nieuw project maken door op het juiste pictogram te klikken
![&#x200B; nieuw-project &#x200B;](assets/new-project.png)

Geef een betekenisvolle naam aan het project en klik op het pictogram API toevoegen
![&#x200B; nieuw-project &#x200B;](assets/new-project2.png)

Experience Cloud selecteren
![&#x200B; new-project3 &#x200B;](assets/new-project3.png)
Selecteer AEM Forms Communications API en klik op Next
![&#x200B; new-project4 &#x200B;](assets/new-project4.png)

Controleer of u server-naar-server verificatie hebt geselecteerd en klik op Volgende
![&#x200B; new-project5 &#x200B;](assets/new-project5.png)
Selecteer de profielen en klik op de knop geconfigureerde API opslaan om uw instellingen op te slaan
![&#x200B; new-project6 &#x200B;](assets/new-project6.png)
Klik in OAuth server-aan-server
![&#x200B; new-project7 &#x200B;](assets/new-project7.png)
Kopieer de client-id, clientgeheim en segmenten
![&#x200B; nieuw-project8 &#x200B;](assets/new-project8.png)

## AEM-instantie configureren om ADC-projectcommunicatie in te schakelen

Als u reeds een project van AEM Forms hebt, [&#x200B; gelieve deze instructies &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis) te volgen om OAuth van het Project van Adobe Developer Console Server-aan-Server referentie ClientID toe te laten om met de instantie van AEM te communiceren

Als u geen project van AEM Forms hebt, te creëren gelieve een [&#x200B; Project van AEM Forms door deze documentatie te volgen.](https://experienceleague.adobe.com/nl/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/getting-started) en laat dan OAuth van het Project van Adobe Developer Console Server-aan-Server referentie ClientID toe om met de instantie van AEM [&#x200B; te communiceren gebruikend deze documentatie.](https://experienceleague.adobe.com/nl/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)


## Volgende stappen

[Toegangstoken genereren](./generate-access-token.md)
