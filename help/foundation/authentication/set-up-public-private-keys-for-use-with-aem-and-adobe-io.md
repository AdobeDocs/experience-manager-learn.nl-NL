---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: 'AEM gebruikt paren met openbare/persoonlijke sleutels om veilig te communiceren met Adobe I/O en andere webservices. Deze korte zelfstudie laat zien hoe compatibele toetsen en sleutelarchieven kunnen worden gegenereerd met het openssl-opdrachtregelprogramma dat zowel met AEM als met Adobe I/O werkt. '
version: 6.4, 6.5
feature: authentication
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
translation-type: tm+mt
source-git-commit: 3f973e36531a2d04cbaf6bb8dd70b39fef7d8b2f
workflow-type: tm+mt
source-wordcount: '722'
ht-degree: 0%

---


# Openbare en persoonlijke sleutels instellen voor gebruik met Adobe I/O

AEM gebruikt paren met openbare/persoonlijke sleutels om veilig te communiceren met Adobe I/O en andere webservices. Deze korte zelfstudie laat zien hoe compatibele toetsen en sleutelarchieven kunnen worden gegenereerd met het opdrachtregelprogramma [!DNL openssl] dat zowel met AEM als met Adobe I/O werkt.

>[!CAUTION]
>
>Deze handleiding maakt zelfondertekende toetsen die handig zijn voor ontwikkeling en gebruik in lagere omgevingen. In productiescenario&#39;s, worden de sleutels typisch geproduceerd en beheerd door het de veiligheidsteam van IT van een organisatie.

## Het openbare/persoonlijke sleutelpaar {#generate-the-public-private-key-pair} genereren

