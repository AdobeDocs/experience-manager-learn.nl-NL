---
title: SAML 2.0 op AEM als cloudservice
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
source-git-commit: 2ed303e316577363f6d1c265ef7f9cd6d81491d8
workflow-type: tm+mt
source-wordcount: '4277'
ht-degree: 0%

---

# SAML 2.0-verificatie{#saml-2-0-authentication}

Leer hoe u eindgebruikers (niet AEM-auteurs) instelt en verifieert op een door u gewenste SAML 2.0 compatibele IDP.

## Welke SAML voor AEM as a Cloud Service?

Dankzij SAML 2.0-integratie met AEM Publish (of Preview) kunnen eindgebruikers van een op AEM gebaseerde webervaring zich verifiëren bij een niet-Adobe IDP (Identity Provider) en hebben ze toegang tot AEM als een benoemde, geautoriseerde gebruiker.

|                       | AEM-auteur | AEM Publiceren |
|-----------------------|:----------:|:-----------:|
| SAML 2.0 ondersteuning | ✘ | ✔ |

+++ Begrijp de SAML 2.0-flow met AEM

De typische flow van een AEM Publish SAML-integratie is als volgt:

1. De gebruiker doet een verzoek om AEM Publish te publiceren, wat aangeeft dat authenticatie vereist is.
   + De gebruiker vraagt om een CUGs/ACL-beschermde bron.
   + De gebruiker vraagt een bron aan die onder een Authenticatievereiste valt.
   + De gebruiker volgt een link naar het inlog-endpoint van AEM (d.w.z. `/system/sling/login`) die expliciet om de inlogactie vraagt.
1. AEM doet een AuthnRequest aan de IDP, waarin de IDP wordt gevraagd het authenticatieproces te starten.
1. De gebruiker authenticeert zich bij IDP.
   + De gebruiker wordt door de IDP gevraagd om inloggegevens.
   + De gebruiker is al geauthenticeerd bij de IDP en hoeft geen verdere inloggegevens te verstrekken.
1. IDP genereert een SAML-assertie met de gegevens van de gebruiker en ondertekent deze met het privécertificaat van de IDP.
1. IDP stuurt de SAML-assertie via HTTP POST, via de webbrowser van de gebruiker (RESPECTIVE_PROTECTED_PATH/saml_login), naar AEM Publish.
1. AEM Publish ontvangt de SAML-assertie en valideert de integriteit en authenticiteit van de SAML-assertie met behulp van het IDP-publieke certificaat.
1. AEM Publish beheert het AEM-gebruikersrecord op basis van de SAML 2.0 OSGi-configuratie en de inhoud van de SAML-assertie.
   + Maakt gebruiker aan
   + Synchroniseert gebruikersattributen
   + Updates van het lidmaatschap van de AEM-gebruikersgroep
1. AEM Publish stelt de AEM-cookie `login-token` in op het HTTP-antwoord, dat wordt gebruikt om opeenvolgende verzoeken aan AEM Publish te authenticeren.
1. AEM Publish verwijst de gebruiker naar de URL op AEM Publish zoals gespecificeerd door de `saml_request_path` cookie.

+++

## Configuratie walkthrough

