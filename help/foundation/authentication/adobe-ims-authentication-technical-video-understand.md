---
title: Adobe IMS-verificatie begrijpen met AEM op Adobe Managed Services
description: Adobe Experience Manager introduceert ondersteuning voor Admin Consoles voor AEM instanties en op Adobe IMS (Identity Management System) gebaseerde verificatie voor AEM op Managed Services.   Dankzij deze integratie kunnen AEM Managed Services-klanten alle Experience Cloud-gebruikers beheren in één geïntegreerde webconsole. Gebruikers en groepen kunnen worden toegewezen aan productprofielen die aan AEM instanties zijn gekoppeld, zodat centraal beheerde toegang tot de specifieke AEM-instanties wordt verleend.
version: 6.4, 6.5
feature: 'Gebruikers en groepen '
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: Architectuur
role: Architect
level: Ervaren
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---


# Adobe IMS-verificatie begrijpen met AEM op door Adobe beheerde services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager introduceert ondersteuning voor Admin Consoles voor AEM instanties en op Adobe IMS (Identity Management System) gebaseerde verificatie voor AEM op Managed Services.   Dankzij deze integratie kunnen AEM Managed Services-klanten alle Experience Cloud-gebruikers beheren in één geïntegreerde webconsole. Gebruikers en groepen kunnen worden toegewezen aan productprofielen die aan AEM instanties zijn gekoppeld, zodat centraal beheerde toegang tot de specifieke AEM-instanties wordt verleend.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience Manager IMS-verificatieondersteuning is alleen bedoeld voor &#39;interne&#39; gebruikers (auteurs, revisoren, beheerders, ontwikkelaars, enz.) en niet voor externe eindgebruikers, zoals bezoekers van websites.
* [In Admin ](https://adminconsole.adobe.com/) Consolewill AEM Managed Services-klanten worden weergegeven als IMS Orgs en de AEM als productcontexten. Systeem- en productbeheerders van Admin Consoles kunnen definiëren en beheren.
* AEM Managed Services synchroniseert uw topologie met Admin Console, creërend een 1-aan-1 afbeelding tussen een Context van het Product en AEM instantie.
* Het Profiel van het product in Admin Console zal bepalen tot welke AEM instanties een gebruiker toegang heeft.
* De steun van de authentificatie omvat klant SAML2 volgzame IDPs voor SSO.
* Alleen Enterprise- of federatieve id&#39;s (voor SSO&#39;s van klanten) worden ondersteund (persoonlijke Adobe-id&#39;s worden niet ondersteund).

** Deze functie wordt ondersteund voor AEM 6.4 SP3 en hoger voor klanten van Adobe Managed Services.*

## Aanbevolen werkwijzen {#best-practices}

### Machtigingen toepassen in Admin Console

Het toepassen van machtigingen en toegang op gebruikersniveau moet zowel in de Admin Console als in Adobe Experience Manager worden vermeden.

In Admin Console moeten gebruikers toegang krijgen via gebruikersgroepen op productcontextniveau. Gebruikersgroepen worden meestal het best uitgedrukt door de logische rol binnen de organisatie om de herbruikbaarheid van de groepen in Adobe Experience Cloud-producten te bevorderen.

>[!NOTE]
>
> Als u AEM als Cloud Service gebruikt, wijst u Admin Consoles rechtstreeks toe aan productprofielen. Overgangsmachtigingen tussen gebruikers van Admin Consoles naar productprofielen via gebruikersgroepen van Admin Consoles worden niet ondersteund voor AEM als Cloud Service.

### Machtigingen toepassen in Adobe Experience Manager

In Adobe Experience Manager, zouden de gebruikersgroepen die van Adobe IMS worden gesynchroniseerd op termijn aan [AEM-verstrekte gebruikersgroepen](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html) moeten worden toegevoegd, die met de aangewezen toestemmingen vooraf gevormd om specifieke reeksen taken in AEM uit te voeren. Gebruikers die via Adobe IMS zijn gesynchroniseerd, mogen niet rechtstreeks worden toegevoegd aan [AEM-opgegeven gebruikersgroepen](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html).
