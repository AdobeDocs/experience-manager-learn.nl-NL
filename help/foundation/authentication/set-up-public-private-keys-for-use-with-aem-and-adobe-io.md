---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: AEM gebruikt openbare/privé zeer belangrijke paren om veilig met Adobe I/O en andere Webdiensten te communiceren. Deze korte zelfstudie laat zien hoe compatibele toetsen en sleutelarchieven kunnen worden gegenereerd met het openssl-opdrachtregelprogramma dat zowel met AEM als met Adobe I/O werkt.
version: 6.4, 6.5
feature: User and Groups
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
topic: Development
role: Developer
level: Experienced
exl-id: 62ed9dcc-6b8a-48ff-8efe-57dabdf4aa66
last-substantial-update: 2022-07-17T00:00:00Z
thumbnail: KT-2450.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# Openbare en persoonlijke sleutels instellen voor gebruik met Adobe I/O

AEM gebruikt openbare/privé zeer belangrijke paren om veilig met Adobe I/O en andere Webdiensten te communiceren. Deze korte zelfstudie laat zien hoe compatibele sleutels en sleutelarchieven kunnen worden gegenereerd met de [!DNL openssl] opdrachtregelprogramma dat zowel met AEM als met Adobe I/O werkt.

>[!CAUTION]
>
>Deze handleiding maakt zelfondertekende toetsen die handig zijn voor ontwikkeling en gebruik in lagere omgevingen. In productiescenario&#39;s, worden de sleutels typisch geproduceerd en beheerd door het de veiligheidsteam van IT van een organisatie.

## Het paar openbare/persoonlijke sleutels genereren {#generate-the-public-private-key-pair}

De [[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) opdrachtregelprogramma&#39;s [[!DNL req] command](https://www.openssl.org/docs/man1.0.2/man1/req.html) kan worden gebruikt om een sleutelpaar te produceren compatibel met Adobe I/O en Adobe Experience Manager.

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

Als u het dialoogvenster [!DNL openssl generate] geeft u de certificaatgegevens op wanneer u hierom wordt gevraagd. Adobe I/O en AEM geven niet om wat deze waarden zijn, maar ze moeten wel worden uitgelijnd en uw sleutel beschrijven.

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

U kunt sleutelparen toevoegen aan een nieuwe [!DNL PKCS12] sleutelarchief. Als onderdeel van [[!DNL openssl]'s [!DNL pcks12] opdracht,](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) de naam van het sleutelarchief (via `-  caname`), de naam van de sleutel (via `-name`) en het wachtwoord van het sleutelarchief (via `-  passout`) zijn gedefinieerd.

Deze waarden zijn vereist om het sleutelarchief en de sleutels in AEM te laden.

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

De uitvoer van deze opdracht is een `keystore.p12` bestand.

>[!NOTE]
>
>De parameterwaarden van **[!DNL my-keystore]**, **[!DNL my-key]** en **[!DNL my-password]** worden vervangen door uw eigen waarden.

## De inhoud van het sleutelarchief controleren {#verify-the-keystore-contents}

Java™ [[!DNL keytool] opdrachtregelprogramma](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) biedt zichtbaarheid in een sleutelarchief om ervoor te zorgen dat de sleutels correct in het sleutelarchiefbestand worden geladen ([!DNL keystore.p12]).

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

AEM gebruikt de gegenereerde **persoonlijke sleutel** om veilig met Adobe I/O en andere Webdiensten te communiceren. De persoonlijke sleutel is alleen toegankelijk voor AEM als deze is geïnstalleerd in het sleutelarchief van een AEM gebruiker.

Navigeren naar **AEM > [!UICONTROL Tools] > [!UICONTROL Security] >[!UICONTROL Users]** en **de gebruiker bewerken** de persoonlijke sleutel moet worden gekoppeld.

### Een AEM sleutelarchief maken {#create-an-aem-keystore}

![KeyStore maken in AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*AEM > [!UICONTROL Tools] > [!UICONTROL Security] > [!UICONTROL Users] > Gebruiker bewerken*

Doe dit wanneer u wordt gevraagd een sleutelarchief te maken. Dit sleutelarchief bestaat alleen in AEM en is NIET het sleutelarchief dat via openssl is gemaakt. Het wachtwoord kan om het even wat zijn en moet niet het zelfde zijn als het wachtwoord in [!DNL openssl] gebruiken.

### De persoonlijke sleutel installeren via het sleutelarchief {#install-the-private-key-via-the-keystore}

![Persoonlijke sleutel toevoegen in AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL User]> [!UICONTROL Keystore] >[!UICONTROL Add private key from keystore]*

Klik in de sleutelarchiefconsole van de gebruiker op **[!UICONTROL Add Private Key form KeyStore file]** en voeg de volgende informatie toe:

* **[!UICONTROL New Alias]**: de alias van de sleutel in AEM. Dit kan om het even wat zijn en moet niet met de naam van keystore beantwoorden die met het open bevel wordt gecreeerd.
* **[!UICONTROL KeyStore File]**: de uitvoer van de opdracht openssl pkcs12 (keystore.p12)
* **[!UICONTROL KeyStore File Password]**: Het wachtwoord dat is ingesteld in de opdracht openssl pkcs12 via `-passout` argument.
* **[!UICONTROL Private Key Alias]**: De waarde die aan de `-name` argument in de bovenstaande opdracht openssl pkcs12 (d.w.z. `my-key`).
* **[!UICONTROL Private Key Password]**: Het wachtwoord dat is ingesteld in de opdracht openssl pkcs12 via `-passout` argument.

>[!CAUTION]
>
>Het wachtwoord voor sleutelarchief-bestanden en het wachtwoord voor persoonlijke sleutels zijn voor beide invoer hetzelfde. Als u een niet-overeenkomend wachtwoord invoert, wordt de sleutel niet geïmporteerd.

### Controleer of de persoonlijke sleutel in het AEM sleutelarchief is geladen {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![Persoonlijke sleutel in AEM verifiëren](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL User]>[!UICONTROL Keystore]*

Wanneer de persoonlijke sleutel van het verstrekte sleutelarchief in AEM sleutelarchief met succes wordt geladen, worden de meta-gegevens van de privé sleutel getoond in de keystore console van de gebruiker.

## De openbare sleutel toevoegen aan Adobe I/O {#adding-the-public-key-to-adobe-i-o}

De passende openbare sleutel moet aan Adobe I/O worden geupload om de AEM de dienstgebruiker toe te staan, die de overeenkomstige privé van de openbare sleutel heeft om veilig te communiceren.

### Een nieuwe Adobe I/O-integratie maken {#create-a-adobe-i-o-new-integration}

![Een nieuwe Adobe I/O-integratie maken](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Create Adobe I/O Integration]](https://developer.adobe.com/console/) >[!UICONTROL New Integration]*

Voor het maken van een integratie in Adobe I/O moet u een openbaar certificaat uploaden. Upload de **certificate.crt** door de `openssl req` gebruiken.

### Controleren of de openbare sleutels zijn geladen in Adobe I/O {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![Openbare sleutels in Adobe I/O verifiëren](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

De geïnstalleerde openbare sleutels en hun vervaldatums zijn vermeld in [!UICONTROL Integrations] console op Adobe I/O. U kunt meerdere openbare sleutels toevoegen via de **[!UICONTROL Add a public key]** knop.

AEM houdt nu de privé sleutel en de integratie van de Adobe I/O houdt de overeenkomstige openbare sleutel, die AEM toestaat om veilig met Adobe I/O te communiceren.
