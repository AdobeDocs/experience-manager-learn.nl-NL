---
title: AEM (november 2018)
seo-title: AEM Security Notification (November 2018)
description: AEM Experience Manager Security Notification Dispatcher
seo-description: AEM Experience Manager Security Notification Dispatcher
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: Security
role: Architect
level: Beginner
exl-id: 1ee11a01-9e49-42f4-aec4-09e51f769f69
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---

# AEM (november 2018)

## Samenvatting

Dit artikel verhelpt een aantal recente en oude kwetsbaarheden die onlangs in AEM zijn gemeld. Merk op dat de meeste geïdentificeerde kwetsbaarheden bekende problemen voor het AEM product waren en dat mitigatie eerder is geïdentificeerd, is een nieuwe versie van de verzender beschikbaar voor de nieuwe kwetsbaarheden. Adobe dringt er ook bij klanten op aan de [Beveiligingschecklist AEM](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) en de relevante richtsnoeren volgen.

## Actie vereist

* AEM implementaties moeten beginnen met de nieuwste versie van Dispatcher.
* De de veiligheidsregels van de verzender moeten worden toegepast zoals op de geadviseerde configuratie.
* De [Beveiligingschecklist AEM](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) moeten worden voltooid voor AEM implementaties.

## Kwetsbaarheid en resoluties

| Probleem | Resolutie | Koppelingen |
|-------|------------|-------|
| Het omzeilen van AEM Dispatcher-regels | Installeer de nieuwste versie van Dispatcher(4.3.1) en volg de aanbevolen configuratie van de verzender. | Zie [Opmerkingen bij de release AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) en [Dispatcher configureren](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Een kwetsbaarheid met betrekking tot het omzeilen van URL-filters die kan worden gebruikt om verzendingsregels te omzeilen - CVE-2016-0957 | Dit is opgelost in een oudere versie van Dispatcher, maar nu wordt u aangeraden de nieuwste versie van Dispatcher (4.3.1) te installeren en de aanbevolen configuratie voor Dispatcher te volgen. | Zie [Opmerkingen bij de release AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) en [Dispatcher configureren](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| XSS-kwetsbaarheid voor opgeslagen SWF-bestanden | Dit probleem is opgelost met eerder vrijgegeven beveiligingsoplossingen. | Zie [AEM beveiligingsbulletin APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| Explosies met betrekking tot wachtwoorden | Volg aanbeveling in checklist van de Veiligheid voor sterkere wachtwoorden. | Zie [Beveiligingschecklist AEM](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| Blootstelling aan schijfgebruik voor anonieme gebruikers | Dit probleem is opgelost voor AEM 6.1 en hoger, want AEM 6.0 kunnen de machtigingen voor de out van de box worden gewijzigd om restrictiever te zijn. | Zie [releaseopmerkingen](https://helpx.adobe.com/experience-manager/aem-previous-versions.html)voor AEM 6.1 en ouder. |
| Belichting van de Open Sociale Volmacht voor anonieme gebruikers | Dit is opgelost in versies die van 6.0 SP2 beginnen. | Zie [releaseopmerkingen](https://helpx.adobe.com/experience-manager/aem-previous-versions.html) voor AEM 6.1 en ouder. |
| CRX Explorer Access op productieinstanties | Het beheren van toegang van de Ontdekkingsreiziger van CRX is reeds behandeld in de Controlelijst van de Veiligheid, zou de Ontdekkingsreiziger CRX uit productieauteur moeten worden verwijderd en publiceren en de veiligheidsgezondheidscontrole meldt het als niet verwijderd. | Zie [Beveiligingschecklist AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html). |
| BGServlets wordt weergegeven | Dit is opgelost sinds AEM 6.2. | Zie [AEM 6.2 Opmerkingen bij de release](https://helpx.adobe.com/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [Gebruikershandleiding voor AEM](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [Opmerkingen bij de release AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [Beveiligingsbulletins AEM](https://helpx.adobe.com/security.html#experience-manager)

