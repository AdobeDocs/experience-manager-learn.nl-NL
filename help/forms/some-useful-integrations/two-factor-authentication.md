---
title: Verificatie van SMS Twee factoren
description: Voeg een extra laag van veiligheid toe helpen de identiteit van een gebruiker bevestigen wanneer zij bepaalde activiteiten willen uitvoeren
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6317
topic: Development
role: Developer
level: Experienced
exl-id: c2c55406-6da6-42be-bcc0-f34426b3291a
last-substantial-update: 2021-07-07T00:00:00Z
duration: 115
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 0%

---

# Gebruikers verifiëren met hun mobiele telefoonnummers

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

Om AEM/AEM Forms met derdetoepassingen te integreren, moeten wij [ gegevensbron ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=nl-NL) in de configuratie van de wolkendiensten tot stand brengen.

## Formuliergegevensmodel maken

De gegevensintegratie van AEM Forms verstrekt een intuïtief gebruikersinterface om tot stand te brengen en met [ modellen van vormgegevens ](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=nl-NL) te werken. Een formuliergegevensmodel is gebaseerd op gegevensbronnen voor gegevensuitwisseling.
Het voltooide model van vormgegevens kan [ van hier worden gedownload ](assets/sms-2fa-fdm.zip)

![ fdm ](assets/2FA-fdm.PNG)

## Adaptief formulier maken

Integreer de POST-aanroepen van het formuliergegevensmodel met het aangepaste formulier om te controleren welk mobiele telefoonnummer de gebruiker in het formulier heeft ingevoerd. U kunt zelf een adaptief formulier maken en de POST-aanroep van het formuliergegevensmodel gebruiken om OTP-code naar wens te verzenden en te verifiëren.

Voer de volgende stappen uit als u de voorbeeldbestanden met uw API-toetsen wilt gebruiken:

* [ Download het model van vormgegevens ](assets/sms-2fa-fdm.zip) en de invoer in AEM gebruikend [ pakketmanager ](http://localhost:4502/crx/packmgr/index.jsp)
* Download de steekproef adaptieve vorm kan [ van hier worden gedownload ](assets/sms-2fa-verification-af.zip). In dit voorbeeldformulier worden de serviceaanroepen van het formuliergegevensmodel gebruikt die als onderdeel van dit artikel worden aangeboden.
* De vorm van de invoer in AEM van [ Forms en Document UI ](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Open het formulier in de bewerkingsmodus. De regeleditor voor het volgende veld openen

![ sms-send ](assets/check-sms.PNG)

* Bewerk de regel die aan het veld is gekoppeld. Geef uw juiste API-sleutels op
* Het formulier opslaan
* [ voorproef de vorm ](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) en test de functionaliteit
