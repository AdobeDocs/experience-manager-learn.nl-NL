---
title: Verifieer gebruikers met OTP
description: Verifieer het mobiele aantal verbonden aan het toepassingsaantal gebruikend OTP.
feature: Adaptieve Forms
type: Tutorial
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
topic: Ontwikkeling
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 0%

---



# Verifieer gebruikers met OTP

SMS Two Factor Authentication (Dual Factor Authentication) is een veiligheidsverificatieprocedure, die wordt geactiveerd door een gebruiker die zich aanmeldt bij een website, software of toepassing. Tijdens het aanmeldingsproces wordt de gebruiker automatisch een SMS-bericht naar zijn mobiele nummer gestuurd dat een unieke numerieke code bevat.

Er zijn een aantal organisaties die deze service aanbieden en zolang deze beschikken over goed gedocumenteerde REST API&#39;s kunt u AEM Forms eenvoudig integreren met de mogelijkheden voor gegevensintegratie van AEM Forms. In het kader van deze zelfstudie heb ik [Nexmo](https://developer.nexmo.com/verify/overview) gebruikt om de SMS 2FA gebruikscase aan te tonen.

De volgende stappen werden gevolgd om SMS 2FA met AEM Forms uit te voeren gebruikend de dienst van Nexmo Verify.

## Ontwikkelaarsaccount maken

Maak een ontwikkelaarsaccount met [Nexmo](https://dashboard.nexmo.com/sign-in). Noteer de API-sleutel en de API-beveiligingssleutel. Deze toetsen zijn nodig om REST API&#39;s van de Nexmo-service aan te roepen.

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

Met de [wagereditor](https://editor.swagger.io/) kunt u uw wagerbestand maken om de bewerkingen te beschrijven die OTP-code verzenden en verifiëren die met SMS is verzonden. Het wagerbestand kan in JSON- of YAML-indeling worden gemaakt. Het voltooide wagerbestand kan worden gedownload van [hier](assets/two-factore-authentication-swagger.zip)

## Gegevensbron maken

Om AEM/AEM Forms met derdetoepassingen te integreren, moeten wij [REST gebaseerde gegevensbron gebruikend het wagerdossier](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) in de configuratie van de wolkendiensten. De voltooide gegevensbron wordt verstrekt aan u als deel van deze cursusactiva.

## Formuliergegevensmodel maken

AEM Forms-gegevensintegratie biedt een intuïtieve gebruikersinterface voor het maken van en werken met formuliergegevensmodellen](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). [ Een formuliergegevensmodel is gebaseerd op gegevensbronnen voor gegevensuitwisseling.
Het ingevulde formuliergegevensmodel kan [hier worden gedownload](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)
