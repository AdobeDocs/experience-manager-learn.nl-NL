---
title: OCR-gegevensextractie
description: Gegevens uit door de overheid uitgegeven documenten extraheren om formulieren in te vullen.
feature: Barcoded Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6679
topic: Development
role: Developer
level: Intermediate
exl-id: 1532a865-4664-40d9-964a-e64463b49587
last-substantial-update: 2019-07-07T00:00:00Z
duration: 145
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 0%

---

# OCR-gegevensextractie

Haal automatisch gegevens uit een groot aantal door de overheid uitgegeven documenten om uw aangepaste formulieren in te vullen.

Er zijn een aantal organisaties die deze service aanbieden en zolang deze beschikken over goed gedocumenteerde REST API&#39;s kunt u eenvoudig integreren met AEM Forms met behulp van de gegevensintegratiefunctie. Voor dit leerprogramma, heb ik [ Analysator van identiteitskaart ](https://www.idanalyzer.com/) gebruikt om de OCR gegevensextractie van geüploade documenten aan te tonen.

De volgende stappen zijn uitgevoerd om de OCR-gegevensextractie te implementeren met AEM Forms via ID Analyzer-service.

## Ontwikkelaarsaccount maken

Creeer een ontwikkelaarrekening met [ Analysator van identiteitskaart ](https://portal.idanalyzer.com/signin.html). Noteer de API-sleutel. Deze sleutel is nodig om REST API&#39;s van de ID Analyzer-service aan te roepen.

## Swagger/OpenAPI-bestand maken

De OpenAPI-specificatie (voorheen Swagger Specification) is een API-beschrijvingsindeling voor REST API&#39;s. Met een OpenAPI-bestand kunt u de volledige API beschrijven, inclusief:

* Beschikbare eindpunten (/gebruikers) en verrichtingen op elk eindpunt (GET /users, POST /users)
* Operatieparameters Invoer en uitvoer voor elke bewerking
Verificatiemethoden
* Contactgegevens, licentie, gebruiksvoorwaarden en andere informatie.
* API-specificaties kunnen worden geschreven in YAML of JSON. De indeling is gemakkelijk te leren en kan zowel voor mensen als voor machines worden gelezen.

Om uw eerste swagger/OpenAPI dossier tot stand te brengen, te volgen gelieve de [ documentatie OpenAPI ](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms ondersteunt OpenAPI Specification versie 2.0 (fka Swagger).

Gebruik de [ kwikredacteur ](https://editor.swagger.io/) om uw kwikdossier tot stand te brengen om de verrichtingen te beschrijven die OTP verzonden code verzenden en verifiëren gebruikend SMS. Het wagerbestand kan in JSON- of YAML-indeling worden gemaakt. Het voltooide dossier van de wagger kan van [ hier ](assets/drivers-license-swagger.zip) worden gedownload

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

## Source voor gegevens maken

Om AEM/AEM Forms met derdetoepassingen te integreren, moeten wij [ gegevensbron ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) in de configuratie van de wolkendiensten tot stand brengen. Gelieve te gebruiken het [ wagerdossier ](assets/drivers-license-swagger.zip) om uw gegevensbron tot stand te brengen.

## Formuliergegevensmodel maken

De gegevensintegratie van AEM Forms verstrekt een intuïtief gebruikersinterface om tot stand te brengen en met [ modellen van vormgegevens ](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html) te werken. Baseer het model van vormgegevens op de gegevensbron die in de vroegere stap wordt gecreeerd.

![ fdm ](assets/test-dl-fdm.PNG)

## Clientbibliotheek maken

Er moet een tekenreeks met base64-codering van het geüploade document worden opgehaald. Deze base64 gecodeerde tekenreeks wordt vervolgens doorgegeven als een van de parameters van onze REST-aanroep.
De cliëntbibliotheek kan [ van hier worden gedownload.](assets/drivers-license-client-lib.zip)

## Adaptief formulier maken

Integreer de POST-aanroepen van het formuliergegevensmodel met het aangepaste formulier om gegevens uit het geüploade document door de gebruiker in het formulier te extraheren. U kunt zelf een adaptief formulier maken en de POST-aanroep van het formuliergegevensmodel gebruiken om de base64-gecodeerde tekenreeks van het geüploade document te verzenden.

## Distribueren op uw server

Voer de volgende stappen uit als u de voorbeeldbestanden met uw API-sleutel wilt gebruiken:

* [ Download de gegevensbron ](assets/drivers-license-source.zip) en de invoer in AEM gebruikend [ pakketmanager ](http://localhost:4502/crx/packmgr/index.jsp)
* [ Download het model van vormgegevens ](assets/drivers-license-fdm.zip) en de invoer in AEM gebruikend [ pakketmanager ](http://localhost:4502/crx/packmgr/index.jsp)
* [Clientbibliotheek downloaden](assets/drivers-license-client-lib.zip)
* Download de steekproef adaptieve vorm kan [ van hier worden gedownload ](assets/adaptive-form-dl.zip). In dit voorbeeldformulier worden de serviceaanroepen van het formuliergegevensmodel gebruikt die als onderdeel van dit artikel worden aangeboden.
* De vorm van de invoer in AEM van [ Forms en Document UI ](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Open de vorm op [ geef wijze uit.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* Geef uw API-sleutel op als standaardwaarde in het veld Toepassen en sla uw wijzigingen op
* Open de regelredacteur voor Basis 64 gebied van het Koord. Let op de service-aanroep wanneer de waarde van dit veld wordt gewijzigd.
* Het formulier opslaan
* [ Voorproef de vorm ](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), upload voorbeeld van uw bestuurdersvergunning
