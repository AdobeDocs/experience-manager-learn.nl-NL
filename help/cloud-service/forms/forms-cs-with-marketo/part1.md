---
title: AEM Forms Cloud Service en Marketo integreren
description: Leer hoe u AEM Forms en Marketo kunt integreren met het AEM Forms-formuliergegevensmodel.
feature: Form Data Model,Integration
version: Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 0%

---

# AEM Forms en Marketo integreren

Marketo, een onderdeel van de Adobe, beschikt over software voor marketingautomatisering die is gericht op marketing op basis van account, zoals e-mail, mobiele, sociale, digitale advertenties, webbeheer en analyses.

Met het formuliergegevensmodel van AEM Forms kunnen we nu AEM formulier naadloos integreren met Marketo.

[ leer meer over het Model van de Gegevens van de Vorm ](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo maakt een REST API beschikbaar waarmee een groot aantal mogelijkheden van het systeem op afstand kan worden uitgevoerd. Er zijn veel opties, variërend van het maken van programma&#39;s tot het bulksgewijs importeren van leads, waarmee u een Marketo-instantie met fijnkorrelige besturing kunt besturen. Met behulp van het formuliergegevensmodel is het heel eenvoudig om AEM Forms te integreren met Marketo.

In deze zelfstudie worden de stappen besproken die nodig zijn voor de integratie van AEM Forms met Marketo met behulp van het formuliergegevensmodel. Bij het voltooien van de zelfstudie hebt u een OSGi-bundel die de aangepaste verificatie uitvoert tegen Marketo. U hebt ook de gegevensbron geconfigureerd met behulp van het meegeleverde wagerbestand.

Om te beginnen wordt het hoogst geadviseerd dat u met de volgende onderwerpen vertrouwd bent die in de sectie van de Vereiste worden vermeld.

## Vereiste

1. Toegang tot AEM Forms Cloud Service-instantie
1. Kennis hebben van formuliergegevensmodel
1. Basiskennis van wagerbestanden
1. Adaptieve Forms maken

**Geheime identiteitskaart van de Cliënt en Geheime Sleutel van de Cliënt**

De eerste stap bij de integratie van Marketo met AEM Forms is het verkrijgen van de API-referenties die nodig zijn om de REST-aanroepen met behulp van API uit te voeren. U hebt het volgende nodig

1. client_id
1. client_geheime
1. identity_end

[ Gelieve te volgen de officiële documentatie van Marketo om de bovengenoemde eigenschappen te krijgen.](https://developers.marketo.com/rest-api/) U kunt ook contact opnemen met de beheerder van uw Marketo-instantie.

**alvorens u** begint

* [De aan deze zelfstudie gerelateerde elementen downloaden en decomprimeren](assets/marketo.zip)

Het ZIP-bestand bevat het volgende:

1. marketo.json - Dit is het wagerbestand dat wordt gebruikt om de gegevensbron te configureren.
1. Zorg ervoor u het gastheerbezit in marketo.json verandert om aan uw instantie te richten Marketo

## Volgende stappen

[Source voor gegevens maken](./part2.md)
