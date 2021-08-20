---
title: AEM Forms met Marketo (Deel 3)
description: Zelfstudie voor de integratie van AEM Forms met Marketo met behulp van het AEM Forms-formuliergegevensmodel.
feature: Adaptief Forms, formuliergegevensmodel
version: 6.3,6.4,6.5
topic: Ontwikkeling
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '385'
ht-degree: 0%

---


# Gegevensbron configureren

Met AEM Forms Data Integration kunt u verschillende gegevensbronnen configureren en verbinden. De volgende types worden gesteund uit-van-de-doos. Met een kleine aanpassing kunt u echter ook integreren met andere gegevensbronnen.

1. Relationele databases - MySQL, Microsoft SQL Server, IBM DB2 en Oracle RDBMS
1. Gebruikersprofiel AEM
1. RESTful-webservices
1. SOAP-webservices
1. OData-diensten

Voor de integratie van AEM Forms met Marketo gebruiken we RESTful-webservices. De eerste stap in het integreren is een [gegevensbron te vormen.](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) Gebruik het wagerbestand dat u opgeeft als onderdeel van deze zelfstudie. De volgende schermafbeelding toont u de belangrijke eigenschappen die moeten worden opgegeven tijdens het configureren van de gegevensbron.
![gegevensbron](assets/datasource.jfif)

Het bestand &quot;marketo.json&quot; is het gumerbestand en wordt geleverd als onderdeel van de elementen van deze zelfstudie.
De Host van het bezit is specifiek voor uw instantie van Marketo.
Verificatietype is aangepast en verificatie-implementatie moet overeenkomen met &#39;AemForms with Marketo&#39;. (Tenzij u dit in uw code hebt gewijzigd).

## Formuliergegevensmodel maken

Na, vormend de gegevensbron is de volgende stap een Model van de Gegevens van de Vorm te creëren dat op de gegevensbron gebaseerd is die in de vroegere stap wordt gevormd. Voer de volgende stappen uit om een formuliergegevensmodel te maken:

Wijs uw browser naar de pagina [gegevensintegratie.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Dit maakt een lijst van alle gegevensintegratie die op uw AEM instantie wordt gecreeerd.

1. Klik op Maken | Formuliergegevensmodel
1. Geef een betekenisvolle titel op, zoals FormsAndMarketo, en klik op Volgende
1. Selecteer de gegevensbron die in de vorige stap is geconfigureerd en klik op Maken en bewerken om het formuliergegevensmodel te openen in de bewerkingsmodus
1. Vouw het knooppunt &quot;FormsAndMarketo&quot; uit. Vouw het knooppunt Services uit
1. Selecteer de eerste bewerking &quot;Ophalen&quot;
1. Klik op Geselecteerde toevoegen
1. Klik op Alles selecteren in het dialoogvenster &quot;Gekoppelde modelobjecten toevoegen&quot; en klik vervolgens op Toevoegen
1. Sla het formuliergegevensmodel op door op de knop Opslaan te klikken
1. Tab naar het tabblad Services
1. Selecteer de enige dienst die wordt vermeld en klik op de Dienst van de Test
1. Geef een geldige leadId op en klik op Testen. Als alles goed gaat, moet u de hoofddetails terugkrijgen, zoals hieronder in de schermafbeelding wordt getoond
   ![testresultaten](assets/testresults.jfif)
