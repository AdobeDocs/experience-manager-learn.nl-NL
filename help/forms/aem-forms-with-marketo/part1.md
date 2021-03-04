---
title: AEM Forms met Marketo (Deel 1)
seo-title: AEM Forms met Marketo (Deel 1)
description: Zelfstudie over de integratie van AEM Forms met Marketo met behulp van het AEM Forms-formuliergegevensmodel.
seo-description: Zelfstudie over de integratie van AEM Forms met Marketo met behulp van het AEM Forms-formuliergegevensmodel.
feature: adaptieve formulieren, formuliergegevensmodel
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 0%

---


# AEM Forms met Marketo

Marketo, een onderdeel van Adobe, biedt software voor marketingautomatisering die is gericht op marketing op basis van account, zoals e-mail, mobiele, sociale, digitale advertenties, webbeheer en analyses.

Met het formuliergegevensmodel van AEM Forms kunnen we nu AEM formulier naadloos integreren met Marketo.

[Meer informatie over het formuliergegevensmodel](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo maakt een REST API beschikbaar die voor verre uitvoering van vele mogelijkheden van het systeem toestaat. Van het creëren van programma&#39;s aan bulklood de invoer, zijn er vele opties die fijnkorrelige controle van een instantie van Marketo toestaan. Met behulp van het formuliergegevensmodel is het heel eenvoudig om AEM Forms te integreren met Marketo.

Deze zelfstudie begeleidt u door de stappen die nodig zijn voor de integratie van AEM Forms met Marketo met behulp van het formuliergegevensmodel. Bij de voltooiing van het leerprogramma zult u een bundel OSGi hebben die de douaneauthentificatie tegen Marketo zal doen. U hebt ook de gegevensbron geconfigureerd met behulp van het meegeleverde wagerbestand.

Om te beginnen wordt het hoogst geadviseerd dat u met de volgende onderwerpen vertrouwd bent die in de sectie van de Vereiste worden vermeld.

## Vereiste

1. [AEM server met AEM Forms Add op pakket geïnstalleerd](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. Ontwikkelomgeving voor lokale AEM
1. Kennis hebben van formuliergegevensmodel
1. Basiskennis van wagerbestanden
1. Adaptieve Forms maken

**Clientgeheim-id en clientgeheim-sleutel**

De eerste stap in de integratie van Marketo met AEM Forms is het verkrijgen van de API geloofsbrieven die nodig zijn om de REST vraag te maken gebruikend API. U hebt het volgende nodig

1. client_id
1. client_geheime
1. identity_end
1. verificatie-URL.

[Volg de officiële Marketo-documentatie om de bovengenoemde eigenschappen te verkrijgen.](https://developers.marketo.com/rest-api/) U kunt ook contact opnemen met de beheerder van de Marketo-instantie.

**Voordat u begint**

[Download en decomprimeer de middelen die aan dit artikel zijn gekoppeld.](assets/aemformsandmarketo.zip) Het ZIP-bestand bevat het volgende:

1. BlankTemplatePackage.zip - dit is het Adaptieve malplaatje van de Vorm. Importeer dit met het pakketbeheer.
1. marketo.json - Dit is het wagerbestand dat wordt gebruikt om de gegevensbron te configureren.
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar - Dit is de bundel die de douaneauthentificatie doet. U kunt dit gratis gebruiken als u de zelfstudie niet kunt voltooien of als uw bundel niet naar behoren werkt.
