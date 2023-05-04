---
title: AEM Forms met Marketo (Deel 1)
description: Zelfstudie voor de integratie van AEM Forms met Marketo met behulp van het AEM Forms-formuliergegevensmodel.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---

# AEM Forms met Marketo

Marketo, een onderdeel van Adobe, beschikt over software voor marketingautomatisering die is gericht op marketing op basis van account, zoals e-mail, mobiele, sociale, digitale advertenties, webbeheer en analyses.

Met het formuliergegevensmodel van AEM Forms kunnen we nu AEM formulier naadloos integreren met Marketo.

[Meer informatie over het formuliergegevensmodel](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo maakt een REST API beschikbaar waarmee een groot aantal mogelijkheden van het systeem op afstand kan worden uitgevoerd. Er zijn veel opties, variërend van het maken van programma&#39;s tot het bulksgewijs importeren van leads, waarmee u een Marketo-instantie met fijnkorrelige besturing kunt besturen. Met het formuliergegevensmodel is het heel eenvoudig om AEM Forms te integreren met Marketo.

In deze zelfstudie worden de stappen besproken die nodig zijn voor de integratie van AEM Forms met Marketo met behulp van het formuliergegevensmodel. Bij het voltooien van de zelfstudie hebt u een OSGi-bundel die de aangepaste verificatie uitvoert tegen Marketo. U hebt ook de gegevensbron geconfigureerd met behulp van het meegeleverde wagerbestand.

Om te beginnen wordt het hoogst geadviseerd dat u met de volgende onderwerpen vertrouwd bent die in de sectie van de Vereiste worden vermeld.

## Vereiste

1. [AEM server met AEM Forms Add op pakket geïnstalleerd](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. Ontwikkelomgeving voor lokale AEM
1. Kennis hebben van formuliergegevensmodel
1. Basiskennis van wagerbestanden
1. Adaptieve Forms maken

**Clientgeheim-id en clientgeheim-sleutel**

De eerste stap bij de integratie van Marketo met AEM Forms is het verkrijgen van de API-referenties die nodig zijn om de REST-aanroepen met behulp van API uit te voeren. U hebt het volgende nodig

1. client_id
1. client_geheime
1. identity_end
1. verificatie-URL

[Volg de officiële documentatie van Marketo om de bovengenoemde eigenschappen te verkrijgen.](https://developers.marketo.com/rest-api/) U kunt ook contact opnemen met de beheerder van uw Marketo-exemplaar.

**Voordat u begint**

[Download en decomprimeer de middelen die aan dit artikel zijn gekoppeld.](assets/aemformsandmarketo.zip) Het ZIP-bestand bevat het volgende:

1. BlankTemplatePackage.zip - dit is het Adaptieve malplaatje van de Vorm. Importeer dit met het pakketbeheer.
1. marketo.json - Dit is het wagerbestand dat wordt gebruikt om de gegevensbron te configureren.
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar - Dit is de bundel die de douaneauthentificatie doet. U kunt dit gratis gebruiken als u de zelfstudie niet kunt voltooien of als uw bundel niet naar behoren werkt.

## Volgende stappen

[Aangepaste verificatie maken](./part2.md)
