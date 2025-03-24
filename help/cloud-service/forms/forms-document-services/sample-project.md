---
title: Voorbeeldproject
description: Voorbeeldproject dat in uw omgeving kan worden geïmporteerd en uitgevoerd
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: f1fcc4bb-cc31-45e8-b7bb-688ef6a236bb
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# Testen op uw lokale omgeving

* Het project importeren

   * Download en haal het [ steekproefproject ](./assets/formsdocumentservices.zip) uit
   * Open uw aangewezen **Milieu van de Ontwikkeling van Java** (IntelliJ IDEA, Eclipse of de Code van VS) en voer het project als Geweven project in
* Referenties configureren

   * Het bestand zoeken `resources/credentials/server_credentials.json`
   * Open het en **werk de geloofsbrieven** specifiek voor uw milieu bij.
   * Zorg ervoor dat het geldige waarden bevat voor:
     `clientId` , `clientSecret`, `adobeIMSV3TokenEndpointURL`, en
     `scopes`

* De klasse Main uitvoeren

   * Ga naar `src/main/java/Main.java` en voer de hoofdmethode uit

* Uitvoering verifiëren
   * Verifieer de output in het eindvenster
