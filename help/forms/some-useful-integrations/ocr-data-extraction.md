---
title: OCR-gegevensextractie
description: Gegevens uit door de overheid uitgegeven documenten extraheren om formulieren in te vullen.
feature: Barcoded Forms
version: 6.4,6.5
jira: KT-6679
topic: Development
role: Developer
level: Intermediate
exl-id: 1532a865-4664-40d9-964a-e64463b49587
last-substantial-update: 2019-07-07T00:00:00Z
duration: 145
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 0%

---

# OCR-gegevensextractie

Haal automatisch gegevens uit een groot aantal door de overheid uitgegeven documenten om uw aangepaste formulieren in te vullen.

Er zijn een aantal organisaties die deze service aanbieden en zolang deze beschikken over goed gedocumenteerde REST API&#39;s kunt u eenvoudig integreren met AEM Forms met behulp van de gegevensintegratiefunctie. Voor deze zelfstudie heb ik [ID-analyse](https://www.idanalyzer.com/) om de OCR-gegevensextractie van geüploade documenten aan te tonen.

De volgende stappen zijn uitgevoerd om de OCR-gegevensextractie te implementeren met AEM Forms via ID Analyzer-service.

## Ontwikkelaarsaccount maken

Een ontwikkelaarsaccount maken met [ID-analyse](https://portal.idanalyzer.com/signin.html). Noteer de API-sleutel. Deze sleutel is nodig om REST API&#39;s van de ID Analyzer-service aan te roepen.

## Swagger/OpenAPI-bestand maken

De OpenAPI-specificatie (voorheen Swagger Specification) is een API-beschrijvingsindeling voor REST API&#39;s. Met een OpenAPI-bestand kunt u de volledige API beschrijven, inclusief:

* Beschikbare eindpunten (/gebruikers) en verrichtingen op elk eindpunt (GET /users, POST /users)
* Operatieparameters Invoer en uitvoer voor elke bewerkingsverificatiemethode
* Contactgegevens, licentie, gebruiksvoorwaarden en andere informatie.
* API-specificaties kunnen worden geschreven in YAML of JSON. De indeling is gemakkelijk te leren en kan zowel voor mensen als voor machines worden gelezen.

Als u uw eerste wagger/OpenAPI-bestand wilt maken, volgt u de [OpenAPI-documentatie](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms ondersteunt OpenAPI Specification versie 2.0 (fka Swagger).

Gebruik de [wagenbewerker](https://editor.swagger.io/) om uw wagerbestand te maken om de bewerkingen te beschrijven die OTP-code verzenden en verifiëren die met SMS is verzonden. Het wagerbestand kan in JSON- of YAML-indeling worden gemaakt. Het voltooide wagerbestand kan worden gedownload van [hier](assets/drivers-license-swagger.zip)

## Overwegingen bij het definiëren van het wagerbestand

* Definities zijn vereist
* $ref moet worden gebruikt voor methodedefinities
* Voorkeur om te hebben verbruikt en secties bepaalt
* Definieer de hoofdparameters of responsparameters van inline-verzoeken niet. Probeer zo veel mogelijk te modulariseren. De volgende definitie wordt bijvoorbeeld niet ondersteund

```json
 "name": "body",
            "in": "body",
            "required": false,
            "schema": {
              "type": "object",
              "properties": {
                "Rollnum": {
                  "type": "string",
                  "description": "Rollnum"
                }
              }
            }
```

Het volgende wordt ondersteund met een verwijzing naar requestBody-definitie

```json
 "name": "requestBody",
            "in": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/requestBody"
            }
```

* [Voorbeeld van wagerbestand ter referentie](assets/sample-swagger.json)

## Gegevensbron maken

Om AEM/AEM Forms met derdetoepassingen te integreren, moeten wij [gegevensbron maken](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) in de configuratie van cloudservices. Gebruik de [wagenbestand](assets/drivers-license-swagger.zip) om uw gegevensbron te maken.

## Formuliergegevensmodel maken

AEM Forms-gegevensintegratie biedt een intuïtieve gebruikersinterface voor het maken van en werken met [formuliergegevensmodellen](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). Baseer het model van vormgegevens op de gegevensbron die in de vroegere stap wordt gecreeerd.

![fdm](assets/test-dl-fdm.PNG)

## Clientbibliotheek maken

Er moet een tekenreeks met base64-codering van het geüploade document worden opgehaald. Deze base64 gecodeerde tekenreeks wordt vervolgens doorgegeven als een van de parameters van onze REST-aanroep.
De clientbibliotheek kan worden gedownload [van hier.](assets/drivers-license-client-lib.zip)

## Adaptief formulier maken

Integreer de aanroepen van de POST van het formuliergegevensmodel met het aangepaste formulier om gegevens uit het geüploade document door de gebruiker in het formulier te extraheren. U kunt zelf een adaptief formulier maken en de POST-aanroep van het formuliergegevensmodel gebruiken om de tekenreeks met base64-codering van het geüploade document te verzenden.

## Distribueren op uw server

Voer de volgende stappen uit als u de voorbeeldbestanden met uw API-sleutel wilt gebruiken:

* [De gegevensbron downloaden](assets/drivers-license-source.zip) en importeren in AEM [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* [Het formuliergegevensmodel downloaden](assets/drivers-license-fdm.zip) en importeren in AEM [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* [Clientbibliotheek downloaden](assets/drivers-license-client-lib.zip)
* Download het voorbeeldadaptieve formulier kan [hier gedownload](assets/adaptive-form-dl.zip). In dit voorbeeldformulier worden de serviceaanroepen van het formuliergegevensmodel gebruikt die als onderdeel van dit artikel worden aangeboden.
* Het formulier importeren in AEM van de [UI Forms en Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Open het formulier in [bewerken.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* Geef uw API-sleutel op als standaardwaarde in het veld Toepassen en sla uw wijzigingen op
* Open de regelredacteur voor Basis 64 gebied van het Koord. Let op de service-aanroep wanneer de waarde van dit veld wordt gewijzigd.
* Het formulier opslaan
* [Een voorbeeld van het formulier bekijken](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), uploadt u een voorbeeld van uw rijbewijs
