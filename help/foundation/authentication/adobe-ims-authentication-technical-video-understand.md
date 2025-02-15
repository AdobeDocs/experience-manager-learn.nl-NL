---
title: Adobe IMS-verificatie begrijpen met AEM op Adobe Managed Services
description: Adobe Experience Manager introduceert ondersteuning voor Admin Consoles voor AEM en verificatie op basis van Adobe IMS (Identity Management System) voor AEM op Managed Services.   Dankzij deze integratie kunnen AEM Managed Services-klanten alle gebruikers van Experiencen Cloud beheren in één geïntegreerde webconsole. Gebruikers en groepen kunnen worden toegewezen aan productprofielen die aan AEM instanties zijn gekoppeld, zodat centraal beheerde toegang tot de specifieke AEM-instanties wordt verleend.
version: 6.4, 6.5
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---

# Adobe IMS-verificatie begrijpen met AEM op Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager introduceert ondersteuning voor Admin Consoles voor AEM instanties en verificatie op basis van het Identity Management System (IMS) van de Adobe voor AEM op Managed Services.   Dankzij deze integratie kunnen AEM Managed Services-klanten alle gebruikers van Experiencen Cloud beheren in één geïntegreerde webconsole. Gebruikers en groepen kunnen worden toegewezen aan productprofielen die aan AEM instanties zijn gekoppeld, zodat centraal beheerde toegang tot de specifieke AEM-instanties wordt verleend.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience Manager IMS-verificatieondersteuning is alleen bedoeld voor &#39;interne&#39; gebruikers (auteurs, revisoren, beheerders, ontwikkelaars, enzovoort) en niet voor externe eindgebruikers, zoals bezoekers van websites.
* [ de Admin Console ](https://adminconsole.adobe.com/) vertegenwoordigt AEM klanten van Managed Services als IMS Orgs en de AEM instanties als Contexten van het Product. Systeem- en productbeheerders van Admin Consoles kunnen definiëren en beheren.
* AEM Managed Services synchroniseert uw topologie met Admin Console, creërend een 1-aan-1 afbeelding tussen een Context van het Product en AEM instantie.
* Het Profiel van het product in Admin Console bepaalt tot welke AEM instanties een gebruiker toegang heeft.
* De steun van de authentificatie omvat klant SAML2 volgzame IDPs voor SSO.
* Alleen Enterprise- of federatieve id&#39;s (voor SSO&#39;s van klanten) worden ondersteund (persoonlijke Adobe-id&#39;s worden niet ondersteund).

*&#42;Deze functie wordt ondersteund voor AEM 6.4 SP3 en hoger voor Adobe Managed Services-klanten.*

## Aanbevolen procedures {#best-practices}

### Machtigingen toepassen in Admin Console

Het toepassen van machtigingen en toegang op gebruikersniveau moet zowel in de Admin Console als in Adobe Experience Manager worden vermeden.

In Admin Console, zouden de gebruikers via Gebruikersgroepen op het niveau van de Context van het Product toegang moeten worden verleend. Gebruikersgroepen worden meestal het best uitgedrukt door de logische rol binnen de organisatie om de herbruikbaarheid van de groepen in Adobe Experience Cloud-producten te bevorderen.

### Machtigingen toepassen in Adobe Experience Manager

In Adobe Experience Manager, zouden de gebruikersgroepen die van Adobe IMS worden gesynchroniseerd op termijn aan [ AEM-verstrekte gebruikersgroepen ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html) moeten worden toegevoegd, die met de aangewezen toestemmingen preconfigured komen om specifieke reeksen taken in AEM uit te voeren. De gebruikers die van IMS van Adobe worden gesynchroniseerd zouden niet direct aan [ AEM-verstrekte gebruikersgroepen ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html) moeten worden toegevoegd.
