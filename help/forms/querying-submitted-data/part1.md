---
title: AEM Forms met JSON-schema en -gegevens[Deel 1]
description: Zelfstudie met meerdere onderdelen om u door de stappen te laten lopen die nodig zijn voor het maken van een adaptief formulier met JSON-schema en het opvragen van de verzonden gegevens.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: c588bdca-b8a8-4de2-97e0-ba08b195699f
duration: 50
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Adaptief formulier maken op basis van JSON-schema


De mogelijkheid om adaptieve Forms te maken op basis van JSON-schema is geïntroduceerd met AEM Forms 6.3-release. De details over het maken van Adaptive Forms met JSON-schema worden in dit detail uitgelegd [artikel](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/adaptive-form-json-schema-form-model.html).

Als u een adaptief formulier hebt gemaakt op basis van het JSON-schema, moet u de verzonden gegevens in de database opslaan. Hiervoor gebruiken we het nieuwe JSON-gegevenstype dat door verschillende databaseleveranciers is geïntroduceerd. Voor dit artikel zullen wij MySql 8 gegevensbestand gebruiken om de voorgelegde gegevens op te slaan.

MySql 8 database is gebruikt voor dit artikel. MySQL introduceerde een nieuw gegevenstype genaamd [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). Hierdoor is het gemakkelijker om JSON-objecten op te slaan en te zoeken. We slaan de verzonden gegevens op in een kolom van het type JSON in onze database.

De volgende schermafbeelding toont de verzonden formuliergegevens die zijn opgeslagen in het gegevenstype JSON. De kolom &quot;formdata&quot; is van het type JSON. We hebben ook de naam opgeslagen van het formulier dat is gekoppeld aan de gegevens in de naam van de kolomindeling

>[!NOTE]
>
>Zorg ervoor dat het json-schemabestand een juiste naam heeft. De naam moet bijvoorbeeld de volgende notatie hebben &lt;name>schema.json. Uw schemabestand kan dus hypotheek.schema.json of credit.schema.json zijn.


![datastast](assets/datastored.gif)


[Voorbeeld-JSON-schema&#39;s die kunnen worden gebruikt om Adaptieve Forms te maken.](assets/samplejsonschemas.zip). Download en decomprimeer het ZIP-bestand om de JSON-schema&#39;s op te halen