>[!VIDEO](https://video.tv.adobe.com/v/3455347?captions=dut&quality=12&learn=on)

Deze video laat zien hoe je SAML 2.0-integratie met AEM opzet als een Cloud Service Publish-dienst en Okta als IDP gebruikt.

## Vereisten

Het volgende is vereist bij het instellen van SAML 2.0-authenticatie:

+ Toegang tot Cloud Manager via Deployment Manager
+ AEM Administrator toegang tot AEM as a Cloud Service-omgeving
+ Beheerderstoegang tot de IDP
+ Naar keuze, toegang tot openbaar/privé sleutelpaar wordt gebruikt aan encryptieSAML nuttige ladingen
+ AEM Sites pagina&#39;s (of paginabomen), die aan AEM worden gepubliceerd, en [&#x200B; beschermd door Gesloten Gebruikersgroepen (CUGs) &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties#permissions)

SAML 2.0 wordt alleen ondersteund voor het verifiëren van toepassingen voor publicatie of voorvertoning in AEM. Om de authentificatie van de Auteur van AEM te beheren die en IDP gebruiken, [&#x200B; integreert IDP met Adobe IMS &#x200B;](https://helpx.adobe.com/nl/enterprise/using/set-up-identity.html).


## IDP openbaar certificaat installeren op AEM

Het publieke certificaat van de IDP wordt toegevoegd aan de Global Trust Store van AEM en gebruikt om te valideren dat de door de IDP verzonden SAML-assertie geldig is.

+++SAML-assertie-ondertekeningsflow

![SAML 2.0 - IDP SAML Assertion ondertekening](./assets/saml-2-0/idp-signing-diagram.png)

1. De gebruiker authenticeert zich bij IDP.
1. IDP genereert een SAML-assertie met de gegevens van de gebruiker.
1. IDP ondertekent de SAML-claim met het privécertificaat van de IDP.
1. IDP start een clientzijde HTTP POST naar het SAML-eindpunt (`.../saml_login`) van AEM Publish dat de ondertekende SAML-assertie bevat.
1. AEM Publish ontvangt de HTTP POST met de ondertekende SAML-assertie, kan de handtekening valideren met het IDP publieke certificaat.

+++

![Voeg het IDP publieke certificaat toe aan de Global Trust Store](./assets/saml-2-0/global-trust-store.png)

1. Verkrijg het __openbare certificaat__ dossier van IDP. Dit certificaat stelt AEM in staat de SAML-assertie te valideren die door de IDP aan AEM wordt geleverd.

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
1. Vouw Certificaat __toevoegen uit het CER-bestand__.
1. Selecteer __Select Certificate File__ en upload het certificaatbestand dat door de IDP wordt geleverd.
1. Laat __het kaartcertificaat aan de gebruiker__ leeg.
1. Selecteer __Indienen__.
1. Het nieuw toegevoegde certificaat verschijnt boven het __gedeelte &#39;Voeg certificaat uit CRT&#39; toe__ .
1. Let op de __alias__, aangezien deze waarde wordt gebruikt in de [SAML 2.0 Authentication Handler OSGi-configuratie](#saml-2-0-authentication-handler-osgi-configuration).
1. Selecteer __Opslaan &amp; Sluiten__.

De Global Trust Store wordt geconfigureerd met het openbare certificaat van IDP voor AEM-auteur, maar aangezien SAML alleen wordt gebruikt in AEM Publish, moet de Global Trust Store worden gerepliceerd naar AEM Publish om het openbare certificaat IDP daar toegankelijk te maken.

![&#x200B; Herhaal de Globale Opslag van het Vertrouwen aan AEM publiceren &#x200B;](./assets/saml-2-0/global-trust-store-replicate.png)

1. Navigeer aan __Hulpmiddelen > Plaatsing > Pakketten__.
1. Een pakket maken
   + Pakketnaam: `Global Trust Store`
   + Versie: `1.0.0`
   + Groep: `com.your.company`
1. Bewerk het nieuwe __Global Trust Store-pakket__ .
1. Selecteer het __tabblad Filters__ en voeg een filter toe voor het wortelpad `/etc/truststore`.
1. Selecteer __Klaar en vervolgens__ Opslaan ____.
1. Selecteer de __Bouw-knop__ voor het __Global Trust Store-pakket__ .
1. Na de bouw selecteer __je More__ > __Replicate__ om de Global Trust Store-node (`/etc/truststore`) naar AEM Publish te activeren.

## Maak authenticatie-service keystore aan{#authentication-service-keystore}

_Het aanmaken van een keystore voor authenticatie-service is vereist wanneer de OSGi-configuratieeigenschap [&#x200B; van de `handleLogout`SAML 2.0-authenticatiehandler is ingesteld of `true`](#saml-20-authenticationsaml-2-0-authentication) wanneer [AuthnRequest-ondertekening/SAML-assertie-versleuteling](#install-aem-public-private-key-pair) vereist is_

1. Log in op AEM Author als AEM-beheerder om de privésleutel te uploaden.
1. Navigeer naar __Tools > Security > Users__, selecteer __authenticatie-service__ user, en selecteer __Properties__ in de bovenste actiebalk.
1. Selecteer het tabblad Keystore ____.
1. Maak de keystore aan of open deze. Als je een keystore aanmaakt, bewaar het wachtwoord dan veilig.
   + Een [publieke/private keystore wordt in deze keystore](#install-aem-public-private-key-pair) alleen geïnstalleerd als AuthnRequest-ondertekening/SAML-assertie-encryptie vereist is.
   + Als deze SAML-integratie uitloggen ondersteunt, maar niet AuthnRequest-ondertekening/SAML-assertie, dan is een lege keystore voldoende.
1. Selecteer __Opslaan &amp; Sluiten__.
1. Maak een pakket aan met de bijgewerkte __authenticatie-servicegebruiker__ .

   _Use de volgende tijdelijke oplossing met behulp van pakketten :_

   1. Navigeer naar __tools > deployment > pakketten__.
   1. Maak een pakket aan
      + Pakketnaam: `Authentication Service`
      + Versie: `1.0.0`
      + Groep: `com.your.company`
   1. Bewerk het nieuwe __Authentication Service Key Store-pakket__ .
   1. Selecteer het __tabblad Filters__ en voeg een filter toe voor het wortelpad `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`.
      + Deze `<AUTHENTICATION SERVICE UUID>` zijn te vinden door te navigeren naar __Tools > Security > Users__ en authenticatie-service __user te selecteren__. De UUID is het laatste deel van de URL.
   1. Selecteer __Gedaan__ en dan __sparen__.
   1. Selecteer de __knoop van de Bouwstijl__ voor het __Belangrijkste pakket van de Opslag van de Dienst van de Authentificatie__.
   1. Zodra gebouwd, selecteer __Meer__ > __Repliceer__ om de belangrijkste opslag van de Dienst van de Authentificatie aan AEM te activeren publiceer.

## AEM-paar met openbare/persoonlijke sleutel installeren{#install-aem-public-private-key-pair}

_het installeren van AEM openbaar/privé zeer belangrijk paar is facultatief_

AEM Publish kan worden geconfigureerd om AuthnRequests (naar IDP) te ondertekenen en SAML-asserties te versleutelen (naar AEM). Dit wordt bereikt door een privésleutel aan AEM Publish te geven, die de publieke sleutel koppelt aan de IDP.

+++ Begrijp de ondertekeningsflow van AuthnRequest (optioneel)

Het AuthnRequest (het verzoek aan de IDP van AEM Publish dat het inlogproces start) kan door AEM Publish worden ondertekend. Om dit te doen ondertekent AEM Publish de AuthnRequest met de private key, waarna de IDP de handtekening valideert met de publieke sleutel. Dit garandeert aan de IDP dat AuthnRequest is gestart en opgevraagd door AEM Publish, en niet door een kwaadaardige derde partij.

![SAML 2.0 - SP AuthenRequest signering](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. De gebruiker doet een HTTP-verzoek aan AEM Publish dat resulteert in een SAML-authenticatieverzoek aan de IDP.
1. AEM Publish genereert het SAML-verzoek om naar de IDP te sturen.
1. AEM Publish ondertekent het SAML-verzoek met de privésleutel van AEM.
1. AEM Publish initieert de AuthnRequest, een HTTP-clientzijde redirect naar de IDP die het ondertekende SAML-verzoek bevat.
1. IDP ontvangt de AuthnRequest en valideert de handtekening met behulp van de publieke sleutel van AEM, waarmee wordt gegarandeerd dat AEM Publish de AuthnRequest heeft geïnitieerd.
1. AEM publiceert dan bevestigt de gedecrypteerde integriteit en de authenticiteit van SAML bewering gebruikend het IDP openbare certificaat.

+++

+++ Begrijp de de beweringsencryptiesstroom van SAML (facultatief)

Alle HTTP-communicatie tussen IDP en AEM Publiceren moet via HTTPS plaatsvinden en moet daarom standaard worden beveiligd. Nochtans, zoals vereist, kunnen de beweringen van SAML worden gecodeerd in het geval wordt de extra vertrouwelijkheid vereist bovenop die verstrekt door HTTPS. Om dit te doen, codeert IDP de gegevens van de Bevestiging van SAML gebruikend de privé sleutel, en AEM publiceert decrypteert de bewering van SAML gebruikend de privé sleutel.

![&#x200B; SAML 2.0 - de encryptie van de Assertion van SP SAML &#x200B;](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. Gebruiker verifieert aan IDP.
1. IDP genereert een SAML-assertie met de gegevens van de gebruiker en ondertekent deze met het privécertificaat van de IDP.
1. IDP versleutelt vervolgens de SAML-assertie met de publieke sleutel van AEM, die vereist dat de private sleutel van AEM ontsleuteld kan worden.
1. De versleutelde SAML-assertie wordt via de webbrowser van de gebruiker naar AEM Publish gestuurd.
1. AEM Publish ontvangt de SAML-assertie en ontsleutelt deze met behulp van de privésleutel van AEM.
1. IDP vraagt de gebruiker om zich te authenticeren.

+++

Zowel AuthnRequest-ondertekening als SAML-assertie-encryptie zijn optioneel, maar beide zijn ingeschakeld, met gebruik van de [SAML 2.0-authenticatiehandler OSGi-configuratieeigenschap, `useEncryption`](#saml-20-authenticationsaml-2-0-authentication)wat betekent dat beide of geen van beide gebruikt kunnen worden.

![AEM-authenticatie-service sleutelopslag](./assets/saml-2-0/authentication-service-key-store.png)

1. Verkrijg de openbare sleutel, de privé sleutel (PKCS#8 in formaat DER), en het dossier van de certificaatketting (dit kan de openbare sleutel zijn) die wordt gebruikt om AuthnRequest te ondertekenen, en de bewering van SAML te coderen. De sleutels worden doorgaans geleverd door het beveiligingsteam van de IT-organisatie.

   + Een zelf-ondertekend zeer belangrijk paar kan worden geproduceerd gebruikend __openssl__:

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. Upload de openbare sleutel aan IDP.
   + Met de `openssl` bovenstaande methode is de publieke sleutel het `aem-public.crt` bestand.
1. Log in op AEM Author als AEM-beheerder om de privésleutel te uploaden.
1. Ga naar __Tools > Security > Trust Store__, selecteer __authenticatie-service__ user, en selecteer __Properties__ in de bovenste actiebalk.
1. Navigeer naar __Tools > Security > Users__, selecteer __authenticatie-service__ user, en selecteer __Properties__ in de bovenste actiebalk.
1. Selecteer het tabblad Keystore ____.
1. Maak de keystore aan of open deze. Als je een keystore aanmaakt, bewaar het wachtwoord dan veilig.
1. Selecteer __Privésleutel toevoegen uit het DER-bestand__ en voeg het privésleutel- en ketenbestand toe aan AEM:
   + __Alias__: Geef een betekenisvolle naam, vaak de naam van de IDP.
   + __Privésleutelbestand__: Upload het privésleutelbestand (PKCS#8 in DER-formaat).
      + Met de `openssl` bovenstaande methode is dit het `aem-private-pkcs8.der` bestand
   + __Selecteer certificaat-ketenbestand__: Upload het bijbehorende ketenbestand (dit kan de publieke sleutel zijn).
      + Met de `openssl` bovenstaande methode is dit het `aem-public.crt` bestand
   + Selecteer __voorleggen__
1. Het onlangs toegevoegde certificaat verschijnt boven __voegt certificaat van CRT dossier__ sectie toe.
   + Maak nota van __alias__ aangezien dit in [&#x200B; SAML 2.0 de configuratie van de authentificatiemanager OSGi &#x200B;](#saml-20-authentication-handler-osgi-configuration) wordt gebruikt
1. Selecteer __sparen &amp; Sluiten__.
1. Maak een pakket aan met de bijgewerkte __authenticatie-servicegebruiker__ .

   _Use de volgende tijdelijke oplossing met behulp van pakketten :_

   1. Navigeer naar __tools > deployment > pakketten__.
   1. Maak een pakket aan
      + Pakketnaam: `Authentication Service`
      + Versie: `1.0.0`
      + Groep: `com.your.company`
   1. Bewerk het nieuwe __Authentication Service Key Store-pakket__ .
   1. Selecteer het __tabblad Filters__ en voeg een filter toe voor het wortelpad `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`.
      + Deze `<AUTHENTICATION SERVICE UUID>` zijn te vinden door te navigeren naar __Tools > Security > Users__ en authenticatie-service __user te selecteren__. De UUID is het laatste deel van de URL.
   1. Selecteer __Klaar en vervolgens__ Opslaan ____.
   1. Selecteer de __Build-knop__ voor het __Authentication Service Key Store-pakket__ .
   1. Eenmaal gebouwd, selecteer __Meer__ > __Replicate__ om de Authentication Service-sleutelopslag naar AEM Publish te activeren.

## SAML 2.0-verificatiehandler configureren{#configure-saml-2-0-authentication-handler}

De configuratie van SAML van AEM wordt uitgevoerd via __Adobe Granite SAML 2.0 de Handler van de Authentificatie__ OSGi configuratie.
De configuratie is een OSGi fabrieksconfiguratie, die één enkele dienst van de Publicatie van AEM as a Cloud Service betekent kan veelvoudige SAML configuratie hebben die discrete middelenbomen van de bewaarplaats behandelen; dit is nuttig voor multi-site plaatsingen van AEM.

+++ SAML 2.0 OSGi-configuratiegids van de Handler van de Authentificatie

### Adobe Granite SAML 2.0 Authentication Handler OSGi-configuratie{#configure-saml-2-0-authentication-handler-osgi-configuration}

|                                   | OSGi-eigenschap | Vereist | Waardeformaat | Standaardwaarde | Beschrijving |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| Paden | `path` | ✔ | Tekenreeksarray | `/` | AEM-paden waarvoor deze authenticatiehandler wordt gebruikt. |
| IDP-URL | `idpUrl` | ✔ | Snaar |                           | IDP URL het de authentificatieverzoek van SAML wordt verzonden. |
| IDP-certificaatalias | `idpCertAlias` | ✔ | String |                           | De alias van het IDP-certificaat in de AEM Global Trust Store |
| IDP HTTP-omleiding | `idpHttpRedirect` | ✘ | Booleans | `false` | Geeft aan of een HTTP-redirect naar de IDP-URL wordt gestuurd in plaats van een AuthnRequest te sturen. Ingesteld op `true` voor IDP-geïnitieerde authenticatie. |
| IDP-identificatie | `idpIdentifier` | ✘ | Snaar |                           | Unieke IDP-ID om de uniciteit van AEM-gebruikers en groepen te waarborgen. Als het leeg is, wordt in plaats daarvan de `serviceProviderEntityId` gebruikt. |
| Bevestigingsservice-URL | `assertionConsumerServiceURL` | ✘ | Snaar |                           | Het URL-kenmerk `AssertionConsumerServiceURL` in de AuthnRequest die opgeeft waar het `<Response>` -bericht naar AEM moet worden verzonden. |
| SP-entiteit-id | `serviceProviderEntityId` | ✔ | String |                           | Identificeert AEM uniek aan de IDP; meestal de hostnaam van AEM. |
| SP-encryptie | `useEncryption` | ✘ | Booleans | `true` | Geeft aan of de IDP SAML-asserties versleutelt. Vereist `spPrivateKeyAlias` en `keyStorePassword` moet worden ingesteld. |
| SP alias van persoonlijke sleutel | `spPrivateKeyAlias` | ✘ | Snaar |                           | De alias van de privésleutel in de sleutelopslag van de `authentication-service` gebruiker. Vereist als `useEncryption` is gezet op `true`. |
| SP sleutelopslagwachtwoord | `keyStorePassword` | ✘ | Snaar |                           | Het wachtwoord van de &#39;authenticatie-service&#39; gebruikerssleutels slaat op. Vereist als `useEncryption` is gezet op `true`. |
| Standaard omleiding | `defaultRedirectUrl` | ✘ | String | `/` | De standaard omleidings-URL na geslaagde verificatie. Kan relatief zijn ten opzichte van de AEM-host (bijvoorbeeld `/content/wknd/us/en/html` ). |
| Gebruiker ID-attribuut | `userIDAttribute` | ✘ | Snaar | `uid` | De naam van het SAML-assertieattribuut dat de gebruikers-ID van de AEM-gebruiker bevat. Laat leeg om de `Subject:NameId`. |
| Automatisch AEM-gebruikers aanmaken | `createUser` | ✘ | Booleans | `true` | Geeft aan of AEM-gebruikers worden aangemaakt bij succesvolle authenticatie. |
| Tussentijdse pad voor AEM-gebruiker | `userIntermediatePath` | ✘ | String |                           | Bij het aanmaken van AEM-gebruikers wordt deze waarde gebruikt als het tussenliggende pad (bijvoorbeeld `/home/users/<userIntermediatePath>/jane@wknd.com`). Moet `createUser` worden ingesteld op `true`. |
| AEM-gebruikersattributen | `synchronizeAttributes` | ✘ | Tekenreeksarray |                           | Lijst van SAML-attribuutmappings om op de AEM-gebruiker op te slaan, in het formaat `[ "saml-attribute-name=path/relative/to/user/node" ]` (bijvoorbeeld `[ "firstName=profile/givenName" ]`). Bekijk de [volledige lijst van native AEM-attributen](#aem-user-attributes). |
| Gebruiker toevoegen aan AEM-groepen | `addGroupMemberships` | ✘ | Boolean | `true` | Geeft aan of een AEM-gebruiker automatisch wordt toegevoegd aan AEM-gebruikersgroepen nadat de verificatie is voltooid. |
| AEM-kenmerk voor groepslidmaatschap | `groupMembershipAttribute` | ✘ | String | `groupMembership` | De naam van het SAML-assertie-attribuut bevat een lijst van AEM-gebruikersgroepen waaraan de gebruiker moet worden toegevoegd. Moet `addGroupMemberships` worden ingesteld op `true`. |
| Standaard AEM-groepen | `defaultGroups` | ✘ | Tekenreeksarray |                           | Een lijst van AEM-gebruikersgroepen waaraan geauthenticeerde gebruikers altijd worden toegevoegd (bijvoorbeeld, `[ "wknd-user" ]`). Moet `addGroupMemberships` worden ingesteld op `true`. |
| NameIDPolicy-formaat | `nameIdFormat` | ✘ | Snaar | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | De waarde van de NameIDPolicy-formaatparameter die in het AuthnRequest-bericht moet worden verzonden. |
| Store SAML-reactie | `storeSAMLResponse` | ✘ | Booleans | `false` | Geeft aan of de `samlResponse` waarde op de AEM-node `cq:User` is opgeslagen. |
| Afmelden handgreep | `handleLogout` | ✘ | Boolean | `false` | Wijst erop als het logout verzoek door deze de authentificatiemanager van SAML wordt behandeld. `logoutUrl` moet worden ingesteld. |
| Afmeldings-URL | `logoutUrl` | ✘ | String |                           | De URL van de IDP waar het SAML-uitlogverzoek naartoe wordt gestuurd. Vereist als `handleLogout` is gezet op `true`. |
| Kloktolerantie | `clockTolerance` | ✘ | Geheel | `60` | Bij het valideren van SAML-beweringen tekent IDP en AEM (SP) de tolerantie voor klokschuintrekken. |
| Digestmethode | `digestMethod` | ✘ | Snaar | `http://www.w3.org/2001/04/xmlenc#sha256` | Het digest-algoritme dat de IDP gebruikt bij het ondertekenen van een SAML-bericht. |
| Handtekeningmethode | `signatureMethod` | ✘ | String | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | Het handtekeningalgoritme dat IDP gebruikt bij het ondertekenen van een SAML-bericht. |
| Type identiteitssynchronisatie | `identitySyncType` | ✘ | `default` of `idp` | `default` | Verander `from` de standaard niet voor AEM als Cloud Service. |
| Dienstranglijst | `service.ranking` | ✘ | Geheel getal | `5002` | Hogere rangconfiguraties worden voor dezelfde `path`voorkeur gegeven. |

### AEM-gebruikersattributen{#aem-user-attributes}

AEM gebruikt de volgende gebruikersattributen, die kunnen worden ingevuld via de `synchronizeAttributes` eigenschap in de Adobe Granite SAML 2.0 Authentication Handler OSGi-configuratie.  Elke IDP-attributen kan worden gesynchroniseerd met elke AEM-gebruikerseigenschap, maar door het toewijzen van AEM-gebruiksattributen (hieronder vermeld) kan AEM ze natuurlijk gebruiken.

| Gebruikersattribuut | Relatieve eigenschapspad van `rep:User` knoop |
|--------------------------------|--------------------------|
| Titel (bijvoorbeeld, `Mrs`) | `profile/title` |
| Voornaam (bijv. voornaam) | `profile/givenName` |
| Familienaam (bijv. achternaam) | `profile/familyName` |
| Functie | `profile/jobTitle` |
| E-mailadres | `profile/email` |
| Straatadres | `profile/street` |
| Stad | `profile/city` |
| Postcode | `profile/postalCode` |
| Land | `profile/country` |
| Telefoonnummer | `profile/phoneNumber` |
| Over mij | `profile/aboutMe` |

+++

1. Creeer een OSGi configuratiedossier in uw project bij `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` en open in uw winde.
   + Verander `/wknd-examples/` naar je `/<project name>/`
   + De identificatie achter de `~` in de bestandsnaam moet deze configuratie uniek identificeren, dus het kan de naam van de IDP zijn, zoals `...~okta.cfg.json`. De waarde moet alfanumeriek zijn met koppeltekens.
1. Plak de volgende JSON in het `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` bestand en werk de referenties `wknd` bij waar nodig.

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

1. Werk de waarden bij zoals vereist door je project. Zie hierboven de __SAML 2.0 Authentication Handler OSGi configuratiewoordenlijst__ voor beschrijvingen van configuratie-eigenschappen. De `path` moeten de contentbomen bevatten die beschermd worden door gesloten gebruikersgroepen (CUG&#39;s) en authenticatie vereisen, en deze authenticatiehandler zou verantwoordelijk moeten zijn voor de beveiliging.
1. Het wordt aanbevolen, maar niet verplicht, om OSGi-omgevingsvariabelen en geheimen te gebruiken wanneer waarden niet synchroon lopen met de releasecyclus, of wanneer de waarden verschillen tussen vergelijkbare omgevingstypen/servicelagen. U kunt standaardwaarden instellen met de syntaxis `$[env:..;default=the-default-value]"` , zoals hierboven wordt weergegeven.

OSGi-configuraties per omgeving (`config.publish.dev`, `config.publish.stage` en `config.publish.prod` ) kunnen met specifieke kenmerken worden gedefinieerd als de SAML-configuratie per omgeving verschilt.

### Codering gebruiken

Wanneer [&#x200B; het coderen van de bevestiging AuthnRequest en SAML &#x200B;](#encrypting-the-authnrequest-and-saml-assertion), worden de volgende eigenschappen vereist: `useEncryption`, `spPrivateKeyAlias`, en `keyStorePassword`. `keyStorePassword` bevat een wachtwoord daarom moet de waarde niet in het OSGi configuratiedossier worden opgeslagen, maar eerder ingespoten gebruikend [&#x200B; geheime configuratiewaarden &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=nl-NL#secret-configuration-values)

+++Optioneel kunt u de OSGi-configuratie bijwerken om encryptie te gebruiken

1. Open `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` in je IDE.
1. Voeg de drie eigenschappen `useEncryption`, `spPrivateKeyAlias`, en `keyStorePassword` zoals hieronder getoond, op.

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

1. De drie OSGi-configuratie-eigenschappen die vereist zijn voor encryptie zijn:

+ `useEncryption` Stel in op `true`
+ `spPrivateKeyAlias` bevat de keystore-invoeralias voor de privésleutel die door de SAML-integratie wordt gebruikt.
+ `keyStorePassword` bevat een [geheime OSGi-configuratievariabele](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=nl-NL#secret-configuration-values) met het wachtwoord van de `authentication-service` user keystore.

+++

## Referentiefilter configureren

Tijdens het SAML-authenticatieproces start de IDP een client-side HTTP POST naar het `.../saml_login` eindpunt van AEM Publish. Als de IDP en AEM Publish op verschillende oorsprong bestaan, wordt het __Referrer Filter__ van AEM Publish via OSGi-configuratie geconfigureerd om HTTP POSTs vanaf de oorsprong van de IDP toe te staan.

1. Maak (of bewerk) een OSGi-configuratiebestand in je project op .`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json`
   + Verander `/wknd-examples/` naar je `/<project name>/`
1. Zorg ervoor dat de `allow.empty` waarde is ingesteld op `true`, de `allow.hosts` (of als je dat liever hebt) `allow.hosts.regexp`bevat de oorsprong van de IDP, en `filter.methods` omvat `POST`. De OSGi-configuratie moet vergelijkbaar zijn met:

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

AEM Publish ondersteunt één enkele Referrer-filterconfiguratie, dus voeg de SAML-configuratievereisten samen met bestaande configuraties.

OSGi-configuraties per omgeving (`config.publish.dev`, `config.publish.stage`, en `config.publish.prod`) kunnen worden gedefinieerd met specifieke attributen als de `allow.hosts` (of `allow.hosts.regex`) variëren tussen omgevingen.

## Configureer Cross-Origin Resource Sharing (CORS)

Tijdens het SAML-authenticatieproces start de IDP een client-side HTTP POST naar het `.../saml_login` eindpunt van AEM Publish. Als de IDP en AEM Publish op verschillende hosts/domeinen bestaan, moet de CRoss-Origin Resource Sharing (CORS)__van AEM Publish__ worden geconfigureerd om HTTP POSTs toe te staan vanaf de host/domein van de IDP.

De header van `Origin` dit HTTP POST-verzoek heeft meestal een andere waarde dan die van de AEM Publish-host, waardoor CORS-configuratie vereist is.

Bij het testen van SAML-verificatie op de lokale AEM SDK (`localhost:4503`), kan de IDP de `Origin` header instellen op `null` . Als dat het geval is, voegt u `"null"` toe aan de lijst `alloworigin` .

1. Creeer een OSGi configuratiedossier in uw project bij `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`
   + Wijzig `/wknd-examples/` in uw projectnaam
   + De id na de naam `~` in de bestandsnaam moet deze configuratie op unieke wijze identificeren, zodat dit de naam van de id kan zijn, zoals `...CORSPolicyImpl~okta.cfg.json` . De waarde moet alfanumeriek zijn met koppeltekens.
1. Plak de volgende JSON in het `com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json` bestand.

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
1. Voeg onderaan het bestand een toelaatregel toe voor HTTP POSTs aan URL&#39;s die eindigen op `/saml_login`.

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

>[!NOTE]
>Bij het uitrollen van meerdere SAML-configuraties in AEM voor verschillende beschermde paden en verschillende IDP-eindpunten, zorg er dan voor dat de IDP post naar het RESPECTIVE_PROTECTED_PATH/saml_login-eindpunt om de juiste SAML-configuratie aan de AEM-zijde te selecteren. Als er dubbele SAML-configuraties zijn voor hetzelfde beschermde pad, zal de selectie van de SAML-configuratie willekeurig plaatsvinden.

Als het herschrijven van de URL op de Apache-webserver is geconfigureerd (`dispatcher/src/conf.d/rewrites/rewrite.rules`), zorg er dan voor dat verzoeken naar de `.../saml_login` eindpunten niet per ongeluk worden verstoord.

## Dynamisch Groepslidmaatschap

Dynamisch groepslidmaatschap is een functie in [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html) die de prestaties van groepsevaluatie en provisioning verhoogt. Deze sectie beschrijft hoe gebruikers en groepen worden opgeslagen wanneer deze functie wordt ingeschakeld en hoe de configuratie van de SAML Authentication Handler kan worden aangepast om deze te activeren voor nieuwe of bestaande omgevingen.

### Hoe Dynamic Group Membership voor SAML-gebruikers in nieuwe omgevingen mogelijk maakt

Om de prestaties van groepsevaluatie in nieuwe AEM as Cloud Service-omgevingen aanzienlijk te verbeteren, wordt de activatie van de functie Dynamic Group Membership aanbevolen in nieuwe omgevingen.
Dit is ook een noodzakelijke stap wanneer datasynchronisatie wordt geactiveerd. Meer details [hier](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier) .
Om dit te doen, voeg je de volgende eigenschap toe aan het OSGI-configuratiebestand:

`/apps/example/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~example.cfg.json`

Met deze configuratie, worden de gebruikers en de groepen gecreeerd als [&#x200B; Externe Gebruikers van Oak &#x200B;](https://jackrabbit.apache.org/oak/docs/security/authentication/identitymanagement.html). In AEM hebben externe gebruikers en groepen de standaardwaarde `rep:principalName` , die wordt samengesteld door `[user name];[idp]` of `[group name];[idp]` .
Merk op dat de Lijsten van het Toegangsbeheer (ACL) met PrincipalName van gebruikers of groepen worden geassocieerd.
Wanneer het opstellen van deze configuratie in een bestaande plaatsing waar eerder `identitySyncType` niet werd gespecificeerd of aan `default` werd geplaatst, zullen de nieuwe gebruikers en de groepen worden gecreeerd en ACL moet op deze nieuwe gebruikers en groepen worden toegepast. Externe groepen kunnen geen lokale gebruikers bevatten. [&#x200B; opnieuw richt &#x200B;](https://sling.apache.org/documentation/bundles/repository-initialization.html) kan worden gebruikt om ACL voor Externe groepen van SAML tot stand te brengen, zelfs als zij slechts zullen worden gecreeerd wanneer de gebruiker login zal uitvoeren.
Om dit refactoring op ACL te vermijden, is een standaard [&#x200B; migratieeigenschap &#x200B;](#automatic-migration-to-dynamic-group-membership-for-existing-environments) uitgevoerd.

### Hoe lidmaatschappen worden opgeslagen in lokale en externe groepen met dynamisch groepslidmaatschap

Bij lokale groepen worden de groepsleden opgeslagen in het eikenattribuut: `rep:members`. Het attribuut bevat de lijst van uid van elk lid van de groep. Meer details zijn hier[&#x200B; te vinden](https://jackrabbit.apache.org/oak/docs/security/user/membership.html#member-representation-in-the-repository).
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

Externe groepen met dynamisch groepslidmaatschap slaan geen lid op in de groepsentry.
Het groepslidmaatschap wordt in plaats daarvan opgeslagen in de gebruikersvermeldingen. Aanvullende documentatie is hier[&#x200B; te vinden](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html). Dit is bijvoorbeeld de OAK-knoop voor de groep:

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

### Hoe Dynamisch Groepslidmaatschap voor SAML-gebruikers in bestaande omgevingen in te schakelen

Zoals uitgelegd in de vorige sectie, is het formaat van externe gebruikers en groepen iets anders dan dat van lokale gebruikers en groepen. Het is mogelijk om een nieuwe ACL te definiëren voor externe groepen en nieuwe externe gebruikers te provisioneren, of de migratietool te gebruiken zoals hieronder beschreven.

#### Dynamisch groepslidmaatschap mogelijk maken voor bestaande omgevingen met externe gebruikers

De SAML-authenticatiehandler creëert externe gebruikers wanneer de volgende eigenschap wordt gespecificeerd: `"identitySyncType": "idp"`. In dit geval kan dynamisch groepslidmaatschap worden ingeschakeld door deze eigenschap aan te passen tot: `"identitySyncType": "idp_dynamic"`. Migratie is niet nodig.

#### Automatische migratie naar dynamisch groepslidmaatschap voor bestaande omgevingen met lokale gebruikers

De SAML-verificatiehandler maakt lokale gebruikers wanneer de volgende eigenschap wordt opgegeven: `"identitySyncType": "default"` . Dit is ook de standaardwaarde wanneer de eigenschap niet wordt opgegeven. In deze sectie beschrijven we de stappen die worden uitgevoerd door de automatische migratieprocedure.

Wanneer deze migratie is ingeschakeld, wordt deze uitgevoerd tijdens gebruikersverificatie en bestaat deze uit de volgende stappen:
1. De lokale gebruiker wordt gemigrerd naar een externe gebruiker terwijl de oorspronkelijke gebruikersnaam behouden blijft. Dit impliceert dat gemigrerde lokale gebruikers, die nu als externe gebruikers optreden, hun oorspronkelijke gebruikersnaam behouden in plaats van de naamgevingssyntaxis te volgen die in de vorige sectie werd genoemd. Er wordt een extra eigenschap toegevoegd met de naam: `rep:externalId` met de waarde van `[user name];[idp]`. De gebruiker `PrincipalName` wordt niet gewijzigd.
2. Voor elke externe groep die in de SAML-assertie wordt ontvangen, wordt een externe groep aangemaakt. Als er een overeenkomstige lokale groep bestaat, wordt de externe groep als lid toegevoegd aan de lokale groep.
3. De gebruiker wordt toegevoegd als lid van de externe groep.
4. De lokale gebruiker wordt vervolgens verwijderd uit alle Saml-lokale groepen waarvan hij lid was. Saml-lokale groepen worden geïdentificeerd door de OAK-eigenschap: `rep:managedByIdp`. Deze eigenschap wordt ingesteld door de Saml Authenticatiehandler wanneer het attribuut `syncType` niet is gespecificeerd of op `default`.

Als bijvoorbeeld vóór de migratie `user1` een lokale gebruiker en lid van een lokale groep `group1`is, zullen na de migratie de volgende wijzigingen plaatsvinden:
`user1` wordt een externe gebruiker. Het attribuut `rep:externalId` wordt aan zijn profiel toegevoegd.
`user1` wordt lid van de externe groep: `group1;idp`
`user1` is geen direct lid meer van de lokale groep: `group1`
`group1;idp` is lid van de lokale groep: `group1`.
`user1` dan lid is van de lokale groep: `group1` hoewel erfelijkheid

Het groepslidmaatschap voor externe groepen wordt opgeslagen in het gebruikersprofiel in de eigenschap `rep:externalPrincipalNames`

### Hoe automatische migratie naar dynamisch groepslidmaatschap te configureren,

1. Schakel de eigenschap `"identitySyncType": "idp_dynamic_simplified_id"` in in het SAML OSGi-configuratiebestand in: `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json`
2. Configureer de nieuwe OSGi-service met fabrieks-PID beginnend met: `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~`. Een PID kan bijvoorbeeld zijn: `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~myIdP`. Stel de volgende eigenschap in:

```
{
  "idpIdentifier": "<value of IDP Identifier (idpIdentifier)" property from the "com.adobe.granite.auth.saml.SamlAuthenticationHandler" configuration to be migrated>"
}
```

Om meerdere SAML-configuraties te migreren, moeten meerdere OSGi-fabrieksconfiguraties `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration` worden aangemaakt, waarbij elk een migratie specificeert `idpIdentifier` .

## Implementatie van SAML-configuratie

De OSGi-configuraties moeten worden toegewijd aan Git en worden uitgezonden naar AEM als een Cloud Service met behulp van Cloud Manager.

```
$ git remote -v            
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (fetch)
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (push)
$ git add .
$ git commit -m "SAML 2.0 configurations"
$ git push adobe saml-auth:develop
```

Deploy de doel-Cloud Manager Git-branch (in dit voorbeeld, `develop`), met behulp van een Full Stack deployment pipeline.

## Het aanroepen van de SAML-authenticatie

De SAML authentificatiestroom kan van een Web-pagina van de Plaats van AEM worden aangehaald, door een speciaal gemaakte verbindingen, of een knopen te creëren. De hieronder beschreven parameters kunnen indien nodig via programmacode worden ingesteld, dus een aanmeldingsknop kan `saml_request_path` bijvoorbeeld instellen op verschillende AEM-pagina&#39;s, op basis van de context van de knop. Dit is de plaats waar de gebruiker naar SAML-verificatie is gegaan.

## Beveiligd in cache plaatsen tijdens gebruik van SAML

Op de AEM-publicatie-instantie worden de meeste pagina&#39;s doorgaans in cache geplaatst. Nochtans, voor SAML-Beschermde wegen, zou caching of moeten worden onbruikbaar gemaakt of beveiligd caching toegelaten gebruikend de configuratie auth_checker. Voor meer informatie kunt u de hier verstrekte [details raadplegen](https://experienceleague.adobe.com/nl/docs/experience-manager-dispatcher/using/configuring/permissions-cache)

Wees je ervan bewust dat als je beschermde paden cachet zonder de auth_checker in te schakelen, je onvoorspelbaar gedrag kunt ervaren.

### GET-verzoek

SAML-verificatie kan worden aangeroepen door een HTTP GET-aanvraag in de volgende indeling te maken:

`HTTP GET /system/sling/login`

en het verstrekken van vraagparameters:

| Naam van query-parameter | Parameterwaarde voor query |
|----------------------|-----------------------|
| `resource` | Elk JCR-pad, of subpad, dat de SAML-authenticatiehandler is, luistert naar, zoals gedefinieerd in de[&#x200B; &#x200B;](#configure-saml-2-0-authentication-handler) eigenschap van de `path`Adobe Granite SAML 2.0 Authentication Handler OSGi-configuratie. |
| `saml_request_path` | Het URL-pad waar de gebruiker naartoe moet worden geleid na succesvolle SAML-authenticatie. |

Deze HTML-link activeert bijvoorbeeld de SAML-inlogflow, en na succes brengt de gebruiker naar `/content/wknd/us/en/protected/page.html`. Deze queryparameters kunnen programmatisch worden ingesteld indien nodig.

```html
<a href="/system/sling/login?resource=/content/wknd&saml_request_path=/content/wknd/us/en/protected/page.html">
    Log in using SAML
</a>
```

## POST-verzoek

SAML-authenticatie kan worden opgeroepen door een HTTP POST-verzoek te maken in het formaat:

`HTTP POST /system/sling/login`

en het aanleveren van de formuliergegevens:

| Formuliergegevensnaam | Formuliergegevenswaarde |
|----------------------|-----------------------|
| `resource` | Elk JCR-pad, of subpad, dat de SAML-authenticatiehandler is, luistert naar, zoals gedefinieerd in de[&#x200B; &#x200B;](#configure-saml-2-0-authentication-handler) eigenschap van de `path`Adobe Granite SAML 2.0 Authentication Handler OSGi-configuratie. |
| `saml_request_path` | Het URL-pad waar de gebruiker naartoe moet worden geleid na succesvolle SAML-authenticatie. |


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

Laat de benodigde URL-patronen toe op basis van of GET of POST is gebruikt

```
# Allow GET-based SAML authentication invocation
/0191 { /type "allow" /method "GET" /url "/system/sling/login" /query "*" }

# Allow POST-based SAML authentication invocation
/0192 { /type "allow" /method "POST" /url "/system/sling/login" }
```
