---
title: AEM Forms met JSON-schema en -gegevens[Deel 1]
seo-title: AEM Forms met JSON-schema en -gegevens[Deel1]
description: Zelfstudie met meerdere onderdelen om u door de stappen te laten lopen die nodig zijn voor het maken van een adaptief formulier met JSON-schema en het opvragen van de verzonden gegevens.
seo-description: Zelfstudie met meerdere onderdelen om u door de stappen te laten lopen die nodig zijn voor het maken van een adaptief formulier met JSON-schema en het opvragen van de verzonden gegevens.
feature: Adaptieve Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Ontwikkeling
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---


# Adaptief formulier maken op basis van JSON-schema


De mogelijkheid om adaptieve Forms te maken op basis van JSON-schema is geïntroduceerd met AEM Forms 6.3-release. De details over het maken van Adaptive Forms met JSON-schema worden in dit [artikel](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html) gedetailleerd uitgelegd.

Als u een adaptief formulier hebt gemaakt op basis van het JSON-schema, moet u de verzonden gegevens in de database opslaan. Hiervoor gebruiken we het nieuwe JSON-gegevenstype dat door verschillende databaseleveranciers is geïntroduceerd. Voor dit artikel zullen wij MySql 8 gegevensbestand gebruiken om de voorgelegde gegevens op te slaan.

MySql 8 database is gebruikt voor dit artikel. MySQL introduceerde een nieuw gegevenstype genoemd [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). Hierdoor is het gemakkelijker om JSON-objecten op te slaan en te zoeken. Wij zullen de voorgelegde gegevens in een kolom van type JSON in ons gegevensbestand opslaan.

De volgende schermafbeelding toont de verzonden formuliergegevens die zijn opgeslagen in het gegevenstype JSON. De kolom &quot;formdata&quot; is van het type JSON. We hebben ook de naam opgeslagen van het formulier dat is gekoppeld aan de gegevens in de naam van de kolomnotatie

>[!NOTE]
>
>Zorg ervoor dat het json-schemabestand een juiste naam heeft. De naam van het bestand moet bijvoorbeeld de volgende notatie &lt;name>schema.json hebben. Uw schemabestand kan dus hypotheek.schema.json of credit.schema.json zijn.


![datastast](assets/datastored.gif)


[Voorbeeld-JSON-schema&#39;s die kunnen worden gebruikt om Adaptieve Forms te maken.](assets/samplejsonschemas.zip). Het ZIP-bestand downloaden en uitpakken om de JSON-schema&#39;s op te halen

