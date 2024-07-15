---
title: De SSL-wizard in AEM gebruiken
description: Adobe Experience Manager SSL opstellingstovenaar om het gemakkelijker te maken om een AEM instantie te plaatsen om over HTTPS te lopen.
version: 6.5, Cloud Service
jira: KT-13839
doc-type: Technical Video
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
last-substantial-update: 2023-08-08T00:00:00Z
duration: 564
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---

# De SSL-wizard in AEM gebruiken

Leer hoe u SSL in Adobe Experience Manager instelt om deze via HTTPS uit te voeren met behulp van de ingebouwde SSL-wizard.

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)


>[!NOTE]
>
>Voor beheerde omgevingen is het het beste voor de IT-afdeling om CA-vertrouwde certificaten en sleutels te leveren.
>
>Zelfondertekende certificaten mogen alleen worden gebruikt voor ontwikkelingsdoeleinden.

## De wizard SSL-configuratie gebruiken

Navigeer aan __AEM Auteur > Hulpmiddelen > Veiligheid > SSL Configuratie__, en open de __SSL Tovenaar van de Configuratie__.

![ SSL de Tovenaar van de Configuratie ](assets/use-the-ssl-wizard/ssl-config-wizard.png)

### Opslagreferenties maken

Om a _Zeer belangrijke Opslag_ te creëren verbonden aan de `ssl-service` systeemgebruiker en een globale _Opslag van het Vertrouwen_, gebruik de __6} tovenaar van de Credentials van de Opslag {.__

1. Ga het wachtwoord in en bevestig wachtwoord voor de __Zeer belangrijke Opslag__ verbonden aan de `ssl-service` systeemgebruiker.
1. Ga het wachtwoord in en bevestig wachtwoord voor de globale __Opslag van het Vertrouwen__. Het is een vertrouwde opslag voor het hele systeem. Als dit al gebeurt, wordt het ingevoerde wachtwoord genegeerd.

   ![ SSL Opstelling - de Referenties van de Opslag ](assets/use-the-ssl-wizard/store-credentials.png)

### Persoonlijke sleutel en certificaat uploaden

Om de _privé sleutel_ en _SSL certificaat_ te uploaden, gebruik de __Sleutel &amp; van het Certificaat__ tovenaarsstap.

Typisch, verstrekt uw afdeling van IT het CA-Vertrouwde certificaat en de sleutel, nochtans zelf-ondertekend certificaat voor __ontwikkeling__ en __het testen__ doeleinden kan worden gebruikt.

Om het zelf-ondertekende certificaat tot stand te brengen of te downloaden, zie de [ Zelfondertekende privé sleutel en het certificaat ](#self-signed-private-key-and-certificate).

1. Upload de __Persoonlijke Sleutel__ in het DER (Distinguished Encoding Regels) formaat. In tegenstelling tot PEM bevatten DER-gecodeerde bestanden geen onbewerkte tekstinstructies zoals `-----BEGIN CERTIFICATE-----`
1. Upload het bijbehorende __SSL Certificaat__ in het `.crt` formaat.

   ![ SSL Opstelling - Persoonlijke Sleutel en Certificaat ](assets/use-the-ssl-wizard/privatekey-and-certificate.png)

### SSL-verbindingsgegevens bijwerken

Om _hostname_ bij te werken en _haven_ gebruik de __SSL Schakelaar__ tovenaarsstap.

1. Werk of verifieer de __HTTPS Hostname__ waarde bij, zou het `Common Name (CN)` van het certificaat moeten aanpassen.
1. Werk of verifieer de __waarde van de Haven HTTPS__ bij.

   ![ SSL Opstelling - SSL Verbindingsdetails ](assets/use-the-ssl-wizard/ssl-connector-details.png)

### De SSL-instelling controleren

1. Om SSL te verifiëren, klik __gaan naar HTTPS URL__ knoop.
1. Als u een niet-geautoriseerd certificaat gebruikt, wordt een fout `Your connection is not private` weergegeven.

   ![ SSL Opstelling - verifieer AEM over HTTPS ](assets/use-the-ssl-wizard/verify-aem-over-ssl.png)

## Zelfondertekende persoonlijke sleutel en certificaat

Het volgende ZIP-bestand bevat [!DNL DER] - en [!DNL CRT] -bestanden die vereist zijn voor het lokaal instellen van AEM SSL en die alleen bedoeld zijn voor lokale ontwikkelingsdoeleinden.

De [!DNL DER] - en [!DNL CERT] -bestanden worden voor het gemak geleverd en gegenereerd met behulp van de stappen in de sectie Persoonlijke sleutel genereren en Zelfondertekend certificaat hieronder.

Indien nodig, is de uitdrukking van de certificaatpas **admin**.

Deze localhost - persoonlijke sleutel en zelfondertekend certificate.zip (verloopt in juli 2028)

[Het certificaatbestand downloaden](assets/use-the-ssl-wizard/certificate.zip)

### Persoonlijke sleutel en zelfondertekende certificaatgeneratie

In de video hierboven worden de installatie en configuratie van SSL op een AEM auteurinstantie met zelfondertekende certificaten weergegeven. Met de onderstaande opdrachten in [[!DNL OpenSSL] ](https://www.openssl.org/) kunt u een persoonlijke sleutel en certificaat genereren voor gebruik in Stap 2 van de wizard.

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -extfile <(printf "subjectAltName=DNS:localhost") -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
