---
title: Campagneprofiel maken met formuliergegevensmodel
seo-title: Campagneprofiel maken met formuliergegevensmodel
description: Stappen die nodig zijn voor het maken van een Adobe Campaign Standard-profiel met behulp van het AEM Forms-formuliergegevensmodel
seo-description: Stappen die nodig zijn voor het maken van een Adobe Campaign Standard-profiel met behulp van het AEM Forms-formuliergegevensmodel
uuid: 3216827e-e1a2-4203-8fe3-4e2a82ad180a
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 461c532e-7a07-49f5-90b7-ad0dcde40984
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 1%

---


# Campagneprofiel maken met formuliergegevensmodel {#create-campaign-profile-using-form-data-model}

Stappen die nodig zijn voor het maken van een Adobe Campaign Standard-profiel met behulp van het AEM Forms-formuliergegevensmodel

## Aangepaste verificatie maken {#create-custom-authentication}

AEM Forms ondersteunt de volgende typen verificatie bij het maken van een gegevensbron met het zwembaarbestand

* Geen
* OAuth 2.0
* Basisverificatie
* API-sleutel
* Aangepaste verificatie

![campagne](assets/campaignfdm.gif)

Wij zullen douaneauthentificatie moeten gebruiken om REST vraag aan Adobe Campaign Standard te maken.

Om douaneauthentificatie te gebruiken, zullen wij een component moeten ontwikkelen OSGi die de interface IAuthentication uitvoert

De methode getAuthDetails moet worden geïmplementeerd. Deze methode retourneert het object AuthenticationDetails. Voor dit object AuthenticationDetails worden de vereiste HTTP-headers ingesteld die nodig zijn om de REST API-aanroep naar Adobe Campaign uit te voeren.

Hier volgt de code die is gebruikt voor het maken van aangepaste verificatie. De methode getAuthDetails doet al het werk. We maken een object AuthenticationDetails. Vervolgens voegen we de juiste HTTPHeaders aan dit object toe en retourneren we dit object.

```java
package aemfd.campaign.core;

import java.io.IOException;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;

import aemforms.campaign.core.CampaignService;
import formsandcampaign.demo.CampaignConfigurationService;
@Component(service=IAuthentication.class,immediate=true)

public class CampaignAuthentication implements IAuthentication {
 @Reference
 CampaignService campaignService;
  @Reference
     CampaignConfigurationService config;
private Logger log = LoggerFactory.getLogger(CampaignAuthentication.class);
 @Override
 public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException {
 try {
   AuthenticationDetails auth = new AuthenticationDetails();
   auth.addHttpHeader("Cache-Control", "no-cache");
   auth.addHttpHeader("Content-Type", "application/json");
   auth.addHttpHeader("X-Api-Key",config.getApiKey() );
         auth.addHttpHeader("Authorization", "Bearer "+campaignService.getAccessToken());
         log.debug("Returning auth");
         return auth;
   
  } catch (NoSuchAlgorithmException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (InvalidKeySpecException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
  
 }

 @Override
 public String getAuthenticationType() {
  // TODO Auto-generated method stub
  return "Campaign Custom Authentication";
 }

}
```

## Gegevensbron maken {#create-data-source}

De eerste stap bestaat uit het maken van het wagerbestand. Het waggerbestand definieert de REST API die wordt gebruikt om een profiel te maken in Adobe Campaign Standard. In het kwikbestand worden de invoerparameters en de uitvoerparameters van de REST API gedefinieerd.

Er wordt een gegevensbron gemaakt met behulp van het wagerbestand. Wanneer u een gegevensbron maakt, kunt u het verificatietype opgeven. In dit geval gebruiken we aangepaste verificatie om verificatie uit te voeren met Adobe Campaign. De bovenstaande code is gebruikt voor verificatie met Adobe Campaign.

Voorbeeldwagerbestand wordt aan u gegeven als onderdeel van het element dat betrekking heeft op dit artikel.**Zorg ervoor u de gastheer en basePath in het wagerdossier verandert om uw ACS instantie aan te passen**

## De oplossing testen {#test-the-solution}

Volg de volgende stappen om de oplossing te testen:
* [Controleer of u de hier beschreven stappen hebt uitgevoerd](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Download en decomprimeer dit bestand om het wagerbestand op te halen](assets/create-acs-profile-swagger-file.zip)
* Gegevensbron maken met behulp van wagerbestandMaak een formuliergegevensmodel en baseer dit op de gegevensbron die u in de vorige stap hebt gemaakt
* Maak een adaptief formulier op basis van het formuliergegevensmodel dat u eerder hebt gemaakt.
* Sleep de volgende elementen van het tabblad Gegevensbronnen naar het adaptieve formulier.

   * E-mail
   * Voornaam
   * Achternaam
   * Mobiele telefoon

* Configureer de verzendactie naar &quot;Verzenden met gebruik van het formuliergegevensmodel&quot;.
* Configureer het gegevensmodel voor de juiste verzending.
* Geef een voorbeeld van het formulier weer. Vul de velden in en verzend deze.
* Controleer of het profiel is gemaakt in Adobe Campaign Standard.
