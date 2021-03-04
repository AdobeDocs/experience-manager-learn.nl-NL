---
title: OKTA configureren met AEM
description: Begrijp diverse configuratiemontages voor het gebruiken van enig teken-binnen gebruikend okta
feature: administratie
topics: development, authentication, security
audience: developer
doc-type: tutorial
activity: setup
version: 6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '763'
ht-degree: 0%

---


# Verifiëren voor AEM-auteur met OKTA

De eerste stap bestaat uit het configureren van uw app op de OKTA-portal. Nadat uw app is goedgekeurd door uw OKTA-beheerder, hebt u toegang tot het IdP-certificaat en een Single Sign on URL. Hieronder ziet u de instellingen die gewoonlijk worden gebruikt voor de registratie van nieuwe toepassingen.

* **Toepassingsnaam:** dit is de naam van uw toepassing. Geef uw toepassing een unieke naam.
* **SAML-ontvanger:** Na verificatie van OKTA is dit de URL die op uw AEM-instantie wordt geactiveerd met de SAML-reactie. SAML de authentificatiemanager onderschept normaal al URL&#39;s met / saml_login maar het zou verkieslijk zijn om het na uw toepassingswortel toe te voegen.
* **SAML-publiek**: Dit is de domein-URL van uw toepassing. Gebruik geen protocol (http of https) in het domein-URL.
* **SAML-naam-id:** selecteer E-mail in de vervolgkeuzelijst.
* **Omgeving**: Kies de juiste omgeving.
* **Kenmerken**: Dit zijn de attributen u over de gebruiker in de reactie van SAML krijgt. Geef ze op naar wens.


![okta-applicatie](assets/okta-app-settings-blurred.PNG)


## Het OKTA-certificaat (IdP) toevoegen aan het AEM Trust Store

Aangezien de beweringen van SAML worden gecodeerd, moeten wij het certificaat IdP (OKTA) aan de AEM vertrouwensopslag toevoegen, om veilige communicatie tussen OKTA en AEM toe te staan.
[Initialiseer vertrouwde opslag](http://localhost:4502/libs/granite/security/content/truststore.html) als deze nog niet is geïnitialiseerd.
Onthoud het wachtwoord voor de vertrouwde opslag. Dit wachtwoord moeten we later tijdens dit proces gebruiken.

* Navigeer naar [Global Trust Store](http://localhost:4502/libs/granite/security/content/truststore.html).
* Klik op Certificaat toevoegen uit CER-bestand. Voeg het IdP-certificaat toe dat door OKTA is opgegeven en klik op Verzenden.

   >[!NOTE]
   >
   >Wijs het certificaat niet toe aan gebruikers

Als u het certificaat toevoegt aan de vertrouwde opslag, krijgt u de certificaatalias die wordt weergegeven in de onderstaande schermafbeelding. De naam van de alias kan in uw geval anders zijn.

![Certificaat-alias](assets/cert-alias.PNG)

**Noteer het alias van het certificaat. U zult dit in de recentere stappen nodig hebben.**

### SAML-verificatiehandler configureren

Navigeer naar [configMgr](http://localhost:4502/system/console/configMgr).
Zoek en open &quot;Adobe Granite SAML 2.0 Authentication Handler&quot;.
Geef de volgende eigenschappen op, zoals hieronder gespecificeerd
Hier volgen de belangrijkste eigenschappen die moeten worden opgegeven:

* **pad**  - Dit is het pad waar de verificatiehandler wordt geactiveerd
* **IdP Url**:Dit is uw IdP url die door OKTA wordt verstrekt
* **IDP-certificaatalias**:Dit is de alias die u hebt gekregen toen u het IdP-certificaat aan AEM vertrouwde opslag toevoegde
* **Identiteitskaart** van de Leverancier van de dienst:Dit is de naam van uw AEMServer
* **Wachtwoord van sleutelarchief**: dit is het wachtwoord voor vertrouwde opslag dat u hebt gebruikt
* **Standaardomleiding**: dit is de URL waarnaar moet worden omgeleid bij geslaagde verificatie
* **Kenmerk** gebruikersnaam:uid
* **Codering** gebruiken:false
* **AutoCreate CRX Users**:true
* **Toevoegen aan groepen**:true
* **Standaardgroepen**:oktausers(Dit is de groep waaraan de gebruikers worden toegevoegd. U kunt elke bestaande groep opgeven (AEM)
* **NamedIDPopolicy**: Geeft beperkingen op aan de naam-id die moet worden gebruikt om het gevraagde onderwerp weer te geven. Kopieer en plak de volgende gemarkeerde tekenreeks **urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**
* **Gesynchroniseerde Attributen**  - Dit zijn de attributen die van bevestiging SAML in AEM profiel worden opgeslagen

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Filter Apache Sling Reference configureren

Navigeer naar [configMgr](http://localhost:4502/system/console/configMgr).
Zoek en open &quot;Apache Sling Referrer Filter&quot;.Stel de volgende eigenschappen in zoals hieronder opgegeven:

* **Lege waarden** toestaan: true
* **Gastheren** toestaan: Hostnaam van idP (zal in uw geval verschillend zijn)
* **Regexp-host** toestaan: Hostnaam van idP (zal in uw geval verschillend zijn) De het schermschot van de Verwijzing van de Filter van de Verkoper van de Verkoper van de Verkoop

![referentie-filter](assets/sling-referrer-filter.PNG)

#### Logboekregistratie voor FOUTOPSPORING configureren voor de OKTA-integratie

Wanneer vestiging de integratie OKTA op AEM, kan het nuttig zijn om de DEBUG logboeken voor AEM de manager van de Authentificatie van SAML te herzien. Om het logboekniveau aan DEBUG te plaatsen, creeer een nieuwe Sling Logger configuratie via de Console van het Web AEM OSGi.

Vergeet niet dit logger in het werkgebied en de productie te verwijderen of uit te schakelen om logruis te verminderen.

Wanneer vestiging de integratie OKTA op AEM, kan het nuttig zijn om DEBUG logboeken voor AEM de manager van de Authentificatie van SAML te herzien. Om het logboekniveau aan DEBUG te plaatsen, creeer een nieuwe Sling Logger configuratie via de Console van het Web AEM OSGi.
**Vergeet niet dit logger in het werkgebied en de productie te verwijderen of uit te schakelen om logruis te verminderen.**
* Navigeer naar [configMgr](http://localhost:4502/system/console/configMgr)

* Zoek en open &quot;Apache Sling Logging Logger Configuration&quot;
* Maak een logger met de volgende configuratie:
   * **Logniveau**: Foutopsporing
   * **Logbestand**: logs/saml.log
   * **Logger**: com.adobe.granite.auth.saml
* Klik op Opslaan om uw instellingen op te slaan



#### Uw OKTA-configuratie testen

Afmelden bij AEM instantie. Probeer de koppeling te openen. U moet OKTA SSO in actie zien.
