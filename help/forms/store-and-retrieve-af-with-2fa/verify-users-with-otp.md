---
title: Verifieer gebruikers met OTP
description: Verifieer het mobiele aantal verbonden aan het toepassingsaantal gebruikend OTP.
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6594
thumbnail: 6594.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d486d5de-efd9-4dd3-9d9c-1bef510c6073
duration: 84
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 0%

---

# Verifieer gebruikers met OTP

SMS Two Factor Authentication (Dual Factor Authentication) is een veiligheidsverificatieprocedure, die wordt geactiveerd door een gebruiker die zich aanmeldt bij een website, software of toepassing. Tijdens het aanmeldingsproces wordt de gebruiker automatisch een SMS-bericht naar zijn mobiele nummer gestuurd dat een unieke numerieke code bevat.

Er zijn een aantal organisaties die deze service aanbieden en zolang deze beschikken over goed gedocumenteerde REST API&#39;s kunt u AEM Forms eenvoudig integreren met de mogelijkheden voor gegevensintegratie van AEM Forms. Voor dit leerprogramma, heb ik [ Nexmo ](https://developer.nexmo.com/verify/overview) gebruikt om het de gebruiksgeval van SMS 2FA aan te tonen.

De volgende stappen werden gevolgd om SMS 2FA met AEM Forms uit te voeren gebruikend de dienst van Nexmo Verify.

## Ontwikkelaarsaccount maken

Creeer een ontwikkelaarrekening met [ Nexmo ](https://dashboard.nexmo.com/sign-in). Noteer de API-sleutel en de API-beveiligingssleutel. Deze toetsen zijn nodig om REST API&#39;s van de Nexmo-service aan te roepen.

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

Gebruik de [ kwikredacteur ](https://editor.swagger.io/) om uw kwikdossier tot stand te brengen om de verrichtingen te beschrijven die OTP verzonden code verzenden en verifiëren gebruikend SMS. Het wagerbestand kan in JSON- of YAML-indeling worden gemaakt. Het voltooide dossier van de wagger kan van [ hier ](assets/two-factore-authentication-swagger.zip) worden gedownload

## Source voor gegevens maken

Om AEM/AEM Forms met derdetoepassingen te integreren, moeten wij [ REST gebaseerde gegevensbron gebruiken gebruikend het wagerdossier ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=nl-NL) in de configuratie van de wolkendiensten. De voltooide gegevensbron wordt verstrekt aan u als deel van deze cursusactiva.

## Formuliergegevensmodel maken

De gegevensintegratie van AEM Forms verstrekt een intuïtief gebruikersinterface om tot stand te brengen en met [ modellen van vormgegevens ](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=nl-NL) te werken. Een formuliergegevensmodel is gebaseerd op gegevensbronnen voor gegevensuitwisseling.
Het voltooide model van vormgegevens kan [ van hier worden gedownload ](assets/sms-2fa-fdm.zip)

![ fdm ](assets/2FA-fdm.PNG)

[Het hoofdformulier maken](./create-the-main-adaptive-form.md)
