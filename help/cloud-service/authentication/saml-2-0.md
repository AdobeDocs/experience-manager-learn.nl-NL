---
title: SAML 2.0 op AEM as a Cloud Service
description: Leer hoe u SAML 2.0-verificatie configureert op de AEM as a Cloud Service Publish-service.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9351
thumbnail: 343040.jpeg
last-substantial-update: 2024-05-15T00:00:00Z
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
duration: 2200
source-git-commit: 49f8df6e658b35aa3ba6e4f70cd39ff225c46120
workflow-type: tm+mt
source-wordcount: '3919'
ht-degree: 0%

---

# SAML 2.0-verificatie{#saml-2-0-authentication}

Leer hoe u eindgebruikers (niet AEM auteurs) instelt en verifieert voor een compatibele IDP voor SAML 2.0 van uw keuze.

## Welke SAML voor AEM as a Cloud Service?

SAML 2.0 integratie met AEM Publish (of Voorproef), staat eind - gebruikers van een AEM-gebaseerde Webervaring toe om aan niet-Adobe IDP (Identiteitsleverancier) voor authentiek te verklaren, en toegang AEM als genoemde, gemachtigde gebruiker.

|                       | AEM auteur | AEM Publish |
|-----------------------|:----------:|:-----------:|
| SAML 2.0-ondersteuning | ✘ | ✔ |

+++ Begrijp SAML 2.0 stroom met AEM

De doorstroming van een AEM integratie van Publish SAML is als volgt:

1. De gebruiker vraagt om Publish te AEM wanneer verificatie is vereist.
   + De gebruiker verzoekt om een van CUGs/ACL beschermde middel.
   + De gebruiker vraagt een middel dat aan een Vereiste van de Authentificatie onderworpen is.
   + De gebruiker volgt een verbinding aan AEM login eindpunt (d.w.z. `/system/sling/login`) dat uitdrukkelijk om de login actie verzoekt.
1. AEM maakt een AuthnRequest aan IDP, die IDP verzoekt om authentificatieproces te beginnen.
1. Gebruiker verifieert aan IDP.
   + De gebruiker wordt veroorzaakt door IDP voor geloofsbrieven.
   + De gebruiker is reeds voor authentiek verklaard met IDP en moet geen verdere geloofsbrieven verstrekken.
1. IDP produceert een bevestiging van SAML die de gegevens van de gebruiker bevat, en ondertekent het gebruikend het privé certificaat van IDP.
1. IDP verzendt de bevestiging van SAML via de POST van HTTP, als Webbrowser van de gebruiker, naar AEM Publish.
1. AEM Publish ontvangt de bevestiging van SAML, en bevestigt de integriteit en de authenticiteit van de bevestiging van SAML gebruikend het openbare certificaat IDP.
1. AEM Publish beheert het AEM gebruikersverslag dat op de configuratie van SAML 2.0 OSGi, en de inhoud van de Assertion van SAML wordt gebaseerd.
   + Maakt gebruiker
   + Gebruikerskenmerken synchroniseren
   + Updates AEM gebruikersgroeplidmaatschap
1. AEM Publish stelt de AEM `login-token` cookie in op de HTTP-respons, die wordt gebruikt om volgende aanvragen bij AEM Publish te verifiëren.
1. AEM Publish leidt de gebruiker om naar de URL op AEM Publish, zoals opgegeven door het cookie `saml_request_path` .

+++

## Configuratie doorlopen

