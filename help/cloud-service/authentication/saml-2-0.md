---
title: SAML 2.0 op AEM as a Cloud Service
description: Leer hoe u SAML 2.0-verificatie configureert op de AEM as a Cloud Service-publicatieservice.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9351
thumbnail: 343040.jpeg
last-substantial-update: 2024-05-15T00:00:00Z
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
duration: 2200
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '4233'
ht-degree: 0%

---

# SAML 2.0-verificatie{#saml-2-0-authentication}

Leer hoe u eindgebruikers (niet AEM-auteurs) instelt en verifieert op een door u gewenste SAML 2.0 compatibele IDP.

## Welke SAML voor AEM as a Cloud Service?

Dankzij SAML 2.0-integratie met AEM Publish (of Preview) kunnen eindgebruikers van een op AEM gebaseerde webervaring zich verifiëren bij een niet-Adobe IDP (Identity Provider) en hebben ze toegang tot AEM als een benoemde, geautoriseerde gebruiker.

|                       | AEM-auteur | AEM Publiceren |
|-----------------------|:----------:|:-----------:|
| SAML 2.0-ondersteuning | ✘ | ✔ |

+++ Begrijp SAML 2.0 stroom met AEM

De typische stroom van een AEM Publish SAML integratie is als volgt:

1. De gebruiker dient een aanvraag in bij AEM Publiceren. Hierin wordt aangegeven dat verificatie vereist is.
   + De gebruiker verzoekt om een van CUGs/ACL beschermde middel.
   + De gebruiker vraagt een middel dat aan een Vereiste van de Authentificatie onderworpen is.
   + De gebruiker volgt een verbinding aan het login eindpunt van AEM (d.w.z. `/system/sling/login`) dat uitdrukkelijk om de login actie verzoekt.
1. AEM doet een AuthnRequest aan IDP, die IDP verzoekt om authentificatieproces te beginnen.
1. Gebruiker verifieert aan IDP.
   + De gebruiker wordt veroorzaakt door IDP voor geloofsbrieven.
   + De gebruiker is reeds voor authentiek verklaard met IDP en moet geen verdere geloofsbrieven verstrekken.
1. IDP produceert een bevestiging van SAML die de gegevens van de gebruiker bevat, en ondertekent het gebruikend het privé certificaat van IDP.
1. IDP verzendt de SAML-bevestiging via HTTP POST via de webbrowser van de gebruiker (RESPECTIVE_PROTECTED_PATH/saml_login) naar AEM Publish.
1. AEM publiceert ontvangt de bevestiging van SAML, en bevestigt de integriteit en de authenticiteit van de bevestiging van SAML gebruikend het openbare certificaat IDP.
1. AEM Publish beheert het AEM gebruikersverslag dat op de configuratie van SAML 2.0 OSGi, en de inhoud van de Assertion van SAML wordt gebaseerd.
   + Maakt gebruiker
   + Gebruikerskenmerken synchroniseren
   + AEM-gebruikersgroeplidmaatschap bijwerken
1. AEM Publish plaatst het AEM `login-token` koekje op de reactie van HTTP, die wordt gebruikt om verdere verzoeken aan AEM voor authentiek te verklaren publiceert.
1. AEM Publish leidt de gebruiker naar URL op AEM Publish zoals gespecificeerd door het `saml_request_path` koekje.

+++

## Configuratie doorlopen

