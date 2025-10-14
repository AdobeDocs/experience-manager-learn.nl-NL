---
title: Functionaliteit voor opslaan en hervatten van een adaptief formulier implementeren
description: Leer hoe u adaptieve formuliergegevens kunt opslaan en ophalen van Azure-opslagaccount.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-13717
exl-id: b40b0ef4-efa9-400e-82d8-aa0c7feb7be4
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 28
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# Inleiding

In deze zelfstudie implementeren we een eenvoudig gebruik waarbij de invullers van het formulier het invullen van het formulier kunnen opslaan en hervatten. Het gebruiksscenario ziet er als volgt uit

* De gebruiker begint een adaptief formulier in te vullen.
* De gebruiker slaat het formulier op en wil het formulier op een latere datum invullen.
* Gebruiker ontvangt een e-mailmelding met een koppeling naar het opgeslagen formulier.

## Voorwaarden

* Ervaar met AEM Forms CS, vooral bij het maken van formuliergegevensmodellen.
* Ervaring met het implementeren van code met gebruik van cloudbeheer.
* Toegang tot de cloudinstantie van AEM Forms CS.

Voor het implementeren van het bovenstaande gebruiksgeval in AEM Forms CS hebt u het volgende nodig

* [&#x200B; AEM Forms CS wolk klaar instantie &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=nl-NL#set-up-aem-author-instance)
* [&#x200B; Azure poortrekening &#x200B;](https://portal.azure.com/)
* [&#x200B; SendGrid Rekening &#x200B;](https://sendgrid.com/)

### Volgende stappen

[Pagina-component maken](./page-component.md)
