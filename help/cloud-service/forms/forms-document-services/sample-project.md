---
title: Voorbeeldproject
description: Voorbeeldproject dat in uw omgeving kan worden geïmporteerd en uitgevoerd
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