>[!VIDEO](https://video.tv.adobe.com/v/343040?quality=12&learn=on)

Deze video doorloopt het opzetten van SAML 2.0 integratie met de publicatieservice van AEM as a Cloud Service en het gebruik van Okta als IDP.

## Vereisten

Het volgende wordt vereist wanneer vestiging SAML 2.0 authentificatie:

+ Toegang tot Cloud Manager via Deployment Manager
+ AEM Administrator toegang tot AEM as a Cloud Service-omgeving
+ Beheerderstoegang tot de IDP
+ Naar keuze, toegang tot openbaar/privé sleutelpaar wordt gebruikt aan encryptieSAML nuttige ladingen

SAML 2.0 wordt alleen ondersteund voor het verifiëren van toepassingen voor publicatie of voorvertoning in AEM. Om de authentificatie van de Auteur van AEM te beheren die en IDP gebruiken, [&#x200B; integreert IDP met Adobe IMS &#x200B;](https://helpx.adobe.com/nl/enterprise/using/set-up-identity.html).


## IDP-certificaat voor iedereen installeren op AEM

Het openbare certificaat van IDP wordt toegevoegd aan de Globale Opslag van het Vertrouwen van AEM, en gebruikt om de bevestiging te bevestigen SAML die door IDP wordt verzonden is geldig.

+++SAML-ondertekeningsstroom voor beweringen

![&#x200B; SAML 2.0 - de Assertion die van IDP SAML &#x200B;](./assets/saml-2-0/idp-signing-diagram.png) ondertekent

1. Gebruiker verifieert aan IDP.
1. IDP produceert een bevestiging van SAML die de gegevens van de gebruiker bevat.
1. IDP ondertekent de bevestiging van SAML gebruikend het privé certificaat van IDP.
1. IDP stelt een cliënt-kant HTTP POST aan het eindpunt van SAML van de Publicatie van AEM (`.../saml_login`) in werking die de ondertekende bevestiging van SAML omvat.
1. AEM Publish ontvangt de POST van HTTP die de ondertekende bevestiging van SAML bevat, kan de handtekening bevestigen gebruikend het IDP openbare certificaat.

+++

![&#x200B; voeg het openbare certificaat IDP aan de Globale Opslag van het Vertrouwen toe &#x200B;](./assets/saml-2-0/global-trust-store.png)

1. Verkrijg het __openbare certificaat__ dossier van IDP. Met dit certificaat kan AEM de SAML-bevestiging valideren die door de IDP aan AEM wordt verstrekt.

   Het certificaat heeft de PEM-indeling en moet lijken op:

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. Meld u aan bij AEM Author als AEM Administrator.
1. Navigeer aan __Hulpmiddelen > Veiligheid > de Opslag van het Vertrouwen__.
1. Maak of open de Global Trust Store. Als u een Global Trust Store maakt, slaat u het wachtwoord op een veilige plaats op.
1. Breid __uit voegt certificaat van het dossier van CER__ toe.
1. Selecteer __Uitgezochte Dossier van het Certificaat__, en upload het certificaatdossier dat door IDP wordt verstrekt.
1. Verlaat __Certificaat van de Kaart aan Gebruiker__ leeg.
1. Selecteer __voorleggen__.
1. Het onlangs toegevoegde certificaat verschijnt boven __voegt certificaat van CRT dossier__ sectie toe.
1. Maak nota van __alias__, aangezien deze waarde in de [&#x200B; SAML 2.0 Configuratie OSGi van de Handler van de Authentificatie OSGi &#x200B;](#saml-2-0-authentication-handler-osgi-configuration) wordt gebruikt.
1. Selecteer __sparen &amp; Sluiten__.

De Global Trust Store wordt geconfigureerd met het openbare certificaat van IDP voor AEM-auteur, maar aangezien SAML alleen wordt gebruikt in AEM Publish, moet de Global Trust Store worden gerepliceerd naar AEM Publish om het openbare certificaat IDP daar toegankelijk te maken.

![&#x200B; Herhaal de Globale Opslag van het Vertrouwen aan AEM publiceren &#x200B;](./assets/saml-2-0/global-trust-store-replicate.png)

1. Navigeer aan __Hulpmiddelen > Plaatsing > Pakketten__.
1. Een pakket maken
   + Pakketnaam: `Global Trust Store`
   + Versie: `1.0.0`
   + Groep: `com.your.company`
1. Bewerk het nieuwe __Globale pakket van de Opslag van het Vertrouwen__.
1. Selecteer het __lusje van Filters__, en voeg een filter voor de wortelweg `/etc/truststore` toe.
1. Selecteer __Gedaan__ en dan __sparen__.
1. Selecteer de __bouwt__ knoop voor het __Globale pakket van de Opslag van het Vertrouwen__.
1. Zodra gebouwd, selecteer __Meer__ > __Repliceer__ om de Globale knoop van de Opslag van het Vertrouwen (`/etc/truststore`) aan AEM te activeren publiceer.

## Het sleutelarchief voor de verificatieservice maken{#authentication-service-keystore}

_Creërend een sleutelarchief voor authentificatie-dienst wordt vereist wanneer [&#x200B; SAML 2.0 het configuratiebezit OSGi van de authentificatiemanager `handleLogout` aan `true`](#saml-20-authenticationsaml-2-0-authentication) wordt geplaatst of wanneer [&#x200B; AuthnRequest het ondertekenen/SAML- assertieecryptie &#x200B;](#install-aem-public-private-key-pair) wordt vereist_

1. Meld u aan bij AEM Author als AEM Administrator om de persoonlijke sleutel te uploaden.
1. Navigeer aan __Hulpmiddelen > Veiligheid > Gebruikers__, en selecteer __authentificatie-dienst__ gebruiker, en selecteer __Eigenschappen__ van de hoogste actiebar.
1. Selecteer __Keystore__ tabel.
1. Maak of open het sleutelarchief. Houd het wachtwoord veilig als u een sleutelarchief maakt.
   + A [&#x200B; openbaar/privé keystore wordt geïnstalleerd in dit keystore &#x200B;](#install-aem-public-private-key-pair) slechts als AuthnRequest het ondertekenen/SAML beweringsencryptie wordt vereist.
   + Als deze integratie SAML logout steunt, maar niet AuthnRequest het ondertekenen/SAML bewering, dan is een leeg sleutelarchief voldoende.
1. Selecteer __sparen &amp; Sluiten__.
1. Creeer een pakket dat de bijgewerkte __authentificatie-dienst__ gebruiker bevat.

   _Gebruik de volgende tijdelijke tijdelijke oplossing gebruikend pakketten :_

   1. Navigeer aan __Hulpmiddelen > Plaatsing > Pakketten__.
   1. Een pakket maken
      + Pakketnaam: `Authentication Service`
      + Versie: `1.0.0`
      + Groep: `com.your.company`
   1. Bewerk het nieuwe __pakket van de Belangrijkste Opslag van de Dienst van de Authentificatie__.
   1. Selecteer het __lusje van Filters__, en voeg een filter voor de wortelweg `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore` toe.
      + `<AUTHENTICATION SERVICE UUID>` kan worden gevonden door aan __Hulpmiddelen te navigeren > Veiligheid > Gebruikers__, en het selecteren van __authentificatie-dienst__ gebruiker. De UUID is het laatste deel van de URL.
   1. Selecteer __Gedaan__ en dan __sparen__.
   1. Selecteer de __knoop van de Bouwstijl__ voor het __Belangrijkste pakket van de Opslag van de Dienst van de Authentificatie__.
   1. Zodra gebouwd, selecteer __Meer__ > __Repliceer__ om de belangrijkste opslag van de Dienst van de Authentificatie aan AEM te activeren publiceer.

## AEM-paar met openbare/persoonlijke sleutel installeren{#install-aem-public-private-key-pair}

_het installeren van AEM openbaar/privé zeer belangrijk paar is facultatief_

AEM Publish kan worden gevormd om AuthnRequests (aan IDP) te ondertekenen, en de beweringen van SAML (aan AEM) te coderen. Dit wordt bereikt door een persoonlijke sleutel voor AEM Publish te verstrekken, en het past openbare sleutel aan IDP aan.

+++ Begrijp de ondertekeningsstroom AuthnRequest (optioneel)

De AuthnRequest (de aanvraag aan IDP vanuit AEM Publish die het aanmeldingsproces initieert) kan worden ondertekend door AEM Publish. Hiertoe ondertekent AEM Publish het AuthnRequest met behulp van de persoonlijke sleutel, die IDP dan de handtekening gebruikend de openbare sleutel bevestigt. Dit garandeert aan de IDP dat AuthnRequest is gestart en aangevraagd door AEM Publish, en niet door een kwaadwillige derde.

![&#x200B; SAML 2.0 - het ondertekenen van SP AuthnRequest &#x200B;](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. De gebruiker doet een HTTP- verzoek aan AEM publiceren die in een de authentificatieverzoek van SAML aan IDP resulteert.
1. AEM Publish produceert het SAML- verzoek om naar IDP te verzenden.
1. AEM Publish ondertekent het SAML verzoek gebruikend de privé sleutel van AEM.
1. AEM Publish stelt AuthnRequest, een cliënt-zijomleiding van HTTP aan IDP in werking die het ondertekende SAML- verzoek bevat.
1. IDP ontvangt de AuthnRequest en valideert de handtekening met behulp van de openbare sleutel AEM, zodat AEM Publish de AuthnRequest heeft geïnitieerd.
1. AEM publiceert dan bevestigt de gedecrypteerde integriteit en de authenticiteit van SAML bewering gebruikend het IDP openbare certificaat.

+++

+++ Begrijp de de beweringsencryptiesstroom van SAML (facultatief)

Alle HTTP-communicatie tussen IDP en AEM Publiceren moet via HTTPS plaatsvinden en moet daarom standaard worden beveiligd. Nochtans, zoals vereist, kunnen de beweringen van SAML worden gecodeerd in het geval wordt de extra vertrouwelijkheid vereist bovenop die verstrekt door HTTPS. Om dit te doen, codeert IDP de gegevens van de Bevestiging van SAML gebruikend de privé sleutel, en AEM publiceert decrypteert de bewering van SAML gebruikend de privé sleutel.

![&#x200B; SAML 2.0 - de encryptie van de Assertion van SP SAML &#x200B;](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. Gebruiker verifieert aan IDP.
1. IDP produceert een bevestiging van SAML die de gegevens van de gebruiker bevat, en ondertekent het gebruikend het privé certificaat van IDP.
1. IDP codeert dan de bewering van SAML met AEM openbare sleutel, die de privé sleutel van AEM vereist om te decrypteren.
1. De gecodeerde SAML-bevestiging wordt als de webbrowser van de gebruiker verzonden naar AEM Publish.
1. AEM Publish ontvangt de bevestiging van SAML, en decrypteert het gebruikend de privé sleutel van AEM.
1. Bij IDP wordt de gebruiker gevraagd om te verifiëren.

+++

Zowel AuthnRequest het ondertekenen, als de bevestiging van SAML encryptie zijn facultatief, nochtans worden zij allebei toegelaten, gebruikend het [&#x200B; SAML 2.0 de configuratiebezit OSGi van de authentificatiemanager OSGi `useEncryption`](#saml-20-authenticationsaml-2-0-authentication), betekenend allebei of geen kunnen worden gebruikt.

![&#x200B; de authentificatie-dienst van AEM belangrijkste opslag &#x200B;](./assets/saml-2-0/authentication-service-key-store.png)

1. Verkrijg de openbare sleutel, de privé sleutel (PKCS#8 in formaat DER), en het dossier van de certificaatketting (dit kan de openbare sleutel zijn) die wordt gebruikt om AuthnRequest te ondertekenen, en de bewering van SAML te coderen. De sleutels worden typisch verstrekt door het de veiligheidsteam van de organisatie van IT.

   + Een zelf-ondertekend zeer belangrijk paar kan worden geproduceerd gebruikend __openssl__:

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. Upload de openbare sleutel aan IDP.
   + Met de bovenstaande methode `openssl` is de openbare sleutel het `aem-public.crt` -bestand.
1. Meld u aan bij AEM Author als AEM Administrator om de persoonlijke sleutel te uploaden.
1. Navigeer aan __Hulpmiddelen > Veiligheid > de Opslag van het Vertrouwen__, en selecteer __authentificatie-dienst__ gebruiker, en selecteer __Eigenschappen__ van de hoogste actiebar.
1. Navigeer aan __Hulpmiddelen > Veiligheid > Gebruikers__, en selecteer __authentificatie-dienst__ gebruiker, en selecteer __Eigenschappen__ van de hoogste actiebar.
1. Selecteer __Keystore__ tabel.
1. Maak of open het sleutelarchief. Houd het wachtwoord veilig als u een sleutelarchief maakt.
1. Selecteer __voeg privé sleutel van het dossier van DE DER__ toe, en voeg de privé sleutel en het kettingdossier aan AEM toe:
   + __alias__: Verstrek een betekenisvolle naam, vaak de naam van IDP.
   + __Persoonlijk zeer belangrijk dossier__: Upload het privé zeer belangrijke dossier (PKCS#8 in formaat DER).
      + Met de bovenstaande methode `openssl` is dit het `aem-private-pkcs8.der` -bestand
   + __Uitgezochte dossier van de certificaatketting__: Upload het begeleidende ketendossier (dit kan de openbare sleutel zijn).
      + Met de bovenstaande methode `openssl` is dit het `aem-public.crt` -bestand
   + Selecteer __voorleggen__
1. Het onlangs toegevoegde certificaat verschijnt boven __voegt certificaat van CRT dossier__ sectie toe.
   + Maak nota van __alias__ aangezien dit in [&#x200B; SAML 2.0 de configuratie van de authentificatiemanager OSGi &#x200B;](#saml-20-authentication-handler-osgi-configuration) wordt gebruikt
1. Selecteer __sparen &amp; Sluiten__.
1. Creeer een pakket dat de bijgewerkte __authentificatie-dienst__ gebruiker bevat.

   _Gebruik de volgende tijdelijke tijdelijke oplossing gebruikend pakketten :_

   1. Navigeer aan __Hulpmiddelen > Plaatsing > Pakketten__.
   1. Een pakket maken
      + Pakketnaam: `Authentication Service`
      + Versie: `1.0.0`
      + Groep: `com.your.company`
   1. Bewerk het nieuwe __pakket van de Belangrijkste Opslag van de Dienst van de Authentificatie__.
   1. Selecteer het __lusje van Filters__, en voeg een filter voor de wortelweg `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore` toe.
      + `<AUTHENTICATION SERVICE UUID>` kan worden gevonden door aan __Hulpmiddelen te navigeren > Veiligheid > Gebruikers__, en het selecteren van __authentificatie-dienst__ gebruiker. De UUID is het laatste deel van de URL.
   1. Selecteer __Gedaan__ en dan __sparen__.
   1. Selecteer de __knoop van de Bouwstijl__ voor het __Belangrijkste pakket van de Opslag van de Dienst van de Authentificatie__.
   1. Zodra gebouwd, selecteer __Meer__ > __Repliceer__ om de belangrijkste opslag van de Dienst van de Authentificatie aan AEM te activeren publiceer.

## SAML 2.0-verificatiehandler configureren{#configure-saml-2-0-authentication-handler}

De configuratie van SAML van AEM wordt uitgevoerd via __Adobe Granite SAML 2.0 de Handler van de Authentificatie__ OSGi configuratie.
De configuratie is een OSGi fabrieksconfiguratie, die één enkele dienst van de Publicatie van AEM as a Cloud Service betekent kan veelvoudige SAML configuratie hebben die discrete middelenbomen van de bewaarplaats behandelen; dit is nuttig voor multi-site plaatsingen van AEM.

+++ SAML 2.0 OSGi-configuratiegids van de Handler van de Authentificatie

### Adobe Granite SAML 2.0 Authentication Handler OSGi-configuratie{#configure-saml-2-0-authentication-handler-osgi-configuration}

|                                   | OSGi, eigenschap | Vereist | Waarde-indeling | Standaardwaarde | Beschrijving |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| Paden | `path` | ✔ | Tekenreeksarray | `/` | AEM-paden waarvoor deze verificatiehandler wordt gebruikt. |
| IDP-URL | `idpUrl` | ✔ | String |                           | IDP URL het de authentificatieverzoek van SAML wordt verzonden. |
| IDP-certificaatalias | `idpCertAlias` | ✔ | String |                           | De alias van het IDP-certificaat in de AEM Global Trust Store |
| IDP HTTP-omleiding | `idpHttpRedirect` | ✘ | Boolean | `false` | Geeft aan of een HTTP-omleiding naar de IDP-URL plaatsvindt in plaats van een AuthnRequest te verzenden. Ingesteld op `true` voor door IDP geïnitieerde verificatie. |
| IDP-id | `idpIdentifier` | ✘ | String |                           | Unieke IDP-id om ervoor te zorgen dat AEM uniek is voor gebruikers en groepen. Als de waarde leeg is, wordt in plaats daarvan `serviceProviderEntityId` gebruikt. |
| Bevestigingsservice-URL | `assertionConsumerServiceURL` | ✘ | String |                           | Het URL-kenmerk `AssertionConsumerServiceURL` in de AuthnRequest die opgeeft waar het `<Response>` -bericht naar AEM moet worden verzonden. |
| SP-entiteit-id | `serviceProviderEntityId` | ✔ | String |                           | Identificeert AEM op unieke wijze aan IDP; gewoonlijk de de gastheernaam van AEM. |
| SP-codering | `useEncryption` | ✘ | Boolean | `true` | Wijst erop als IDP de beweringen van SAML codeert. `spPrivateKeyAlias` en `keyStorePassword` moeten worden ingesteld. |
| SP alias van persoonlijke sleutel | `spPrivateKeyAlias` | ✘ | String |                           | De alias van de persoonlijke sleutel in de sleutelarchief van de `authentication-service` gebruiker. Vereist als `useEncryption` is ingesteld op `true` . |
| Wachtwoord voor opslagmap met SP-sleutel | `keyStorePassword` | ✘ | String |                           | Het wachtwoord van de sleutelarchief van de &quot;authentificatie-dienst&quot;gebruiker. Vereist als `useEncryption` is ingesteld op `true` . |
| Standaardomleiding | `defaultRedirectUrl` | ✘ | String | `/` | De standaard omleidings-URL na geslaagde verificatie. Kan relatief zijn ten opzichte van de AEM-host (bijvoorbeeld `/content/wknd/us/en/html` ). |
| Kenmerk Gebruikersnaam | `userIDAttribute` | ✘ | String | `uid` | The name of the SAML assertion attribute containing the user ID of the AEM user. Laat leeg om de `Subject:NameId` te gebruiken. |
| AEM-gebruikers automatisch maken | `createUser` | ✘ | Boolean | `true` | Geeft aan of AEM-gebruikers zijn gemaakt op geslaagde verificatie. |
| Tussentijdse pad voor AEM-gebruiker | `userIntermediatePath` | ✘ | String |                           | Wanneer u AEM-gebruikers maakt, wordt deze waarde gebruikt als het tussenliggende pad (bijvoorbeeld `/home/users/<userIntermediatePath>/jane@wknd.com` ). `createUser` moet worden ingesteld op `true` . |
| AEM-gebruikerskenmerken | `synchronizeAttributes` | ✘ | Tekenreeksarray |                           | Lijst met SAML-kenmerktoewijzingen die moeten worden opgeslagen op de AEM-gebruiker, in de indeling `[ "saml-attribute-name=path/relative/to/user/node" ]` (bijvoorbeeld `[ "firstName=profile/givenName" ]` ). Zie de [&#x200B; volledige lijst van inheemse attributen van AEM &#x200B;](#aem-user-attributes). |
| Gebruiker toevoegen aan AEM-groepen | `addGroupMemberships` | ✘ | Boolean | `true` | Geeft aan of een AEM-gebruiker automatisch wordt toegevoegd aan AEM-gebruikersgroepen nadat de verificatie is voltooid. |
| AEM-kenmerk voor groepslidmaatschap | `groupMembershipAttribute` | ✘ | String | `groupMembership` | De naam van het SAML-assertiekenmerk met een lijst van AEM-gebruikersgroepen waaraan de gebruiker moet worden toegevoegd. `addGroupMemberships` moet worden ingesteld op `true` . |
| Standaard AEM-groepen | `defaultGroups` | ✘ | Tekenreeksarray |                           | Er wordt altijd een lijst met AEM-gebruikersgroepen waaraan geverifieerde gebruikers zijn toegevoegd (bijvoorbeeld `[ "wknd-user" ]` ). `addGroupMemberships` moet worden ingesteld op `true` . |
| NameIDPopolicy-indeling | `nameIdFormat` | ✘ | String | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | De waarde van de NameIDPopolicy formaatparameter in het AuthnRequest bericht te verzenden. |
| SAML-respons opslaan | `storeSAMLResponse` | ✘ | Boolean | `false` | Geeft aan of de `samlResponse` -waarde wordt opgeslagen op het AEM `cq:User` -knooppunt. |
| Afmelden handgreep | `handleLogout` | ✘ | Boolean | `false` | Wijst erop als het logout verzoek door deze de authentificatiemanager van SAML wordt behandeld. `logoutUrl` moet worden ingesteld. |
| Afmeldings-URL | `logoutUrl` | ✘ | String |                           | De URL van IDP waar het SAML logout verzoek wordt verzonden naar. Vereist als `handleLogout` is ingesteld op `true` . |
| Kloktolerantie | `clockTolerance` | ✘ | Geheel | `60` | Bij het valideren van SAML-beweringen tekent IDP en AEM (SP) de tolerantie voor klokschuintrekken. |
| Methode Digest | `digestMethod` | ✘ | String | `http://www.w3.org/2001/04/xmlenc#sha256` | Het samenvattingsalgoritme dat IDP gebruikt wanneer het ondertekenen van een SAML- bericht. |
| Handtekeningmethode | `signatureMethod` | ✘ | String | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | Het handtekeningalgoritme dat IDP gebruikt bij het ondertekenen van een SAML-bericht. |
| Type identiteitssynchronisatie | `identitySyncType` | ✘ | `default` of `idp` | `default` | Wijzig de standaardinstelling voor AEM as a Cloud Service niet. `from` |
| Servicerangschikking | `service.ranking` | ✘ | Geheel | `5002` | Hogere rangschikkingsconfiguraties hebben de voorkeur voor dezelfde `path` . |

### AEM-gebruikerskenmerken{#aem-user-attributes}

AEM gebruikt de volgende gebruikersattributen, die via het `synchronizeAttributes` bezit in Adobe Granite SAML 2.0 de Configuratie OSGi van de Handler van de Authentificatie kunnen worden bevolkt.  Om het even welke IDP attributen kunnen aan om het even welke AEM gebruikersbezit worden gesynchroniseerd, nochtans het in kaart brengen aan AEM (hieronder vermeld) eigenschappen van het gebruiksattribuut staat AEM toe om hen natuurlijk te gebruiken.

| Gebruikerskenmerk | Relatief eigenschapspad van knooppunt `rep:User` |
|--------------------------------|--------------------------|
| Titel (bijvoorbeeld `Mrs`) | `profile/title` |
| Voornaam (bijv. voornaam) | `profile/givenName` |
| Familienaam (bijv. achternaam) | `profile/familyName` |
| Functie | `profile/jobTitle` |
| E-mailadres | `profile/email` |
| Adres | `profile/street` |
| Plaats | `profile/city` |
| Postcode | `profile/postalCode` |
| Land | `profile/country` |
| Telefoonnummer | `profile/phoneNumber` |
| Over mij | `profile/aboutMe` |

+++

1. Creeer een OSGi configuratiedossier in uw project bij `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` en open in uw winde.
   + Wijzig `/wknd-examples/` in uw `/<project name>/`
   + De id na de naam `~` in de bestandsnaam moet deze configuratie op unieke wijze identificeren, zodat dit de naam van de id kan zijn, zoals `...~okta.cfg.json` . De waarde moet alfanumeriek zijn met afbreekstreepjes.
1. Plak de volgende JSON-code in het `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` -bestand en werk de `wknd` -verwijzingen zo nodig bij.

   ```json
   {
       "path": [ "/content/wknd", "/content/dam/wknd" ], 
       "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1652125559800]",
       "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
       "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-5511372_aemasacloudservice_1/exk4z55r44Jz9C6am5d7/sso/saml]",
       "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
       "useEncryption": false,
       "createUser": true,
       "userIntermediatePath": "wknd/idp",
       "synchronizeAttributes":[
           "firstName=profile/givenName"
       ],
       "addGroupMemberships": true,
       "defaultGroups": [ 
           "wknd-users"
       ]
   }
   ```

1. Werk de waarden bij zoals vereist door uw project. Zie __SAML 2.0 Handler OSGi van de Authentificatie verklaringswoordenlijst OSGi hierboven voor beschrijvingen van het configuratiebezit__
1. Het wordt aanbevolen, maar niet vereist, om OSGi-omgevingsvariabelen en -geheimen te gebruiken, wanneer waarden niet synchroon kunnen veranderen met de releasecyclus of wanneer de waarden verschillen tussen vergelijkbare omgevingstypen/serviceniveaus. U kunt standaardwaarden instellen met de syntaxis `$[env:..;default=the-default-value]"` , zoals hierboven wordt weergegeven.

OSGi-configuraties per omgeving (`config.publish.dev`, `config.publish.stage` en `config.publish.prod` ) kunnen met specifieke kenmerken worden gedefinieerd als de SAML-configuratie per omgeving verschilt.

### Codering gebruiken

Wanneer [&#x200B; het coderen van de bevestiging AuthnRequest en SAML &#x200B;](#encrypting-the-authnrequest-and-saml-assertion), worden de volgende eigenschappen vereist: `useEncryption`, `spPrivateKeyAlias`, en `keyStorePassword`. `keyStorePassword` bevat een wachtwoord daarom moet de waarde niet in het OSGi configuratiedossier worden opgeslagen, maar eerder ingespoten gebruikend [&#x200B; geheime configuratiewaarden &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values)

+++Werk eventueel de configuratie OSGi bij om encryptie te gebruiken

1. Open `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` in uw winde.
1. Voeg de drie eigenschappen `useEncryption`, `spPrivateKeyAlias` en `keyStorePassword` toe, zoals hieronder wordt weergegeven.

   ```json
   {
   "path": [ "/content/wknd", "/content/dam/wknd" ], 
   "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1234567890]",
   "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/abcdef1235678]",
   "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-123567890_aemasacloudservice_1/abcdef1235678/sso/saml]",
   "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
   "useEncryption": true,
   "spPrivateKeyAlias": "$[env:SAML_AEM_KEYSTORE_ALIAS;default=aem-saml-encryption]",
   "keyStorePassword": "$[secret:SAML_AEM_KEYSTORE_PASSWORD]",
   "createUser": true,
   "userIntermediatePath": "wknd/idp"
   "synchronizeAttributes":[
       "firstName=profile/givenName"
   ],
   "addGroupMemberships": true,
   "defaultGroups": [ 
       "wknd-users"
   ]
   }
   ```

1. De drie OSGi configuratieeigenschappen die voor encryptie worden vereist zijn:

+ `useEncryption` ingesteld op `true`
+ `spPrivateKeyAlias` bevat de alias van het sleutelarchiefitem voor de persoonlijke sleutel die door de integratie van SAML wordt gebruikt.
+ `keyStorePassword` bevat een [&#x200B; OSGi geheime configuratievariabele &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values) die het `authentication-service` wachtwoord van het gebruikerssleutelarchief bevat.

+++

## Referentiefilter configureren

Tijdens het SAML-verificatieproces start IDP een HTTP POST aan de clientzijde naar het eindpunt van AEM Publish `.../saml_login` . Als IDP en AEM op verschillende oorsprong publiceren, publiceert AEM __de Filter van de Referateur van de Publicatie__ wordt gevormd via configuratie OSGi om HTTP POSTs van de oorsprong van IDP toe te staan.

1. Creeer (of geef) een OSGi configuratiedossier in uw project bij `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json` uit.
   + Wijzig `/wknd-examples/` in uw `/<project name>/`
1. Zorg ervoor dat de `allow.empty` -waarde is ingesteld op `true` , `allow.hosts` (of als u dat liever doet, `allow.hosts.regexp` ) de oorsprong van de IDP bevat en `filter.methods` `POST` . De OSGi-configuratie moet vergelijkbaar zijn met:

   ```json
   {
       "allow.empty": true,
       "allow.hosts.regexp": [ ],
       "allow.hosts": [ 
           "$[env:SAML_IDP_REFERRER;default=dev-123567890.okta.com]"
       ],
       "filter.methods": [
           "POST",
       ],
       "exclude.agents.regexp": [ ]
   }
   ```

AEM publiceert steunt één enkele het filterconfiguratie van de Referateur, zo fusie de configuratievereisten van SAML, met om het even welke bestaande configuraties.

OSGi-configuraties per omgeving (`config.publish.dev`, `config.publish.stage` en `config.publish.prod` ) kunnen met specifieke kenmerken worden gedefinieerd als de `allow.hosts` (of `allow.hosts.regex` ) per omgeving verschilt.

## Resource Sharing (CORS) voor meerdere bronnen configureren

Tijdens het SAML-verificatieproces start IDP een HTTP POST aan de clientzijde naar het eindpunt van AEM Publish `.../saml_login` . Als IDP en AEM op verschillende gastheren/domeinen publiceren, publiceert AEM __CRoss-Origin Middel dat (CORS) deelt__ moet worden gevormd om HTTP POSTs van de gastheer/het domein van IDP toe te staan.

De header `Origin` van deze HTTP POST-aanvraag heeft gewoonlijk een andere waarde dan de AEM Publish-host, waardoor CORS-configuratie vereist is.

Bij het testen van SAML-verificatie op de lokale AEM SDK (`localhost:4503`), kan de IDP de `Origin` header instellen op `null` . Als dat het geval is, voegt u `"null"` toe aan de lijst `alloworigin` .

1. Creeer een OSGi configuratiedossier in uw project bij `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`
   + Wijzig `/wknd-examples/` in uw projectnaam
   + De id na de naam `~` in de bestandsnaam moet deze configuratie op unieke wijze identificeren, zodat dit de naam van de id kan zijn, zoals `...CORSPolicyImpl~okta.cfg.json` . De waarde moet alfanumeriek zijn met afbreekstreepjes.
1. Plak de volgende JSON in het `com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json` -bestand.

```json
{
    "alloworigin": [ 
        "$[env:SAML_IDP_ORIGIN;default=https://dev-1234567890.okta.com]", 
        "null"
    ],
    "allowedpaths": [ 
        ".*/saml_login"
    ],
    "supportedmethods": [ 
        "POST"
    ]
}
```

OSGi-configuraties per omgeving (`config.publish.dev`, `config.publish.stage` en `config.publish.prod` ) kunnen met specifieke kenmerken worden gedefinieerd als de `alloworigin` en `allowedpaths` per omgeving verschillen.

## AEM Dispatcher configureren om SAML HTTP POST&#39;s toe te staan

Na succesvolle authentificatie aan IDP, zal IDP een POST van HTTP terug naar AEM geregistreerde `/saml_login` eindpunt (gevormd in IDP) orchestreren. Deze HTTP POST aan `/saml_login` wordt door gebrek geblokkeerd bij Dispatcher, zodat moet het uitdrukkelijk worden toegestaan gebruikend de volgende regel van Dispatcher:

1. Open `dispatcher/src/conf.dispatcher.d/filters/filters.any` in uw winde.
1. Voeg onder aan het bestand toe en sta regel voor HTTP POST&#39;s toe aan URL&#39;s die eindigen met `/saml_login` .

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

>[!NOTE]
>Wanneer het opstellen van veelvoudige configuraties SAML in AEM voor diverse beschermde wegen en verschillende eindpunten IDP, zorg ervoor dat IDP aan RESPECTIVE_PROTECTED_PATH/saml_login eindpunt posten om de aangewezen configuratie van SAML op de kant van AEM te selecteren. Als er dubbele configuraties SAML voor de zelfde beschermde weg zijn, zal de selectie van de configuratie van SAML willekeurig voorkomen.

Als URL die bij de Apache webserver herschrijft (`dispatcher/src/conf.d/rewrites/rewrite.rules`) wordt gevormd, zorg ervoor dat de verzoeken aan de `.../saml_login` eindpunten niet per ongeluk worden beheerd.

## Dynamisch groepslidmaatschap

Het dynamische Lidmaatschap van de Groep is een eigenschap in [&#x200B; Apache Jackrabbit Oak &#x200B;](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html) die de prestaties van groepsevaluatie en levering verhoogt. Deze sectie beschrijft hoe de gebruikers en de groepen worden opgeslagen wanneer deze eigenschap wordt toegelaten en hoe te om de configuratie van de Handler van de Authentificatie van SAML te wijzigen om het voor nieuwe of bestaande milieu&#39;s toe te laten.

### Hoe te om Dynamisch Lidmaatschap van de Groep voor de Gebruikers van SAML in nieuwe milieu&#39;s toe te laten

Om de prestaties van de groepsevaluatie in nieuwe AEM as a Cloud Service-omgevingen aanzienlijk te verbeteren, wordt de activering van de functie Dynamic Group Membership aanbevolen in nieuwe omgevingen.
Dit is ook een noodzakelijke stap wanneer de gegevenssynchronisatie wordt geactiveerd. Meer details [&#x200B; hier &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier).
Hiervoor voegt u de volgende eigenschap toe aan het configuratiebestand van OSGI:

`/apps/example/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~example.cfg.json`

Met deze configuratie, worden de gebruikers en de groepen gecreeerd als [&#x200B; Externe Gebruikers van Oak &#x200B;](https://jackrabbit.apache.org/oak/docs/security/authentication/identitymanagement.html). In AEM hebben externe gebruikers en groepen de standaardwaarde `rep:principalName` , die wordt samengesteld door `[user name];[idp]` of `[group name];[idp]` .
Merk op dat de Lijsten van het Toegangsbeheer (ACL) met PrincipalName van gebruikers of groepen worden geassocieerd.
Wanneer het opstellen van deze configuratie in een bestaande plaatsing waar eerder `identitySyncType` niet werd gespecificeerd of aan `default` werd geplaatst, zullen de nieuwe gebruikers en de groepen worden gecreeerd en ACL moet op deze nieuwe gebruikers en groepen worden toegepast. Externe groepen kunnen geen lokale gebruikers bevatten. [&#x200B; opnieuw richt &#x200B;](https://sling.apache.org/documentation/bundles/repository-initialization.html) kan worden gebruikt om ACL voor Externe groepen van SAML tot stand te brengen, zelfs als zij slechts zullen worden gecreeerd wanneer de gebruiker login zal uitvoeren.
Om dit refactoring op ACL te vermijden, is een standaard [&#x200B; migratieeigenschap &#x200B;](#automatic-migration-to-dynamic-group-membership-for-existing-environments) uitgevoerd.

### Hoe de lidmaatschappen in lokale en externe groepen met dynamisch groepslidmaatschap worden opgeslagen

In lokale groepen worden de groepsleden opgeslagen in het oak-kenmerk: `rep:members` . Het attribuut bevat de lijst van uid van elk lid van de groep. De extra details kunnen [&#x200B; hier &#x200B;](https://jackrabbit.apache.org/oak/docs/security/user/membership.html#member-representation-in-the-repository) worden gevonden.
Voorbeeld:

```
{
  "jcr:primaryType": "rep:Group",
  "rep:principalName": "operators",
  "rep:managedByIdp": "SAML",
  "rep:members": [
    "635afa1c-beeb-3262-83c4-38ea31e5549e",
    "5e496093-feb6-37e9-a2a1-7c87b1cec4b0",
    ...
  ],
   ...
}
```

Externe groepen met dynamisch groepslidmaatschap slaan geen leden op in het groepsitem.
Het groepslidmaatschap wordt in plaats daarvan opgeslagen in de gebruikersingangen. De extra documentatie kan [&#x200B; hier &#x200B;](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html) worden gevonden. Dit is bijvoorbeeld het OAK-knooppunt voor de groep:

```
{
  "jcr:primaryType": "rep:Group",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "jcr:createdBy": "",
  "jcr:created": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "rep:principalName": "GROUP_1;aem-saml-idp-1",
  "rep:lastSynced": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "jcr:uuid": "d9c6af8a-35c0-3064-899a-59af55455cd0",
  "rep:externalId": "GROUP_1;aem-saml-idp-1",
  "rep:authorizableId": "GROUP_1;aem-saml-idp-1"
}
```

Dit is de knoop voor een gebruikerslid van die groep:

```
{
  "jcr:primaryType": "rep:User",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "surname": "Test",
  "rep:principalName": "testUser",
  "rep:externalId": "test;aem-saml-idp-1",
  "rep:authorizableId": "test",
  "rep:externalPrincipalNames": [
    "projects-users;aem-saml-idp-1",
    "GROUP_2;aem-saml-idp-1",
    "GROUP_1;aem-saml-idp-1",
    "operators;aem-saml-idp-1"
  ],
  ...
}
```

### Hoe te om Dynamisch Lidmaatschap van de Groep voor de Gebruikers van SAML in bestaande milieu&#39;s toe te laten

Zoals in de vorige sectie wordt uitgelegd, wijkt de indeling van externe gebruikers en groepen enigszins af van de indeling die wordt gebruikt voor lokale gebruikers en groepen. Het is mogelijk om nieuwe ACL voor externe groepen en voorziening nieuwe externe gebruikers te bepalen, of het migratiehulpmiddel te gebruiken zoals hieronder beschreven.

#### Dynamisch groepslidmaatschap voor bestaande omgevingen met externe gebruikers inschakelen

De SAML-verificatiehandler maakt externe gebruikers wanneer de volgende eigenschap wordt opgegeven: `"identitySyncType": "idp"` . In dit geval kan dynamisch groepslidmaatschap worden ingeschakeld waarbij deze eigenschap wordt gewijzigd in: `"identitySyncType": "idp_dynamic"` . Er is geen migratie vereist.

#### Automatische migratie naar dynamisch groepslidmaatschap voor bestaande omgevingen met lokale gebruikers

De SAML-verificatiehandler maakt lokale gebruikers wanneer de volgende eigenschap wordt opgegeven: `"identitySyncType": "default"` . Dit is ook de standaardwaarde wanneer de eigenschap niet wordt opgegeven. In deze sectie beschrijven we de stappen die worden uitgevoerd door de automatische migratieprocedure.

Wanneer deze migratie is ingeschakeld, wordt deze uitgevoerd tijdens gebruikersverificatie en bestaat deze uit de volgende stappen:
1. De lokale gebruiker wordt gemigreerd naar een externe gebruiker met behoud van de oorspronkelijke gebruikersnaam. Dit betekent dat gemigreerde lokale gebruikers, die nu als externe gebruikers fungeren, hun oorspronkelijke gebruikersnaam behouden in plaats van de naamgevingssyntaxis te volgen die in de vorige sectie wordt vermeld. Er wordt een extra eigenschap toegevoegd, namelijk `rep:externalId` met de waarde van `[user name];[idp]` . De gebruiker `PrincipalName` wordt niet gewijzigd.
2. Voor elke externe groep die in de SAML Assertion wordt ontvangen, wordt een externe groep gecreeerd. Als er een overeenkomende lokale groep bestaat, wordt de externe groep als lid toegevoegd aan de lokale groep.
3. De gebruiker wordt toegevoegd als lid van de externe groep.
4. De lokale gebruiker wordt dan verwijderd uit alle lokale Saml-groepen waarvan hij lid was. Lokale voorbeeldgroepen worden aangeduid met de OAK-eigenschap: `rep:managedByIdp` . Deze eigenschap wordt ingesteld door de handler Saml Authentication wanneer het kenmerk `syncType` niet is opgegeven of ingesteld op `default` .

Als vóór de migratie `user1` bijvoorbeeld een lokale gebruiker en een lid van een lokale groep `group1` is, worden na de migratie de volgende wijzigingen doorgevoerd:
`user1` wordt een externe gebruiker. Het kenmerk `rep:externalId` wordt toegevoegd aan dit profiel.
`user1` wordt lid van externe groep: `group1;idp`
`user1` is geen direct lid meer van de lokale groep: `group1`
`group1;idp` is lid van de lokale groep: `group1` .
`user1` is dan lid van de lokale groep: `group1` hoewel overerving

Het groepslidmaatschap voor externe groepen wordt opgeslagen in het gebruikersprofiel in de eigenschap `rep:externalPrincipalNames`

### Automatische migratie naar dynamisch groepslidmaatschap configureren

1. Schakel de eigenschap `"identitySyncType": "idp_dynamic_simplified_id"` in het SAML OSGi-configuratiebestand in: `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json`
2. Vorm de nieuwe dienst OSGi met PID van de Fabriek die met begint: `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~`. Een PID kan bijvoorbeeld: `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~myIdP` zijn. Stel de volgende eigenschap in:

```
{
  "idpIdentifier": "<value of IDP Identifier (idpIdentifier)" property from the "com.adobe.granite.auth.saml.SamlAuthenticationHandler" configuration to be migrated>"
}
```

Als u meerdere SAML-configuraties wilt migreren, moeten er meerdere OSGi-fabrieksconfiguraties voor `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration` worden gemaakt. Elke configuratie geeft een `idpIdentifier` op die moet worden gemigreerd.

## SAML-configuratie implementeren

De configuraties OSGi moeten aan Git worden geëngageerd en aan AEM as a Cloud Service worden opgesteld gebruikend Cloud Manager.

```
$ git remote -v            
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (fetch)
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (push)
$ git add .
$ git commit -m "SAML 2.0 configurations"
$ git push adobe saml-auth:develop
```

Implementeer de Cloud Manager Git-doelvertakking (in dit voorbeeld `develop` ) met behulp van een implementatiepijplijn Volledig stapelen.

## De SAML-verificatie aanroepen

De SAML authentificatiestroom kan van een Web-pagina van de Plaats van AEM worden aangehaald, door een speciaal gemaakte verbindingen, of een knopen te creëren. De hieronder beschreven parameters kunnen indien nodig via programmacode worden ingesteld, dus een aanmeldingsknop kan `saml_request_path` bijvoorbeeld instellen op verschillende AEM-pagina&#39;s, op basis van de context van de knop. Dit is de plaats waar de gebruiker naar SAML-verificatie is gegaan.

## Beveiligd in cache plaatsen tijdens gebruik van SAML

Op de AEM-publicatie-instantie worden de meeste pagina&#39;s doorgaans in cache geplaatst. Nochtans, voor SAML-Beschermde wegen, zou caching of moeten worden onbruikbaar gemaakt of beveiligd caching toegelaten gebruikend de configuratie auth_checker. Voor meer informatie, gelieve te verwijzen naar de details [&#x200B; hier &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-dispatcher/using/configuring/permissions-cache) worden verstrekt

Houd er rekening mee dat als u beveiligde paden in de cache plaatst zonder de AUth_checker in te schakelen, dit onvoorspelbaar gedrag kan veroorzaken.

### GET-verzoek

SAML-verificatie kan worden aangeroepen door een HTTP GET-aanvraag in de volgende indeling te maken:

`HTTP GET /system/sling/login`

en het verstrekken van vraagparameters:

| Naam van query-parameter | Parameterwaarde voor query |
|----------------------|-----------------------|
| `resource` | Om het even welke weg JCR, of sub-weg, die de authentificatiemanager van SAML is luistert, zoals bepaald in [&#x200B; Adobe Granite SAML 2.0 de Handler OSGi van de Authentificatie 1&rbrace; &#x200B;](#configure-saml-2-0-authentication-handler) bezit.`path` |
| `saml_request_path` | Het URL-pad waar de gebruiker naartoe moet gaan nadat de SAML-verificatie is voltooid. |

Deze HTML-koppeling activeert bijvoorbeeld de SAML-aanmeldstroom en als de gebruiker hiermee slaagt, gaat u naar `/content/wknd/us/en/protected/page.html` . Deze vraagparameters kunnen programmatic worden geplaatst zoals nodig.

```html
<a href="/system/sling/login?resource=/content/wknd&saml_request_path=/content/wknd/us/en/protected/page.html">
    Log in using SAML
</a>
```

## POST-verzoek

De authentificatie van SAML kan worden aangehaald door een HTTP POST- verzoek in het formaat te creëren:

`HTTP POST /system/sling/login`

en het verstrekken van de formuliergegevens:

| Naam van formuliergegevens | Waarde van formuliergegevens |
|----------------------|-----------------------|
| `resource` | Om het even welke weg JCR, of sub-weg, die de authentificatiemanager van SAML is luistert, zoals bepaald in [&#x200B; Adobe Granite SAML 2.0 de Handler OSGi van de Authentificatie 1&rbrace; &#x200B;](#configure-saml-2-0-authentication-handler) bezit.`path` |
| `saml_request_path` | Het URL-pad waar de gebruiker naartoe moet gaan nadat de SAML-verificatie is voltooid. |


Deze HTML-knop gebruikt bijvoorbeeld een HTTP POST om de SAML-aanmeldstroom te activeren en neem de gebruiker naar `/content/wknd/us/en/protected/page.html` wanneer dit lukt. Deze parameters voor formuliergegevens kunnen zo nodig via programmacode worden ingesteld.

```html
<form action="/system/sling/login" method="POST">
    <input type="hidden" name="resource" value="/content/wknd">
    <input type="hidden" name="saml_request_path" value="/content/wknd/us/en/protected/page.html">
    <input type="submit" value="Log in using SAML">
</form>
```

### Dispatcher-configuratie

Zowel de HTTP GET- als POST-methoden vereisen clienttoegang tot eindpunten van AEM `/system/sling/login` en moeten daarom via AEM Dispatcher worden toegestaan.

De benodigde URL-patronen toestaan op basis van het gebruik van GET of POST

```
# Allow GET-based SAML authentication invocation
/0191 { /type "allow" /method "GET" /url "/system/sling/login" /query "*" }

# Allow POST-based SAML authentication invocation
/0192 { /type "allow" /method "POST" /url "/system/sling/login" }
```
