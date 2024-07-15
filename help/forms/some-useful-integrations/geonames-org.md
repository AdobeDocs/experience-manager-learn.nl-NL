---
title: Cascading drop-down-lists
description: Vul vervolgkeuzelijsten in op basis van een vorige keuzelijst.
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-9724
topic: Development
role: Developer
level: Intermediate
exl-id: f1f2cacc-9ec4-46d6-a6af-dac3f663de78
last-substantial-update: 2021-02-07T00:00:00Z
duration: 185
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 0%

---

# Cascading vervolgkeuzelijsten

Een trapsgewijze drop-down lijst is een reeks afhankelijke controles DropDownList waarin één controle DropDownList van de ouder of vorige controles DropDownList afhangt. De punten in de controle DropDownList worden bevolkt gebaseerd op een punt dat door de gebruiker van een andere controle DropDownList wordt geselecteerd.

## Bewijs van de gebruikszaak

>[!VIDEO](https://video.tv.adobe.com/v/340344?quality=12&learn=on)

Voor dit leerprogramma, heb ik [ Geonames REST API ](https://www.geonames.org/export/web-services.html) gebruikt om dit vermogen aan te tonen.
Er zijn een aantal organisaties die dit soort service aanbieden en zolang ze beschikken over goed gedocumenteerde REST API&#39;s kunt u eenvoudig integreren met AEM Forms met behulp van de mogelijkheden voor gegevensintegratie

De volgende stappen zijn uitgevoerd voor het implementeren van trapsgewijze vervolgkeuzelijsten in AEM Forms

## Ontwikkelaarsaccount maken

Creeer een ontwikkelaarrekening met [ Geonames ](https://www.geonames.org/login). Noteer de gebruikersnaam. Deze gebruikersnaam is nodig om REST API&#39;s van de map geonames.org aan te roepen.

## Swagger/OpenAPI-bestand maken

De OpenAPI-specificatie (voorheen Swagger Specification) is een API-beschrijvingsindeling voor REST API&#39;s. Met een OpenAPI-bestand kunt u de volledige API beschrijven, inclusief:

* Beschikbare eindpunten (/gebruikers) en verrichtingen op elk eindpunt (GET /users, POST /users)
* Operatieparameters Invoer en uitvoer voor elke bewerking
Verificatiemethoden
* Contactgegevens, licentie, gebruiksvoorwaarden en andere informatie.
* API-specificaties kunnen worden geschreven in YAML of JSON. De indeling is gemakkelijk te leren en kan zowel voor mensen als voor machines worden gelezen.

Om uw eerste swagger/OpenAPI dossier tot stand te brengen, te volgen gelieve de [ documentatie OpenAPI ](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms ondersteunt OpenAPI Specification versie 2.0 (FKA Swagger).

Gebruik de [ kwikredacteur ](https://editor.swagger.io/) om uw kwikdossier tot stand te brengen om de verrichtingen te beschrijven die alle landen en kindelementen van het land of de staat halen. Het wagerbestand kan in JSON- of YAML-indeling worden gemaakt.

## Gegevensbronnen maken

Om AEM/AEM Forms met derdetoepassingen te integreren, moeten wij [ gegevensbron ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) in de configuratie van de wolkendiensten tot stand brengen. Gelieve te gebruiken de [ kwikdossiers ](assets/geonames-swagger-files.zip) om uw gegevensbronnen tot stand te brengen.
U moet twee gegevensbronnen maken (één om alle landen en andere op te halen om onderliggende elementen op te halen)


## Formuliergegevensmodel maken

De gegevensintegratie van AEM Forms verstrekt een intuïtief gebruikersinterface om tot stand te brengen en met [ modellen van vormgegevens ](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html) te werken. Baseer het model van vormgegevens op de gegevensbronnen die in de vroegere stap worden gecreeerd. Formuliergegevensmodel met twee gegevensbronnen

![ fdm ](assets/geonames-fdm.png)


## Adaptief formulier maken

Integreer de GET-aanroepen van het formuliergegevensmodel met het aangepaste formulier om de vervolgkeuzelijsten te vullen.
Maak een adaptief formulier met twee vervolgkeuzelijsten. Eén om de landen weer te geven en één om de staten/provincies op te nemen, afhankelijk van het geselecteerde land.

### Vervolgkeuzelijst Landen vullen

De lijst met landen wordt gevuld wanneer het formulier voor het eerst wordt geïnitialiseerd. Het volgende het schermschot toont u de regelredacteur die wordt gevormd om de opties van de landdrop-down lijst te bevolken. Dit werkt alleen als u uw gebruikersnaam de account met de geonames opgeeft.
![ get-countries ](assets/get-countries-rule-editor.png)

#### De vervolgkeuzelijst Staat/provincie vullen

De vervolgkeuzelijst Staat/provincie moet worden ingevuld op basis van het geselecteerde land. Het volgende scherm-schot toont u de configuratie van de regelredacteur
![ staat-provincie-opties ](assets/state-province-options.png)

### Uitoefening

Voeg twee vervolgkeuzelijsten met de naam provincies en steden toe in het formulier om de provincies en steden weer te geven op basis van het geselecteerde land en de geselecteerde staat/provincie.
![ oefening ](assets/cascading-drop-down-exercise.png)


### Voorbeeld-Assets

U kunt de volgende elementen downloaden om een begin te maken met het maken van het trapsgewijze vervolgkeuzelijstvoorbeeld
De voltooide swaggerdossiers kunnen van [ hier ](assets/geonames-swagger-files.zip) worden gedownload
De kwikbestanden beschrijven de volgende REST API
* [ krijgt Alle Landen ](https://secure.geonames.org/countryInfoJSON?username=yourusername)
* [ krijgt Kinderen van voorwerp Geoname ](https://secure.geonames.org/children?formatted=true&amp;geonameId=6252001&amp;username=yourusername)

Het voltooide [ Model van de Gegevens van de Vorm kan van hier worden gedownload ](assets/geonames-api-form-data-model.zip)
