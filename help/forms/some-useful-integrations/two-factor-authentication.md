---
title: Verificatie van SMS Twee factoren
description: Voeg een extra laag van veiligheid toe helpen de identiteit van een gebruiker bevestigen wanneer zij bepaalde activiteiten willen uitvoeren
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6317
translation-type: tm+mt
source-git-commit: 4c08b09f59be0eb6644aaec729807b92bc339e82
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---



# Gebruikers verifiëren met hun mobiele telefoonnummers

SMS Two Factor Authentication (Dual Factor Authentication) is een veiligheidsverificatieprocedure, die wordt geactiveerd door een gebruiker die zich aanmeldt bij een website, software of toepassing. Tijdens het aanmeldingsproces wordt de gebruiker automatisch een SMS-bericht naar zijn mobiele nummer gestuurd dat een unieke numerieke code bevat.

Er zijn een aantal organisaties die deze service aanbieden en zolang deze beschikken over goed gedocumenteerde REST API&#39;s kunt u AEM Forms eenvoudig integreren met de mogelijkheden voor gegevensintegratie van AEM Forms. In het kader van deze zelfstudie heb ik [Nexmo](https://developer.nexmo.com/verify/overview) gebruikt om de SMS 2FA-gebruikszaak aan te tonen.

De volgende stappen werden gevolgd om SMS 2FA met AEM Forms uit te voeren gebruikend de dienst van Nexmo Verify.

## Ontwikkelaarsaccount maken

Maak een ontwikkelaarsaccount met [Nexmo](https://dashboard.nexmo.com/sign-in). Noteer de API-sleutel en de API-beveiligingssleutel. Deze toetsen zijn nodig om REST API&#39;s van de Nexmo-service aan te roepen.

## Swagger/OpenAPI-bestand maken

De OpenAPI-specificatie (voorheen Swagger Specification) is een API-beschrijvingsindeling voor REST API&#39;s. Met een OpenAPI-bestand kunt u de volledige API beschrijven, inclusief:

* Beschikbare eindpunten (/gebruikers) en verrichtingen op elk eindpunt (GET /users, POST /users)
* Operatieparameters Invoer en uitvoer voor elke bewerkingsverificatiemethoden
* Contactgegevens, licentie, gebruiksvoorwaarden en andere informatie.
* API-specificaties kunnen worden geschreven in YAML of JSON. De indeling is gemakkelijk te leren en kan zowel voor mensen als voor machines worden gelezen.

Volg de documentatie van [OpenAPI om uw eerste wagger/OpenAPI-bestand te maken](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms biedt ondersteuning voor OpenAPI Specification versie 2.0 (fka Swagger).

Met de [wagereditor](https://editor.swagger.io/) kunt u uw wagerbestand maken om de bewerkingen te beschrijven die OTP-code verzenden en verifiëren die met SMS is verzonden. Het wagerbestand kan in JSON- of YAML-indeling worden gemaakt. Het voltooide wagerbestand kan [hier worden gedownload](assets/two-factore-authentication-swagger.zip)

## Gegevensbron maken

Om AEM/AEM Forms met derdetoepassingen te integreren, moeten wij gegevensbron [in de configuratie van de wolkendiensten](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) creëren.

## Formuliergegevensmodel maken

AEM Forms-gegevensintegratie biedt een intuïtieve gebruikersinterface voor het maken van en werken met [formuliergegevensmodellen](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Een formuliergegevensmodel is gebaseerd op gegevensbronnen voor gegevensuitwisseling.
Het ingevulde formuliergegevensmodel kan hier worden [gedownload](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## Adaptief formulier maken

Integreer de aanroepen van de POST van het formuliergegevensmodel met het aangepaste formulier om te controleren welk mobiele telefoonnummer de gebruiker in het formulier heeft ingevoerd. U kunt zelf een adaptief formulier maken en de POST-aanroeping van het formuliergegevensmodel gebruiken om OTP-code te verzenden en te verifiëren naar wens.

Voer de volgende stappen uit als u de voorbeeldbestanden met uw API-toetsen wilt gebruiken:

* [Download het formuliergegevensmodel](assets/sms-2fa-fdm.zip) en importeer het in AEM met [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* Download het voorbeeldadaptieve formulier dat u hier kunt [downloaden](assets/sms-2fa-verification-af.zip). In dit voorbeeldformulier worden de serviceaanroepen van het formuliergegevensmodel gebruikt die als onderdeel van dit artikel worden aangeboden.
* Het formulier importeren in AEM vanuit de gebruikersinterface van [Forms en Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Open het formulier in de bewerkingsmodus. De regeleditor voor het volgende veld openen

![sms-send](assets/check-sms.PNG)

* Bewerk de regel die aan het veld is gekoppeld. Geef uw juiste API-sleutels op
* Het formulier opslaan
* [Een voorbeeld van het formulier](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) bekijken en de functionaliteit testen


