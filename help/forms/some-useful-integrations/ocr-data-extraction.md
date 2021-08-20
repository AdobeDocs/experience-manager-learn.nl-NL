---
title: OCR-gegevensextractie
description: Gegevens uit door de overheid uitgegeven documenten extraheren om formulieren in te vullen.
feature: Barcoded Forms
version: 6.4,6.5
kt: 6679
topic: Ontwikkeling
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---



# OCR-gegevensextractie

Haal automatisch gegevens uit een groot aantal door de overheid uitgegeven documenten om uw aangepaste formulieren in te vullen.

Er zijn een aantal organisaties die deze service aanbieden en zolang deze beschikken over goed gedocumenteerde REST API&#39;s kunt u eenvoudig integreren met AEM Forms met behulp van de gegevensintegratiefunctie. In het kader van deze zelfstudie heb ik [ID Analyzer](https://www.idanalyzer.com/) gebruikt om de OCR-gegevensextractie van geüploade documenten te demonstreren.

De volgende stappen zijn uitgevoerd om de OCR-gegevensextractie te implementeren met AEM Forms via ID Analyzer-service.

## Ontwikkelaarsaccount maken

Maak een ontwikkelaarsaccount met [ID Analyzer](https://portal.idanalyzer.com/signin.html). Noteer de API-sleutel. Deze sleutel zal nodig zijn om REST API&#39;s van de dienst van de Analysator van identiteitskaart aan te halen.

## Swagger/OpenAPI-bestand maken

De OpenAPI-specificatie (voorheen Swagger Specification) is een API-beschrijvingsindeling voor REST API&#39;s. Met een OpenAPI-bestand kunt u de volledige API beschrijven, inclusief:

* Beschikbare eindpunten (/gebruikers) en verrichtingen op elk eindpunt (GET /users, POST /users)
* Operatieparameters Invoer en uitvoer voor elke bewerking
Verificatiemethoden
* Contactgegevens, licentie, gebruiksvoorwaarden en andere informatie.
* API-specificaties kunnen worden geschreven in YAML of JSON. De indeling is gemakkelijk te leren en kan zowel voor mensen als voor machines worden gelezen.

Volg de [OpenAPI-documentatie](https://swagger.io/docs/specification/2-0/basic-structure/) om uw eerste wagger/OpenAPI-bestand te maken

>[!NOTE]
> AEM Forms biedt ondersteuning voor OpenAPI Specification versie 2.0 (fka Swagger).

Met de [wagereditor](https://editor.swagger.io/) kunt u uw wagerbestand maken om de bewerkingen te beschrijven die OTP-code verzenden en verifiëren die met SMS is verzonden. Het wagerbestand kan in JSON- of YAML-indeling worden gemaakt. Het voltooide wagerbestand kan worden gedownload van [hier](assets/drivers-license-swagger.zip)

## Gegevensbron maken

Om AEM/AEM Forms met derdetoepassingen te integreren, moeten wij [gegevensbron ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) in de configuratie van de wolkendiensten creëren. Gebruik het [wagggerbestand](assets/drivers-license-swagger.zip) om uw gegevensbron te maken.

## Formuliergegevensmodel maken

AEM Forms-gegevensintegratie biedt een intuïtieve gebruikersinterface voor het maken van en werken met formuliergegevensmodellen](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). [ Baseer het model van vormgegevens op de gegevensbron die in de vroegere stap wordt gecreeerd.

![fdm](assets/test-dl-fdm.PNG)

## Clientbibliotheek maken

Er moet een tekenreeks met base64-codering van het geüploade document worden opgehaald. Deze base64 gecodeerde tekenreeks wordt vervolgens doorgegeven als een van de parameters van onze REST-aanroep.
De clientbibliotheek kan [hier worden gedownload.](assets/drivers-license-client-lib.zip)

## Adaptief formulier maken

Integreer de aanroepen van de POST van het formuliergegevensmodel met het aangepaste formulier om gegevens uit het geüploade document door de gebruiker in het formulier te extraheren. U kunt zelf een adaptief formulier maken en de POST-aanroep van het formuliergegevensmodel gebruiken om de tekenreeks met base64-codering van het geüploade document te verzenden.

## Distribueren op uw server

Voer de volgende stappen uit als u de voorbeeldbestanden met uw API-sleutel wilt gebruiken:

* [Download de gegevensbron ](assets/drivers-license-source.zip) en importeer deze naar AEM met  [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* [Download het ](assets/drivers-license-fdm.zip) formuliergegevensmodel en importeer het in AEM met  [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* [Clientbibliotheek downloaden](assets/drivers-license-client-lib.zip)
* Download het voorbeeldadaptieve formulier kan [hier worden gedownload](assets/adaptive-form-dl.zip). In dit voorbeeldformulier worden de serviceaanroepen van het formuliergegevensmodel gebruikt die als onderdeel van dit artikel worden aangeboden.
* Importeer het formulier in AEM vanuit de gebruikersinterface [Forms en Document UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Open het formulier in [bewerkingsmodus.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* Geef uw API-sleutel op als standaardwaarde in het veld Toepassen en sla uw wijzigingen op
* Open de regelredacteur voor Basis 64 gebied van het Koord. Let op de service-aanroep wanneer de waarde van dit veld wordt gewijzigd.
* Het formulier opslaan
* [Bekijk een voorbeeld van het formulier](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled) en upload een voorbeeld van uw rijbewijs