Met de [[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) opdrachtregelprogramma&#39;s [[!DNL req] command](https://www.openssl.org/docs/man1.0.2/man1/req.html) kunt u een sleutelpaar genereren dat compatibel is met Adobe I/O en Adobe Experience Manager.

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

Als u de opdracht [!DNL openssl generate] wilt voltooien, geeft u de certificaatgegevens op wanneer u hierom wordt gevraagd. Adobe I/O en AEM geven niet om wat deze waarden zijn, maar ze moeten wel worden uitgelijnd en uw sleutel beschrijven.

```
Generating a 2048 bit RSA private key
...........................................................+++
...+++
writing new private key to 'private.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) []:US
State or Province Name (full name) []:CA
Locality Name (eg, city) []:San Jose
Organization Name (eg, company) []:Example Co
Organizational Unit Name (eg, section) []:Digital Marketing
Common Name (eg, fully qualified host name) []:com.example
Email Address []:me@example.com
```

## Sleutelpaar toevoegen aan een nieuw sleutelarchief {#add-key-pair-to-a-new-keystore}

De zeer belangrijke paren kunnen aan nieuw [!DNL PKCS12] keystore worden toegevoegd. Als deel van [[!DNL openssl]'s [!DNL pcks12] bevel, ](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) worden de naam van keystore (via `-  caname`), de naam van de sleutel (via `-name`) en het keystore wachtwoord (via `-  passout`) bepaald.

Deze waarden zijn vereist om het sleutelarchief en de sleutels in AEM te laden.

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

De uitvoer van deze opdracht is een `keystore.p12`-bestand.

>[!NOTE]
>
>De parameterwaarden **[!DNL my-keystore]**, **[!DNL my-key]** en **[!DNL my-password]** moeten door uw eigen waarden worden vervangen.

## Verifieer de keystore inhoud {#verify-the-keystore-contents}

Het Java [[!DNL keytool] opdrachtregelprogramma](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) biedt zichtbaarheid in een sleutelarchief om ervoor te zorgen dat de sleutels correct worden geladen in het sleutelarchiefbestand ([!DNL keystore.p12]).

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![sleutelarchief in Adobe I/O verifiëren](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## Het sleutelarchief toevoegen aan AEM {#adding-the-keystore-to-aem}

AEM gebruikt de gegenereerde **persoonlijke sleutel** om veilig te communiceren met Adobe I/O en andere webservices. De persoonlijke sleutel is alleen toegankelijk voor AEM als deze is geïnstalleerd in het sleutelarchief van een AEM gebruiker.

Navigeer naar **AEM > [!UICONTROL Tools] > [!UICONTROL Security] >[!UICONTROL Users]** en **bewerk de gebruiker** waaraan de persoonlijke sleutel moet worden gekoppeld.

### Een AEM sleutelarchief maken {#create-an-aem-keystore}

![KeyStore maken in ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*AEMAEM >  [!UICONTROL Tools] >  [!UICONTROL Security] >  [!UICONTROL Users] > Gebruiker bewerken*

Als u wordt gevraagd een sleutelarchief te maken, doet u dat. Dit sleutelarchief bestaat alleen in AEM en is NIET het sleutelarchief dat via openssl is gemaakt. Het wachtwoord kan om het even wat zijn en moet niet het zelfde zijn als het wachtwoord dat in [!DNL openssl] bevel wordt gebruikt.

### De persoonlijke sleutel installeren via het sleutelarchief {#install-the-private-key-via-the-keystore}

![Persoonlijke sleutel toevoegen in AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL User]>  [!UICONTROL Keystore] >[!UICONTROL Add private key from keystore]*

Klik in de sleutelarchiefconsole van de gebruiker op **[!UICONTROL Add Private Key form KeyStore file]** en voeg de volgende informatie toe:

* **[!UICONTROL New Alias]**: de alias van de sleutel in AEM. Dit kan om het even wat zijn en moet niet met de naam van keystore beantwoorden die met het open bevel wordt gecreeerd.
* **[!UICONTROL KeyStore File]**: de uitvoer van de opdracht openssl pkcs12 (keystore.p12)
* **[!UICONTROL KeyStore File Password]**: Het wachtwoord dat via  `-passout` argument in de opdracht openssl pkcs12 is ingesteld.
* **[!UICONTROL Private Key Alias]**: De waarde die aan het  `-name` argument wordt verstrekt in de opdracht openssl pkcs12 hierboven (d.w.z.  `my-key`).
* **[!UICONTROL Private Key Password]**: Het wachtwoord dat via  `-passout` argument in de opdracht openssl pkcs12 is ingesteld.

>[!CAUTION]
>
>Het wachtwoord voor sleutelarchief-bestanden en het wachtwoord voor persoonlijke sleutels zijn voor beide invoer hetzelfde. Als u een niet-overeenkomend wachtwoord invoert, wordt de sleutel niet geïmporteerd.

### Controleer of de persoonlijke sleutel is geladen in het AEM sleutelarchief {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![Persoonlijke sleutel verifiëren in AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL User]>[!UICONTROL Keystore]*

Wanneer de persoonlijke sleutel van het verstrekte sleutelarchief in AEM sleutelarchief met succes wordt geladen, worden de meta-gegevens van de privé sleutel getoond in de keystore console van de gebruiker.

## De openbare sleutel toevoegen aan Adobe I/O {#adding-the-public-key-to-adobe-i-o}

De overeenkomende openbare sleutel moet naar Adobe I/O worden geüpload om de gebruiker van de AEM service te kunnen toestaan, die de overeenkomende persoonlijke sleutel van de openbare sleutel heeft om veilig te kunnen communiceren.

### Een nieuwe Adobe I/O-integratie maken {#create-a-adobe-i-o-new-integration}

![Nieuwe Adobe I/O-integratie maken](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Create Adobe I/O Integration]](https://console.adobe.io/) >[!UICONTROL New Integration]*

Voor een nieuwe integratie in Adobe I/O moet u een openbaar certificaat uploaden. Upload **certificate.crt** die door het `openssl req` bevel wordt geproduceerd.

### Controleren of de openbare sleutels zijn geladen in Adobe I/O {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![Openbare sleutels in Adobe I/O verifiëren](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

De geïnstalleerde openbare sleutels en hun vervaldatums zijn vermeld in [!UICONTROL Integrations] console op Adobe I/O. U kunt meerdere openbare sleutels toevoegen via de knop **[!UICONTROL Add a public key]**.

AEM houden nu de persoonlijke sleutel en de integratie van Adobe I/O houdt de overeenkomstige openbare sleutel, die AEM toestaat om veilig met Adobe I/O te communiceren.