>[!VIDEO](https://video.tv.adobe.com/v/343040?quality=12&learn=on)

Deze video doorloopt het opzetten van SAML 2.0 integratie met de dienst van AEM as a Cloud Service Publish, en het gebruiken van Okta als IDP.

## Vereisten

Het volgende wordt vereist wanneer vestiging SAML 2.0 authentificatie:

+ Toegang tot Cloud Manager via Deployment Manager
+ Toegang van beheerders tot de AEM as a Cloud Service-omgeving AEM
+ Beheerderstoegang tot de IDP
+ Naar keuze, toegang tot openbaar/privé sleutelpaar wordt gebruikt aan encryptieSAML nuttige ladingen

SAML 2.0 wordt alleen ondersteund voor het verifiëren van toepassingen voor AEM Publish of Voorvertoning. Om de authentificatie van AEM Auteur te beheren die en IDP gebruiken, [ integreert IDP met Adobe IMS ](https://helpx.adobe.com/nl/enterprise/using/set-up-identity.html).


## Openbaar certificaat van IDP installeren op AEM

Het openbare certificaat van IDP wordt toegevoegd aan AEM Globale Opslag van het Vertrouwen, en gebruikt om de bevestiging te bevestigen SAML die door IDP wordt verzonden is geldig.

+++SAML-ondertekeningsstroom

![ SAML 2.0 - de Assertion die van IDP SAML ](./assets/saml-2-0/idp-signing-diagram.png) ondertekent

1. Gebruiker verifieert aan IDP.
1. IDP produceert een bevestiging van SAML die de gegevens van de gebruiker bevat.
1. IDP ondertekent de bevestiging van SAML gebruikend het privé certificaat van IDP.
1. IDP stelt een cliënt-kant POST van HTTP in werking om het eindpunt van SAML van Publish te AEM (`.../saml_login`) dat de ondertekende bevestiging van SAML omvat.
1. AEM Publish ontvangt de HTTP-POST die de ondertekende SAML-bevestiging bevat, kan de handtekening valideren met behulp van het IDP-openbare certificaat.

+++

![ voeg het openbare certificaat IDP aan de Globale Opslag van het Vertrouwen toe ](./assets/saml-2-0/global-trust-store.png)

1. Verkrijg het __openbare certificaat__ dossier van IDP. Dit certificaat staat AEM toe om de bevestiging te bevestigen SAML die aan AEM door IDP wordt verstrekt.

   Het certificaat heeft de PEM-indeling en moet lijken op:

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. Meld u aan bij AEM auteur als AEM beheerder.
1. Navigeer aan __Hulpmiddelen > Veiligheid > de Opslag van het Vertrouwen__.
1. Maak of open de Global Trust Store. Als u een Global Trust Store maakt, slaat u het wachtwoord op een veilige plaats op.
1. Breid __uit voegt certificaat van het dossier van CER__ toe.
1. Selecteer __Uitgezochte Dossier van het Certificaat__, en upload het certificaatdossier dat door IDP wordt verstrekt.
1. Verlaat __Certificaat van de Kaart aan Gebruiker__ leeg.
1. Selecteer __voorleggen__.
1. Het onlangs toegevoegde certificaat verschijnt boven __voegt certificaat van CRT dossier__ sectie toe.
1. Maak nota van __alias__, aangezien deze waarde in de [ SAML 2.0 Configuratie OSGi van de Handler van de Authentificatie OSGi ](#saml-2-0-authentication-handler-osgi-configuration) wordt gebruikt.
1. Selecteer __sparen &amp; Sluiten__.

De globale opslag van het Vertrouwen wordt gevormd met het openbare certificaat van IDP op AEM Auteur, maar aangezien SAML slechts op AEM Publish wordt gebruikt, moet de Globale Opslag van het Vertrouwen aan AEM Publish voor het openbare certificaat worden herhaald IDP om daar toegankelijk te zijn.

![ Repliceer de Globale Opslag van het Vertrouwen aan AEM Publish ](./assets/saml-2-0/global-trust-store-replicate.png)

1. Navigeer aan __Hulpmiddelen > Plaatsing > Pakketten__.
1. Een pakket maken
   + Pakketnaam: `Global Trust Store`
   + Versie: `1.0.0`
   + Groep: `com.your.company`
1. Bewerk het nieuwe __Globale pakket van de Opslag van het Vertrouwen__.
1. Selecteer het __lusje van Filters__, en voeg een filter voor de wortelweg `/etc/truststore` toe.
1. Selecteer __Gedaan__ en dan __sparen__.
1. Selecteer de __bouwt__ knoop voor het __Globale pakket van de Opslag van het Vertrouwen__.
1. Zodra gebouwd, selecteer __Meer__ > __Repliceer__ om de Globale knoop van de Opslag van het Vertrouwen (`/etc/truststore`) te activeren om Publish te AEM.

## Het sleutelarchief voor de verificatieservice maken{#authentication-service-keystore}

_Creërend een sleutelarchief voor authentificatie-dienst wordt vereist wanneer [ SAML 2.0 het configuratiebezit OSGi van de authentificatiemanager `handleLogout` aan `true`](#saml-20-authenticationsaml-2-0-authentication) wordt geplaatst of wanneer [ AuthnRequest het ondertekenen/SAML- assertieecryptie ](#install-aem-public-private-key-pair) wordt vereist_

1. Meld u aan bij AEM auteur als AEM beheerder om de persoonlijke sleutel te uploaden.
1. Navigeer aan __Hulpmiddelen > Veiligheid > Gebruikers__, en selecteer __authentificatie-dienst__ gebruiker, en selecteer __Eigenschappen__ van de hoogste actiebar.
1. Selecteer __Keystore__ tabel.
1. Maak of open het sleutelarchief. Houd het wachtwoord veilig als u een sleutelarchief maakt.
   + A [ openbaar/privé keystore wordt geïnstalleerd in dit keystore ](#install-aem-public-private-key-pair) slechts als AuthnRequest het ondertekenen/SAML beweringsencryptie wordt vereist.
   + Als deze integratie SAML logout steunt, maar niet AuthnRequest het ondertekenen/SAML bewering, dan is een leeg sleutelarchief voldoende.
1. Selecteer __sparen &amp; Sluiten__.
1. Creeer een pakket dat de bijgewerkte __authentificatie-dienst__ gebruiker bevat.

   _gebruik de volgende tijdelijke tijdelijke tijdelijke oplossing gebruikend pakketten:_

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
   1. Zodra gebouwd, selecteer __Meer__ > __Repliceer__ om de belangrijkste opslag van de Dienst van de Authentificatie aan AEM Publish te activeren.

## Paar AEM openbare/persoonlijke sleutel installeren{#install-aem-public-private-key-pair}

_het installeren van het AEM openbare/privé zeer belangrijke paar is facultatief_

AEM Publish kan worden gevormd om AuthnRequests (aan IDP) te ondertekenen, en de beweringen van SAML (aan AEM) te coderen. Dit wordt bereikt door een persoonlijke sleutel aan AEM Publish te verstrekken, en het past openbare sleutel aan IDP aan.

+++ Begrijp de ondertekeningsstroom AuthnRequest (optioneel)

De AuthnRequest (het verzoek aan IDP van AEM Publish dat het login proces in werking stelt) kan door AEM Publish worden ondertekend. Hiervoor ondertekent AEM Publish de AuthnRequest met behulp van de persoonlijke sleutel, die IDP vervolgens de handtekening valideert met behulp van de openbare sleutel. Dit garandeert aan de IDP dat AuthnRequest is gestart en aangevraagd door AEM Publish, en niet door een kwaadwillige derde.

![ SAML 2.0 - het ondertekenen van SP AuthnRequest ](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. De gebruiker doet een HTTP- verzoek aan AEM Publish die in een de authentificatieverzoek van SAML aan IDP resulteert.
1. AEM Publish produceert het SAML verzoek om naar IDP te verzenden.
1. AEM Publish ondertekent het SAML-verzoek met behulp van AEM persoonlijke sleutel.
1. AEM Publish de AuthnRequest, een cliënt-zijomleiding van HTTP aan IDP in werking stelt die het ondertekende verzoek van SAML bevat.
1. IDP ontvangt het AuthnRequest, en bevestigt de handtekening gebruikend AEM openbare sleutel, die ervoor zorgt AEM Publish in werking stelde AuthnRequest.
1. AEM Publish bevestigt dan de gedecrypteerde integriteit en de authenticiteit van SAML bewering gebruikend het IDP openbare certificaat.

+++

+++ Begrijp de de beweringsencryptiesstroom van SAML (facultatief)

Alle HTTP-communicatie tussen IDP en AEM Publish moet via HTTPS plaatsvinden en moet daarom standaard worden beveiligd. Nochtans, zoals vereist, kunnen de beweringen van SAML worden gecodeerd in het geval wordt de extra vertrouwelijkheid vereist bovenop die verstrekt door HTTPS. Om dit te doen, codeert IDP de gegevens van de Bevestiging van SAML gebruikend de privé sleutel, en AEM Publish decrypteert de bewering van SAML gebruikend de privé sleutel.

![ SAML 2.0 - de encryptie van de Assertion van SP SAML ](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. Gebruiker verifieert aan IDP.
1. IDP produceert een bevestiging van SAML die de gegevens van de gebruiker bevat, en ondertekent het gebruikend het privé certificaat van IDP.
1. IDP codeert dan de bewering van SAML met AEM openbare sleutel, die de AEM privé sleutel vereist om te decrypteren.
1. De gecodeerde SAML-bewering wordt via de webbrowser van de gebruiker naar AEM Publish verzonden.
1. AEM Publish ontvangt de bevestiging van SAML, en decrypteert het gebruikend AEM privé sleutel.
1. Bij IDP wordt de gebruiker gevraagd om te verifiëren.

+++

Zowel AuthnRequest het ondertekenen, als de bevestiging van SAML encryptie zijn facultatief, nochtans worden zij allebei toegelaten, gebruikend het [ SAML 2.0 de configuratiebezit OSGi van de authentificatiemanager OSGi `useEncryption`](#saml-20-authenticationsaml-2-0-authentication), betekenend allebei of geen kunnen worden gebruikt.

![ AEM authentificatie-dienst zeer belangrijke opslag ](./assets/saml-2-0/authentication-service-key-store.png)

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
1. Meld u aan bij AEM auteur als AEM beheerder om de persoonlijke sleutel te uploaden.
1. Navigeer aan __Hulpmiddelen > Veiligheid > de Opslag van het Vertrouwen__, en selecteer __authentificatie-dienst__ gebruiker, en selecteer __Eigenschappen__ van de hoogste actiebar.
1. Navigeer aan __Hulpmiddelen > Veiligheid > Gebruikers__, en selecteer __authentificatie-dienst__ gebruiker, en selecteer __Eigenschappen__ van de hoogste actiebar.
1. Selecteer __Keystore__ tabel.
1. Maak of open het sleutelarchief. Houd het wachtwoord veilig als u een sleutelarchief maakt.
1. Selecteer __voeg privé sleutel van het dossier van de DER__ toe, en voeg de privé sleutel en het kettingdossier aan AEM toe:
   + __alias__: Verstrek een betekenisvolle naam, vaak de naam van IDP.
   + __Persoonlijk zeer belangrijk dossier__: Upload het privé zeer belangrijke dossier (PKCS#8 in formaat DER).
      + Met de bovenstaande methode `openssl` is dit het `aem-private-pkcs8.der` -bestand
   + __Uitgezochte dossier van de certificaatketting__: Upload het begeleidende ketendossier (dit kan de openbare sleutel zijn).
      + Met de bovenstaande methode `openssl` is dit het `aem-public.crt` -bestand
   + Selecteer __voorleggen__
1. Het onlangs toegevoegde certificaat verschijnt boven __voegt certificaat van CRT dossier__ sectie toe.
   + Maak nota van __alias__ aangezien dit in [ SAML 2.0 de configuratie van de authentificatiemanager OSGi ](#saml-20-authentication-handler-osgi-configuration) wordt gebruikt
1. Selecteer __sparen &amp; Sluiten__.
1. Creeer een pakket dat de bijgewerkte __authentificatie-dienst__ gebruiker bevat.

   _gebruik de volgende tijdelijke tijdelijke tijdelijke oplossing gebruikend pakketten:_

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
   1. Zodra gebouwd, selecteer __Meer__ > __Repliceer__ om de belangrijkste opslag van de Dienst van de Authentificatie aan AEM Publish te activeren.

## SAML 2.0-verificatiehandler configureren{#configure-saml-2-0-authentication-handler}

AEM de configuratie van SAML wordt uitgevoerd via de __Adobe granite SAML 2.0 Handler van de Authentificatie__ OSGi configuratie.
De configuratie is een OSGi fabrieksconfiguratie, die één enkele dienst van AEM as a Cloud Service Publish betekent kan veelvoudige SAML configuratie hebben die discrete middelenbomen van de bewaarplaats behandelen; dit is nuttig voor multi-plaats AEM plaatsingen.

+++ SAML 2.0 OSGi-configuratiegids van de Handler van de Authentificatie

### Adobe granite SAML 2.0 de Configuratie van de Handler OSGi van de Verificatie{#configure-saml-2-0-authentication-handler-osgi-configuration}

|                                   | OSGi, eigenschap | Vereist | Waarde-indeling | Standaardwaarde | Beschrijving |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| Paden | `path` | ✔ | Tekenreeksarray | `/` | AEM paden waarvoor deze verificatiehandler wordt gebruikt. |
| IDP-URL | `idpUrl` | ✔ | String |                           | IDP URL het de authentificatieverzoek van SAML wordt verzonden. |
| IDP-certificaatalias | `idpCertAlias` | ✔ | String |                           | De alias van het IDP-certificaat in de AEM Global Trust Store |
| IDP HTTP-omleiding | `idpHttpRedirect` | ✘ | Boolean | `false` | Geeft aan of een HTTP-omleiding naar de IDP-URL plaatsvindt in plaats van een AuthnRequest te verzenden. Ingesteld op `true` voor door IDP geïnitieerde verificatie. |
| IDP-id | `idpIdentifier` | ✘ | String |                           | Unieke IDP-id om ervoor te zorgen dat AEM gebruiker en groep uniek zijn. Als de waarde leeg is, wordt in plaats daarvan `serviceProviderEntityId` gebruikt. |
| Bevestigingsservice-URL | `assertionConsumerServiceURL` | ✘ | String |                           | Het URL-kenmerk `AssertionConsumerServiceURL` in de AuthnRequest die opgeeft waar het `<Response>` -bericht naar AEM moet worden verzonden. |
| SP-entiteit-id | `serviceProviderEntityId` | ✔ | String |                           | Identificeert uniek AEM aan IDP; gewoonlijk de AEM gastheernaam. |
| SP-codering | `useEncryption` | ✘ | Boolean | `true` | Wijst erop als IDP de beweringen van SAML codeert. `spPrivateKeyAlias` en `keyStorePassword` moeten worden ingesteld. |
| SP alias van persoonlijke sleutel | `spPrivateKeyAlias` | ✘ | String |                           | De alias van de persoonlijke sleutel in de sleutelarchief van de `authentication-service` gebruiker. Vereist als `useEncryption` is ingesteld op `true` . |
| Wachtwoord voor opslagmap met SP-sleutel | `keyStorePassword` | ✘ | String |                           | Het wachtwoord van de sleutelarchief van de &quot;authentificatie-dienst&quot;gebruiker. Vereist als `useEncryption` is ingesteld op `true` . |
| Standaardomleiding | `defaultRedirectUrl` | ✘ | String | `/` | De standaard omleidings-URL na geslaagde verificatie. Kan relatief zijn ten opzichte van de AEM host (bijvoorbeeld `/content/wknd/us/en/html` ). |
| Kenmerk Gebruikersnaam | `userIDAttribute` | ✘ | String | `uid` | De naam van het SAML-assertiekenmerk dat de gebruikers-id van de AEM gebruiker bevat. Laat leeg om de `Subject:NameId` te gebruiken. |
| AEM gebruikers automatisch maken | `createUser` | ✘ | Boolean | `true` | Hiermee geeft u aan of AEM gebruikers worden gemaakt op geslaagde verificatie. |
| Tussenpad voor gebruiker AEM | `userIntermediatePath` | ✘ | String |                           | Wanneer u AEM gebruikers maakt, wordt deze waarde gebruikt als het tussenliggende pad (bijvoorbeeld `/home/users/<userIntermediatePath>/jane@wknd.com` ). `createUser` moet worden ingesteld op `true` . |
| Gebruikerskenmerken AEM | `synchronizeAttributes` | ✘ | Tekenreeksarray |                           | Lijst met SAML-kenmerktoewijzingen die worden opgeslagen op de AEM gebruiker, in de indeling `[ "saml-attribute-name=path/relative/to/user/node" ]` (bijvoorbeeld `[ "firstName=profile/givenName" ]` ). Zie de [ volledige lijst van inheemse AEM attributen ](#aem-user-attributes). |
| Gebruiker toevoegen aan AEM groepen | `addGroupMemberships` | ✘ | Boolean | `true` | Geeft aan of een AEM gebruiker automatisch wordt toegevoegd aan AEM gebruikersgroepen nadat de verificatie is voltooid. |
| AEM kenmerk voor groepslidmaatschap | `groupMembershipAttribute` | ✘ | String | `groupMembership` | De naam van de de assertieattributen van SAML die een lijst van AEM gebruikersgroepen bevatten de gebruiker zou moeten worden toegevoegd aan. `addGroupMemberships` moet worden ingesteld op `true` . |
| Standaard AEM | `defaultGroups` | ✘ | Tekenreeksarray |                           | Er wordt altijd een lijst met AEM gebruikersgroepen waaraan geverifieerde gebruikers zijn toegevoegd (bijvoorbeeld `[ "wknd-user" ]` ). `addGroupMemberships` moet worden ingesteld op `true` . |
| NameIDPopolicy-indeling | `nameIdFormat` | ✘ | String | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | De waarde van de NameIDPopolicy formaatparameter in het AuthnRequest bericht te verzenden. |
| SAML-respons opslaan | `storeSAMLResponse` | ✘ | Boolean | `false` | Geeft aan of de `samlResponse` -waarde wordt opgeslagen op het AEM `cq:User` -knooppunt. |
| Afmelden handgreep | `handleLogout` | ✘ | Boolean | `false` | Wijst erop als het logout verzoek door deze de authentificatiemanager van SAML wordt behandeld. `logoutUrl` moet worden ingesteld. |
| Afmeldings-URL | `logoutUrl` | ✘ | String |                           | De URL van IDP waar het SAML logout verzoek wordt verzonden naar. Vereist als `handleLogout` is ingesteld op `true` . |
| Kloktolerantie | `clockTolerance` | ✘ | Geheel | `60` | Bij het valideren van SAML-beweringen tekent IDP en AEM (SP) de tolerantie voor klokschuintrekken. |
| Methode Digest | `digestMethod` | ✘ | String | `http://www.w3.org/2001/04/xmlenc#sha256` | Het samenvattingsalgoritme dat IDP gebruikt wanneer het ondertekenen van een SAML- bericht. |
| Handtekeningmethode | `signatureMethod` | ✘ | String | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | Het handtekeningalgoritme dat IDP gebruikt bij het ondertekenen van een SAML-bericht. |
| Type identiteitssynchronisatie | `identitySyncType` | ✘ | `default` of `idp` | `default` | Wijzig de standaardinstelling voor AEM as a Cloud Service niet. `from` |
| Servicerangschikking | `service.ranking` | ✘ | Geheel | `5002` | Hogere rangschikkingsconfiguraties hebben de voorkeur voor dezelfde `path` . |

### Gebruikerskenmerken AEM{#aem-user-attributes}

AEM gebruikt de volgende gebruikersattributen, die via het `synchronizeAttributes` bezit in de Adobe Granite SAML 2.0 de Configuratie OSGi van de Handler van de Authentificatie kunnen worden bevolkt.  Om het even welke attributen IDP kunnen aan om het even welk AEM gebruikersbezit worden gesynchroniseerd, nochtans het in kaart brengen aan AEM (hieronder vermeld) eigenschappen van het gebruiksattribuut laat AEM toe om hen natuurlijk te gebruiken.

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

Wanneer [ het coderen van de bevestiging AuthnRequest en SAML ](#encrypting-the-authnrequest-and-saml-assertion), worden de volgende eigenschappen vereist: `useEncryption`, `spPrivateKeyAlias`, en `keyStorePassword`. `keyStorePassword` bevat een wachtwoord daarom moet de waarde niet in het OSGi configuratiedossier worden opgeslagen, maar eerder ingespoten gebruikend [ geheime configuratiewaarden ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values)

+++Optioneel, werk de configuratie OSGi bij om encryptie te gebruiken

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
+ `keyStorePassword` bevat een [ OSGi geheime configuratievariabele ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values) die het `authentication-service` wachtwoord van het gebruikerssleutelarchief bevat.

+++

## Referentiefilter configureren

Tijdens het SAML authentificatieproces, stelt IDP een cliënt-kant POST van HTTP in werking om het eindpunt van Publish `.../saml_login` te AEM. Als IDP en AEM Publish op verschillende oorsprong bestaan, AEM de __Filter van de Referateur van Publish__ via configuratie OSGi wordt gevormd om HTTP POSTs van de oorsprong van IDP toe te staan.

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

AEM Publish steunt één enkele de filterconfiguratie van de Referateur, zo verenigt de configuratievereisten van SAML, met om het even welke bestaande configuraties.

OSGi-configuraties per omgeving (`config.publish.dev`, `config.publish.stage` en `config.publish.prod` ) kunnen met specifieke kenmerken worden gedefinieerd als de `allow.hosts` (of `allow.hosts.regex` ) per omgeving verschilt.

## Resource Sharing (CORS) voor meerdere bronnen configureren

Tijdens het SAML authentificatieproces, stelt IDP een cliënt-kant POST van HTTP in werking om het eindpunt van Publish `.../saml_login` te AEM. Als IDP en AEM Publish op verschillende gastheren/domeinen bestaan, AEM Publish __CRoss-Origin Middel dat (CORS) deelt__ moet worden gevormd om HTTP POSTs van de gastheer/het domein van IDP toe te staan.

De header `Origin` van deze HTTP POST request heeft gewoonlijk een andere waarde dan de AEM Publish host, waardoor CORS-configuratie vereist is.

Bij het testen van SAML-verificatie op de lokale AEM-SDK (`localhost:4503`), kan de IDP de `Origin` header instellen op `null` . Als dat het geval is, voegt u `"null"` toe aan de lijst `alloworigin` .

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

## Configureer AEM Dispatcher om SAML HTTP POST&#39;s toe te staan

Na succesvolle authentificatie aan IDP, zal IDP een POST van HTTP terug naar AEM geregistreerd `/saml_login` eindpunt (gevormd in IDP) orchestreren. Deze HTTP-POST naar `/saml_login` wordt standaard geblokkeerd in Dispatcher, dus moet dit expliciet worden toegestaan met de volgende Dispatcher-regel:

1. Open `dispatcher/src/conf.dispatcher.d/filters/filters.any` in uw winde.
1. Voeg onder aan het bestand toe en sta regel voor HTTP POST&#39;s toe aan URL&#39;s die eindigen met `/saml_login` .

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

Als URL die bij de Apache webserver herschrijft (`dispatcher/src/conf.d/rewrites/rewrite.rules`) wordt gevormd, zorg ervoor dat de verzoeken aan de `.../saml_login` eindpunten niet per ongeluk worden beheerd.

### Hoe te om Dynamisch Lidmaatschap van de Groep voor de Gebruikers van SAML in nieuwe milieu&#39;s toe te laten

Om de prestaties van de groepsevaluatie in nieuwe AEM as a Cloud Service-omgevingen aanzienlijk te verbeteren, wordt de activering van de functie Dynamic Group Membership aanbevolen in nieuwe omgevingen.
Dit is ook een noodzakelijke stap wanneer de gegevenssynchronisatie wordt geactiveerd. Meer details [ hier ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier).
Hiervoor voegt u de volgende eigenschap toe aan het configuratiebestand van OSGI:

`/apps/example/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~example.cfg.json`

Met deze configuratie, worden de gebruikers en de groepen gecreeerd als [ Externe Gebruikers van Oak ](https://jackrabbit.apache.org/oak/docs/security/authentication/identitymanagement.html). In AEM hebben externe gebruikers en groepen de standaardwaarde `rep:principalName` die wordt samengesteld door `[user name];[idp]` of `[group name];[idp]` .
Merk op dat de Lijsten van het Toegangsbeheer (ACL) met PrincipalName van gebruikers of groepen worden geassocieerd.
Wanneer het opstellen van deze configuratie in een bestaande plaatsing waar eerder `identitySyncType` niet werd gespecificeerd of aan `default` werd geplaatst, zullen de nieuwe gebruikers en de groepen worden gecreeerd en ACL moet op deze nieuwe gebruikers en groepen worden toegepast. Externe groepen kunnen geen lokale gebruikers bevatten. [ opnieuw richt ](https://sling.apache.org/documentation/bundles/repository-initialization.html) kan worden gebruikt om ACL voor Externe groepen van SAML tot stand te brengen, zelfs als zij slechts zullen worden gecreeerd wanneer de gebruiker login zal uitvoeren.
Om dit refactoring op ACL te vermijden, is een standaard [ migratieeigenschap ](#automatic-migration-to-dynamic-group-membership-for-existing-environments) uitgevoerd.

### Hoe de lidmaatschappen in lokale en externe groepen met dynamisch groepslidmaatschap worden opgeslagen

In lokale groepen worden de groepsleden opgeslagen in het oak-kenmerk: `rep:members` . Het attribuut bevat de lijst van uid van elk lid van de groep. De extra details kunnen [ hier ](https://jackrabbit.apache.org/oak/docs/security/user/membership.html#member-representation-in-the-repository) worden gevonden.
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
Het groepslidmaatschap wordt in plaats daarvan opgeslagen in de gebruikersingangen. De extra documentatie kan [ hier ](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html) worden gevonden. Dit is bijvoorbeeld het OAK-knooppunt voor de groep:

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

### Automatische migratie naar dynamisch groepslidmaatschap voor bestaande omgevingen

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

Het groepslidmaatschap voor externe groepen wordt opgeslagen in het gebruikersprofiel in het kenmerk `rep:authorizableId`

### Automatische migratie naar dynamisch groepslidmaatschap configureren

1. De eigenschap `"identitySyncType": "idp_dynamic_simplified_id"` inschakelen in het SAML OSGI-configuratiebestand: `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json`
2. Vorm de nieuwe dienst OSGI met PID: `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~...` met het bezit:

```
{
  "idpIdentifier": "<vaule of identitySyncType of saml configuration to be migrated>"
}
```

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

De SAML authentificatiestroom kan van een AEM Web-pagina van de Plaats worden aangehaald, door een speciaal gemaakte verbindingen, of een knopen te creëren. De hieronder beschreven parameters kunnen indien nodig via programmacode worden ingesteld, dus een aanmeldknop kan `saml_request_path` bijvoorbeeld instellen op verschillende AEM pagina&#39;s, op basis van de context van de knop.

### Verzoek om GET

De authentificatie van SAML kan worden aangehaald door een verzoek van de GET van HTTP in het formaat te creëren:

`HTTP GET /system/sling/login`

en het verstrekken van vraagparameters:

| Naam van query-parameter | Parameterwaarde voor query |
|----------------------|-----------------------|
| `resource` | Om het even welke weg JCR, of sub-weg, die de authentificatiemanager van SAML is luistert, zoals bepaald in het ](#configure-saml-2-0-authentication-handler) `path` bezit van de Adobe granite SAML 2.0 van de Authentificatie Handler OSGi van de configuratie 1} {.[ |
| `saml_request_path` | Het URL-pad waar de gebruiker naartoe moet gaan nadat de SAML-verificatie is voltooid. |

Deze HTML-koppeling activeert bijvoorbeeld de SAML-aanmeldstroom en neemt de gebruiker met succes naar `/content/wknd/us/en/protected/page.html` . Deze vraagparameters kunnen programmatic worden geplaatst zoals nodig.

```html
<a href="/system/sling/login?resource=/content/wknd&saml_request_path=/content/wknd/us/en/protected/page.html">
    Log in using SAML
</a>
```

## Aanvraag POST

De authentificatie van SAML kan worden aangehaald door een verzoek van de POST van HTTP in het formaat te creëren:

`HTTP POST /system/sling/login`

en het verstrekken van de formuliergegevens:

| Naam van formuliergegevens | Waarde van formuliergegevens |
|----------------------|-----------------------|
| `resource` | Om het even welke weg JCR, of sub-weg, die de authentificatiemanager van SAML is luistert, zoals bepaald in het ](#configure-saml-2-0-authentication-handler) `path` bezit van de Adobe granite SAML 2.0 van de Authentificatie Handler OSGi van de configuratie 1} {.[ |
| `saml_request_path` | Het URL-pad waar de gebruiker naartoe moet gaan nadat de SAML-verificatie is voltooid. |


Bijvoorbeeld, zal deze knoop van HTML een POST van HTTP gebruiken om de het login van SAML stroom teweeg te brengen, en wanneer succes, neem de gebruiker aan `/content/wknd/us/en/protected/page.html`. Deze parameters voor formuliergegevens kunnen zo nodig via programmacode worden ingesteld.

```html
<form action="/system/sling/login" method="POST">
    <input type="hidden" name="resource" value="/content/wknd">
    <input type="hidden" name="saml_request_path" value="/content/wknd/us/en/protected/page.html">
    <input type="submit" value="Log in using SAML">
</form>
```

### Dispatcher-configuratie

Zowel de HTTP-methode voor GET als de HTTP-methode vereisen clienttoegang tot AEM eindpunten van `/system/sling/login` , zodat deze via AEM Dispatcher moeten worden toegestaan.

De benodigde URL-patronen toestaan op basis van het gebruik van GET of POST

```
# Allow GET-based SAML authentication invocation
/0191 { /type "allow" /method "GET" /url "/system/sling/login" /query="*" }

# Allow POST-based SAML authentication invocation
/0192 { /type "allow" /method "POST" /url "/system/sling/login" }
```
