---
title: De SSL-wizard in AEM gebruiken
description: Adobe Experience Manager SSL opstellingstovenaar om het gemakkelijker te maken om een AEM instantie te plaatsen om over HTTPS te lopen.
seo-description: Adobe Experience Manager SSL opstellingstovenaar om het gemakkelijker te maken om een AEM instantie te plaatsen om over HTTPS te lopen.
version: 6.3, 6,4, 6.5
feature: null
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 0%

---


# De SSL-wizard in AEM gebruiken

Adobe Experience Manager SSL opstellingstovenaar om het gemakkelijker te maken om een AEM instantie te plaatsen om over HTTPS te lopen.

>[!VIDEO](https://video.tv.adobe.com/v/17993/?quality=12&learn=on)

>[!NOTE]
>
>Voor beheerde omgevingen is het het beste voor de IT-afdeling om CA-vertrouwde certificaten en sleutels te leveren.
>
>Zelfondertekende certificaten mogen alleen worden gebruikt voor ontwikkelingsdoeleinden.

## Persoonlijke sleutel en zelfondertekende certificaatdownload

Het volgende ZIP-bestand bevat [!DNL DER] en [!DNL CRT] bestanden die vereist zijn voor het instellen van AEM SSL op localhost en die alleen bedoeld zijn voor lokale ontwikkelingsdoeleinden.

De [!DNL DER] bestanden en [!DNL CERT] bestanden worden voor het gemak geleverd en gegenereerd met de stappen die worden beschreven in de sectie Persoonlijke sleutel genereren en Zelfondertekend certificaat hieronder.

Indien nodig, is de uitdrukking van de certificaatpas **admin**.

localhost - persoonlijke sleutel en zelfondertekend certificate.zip (verloopt in juli 2028)

[Het certificaatbestand downloaden](assets/use-the-ssl-wizard/certificate.zip)

## Persoonlijke sleutel en zelfondertekende certificaatgeneratie

In de video hierboven worden de installatie en configuratie van SSL op een AEM auteurinstantie met zelfondertekende certificaten weergegeven. Met de onderstaande opdrachten met [[!DNL OpenSSL]](https://www.openssl.org/) kunt u een persoonlijke sleutel en certificaat genereren voor gebruik in stap 2 van de wizard.

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
