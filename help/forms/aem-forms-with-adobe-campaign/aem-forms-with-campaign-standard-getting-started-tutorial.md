---
title: AEM Forms en Adobe Campaign Standard integreren
description: Integreer AEM Forms met Adobe Campaign Standard door gebruik te maken van het AEM Forms Form Data Model voor het ophalen van informatie over het ACS-campagneprofiel enzovoort.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: e028837b-13d8-4058-ac25-ed095f49524c
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 44
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---

# AEM Forms en Adobe Campaign Standard integreren

![ formsandcampagne ](assets/helpx-cards-forms.png)

Leer hoe u AEM Forms kunt integreren met Adobe Campaign Standard (ACS).

ACS heeft een rijke reeks blootgestelde API&#39;s die ACS om met de technologie van onze keus toelaat worden verbonden. In deze zelfstudie zullen we ons concentreren op het koppelen van AEM Forms met ACS.

Om AEM Forms met ACS te integreren zult u de volgende stappen moeten volgen:

* [ de toegang van opstelling API op uw instantie ACS.](https://experienceleague.adobe.com/docs/campaign-standard/using/working-with-apis/get-started-apis.html?lang=en)
* JSON-webtoken maken.
* Uitwisseling de Token van het Web JSON met de Dienst van Adobe Identity Management voor een Token van de Toegang.
* Neem dit toegangstoken op in HTTP-header voor autorisatie, samen met de X-API-sleutel in elke aanvraag voor een ACS-instantie.

Volg de volgende instructies om aan de slag te gaan

* [Download en decomprimeer de middelen die aan deze zelfstudie zijn gekoppeld.](assets/aem-forms-and-acs-bundles.zip)
* Stel de bundels op gebruikend [ het Webconsole van de Felix ](http://localhost:4502/system/console/bundles)
* Geef de juiste instellingen op voor Adobe Campaign in Felix OSGI Configuration.
* [ creeer een de dienstgebruiker zoals vermeld in dit artikel ](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Zorg ervoor om de bundel OSGi op te stellen verbonden aan het artikel.
* Sla de persoonlijke ACS-sleutel op in etc/key/campaign/private.key. U moet een map maken met de naam Campagne onder e.d./key.
* [ verstrek gelezen toegang tot de campagnemap aan de de dienstgebruiker &quot;gegevens&quot;.](http://localhost:4502/useradmin)

## Volgende stappen

[JWT- en toegangstoken genereren](partone.md)
