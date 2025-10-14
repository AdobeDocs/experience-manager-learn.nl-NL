---
title: OKTA configureren met AEM
description: Begrijp diverse configuratiemontages voor het gebruiken van enig teken-binnen gebruikend okta
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Administration
role: Admin
level: Experienced
exl-id: 85c9b51e-92bb-4376-8684-57c9c3204b2f
last-substantial-update: 2021-06-09T00:00:00Z
duration: 153
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '733'
ht-degree: 0%

---

# Verifiëren voor AEM-auteur met OKTA

De eerste stap bestaat uit het configureren van uw app op de OKTA-portal. Nadat uw app is goedgekeurd door uw OKTA-beheerder, hebt u toegang tot het IdP-certificaat en een Single Sign on URL. Hieronder ziet u de instellingen die gewoonlijk worden gebruikt voor de registratie van nieuwe toepassingen.

* **Naam van de Toepassing:** dit is uw toepassingsnaam. Geef uw toepassing een unieke naam.
* **Ontvanger SAML:** Na authentificatie van OKTA, is dit URL die op uw instantie van AEM met de reactie SAML zou worden geraakt. SAML de authentificatiemanager onderschept normaal al URL&#39;s met / saml_login maar het zou verkieslijk zijn om het na uw toepassingswortel toe te voegen.
* **SAML Publiek**: Dit is het domein URL van uw toepassing. Gebruik geen protocol (http of https) in het domein-URL.
* **identiteitskaart van de Naam van SAML:** Selecteer E-mail van de drop-down lijst.
* **Milieu**: Kies uw aangewezen milieu.
* **Attributen**: Dit zijn de attributen u over de gebruiker in de reactie van SAML krijgt. Geef ze op naar wens.


![&#x200B; okta-application &#x200B;](assets/okta-app-settings-blurred.PNG)


## Het OKTA-certificaat (IdP) toevoegen aan de AEM Trust Store

Aangezien de beweringen van SAML worden gecodeerd, moeten wij het certificaat IdP (OKTA) aan de vertrouwde opslag van AEM toevoegen, om veilige communicatie tussen OKTA en AEM toe te staan.
[&#x200B; initialiseert vertrouwensopslag &#x200B;](http://localhost:4502/libs/granite/security/content/truststore.html), als niet reeds geïnitialiseerd.
Onthoud het wachtwoord voor de vertrouwde opslag. Dit wachtwoord moeten we later tijdens dit proces gebruiken.

* Navigeer aan [&#x200B; Globale Opslag van het Vertrouwen &#x200B;](http://localhost:4502/libs/granite/security/content/truststore.html).
* Klik op Certificaat toevoegen uit CER-bestand. Voeg het IdP-certificaat toe dat door OKTA is opgegeven en klik op Verzenden.

  >[!NOTE]
  >
  >Wijs het certificaat niet toe aan gebruikers

Als u het certificaat toevoegt aan de vertrouwde opslag, krijgt u de certificaatalias die wordt weergegeven in de onderstaande schermafbeelding. De naam van de alias kan in uw geval anders zijn.

![&#x200B; certificaat-alias &#x200B;](assets/cert-alias.PNG)

**maak een nota van het certificaat alias. U hebt dit in de recentere stappen nodig.**

### SAML-verificatiehandler configureren

Navigeer aan [&#x200B; configMgr &#x200B;](http://localhost:4502/system/console/configMgr).
Zoek en open &quot;Adobe Granite SAML 2.0 Authentication Handler&quot;.
Geef de volgende eigenschappen op, zoals hieronder gespecificeerd
Hier volgen de belangrijkste eigenschappen die moeten worden opgegeven:

* **weg** - dit is de weg waar de authentificatiemanager wordt teweeggebracht
* **IdP Url**:Dit is uw URL IdP die door OKTA wordt verstrekt
* **Alias van het Certificaat IDP**:Dit alias u kreeg toen u het certificaat IdP in de vertrouwde opslag van AEM toevoegde
* **Identiteitskaart van de Entiteit van de Dienstverlener van de Dienst**:Dit is de naam van uw Server van AEM
* **Wachtwoord van Zeer belangrijke opslag**:Dit is het wachtwoord van de vertrouwensopslag dat u gebruikte
* **Gebrek richt opnieuw**:Dit is URL aan redirect op succesvolle authentificatie
* **Attribuut UserID**:uid
* **Encryptie van het Gebruik**:vals
* **autocreate de Gebruikers van CRX**:waar
* **voeg aan Groepen** toe:waar
* **StandaardGroepen**:oktausers (Dit is de groep waaraan de gebruikers worden toegevoegd. U kunt elke bestaande groep opgeven in AEM)
* **NamedIDPopolicy**: Specificeert beperkingen op het naamherkenningsteken dat moet worden gebruikt om het gevraagde onderwerp te vertegenwoordigen. Kopieer en kleef het volgende benadrukte koord **urn :oasis: namen :tc: SAML:2.0 :nameidformat: emailAddress**
* **Gesynchroniseerde Attributen** - dit zijn de attributen die van de bewering van SAML in het profiel van AEM worden opgeslagen

![&#x200B; voorbeeld-authentificatie-manager &#x200B;](assets/saml-authentication-settings-blurred.PNG)

### Filter Apache Sling Reference configureren

Navigeer aan [&#x200B; configMgr &#x200B;](http://localhost:4502/system/console/configMgr).
Zoek en open &quot;Apache Sling Referrer Filter&quot;.Stel de volgende eigenschappen in zoals hieronder opgegeven:

* **staat Lege** toe: vals
* **staat Gastheren** toe: IdP hostname (dit is verschillend in uw geval)
* **staat Gastheer Regexp** toe: IdP hostname (Dit is verschillend in uw geval)
Schermopname van de eigenschappen van de verwijzing Verwijzer van de Verkenner

![&#x200B; verwijzing-filter &#x200B;](assets/okta-referrer.png)

#### Logboekregistratie voor FOUTOPSPORING configureren voor de OKTA-integratie

Wanneer vestiging de integratie OKTA op AEM, kan het nuttig zijn om de DEBUG- logboeken voor de manager van de Authentificatie van AEM te herzien SAML. Als u het logniveau wilt instellen op DEBUG, maakt u een nieuwe configuratie voor Sling Logger via de AEM OSGi-webconsole.

Vergeet niet dit logger in het werkgebied en de productie te verwijderen of uit te schakelen om logruis te verminderen.

Bij het instellen van de OKTA-integratie in AEM kan het handig zijn om DEBUG-logboeken te bekijken voor de AEM SAML-verificatiehandler. Als u het logniveau wilt instellen op DEBUG, maakt u een nieuwe configuratie voor Sling Logger via de AEM OSGi-webconsole.
**Herinner me om dit registreerapparaat op Stadium en Productie te verwijderen of onbruikbaar te maken om logboek-lawaai te verminderen.**
* Ga aan [&#x200B; configMgr &#x200B;](http://localhost:4502/system/console/configMgr)

* Zoek en open &quot;Apache Sling Logging Logger Configuration&quot;
* Maak een logger met de volgende configuratie:
   * **Niveau van het Logboek**: Zuiver
   * **Dossier van het Logboek**: logs/saml.log
   * **Logger**: com.adobe.granite.auth.saml
* Klik op Opslaan om uw instellingen op te slaan

#### Uw OKTA-configuratie testen

Afmelden van uw AEM-exemplaar. Probeer de koppeling te openen. U moet OKTA SSO in actie zien.
