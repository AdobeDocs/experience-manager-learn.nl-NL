---
title: Aangepaste domeinnaam toevoegen
description: Leer hoe u een aangepaste domeinnaam toevoegt om te AEM als door de cloudservice gehoste site.
version: Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: 1042
last-substantial-update: 2024-03-12T00:00:00Z
jira: KT-15121
thumbnail: KT-15121.jpeg
exl-id: 8936c3ae-2daf-4d0f-b260-28376ae28087
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 0%

---

# Aangepaste domeinnaam toevoegen

Leer hoe u een aangepaste domeinnaam aan de AEM as a Cloud Service-website kunt toevoegen.

In dit leerprogramma, wordt het branding van de steekproef [ AEM WKND ](https://github.com/adobe/aem-guides-wknd) plaats verbeterd door een HTTPS-Adressable naam van het douanedomein `wknd.enablementadobe.com` met de Veiligheid van de Laag van het Vervoer (TLS) toe te voegen.

>[!VIDEO](https://video.tv.adobe.com/v/3427903?quality=12&learn=on)

De stappen op hoog niveau zijn:

![ Hoge Naam van het Domein van de Douane ](./assets/add-custom-domain-name-steps.png){width="800" zoomable="yes"}

## Vereisten

>[!VIDEO](https://video.tv.adobe.com/v/3427909?quality=12&learn=on)

- [ OpenSSL ](https://www.openssl.org/) en [ graven ](https://www.isc.org/blogs/dns-checker/) zijn geïnstalleerd op uw lokale machine.
- Toegang tot diensten van derden:
   - De Autoriteit van het certificaat (CA) - om het ondertekende certificaat voor uw plaatsdomein, als [ DigitCert ](https://www.digicert.com/) te verzoeken
   - De het ontvangen dienst van het Systeem van de Naam van het domein (DNS) - om DNS verslagen voor uw douanedomein, zoals Azure DNS, of Route 53 van AWS toe te voegen.
- Toegang tot [ Adobe Cloud Manager ](https://my.cloudmanager.adobe.com/) als BedrijfsEigenaar of de rol van de Manager van de Plaatsing.
- De steekproef [ AEM WKND ](https://github.com/adobe/aem-guides-wknd) plaats wordt opgesteld aan milieu AEMCS van [ het type van het productieprogramma ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs).

Als u geen toegang tot derdediensten hebt, _samenwerken met uw veiligheid of het ontvangen team om de stappen_ te voltooien.

## SSL-certificaat genereren

>[!VIDEO](https://video.tv.adobe.com/v/3427908?quality=12&learn=on)

U hebt twee opties:

- Met het opdrachtregelprogramma van `openssl` kunt u een persoonlijke sleutel en een CSR-bestand (Certificate Signing Request) voor uw sitedomein genereren. Om een ondertekend certificaat aan te vragen, dient u de CSR in bij een certificeringsinstantie (CA).

- Uw hostingteam beschikt over de vereiste persoonlijke sleutel en het vereiste ondertekende certificaat voor uw site.

Laten we de stappen voor de eerste optie bekijken.

Als u een persoonlijke sleutel en een CSR wilt genereren, voert u de volgende opdrachten uit en verstrekt u de vereiste informatie wanneer u daarom wordt gevraagd:

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

Om een ondertekend certificaat aan te vragen, verstrek geproduceerde CSR aan CA door hun documentatie te volgen. Zodra CA CSR ondertekent, ontvangt u het ondertekende certificaatdossier.

### Ondertekend certificaat bekijken

Het is een goede gewoonte om het ondertekende certificaat te beoordelen voordat u het aan de Cloud Manager toevoegt. U kunt de certificaatdetails bekijken met de volgende opdracht:

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

Het ondertekende certificaat kan de certificaatketen bevatten, die de basis- en tussenliggende certificaten bevat, samen met het certificaat van de eindentiteit.

De Adobe Cloud Manager keurt het eindentiteitcertificaat en de certificaatketting _in afzonderlijke vormgebieden_ goed, zodat moet u het eindentiteitcertificaat en de certificaatketting van het ondertekende certificaat halen.

In dit leerprogramma, wordt het ](https://www.digicert.com/) ondertekende certificaat DigitCert [ dat tegen `*.enablementadobe.com` domein wordt uitgegeven gebruikt als voorbeeld. De eindentiteit- en certificaatketen wordt geëxtraheerd door het ondertekende certificaat te openen in een teksteditor en de inhoud te kopiëren tussen de markeringen `-----BEGIN CERTIFICATE-----` en `-----END CERTIFICATE-----` .

## SSL-certificaat toevoegen in Cloud Manager

>[!VIDEO](https://video.tv.adobe.com/v/3427906?quality=12&learn=on)

Om het SSL certificaat in Cloud Manager toe te voegen, volg [ SSL Certificaat ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-ssl-certificates/add-ssl-certificate) documentatie toevoegen.

## Domeinnaamverificatie

>[!VIDEO](https://video.tv.adobe.com/v/3427905?quality=12&learn=on)

Voer de volgende stappen uit om de domeinnaam te controleren:

- Voeg domeinnaam in Cloud Manager toe door [ te volgen voeg de documentatie van de Naam van het Domein van de Douane ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-custom-domain-name) toe.
- Voeg een AEM-specifiek [ TXT verslag ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-text-record) in uw DNS het ontvangen dienst toe.
- Verifieer de bovenstaande stappen door de DNS servers te vragen gebruikend het `dig` bevel.

```bash
# General syntax, the `_aemverification` is prefix provided by Adobe
$ dig _aemverification.[YOUR-DOMAIN-NAME] -t txt

# This tutorial specific example, as the subdomain `wknd.enablementadobe.com` is used
$ dig _aemverification.wknd.enablementadobe.com -t txt
```

De geslaagde reactie van het voorbeeld ziet er als volgt uit:

```bash
; <<>> DiG 9.10.6 <<>> _aemverification.wknd.enablementadobe.com -t txt
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8636
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1220
;; QUESTION SECTION:
;_aemverification.wknd.enablementadobe.com. IN TXT

;; ANSWER SECTION:
_aemverification.wknd.enablementadobe.com. 3600    IN TXT "adobe-aem-verification=wknd.enablementadobe.com/105881/991000/bef0e843-9280-4385-9984-357ed9a4217b"

;; Query time: 81 msec
;; SERVER: 153.32.14.247#53(153.32.14.247)
;; WHEN: Tue Mar 12 15:54:25 EDT 2024
;; MSG SIZE  rcvd: 181
```

In dit leerprogramma, wordt Azure DNS gebruikt als voorbeeld. Om het TXT- verslag toe te voegen, moet u de documentatie van uw DNS het ontvangen dienst volgen.

Herzie de [ Controlerende de Status van de Naam van het Domein ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-domain-name-status) documentatie als er een kwestie is.

## DNS-record configureren

>[!VIDEO](https://video.tv.adobe.com/v/3427907?quality=12&learn=on)

Om het DNS verslag voor uw douanedomein te vormen volg deze stappen:

- Bepaal het DNS verslagtype (CNAME of APEX) dat op het domeintype, zoals worteldomein (APEX) of subdomain (CNAME) wordt gebaseerd, en volg de [ Vormende DNS documentatie van Montages ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/configure-dns-settings).
- Voeg het DNS-record toe aan uw DNS-hostingservice.
- Trigger de DNS verslagbevestiging door de [ controlerende DNS documentatie van de Status van het Verslag ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-dns-record-status) te volgen.

In dit leerprogramma, aangezien a **subdomain** `wknd.enablementadobe.com` wordt gebruikt, wordt het CNAME- verslagtype dat aan `cdn.adobeaemcloud.com` richt toegevoegd.

Nochtans, als u het **worteldomein** gebruikt, moet u APEX- verslagtype (alias A, ALIAS, of ANAME) toevoegen dat aan de specifieke IP adressen richt die door Adobe worden verstrekt.

## Site-verificatie

>[!VIDEO](https://video.tv.adobe.com/v/3427904?quality=12&learn=on)

Als u wilt controleren of de site toegankelijk is met de aangepaste domeinnaam, opent u een webbrowser en navigeert u naar de URL van het aangepaste domein. Controleer of de site toegankelijk is en of de browser een veilige verbinding met het hangslotpictogram heeft.

## Video beëindigen tot einde

U kunt ook de end-to-end video bekijken die het overzicht, de eerste vereisten, en bovenstaande stappen toont om een douanedomeinaam aan AEM as a Cloud Service-ontvangen plaats toe te voegen.

>[!VIDEO](https://video.tv.adobe.com/v/3427817?quality=12&learn=on)
