---
title: Cascading drop-down-lists
description: Vul vervolgkeuzelijsten in op basis van een vorige keuzelijst.
feature: Adaptive Forms
version: 6.4,6.5
kt: 9724
topic: Development
role: Developer
level: Intermediate
source-git-commit: 15b57ec6792bc47d0041946014863b13867adf22
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 0%

---

# Cascading vervolgkeuzelijsten

Een trapsgewijze drop-down lijst is een reeks afhankelijke controles DropDownList waarin één controle DropDownList van de ouder of vorige controles DropDownList afhangt. De punten in de controle DropDownList worden bevolkt gebaseerd op een punt dat door de gebruiker van een andere controle DropDownList wordt geselecteerd.

## Bewijs van de gebruikszaak

>[!VIDEO](https://video.tv.adobe.com/v/340344?quality=9&learn=on)

Voor deze zelfstudie heb ik [Geonames REST API](http://api.geonames.org/) om deze bekwaamheid aan te tonen.
Er zijn een aantal organisaties die dit soort service aanbieden en zolang ze beschikken over goed gedocumenteerde REST API&#39;s kunt u eenvoudig integreren met AEM Forms met behulp van de mogelijkheden voor gegevensintegratie

De volgende stappen zijn uitgevoerd voor het implementeren van trapsgewijze vervolgkeuzelijsten in AEM Forms

## Ontwikkelaarsaccount maken

Een ontwikkelaarsaccount maken met [Geonames](https://www.geonames.org/login). Noteer de gebruikersnaam. Deze gebruikersnaam is nodig om REST API&#39;s van de geonames.org aan te roepen.

## Swagger/OpenAPI-bestand maken

De OpenAPI-specificatie (voorheen Swagger Specification) is een API-beschrijvingsindeling voor REST API&#39;s. Met een OpenAPI-bestand kunt u de volledige API beschrijven, inclusief:

* Beschikbare eindpunten (/gebruikers) en verrichtingen op elk eindpunt (GET /users, POST /users)
* Operatieparameters Invoer en uitvoer voor elke bewerkingsverificatiemethode
* Contactgegevens, licentie, gebruiksvoorwaarden en andere informatie.
* API-specificaties kunnen worden geschreven in YAML of JSON. De indeling is gemakkelijk te leren en kan zowel voor mensen als voor machines worden gelezen.

Als u uw eerste wagger/OpenAPI-bestand wilt maken, volgt u de [OpenAPI-documentatie](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms biedt ondersteuning voor OpenAPI Specification versie 2.0 (FKA Swagger).

Gebruik de [wageneditor](https://editor.swagger.io/) om uw wagerbestand te maken waarin de bewerkingen worden beschreven die alle landen en onderliggende elementen van het land of de staat ophalen. Het wagerbestand kan in JSON- of YAML-indeling worden gemaakt. Het voltooide wagerbestand kan worden gedownload van [hier](assets/swagger-files.zip)
De kwikbestanden beschrijven de volgende REST API
* [Alle landen ophalen](http://api.geonames.org/countryInfoJSON?username=yourusername)
* [Onderliggende items van Geoname-object ophalen](http://api.geonames.org/childrenJSON?formatted=true&amp;geonameId=6252001&amp;username=yourusername)

## Gegevensbronnen maken

Om AEM/AEM Forms met derdetoepassingen te integreren, moeten wij [gegevensbron maken](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) in de configuratie van cloudservices. Gebruik de [wagenbestanden](assets/swagger-files.zip) om uw gegevensbronnen te maken.
U moet twee gegevensbronnen maken (één om alle landen en andere op te halen om onderliggende elementen op te halen)


## Formuliergegevensmodel maken

AEM Forms-gegevensintegratie biedt een intuïtieve gebruikersinterface voor het maken van en werken met [formuliergegevensmodellen](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). Baseer het model van vormgegevens op de gegevensbronnen die in de vroegere stap worden gecreeerd. Formuliergegevensmodel met twee gegevensbronnen

![fdm](assets/geonames-fdm.png)


## Adaptief formulier maken

Integreer de GET-aanroepen van het formuliergegevensmodel met het aangepaste formulier om de vervolgkeuzelijsten te vullen.
Maak een adaptief formulier met twee vervolgkeuzelijsten. Eén om de landen weer te geven en één om de staten/provincies op te nemen, afhankelijk van het geselecteerde land.

### Vervolgkeuzelijst Landen vullen

De lijst met landen wordt gevuld wanneer het formulier voor het eerst wordt geïnitialiseerd. Het volgende het schermschot toont u de regelredacteur die wordt gevormd om de opties van de landdrop-down lijst te bevolken. Dit werkt alleen als u uw gebruikersnaam de account met de geonames opgeeft.
![landen](assets/get-countries-rule-editor.png)

#### De vervolgkeuzelijst Staat/provincie vullen

De vervolgkeuzelijst Staat/provincie moet worden ingevuld op basis van het geselecteerde land. Het volgende scherm-schot toont u de configuratie van de regelredacteur
![provincie-opties](assets/state-province-options.png)

### Uitoefening

Voeg twee vervolgkeuzelijsten met de naam provincies en steden toe in het formulier om de provincies en steden weer te geven op basis van het geselecteerde land en de geselecteerde staat/provincie.
![oefening](assets/cascading-drop-down-exercise.png)




