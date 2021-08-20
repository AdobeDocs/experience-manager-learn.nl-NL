---
title: Aan de slag met AEM Forms en Adobe Campaign Standard
description: Integreer AEM Forms met Adobe Campaign Standard door gebruik te maken van het AEM Forms Form Data Model voor het ophalen van informatie over het ACS-campagneprofiel enzovoort.
feature: Adaptief Forms, formuliergegevensmodel
version: 6.3,6.4,6.5
topic: Ontwikkeling
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---


# Aan de slag met AEM Forms en Adobe Campaign Standard {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampagne](assets/helpx-cards-forms.png)

Deze zelfstudie bevat een overzicht van de verschillende gebruiksscenario&#39;s voor de integratie van AEM Forms met Adobe Campaign Standard (ACS).

ACS heeft een rijke reeks blootgestelde API&#39;s die ACS om met de technologie van onze keus toelaat worden verbonden. In deze zelfstudie zullen we ons concentreren op het koppelen van AEM Forms met ACS.

Om AEM Forms met ACS te integreren zult u de volgende stappen moeten volgen:

* [Opstelling API toegang op uw instantie ACS.](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* JSON-webtoken maken.
* Uitwisseling het Token van het Web JSON met de Dienst van Adobe Identity Management voor een Token van de Toegang.
* Neem dit toegangstoken op in HTTP-header voor autorisatie, samen met de X-API-sleutel in elke aanvraag voor een ACS-instantie.

Volg de volgende instructies om aan de slag te gaan

* [Download en decomprimeer de middelen die aan deze zelfstudie zijn gekoppeld.](assets/aem-forms-and-acs-bundles.zip)
* De bundels implementeren met [Felix-webconsole](http://localhost:4502/system/console/bundles)
* Geef de juiste instellingen op voor Adobe Campaign in Felix OSGI Configuration.
* [Maak een servicegebruiker zoals vermeld in dit artikel](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Zorg ervoor om de bundel OSGi op te stellen verbonden aan het artikel.
* Sla de persoonlijke ACS-sleutel op in etc/key/campaign/private.key. U moet een map maken met de naam Campagne onder e.d./key.
* [Leestoegang tot de campagnemap aan de de dienstgebruiker &quot;gegevens&quot;verlenen.](http://localhost:4502/useradmin)
