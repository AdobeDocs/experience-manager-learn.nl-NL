---
title: De domeinnaam van de douane met klant-beheerde CDN
description: Leer hoe te om een naam van het douanedomein aan de website van AEM as a Cloud Service uit te voeren die klant-beheerde CDN gebruikt.
version: Experience Manager as a Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-06-21T00:00:00Z
jira: KT-15945
thumbnail: KT-15945.jpeg
exl-id: fa9ee14f-130e-491b-91b6-594ba47a7278
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# De domeinnaam van de douane met klant-beheerde CDN

Leer hoe te om een naam van het douanedomein aan een website van AEM as a Cloud Service toe te voegen die a **klant-geleide CDN** gebruikt.

In dit leerprogramma, wordt het branding van de steekproef [ AEM WKND ](https://github.com/adobe/aem-guides-wknd) plaats verbeterd door een HTTPS-adresseerbare naam van het douanedomein `wkndviaawscdn.enablementadobe.com` met de Veiligheid van de Laag van het Vervoer (TLS) toe te voegen gebruikend een klant-beheerde CDN. In deze zelfstudie wordt AWS CloudFront gebruikt als door de klant beheerde CDN, maar elke CDN-provider moet compatibel zijn met AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3432561?quality=12&learn=on)

De stappen op hoog niveau zijn:

![ Naam van het Domein van de Douane met Klant CDN ](./assets/add-custom-domain-name-with-customer-CDN.png){width="800" zoomable="yes"}

## Vereisten

>[!VIDEO](https://video.tv.adobe.com/v/3432562?quality=12&learn=on)

- [ OpenSSL ](https://www.openssl.org/) en [ graven ](https://www.isc.org/blogs/dns-checker/) zijn geïnstalleerd op uw lokale machine.
- Toegang tot diensten van derden:
   - De Autoriteit van het certificaat (CA) - om het ondertekende certificaat voor uw plaatsdomein, als [ DigitCert ](https://www.digicert.com/) te verzoeken
   - CDN van de klant - aan opstelling CDN van de klant en voeg SSL certificaten en domeindetails toe, zoals AWS CloudFront, Azure CDN, of Akamai.
   - De het ontvangen dienst van het Systeem van de Naam van het domein (DNS) - om DNS verslagen voor uw douanedomein, zoals Azure DNS, of Route 53 van AWS toe te voegen.
- De toegang tot [ Cloud Manager van Adobe ](https://my.cloudmanager.adobe.com/) om de bevestigingsregel CDN van de Kopbal van HTTP op te stellen aan het milieu van AEM as a Cloud Service.
- De plaats van de steekproef [ AEM WKND ](https://github.com/adobe/aem-guides-wknd) wordt opgesteld aan het milieu van AEM as a Cloud Service van [ het type van het productieprogramma ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs).

Als u geen toegang tot derdediensten hebt, _samenwerken met uw veiligheid of het ontvangen team om de stappen_ te voltooien.

## SSL-certificaat genereren

>[!VIDEO](https://video.tv.adobe.com/v/3427908?quality=12&learn=on)

U hebt twee opties:

1. Met het opdrachtregelprogramma van `openssl` kunt u een persoonlijke sleutel en een CSR-bestand (Certificate Signing Request) voor uw sitedomein genereren. Als u een ondertekend certificaat wilt aanvragen, dient u de CSR in bij een certificeringsinstantie (CA).
1. Uw hostingteam beschikt over de vereiste persoonlijke sleutel en het vereiste ondertekende certificaat voor uw site.

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

Adobe Cloud Manager keurt het eindentiteitcertificaat en de certificaatketting _in afzonderlijke vormgebieden_ goed, zodat moet u het eindentiteitcertificaat en de certificaatketting van het ondertekende certificaat halen.

In dit leerprogramma, wordt het [ ondertekende certificaat DigitCert ](https://www.digicert.com/) dat tegen `*.enablementadobe.com` domein wordt uitgegeven gebruikt als voorbeeld. De eindentiteit- en certificaatketen wordt geëxtraheerd door het ondertekende certificaat te openen in een teksteditor en de inhoud te kopiëren tussen de markeringen `-----BEGIN CERTIFICATE-----` en `-----END CERTIFICATE-----` .

## CDN met klantbeheer instellen

>[!VIDEO](https://video.tv.adobe.com/v/3432563?quality=12&learn=on)

Stel de CDN van de klant in, zoals AWS CloudFront, Azure CDN of Akamai, en voeg het SSL-certificaat en de domeingegevens toe. In deze zelfstudie wordt AWS CloudFront als voorbeeld gebruikt. Afhankelijk van uw CDN-leverancier kunnen de stappen echter afwijken. De belangrijkste callouts zijn:

- Voeg het SSL-certificaat toe aan de CDN.
- Voeg de aangepaste domeinnaam toe aan de CDN.
- Configureer de CDN om de inhoud in cache te plaatsen, zoals afbeeldingen, CSS- en JavaScript-bestanden.
- Voeg de header `X-Forwarded-Host` HTTP toe aan de CDN-instellingen, zodat de CDN deze header opneemt in alle aanvragen die het naar de AEMCD-oorsprong verzendt.
- Zorg ervoor dat de headerwaarde `Host` is ingesteld op het standaard AEM as a Cloud Service-domein met de programma- en omgeving-id en dat eindigt met `adobeaemcloud.com` . De HTTP-hostheaderwaarde die door de CDN van de klant aan de CDN van de Adobe wordt doorgegeven, moet het standaard AEM as a Cloud Service-domein zijn. Elke andere waarde resulteert in een foutstatus.

## DNS-records configureren

>[!VIDEO](https://video.tv.adobe.com/v/3432564?quality=12&learn=on)

Om het DNS verslag voor uw douanedomein te vormen volg deze stappen:

1. Voeg een CNAME-record toe voor het aangepaste domein dat naar de CDN-domeinnaam verwijst.

Deze zelfstudie voegt een CNAME-record toe aan Azure DNS voor het aangepaste domein `wkndviaawscdn.enablementadobe.com` en wijst deze toe aan de AWS CloudFront-domeinnaam voor distributie.

### Site-verificatie

Controleer de aangepaste domeinnaam door de site te openen met de aangepaste domeinnaam.
Het werkt mogelijk wel of niet afhankelijk van de configuratie van de host in de AEM as a Cloud Service-omgeving.

Een cruciale beveiligingsstap is het implementeren van de CDN-regel voor HTTP-headervalidatie in de AEM as a Cloud Service-omgeving. De regel zorgt ervoor dat het verzoek uit klant CDN en niet uit een andere bron komt.

## Huidige werkstatus zonder CDN-regel voor HTTP-headervalidatie

>[!VIDEO](https://video.tv.adobe.com/v/3432565?quality=12&learn=on)

Zonder de CDN-regel voor HTTP-headervalidatie wordt de `Host` -headerwaarde ingesteld op het standaard AEM as a Cloud Service-domein dat de programma- en milieu-id bevat en eindigt met `adobeaemcloud.com` . Adobe CDN transformeert de headerwaarde `Host` alleen naar de waarde van de `X-Forwarded-Host` die wordt ontvangen van de CDN van de klant als de CDN-regel voor HTTP-headervalidatie wordt geïmplementeerd. Anders wordt de headerwaarde van `Host` op dezelfde manier doorgegeven aan de AEM as a Cloud Service-omgeving en wordt de header van `X-Forwarded-Host` niet gebruikt.

### Voorbeeldservletcode om de headerwaarde van de host af te drukken

De volgende servlet-code drukt de headerwaarden `Host` , `X-Forwarded-*` , `Referer` en `Via` HTTP af in het JSON-antwoord.

```java
package com.adobe.aem.guides.wknd.core.servlets;

import java.io.IOException;
import java.util.Enumeration;

import javax.servlet.Servlet;
import javax.servlet.ServletException;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.ResourceResolverFactory;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.ServletResolverConstants;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

@Component(service = Servlet.class, property = {
        ServletResolverConstants.SLING_SERVLET_PATHS + "=/bin/verify-headers",
        ServletResolverConstants.SLING_SERVLET_METHODS + "=" + HttpConstants.METHOD_GET
})
public class VerifyHeadersServlet extends SlingSafeMethodsServlet {

    @Reference
    private ResourceResolverFactory resourceResolverFactory;

    @Override
    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");

        // Create JSON response
        StringBuilder jsonResponse = new StringBuilder();
        jsonResponse.append("{");

        Enumeration<String> headerNames = request.getHeaderNames();
        boolean firstHeader = true;

        while (headerNames.hasMoreElements()) {
            String headerName = headerNames.nextElement();

            if (headerName.startsWith("X-Forwarded-") || headerName.startsWith("Host")
                    || headerName.startsWith("Referer") || headerName.startsWith("Via")) {
                if (!firstHeader) {
                    jsonResponse.append(",");
                }
                jsonResponse.append("\"").append(headerName).append("\": \"").append(request.getHeader(headerName))
                        .append("\"");
                firstHeader = false;
            }
        }

        jsonResponse.append("}");

        response.getWriter().write(jsonResponse.toString());
    }
}
```

Als u de servlet wilt testen, werkt u het `../dispatcher/src/conf.dispatcher.d/filters/filters.any` -bestand bij met de volgende configuratie. Zorg ook ervoor dat CDN aan **NIET** `/bin/*` weg {wordt gevormd in het voorgeheugen onderbrengen.

```plaintext
# Testing purpose bin
/0300 { /type "allow" /extension "json" /path "/bin/*"}
/0301 { /type "allow" /path "/bin/*"}
/0302 { /type "allow" /url "/bin/*"}
```

## De CDN-regel voor HTTP-headervalidatie configureren en implementeren

>[!VIDEO](https://video.tv.adobe.com/v/3432566?quality=12&learn=on)

Voer de volgende stappen uit om de CDN-regel voor HTTP-headervalidatie te configureren en implementeren:

- Voeg de CDN-regel voor HTTP-headervalidatie toe in het `cdn.yaml` -bestand. Hieronder ziet u een voorbeeld.

  ```yaml
  kind: "CDN"
  version: "1"
  metadata:
    envTypes: ["prod"]
  data:
    authentication:
      authenticators:
        - name: edge-auth
          type: edge
          edgeKey1: ${{CDN_EDGEKEY_080124}}
          edgeKey2: ${{CDN_EDGEKEY_110124}}
      rules:
        - name: edge-auth-rule
          when: { reqProperty: tier, equals: "publish" }
          action:
            type: authenticate
            authenticator: edge-auth
  ```

- Creeer geheim-type milieuvariabelen (CDN_EDGEKEY_080124, CDN_EDGEKEY_110124) gebruikend Cloud Manager UI.
- Implementeer de CDN-regel voor HTTP-headervalidatie in de AEM as a Cloud Service-omgeving met behulp van de Cloud Manager-pijplijn.

## Geef het geheim door in de X-AEM-Edge-Key HTTP-header

>[!VIDEO](https://video.tv.adobe.com/v/3432567?quality=12&learn=on)

Werk de CDN van de klant bij om het geheim in de `X-AEM-Edge-Key` Kopbal van HTTP over te gaan. Het geheim wordt gebruikt door CDN van Adobe om te bevestigen het verzoek komt van CDN van de klant en zet de `Host` kopbalwaarde in de waarde van `X-Forwarded-Host` om die van CDN van de klant wordt ontvangen.

## Video beëindigen tot einde

U kunt de video van begin tot eind ook bekijken die de bovengenoemde stappen aantoont om een naam van het douanedomein met een klant-beheerde CDN aan een op AEM as a Cloud Service-Ghost plaats toe te voegen.

>[!VIDEO](https://video.tv.adobe.com/v/3432568?quality=12&learn=on)
