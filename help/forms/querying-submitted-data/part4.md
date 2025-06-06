---
title: AEM Forms met JSON-schema en -gegevens[Deel4]
description: Zelfstudie met meerdere onderdelen om u door de stappen te laten lopen die nodig zijn voor het maken van een adaptief formulier met JSON-schema en het opvragen van de verzonden gegevens.
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: a8d8118d-f4a1-483f-83b4-77190f6a42a4
duration: 99
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# Ingediende gegevens opvragen


De volgende stap is de voorgelegde gegevens te vragen en de resultaten in tabelvorm te tonen. Hiervoor gebruiken we de volgende software:

[ QueryBuilder ](https://querybuilder.js.org/) - component UI om vragen tot stand te brengen

[ Lijsten van Gegevens ](https://datatables.net/) - om de vraagresultaten in tabelvorm te tonen.

De volgende UI werd gebouwd om het vragen van de voorgelegde gegevens toe te laten. Alleen de elementen die zijn gemarkeerd als vereist in het JSON-schema, worden beschikbaar gesteld voor query. In het onderstaande screenshot vragen we om alle inzendingen waar de bezorgpref SMS is.

De steekproef UI om de voorgelegde gegevens te vragen gebruikt niet alle geavanceerde mogelijkheden beschikbaar in QueryBuilder. Je wordt aangemoedigd om ze zelf uit te proberen.

![ querybuilder ](assets/querybuilderui.gif)

>[!NOTE]
>
>De huidige versie van deze zelfstudie ondersteunt het opvragen van meerdere kolommen niet.

Wanneer u een formulier selecteert om uw query uit te voeren, wordt een GET-aanroep uitgevoerd naar **/bin/getdatakeysfromschema** . Deze GET-aanroep retourneert de vereiste velden die zijn gekoppeld aan het schema van het formulier. De vereiste gebieden worden dan bevolkt in de drop-down lijst van QueryBuilder voor u om de vraag te bouwen.

Het volgende codefragment maakt een vraag aan de methode getRequiredColumnsFromSchema van de dienst JSONSchemaOperations. Wij gaan de eigenschappen en de vereiste elementen van het schema tot deze methodevraag over. De serie die door deze functievraag is teruggekeerd wordt dan gebruikt om de drop-down lijst van de vraagbouwer te bevolken

```java
public JSONArray getData(String formName) throws SQLException, IOException {

  org.json.JSONArray arrayOfDataKeys = new org.json.JSONArray();
  JSONObject jsonSchema = jsonSchemaOperations.getJSONSchemaFromDataBase(formName);
  Map<String, String> refKeys = new HashMap<String, String>();

  try {
   JSONObject properties = jsonSchema.getJSONObject("properties");
   JSONArray requiredFields = jsonSchema.has("required") ? jsonSchema.getJSONArray("required") : null;
   jsonSchemaOperations.getRequiredColumnsFromSchema(properties, arrayOfDataKeys, "", jsonSchema, refKeys,
     requiredFields);
  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return arrayOfDataKeys;

 }
```

Wanneer GetResult knoop wordt geklikt krijgt vraag wordt gemaakt aan **&quot;/bin/querydata&quot;**. Wij gaan de vraag door VraagBuilder UI aan servlet door de vraagparameter wordt gebouwd die over. De servlet massages deze vraag in SQL vraag dan die kan worden gebruikt om het gegevensbestand te vragen. Als u bijvoorbeeld alle producten met de naam &#39;Mouse&#39; wilt ophalen, is de queryreeks van Query Builder `$.productname = 'Mouse'` . Deze query wordt vervolgens omgezet in de volgende code

SELECTEER &#42; vanuit aemformswithjson.  formulieren waarbij JSON_EXTRACT( formsubmission.formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;

Het resultaat van deze query wordt vervolgens geretourneerd om de tabel in de gebruikersinterface te vullen.

Voer de volgende stappen uit om dit voorbeeld op uw lokale systeem uit te voeren

1. [ zorg ervoor u alle hier vermelde stappen hebt gevolgd ](part2.md)
1. [ voer Dashboardv2.zip in gebruikend de Manager van het Pakket van AEM.](assets/dashboardv2.zip) Dit pakket bevat alle benodigde bundels, configuratie-instellingen, aangepaste verzendpagina en voorbeeldpagina voor querygegevens.
1. Een adaptief formulier maken met het JSON-voorbeeldschema
1. Configureer het adaptieve formulier voor verzending naar de aangepaste verzendactie &quot;custom submit&quot;
1. Het formulier invullen en verzenden
1. Punt uw browser aan [ dashboard.html ](http://localhost:4502/content/AemForms/dashboard.html)
1. Selecteer het formulier en voer een eenvoudige query uit
