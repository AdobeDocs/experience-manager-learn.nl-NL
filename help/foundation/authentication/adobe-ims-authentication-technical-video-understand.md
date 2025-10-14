---
title: Adobe IMS-verificatie met AEM op Adobe Managed Services
description: Adobe Experience Manager introduceert Admin Console-ondersteuning voor AEM-instanties en verificatie op basis van Adobe IMS (Identity Management System) voor AEM op Managed Services.   Dankzij deze integratie kunnen AEM Managed Services-klanten alle Experience Cloud-gebruikers in één enkele webconsole beheren. Gebruikers en groepen kunnen worden toegewezen aan productprofielen die aan AEM-instanties zijn gekoppeld, zodat centraal beheerde toegang tot de specifieke AEM-instanties wordt verleend.
version: Experience Manager 6.4, Experience Manager 6.5
feature: User and Groups
doc-type: Technical Video
jira: KT-781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
duration: 405
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---

# Adobe IMS-verificatie met AEM op Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager introduceert Admin Console-ondersteuning voor AEM-instanties en op Adobe Identity Management System (IMS) gebaseerde verificatie voor AEM op Managed Services.   Dankzij deze integratie kunnen AEM Managed Services-klanten alle Experience Cloud-gebruikers in één enkele webconsole beheren. Gebruikers en groepen kunnen worden toegewezen aan productprofielen die aan AEM-instanties zijn gekoppeld, zodat centraal beheerde toegang tot de specifieke AEM-instanties wordt verleend.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience Manager IMS-verificatieondersteuning is alleen bedoeld voor &#39;interne&#39; gebruikers (auteurs, revisoren, beheerders, ontwikkelaars, enzovoort) en niet voor externe eindgebruikers, zoals bezoekers van websites.
* [&#x200B; Admin Console &#x200B;](https://adminconsole.adobe.com/) vertegenwoordigt de klanten van AEM Managed Services als IMS Orgs en de instanties van AEM als Contexten van het Product. Admin Console System and Product Admins kan definiëren en beheren.
* AEM Managed Services synchroniseert uw topologie met Admin Console, waardoor een 1-op-1-toewijzing wordt gemaakt tussen een Product Context en een AEM-instantie.
* Met Productprofiel in Admin Console wordt bepaald tot welke AEM-instanties een gebruiker toegang heeft.
* De steun van de authentificatie omvat klant SAML2 volgzame IDPs voor SSO.
* Alleen Enterprise- of federatieve id&#39;s (voor SSO&#39;s van klanten) worden ondersteund (persoonlijke Adobe-id&#39;s worden niet ondersteund).

*&#42;Deze functie wordt ondersteund voor AEM 6.4 SP3 en hoger voor Adobe Managed Services-klanten.*

## Aanbevolen procedures {#best-practices}

### Machtigingen toepassen in Admin Console

Het toepassen van machtigingen en toegang op gebruikersniveau moet zowel in Admin Console als in Adobe Experience Manager worden vermeden.

In Admin Console moeten gebruikers toegang krijgen via gebruikersgroepen op het niveau van de productcontext. Gebruikersgroepen worden meestal het best uitgedrukt door de logische rol binnen de organisatie om de herbruikbaarheid van de groepen in Adobe Experience Cloud-producten te bevorderen.

### Machtigingen toepassen in Adobe Experience Manager

In Adobe Experience Manager, zouden de gebruikersgroepen die van Adobe IMS worden gesynchroniseerd op termijn aan [&#x200B; AEM-Geleide gebruikersgroepen &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=nl-NL) moeten worden toegevoegd, die met de aangewezen toestemmingen preconfigured komen om specifieke reeksen taken in AEM uit te voeren. De gebruikers die van Adobe IMS worden gesynchroniseerd zouden niet direct aan [&#x200B; AEM-Geleide gebruikersgroepen &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=nl-NL) moeten worden toegevoegd.
