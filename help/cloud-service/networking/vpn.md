---
title: Virtual Private Network (VPN)
description: Leer hoe te om AEM as a Cloud Service met uw VPN te verbinden om veilige communicatie kanalen tussen AEM en de interne diensten tot stand te brengen.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9352
thumbnail: KT-9352.jpeg
exl-id: 74cca740-bf5e-4cbd-9660-b0579301a3b4
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '1321'
ht-degree: 0%

---

# Virtual Private Network (VPN)

Leer hoe te om AEM as a Cloud Service met uw VPN te verbinden om veilige communicatie kanalen tussen AEM en de interne diensten tot stand te brengen.

## Wat is Virtual Private Network?

Het virtuele Privé Netwerk (VPN) staat een AEM as a Cloud Service klant toe om te verbinden **de AEM** binnen een Cloud Manager-programma naar een bestaand programma [ondersteund](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn) VPN. Dit staat veilige, en gecontroleerde verbindingen tussen AEM as a Cloud Service en de diensten binnen het netwerk van de klant toe.

Een Cloud Manager-programma kan alleen een __enkel__ type netwerkinfrastructuur. Zorg ervoor dat Virtual Private Network het meest is [geschikt type netwerkinfrastructuur](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#general-vpn-considerations) voor uw AEM as a Cloud Service alvorens de volgende bevelen uit te voeren.

>[!NOTE]
>
>Opmerking: het verbinden van de ontwikkelomgeving van Cloud Manager met een VPN wordt niet ondersteund. Als u binaire artefacten van een privé bewaarplaats moet toegang hebben moet u opstelling een veilige en wachtwoord beschermde bewaarplaats met een URL die op het openbare Internet beschikbaar is [zoals hier beschreven](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/create-application-project/setting-up-project.html#password-protected-maven-repositories).

>[!MORELIKETHIS]
>
> De as a Cloud Service AEM lezen [geavanceerde documentatie van de netwerkconfiguratie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn) voor meer details over Virtueel Privé Netwerk.

## Vereisten

Voor het instellen van Virtual Private Network is het volgende vereist:

+ Adobe-account met [Machtigingen van Business Owner van Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/permissions/#cloud-manager-api-permissions)
+ Toegang tot [Verificatiereferenties van de API van Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + Organisatie-id (ook bekend als IMS Org ID)
   + Client-id (ook bekend als API-sleutel)
   + Toegangstoken (ook bekend als Dragertoken)
+ Programma-id van Cloud Manager
+ De omgevings-id&#39;s van Cloud Manager
+ Een virtueel Privé Netwerk, met toegang tot alle noodzakelijke verbindingsparameters.

Deze zelfstudie gebruikt `curl` om de API-configuraties van Cloud Manager te maken. De verstrekte `curl` voor opdrachten wordt uitgegaan van een Linux/macOS-syntaxis. Als het gebruiken van de het bevelherinnering van Vensters, vervang `\` regeleindeteken met `^`.

## Virtuele privénetwerk per programma inschakelen

Begin door het Virtuele Privé Netwerk op AEM as a Cloud Service toe te laten.

1. Bepaal eerst met behulp van de API van Cloud Manager in welk gebied de geavanceerde netwerken worden ingesteld [listRegions](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getProgramRegions) bewerking. De `region name` zijn vereist om volgende API-aanroepen van Cloud Manager uit te voeren. Doorgaans wordt de regio waarin de productieomgeving zich bevindt, gebruikt.

   __listRegions HTTP request__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Virtual Private Network inschakelen voor een Cloud Manager-programma met Cloud Manager-API&#39;s [createNetworkInfrastructure](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/createNetworkInfrastructure) bewerking. Gebruik de juiste `region` code die is verkregen via de API voor Cloud Manager `listRegions` bewerking.

   __createNetworkInfrastructure HTTP request__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   De JSON-parameters definiëren in een `vpn-create.json` en verstrekt via `... -d @./vpn-create.json`.

[Download het voorbeeld vpn-create.json](./assets/vpn-create.json)

   ```json
   {
       "kind": "vpn",
       "region": "va7",
       "addressSpace": [
           "10.104.182.64/26"
       ],
       "dns": {
           "resolvers": [
               "10.151.201.22",
               "10.151.202.22",
               "10.154.155.22"
           ]
       },
       "connections": [{
           "name": "connection-1",
           "gateway": {
               "address": "195.231.212.78",
               "addressSpace": [
                   "10.151.0.0/16",
                   "10.152.0.0/16",
                   "10.153.0.0/16",
                   "10.154.0.0/16",
                   "10.142.0.0/16",
                   "10.143.0.0/16",
                   "10.124.128.0/17"
               ]
           },
           "sharedKey": "<secret_shared_key>",
           "ipsecPolicy": {
               "dhGroup": "ECP256",
               "ikeEncryption": "AES256",
               "ikeIntegrity": "SHA256",
               "ipsecEncryption": "AES256",
               "ipsecIntegrity": "SHA256",
               "pfsGroup": "ECP256",
               "saDatasize": 102400000,
               "saLifetime": 3600
           }
       }]
   }
   ```

   Wacht 45-60 minuten op het Programma van de Manager van de Wolk om de netwerkinfrastructuur te verstrekken.

1. Controleren of de omgeving gereed is __Virtueel privé netwerk__ configuratie met de API voor Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) bewerking, gebruiken van de `id` is in de vorige stap geretourneerd van de HTTP-aanvraag createNetworkInfrastructure.

   __getNetworkInfrastructure HTTP-aanvraag__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_BEARER_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Controleer of de HTTP-respons een __status__ van __klaar__. Controleer de status om de paar minuten als u nog niet klaar bent.

## Virtual Private Network-proxy&#39;s per omgeving configureren

1. Schakel de __Virtueel privé netwerk__ configuratie op elke AEM as a Cloud Service omgeving met gebruik van de API voor Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) bewerking.

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP request__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   De JSON-parameters definiëren in een `vpn-configure.json` en verstrekt via `... -d @./vpn-configure.json`.

[Download het voorbeeld vpn-configure.json](./assets/vpn-configure.json)

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org"
       ],
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.com",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   `nonProxyHosts` verklaart een reeks gastheren waarvoor haven 80 of 443 door de standaard gedeelde IP adreswaaiers eerder dan specifieke uitgang IP zou moeten worden verpletterd. Dit kan nuttig zijn aangezien het verkeer dat door gedeelde IPs wordt behandeld verder automatisch door Adobe wordt geoptimaliseerd.

   Voor elke `portForwards` afbeelding, bepaalt het geavanceerde voorzien van een netwerk de volgende het door:sturen regel:

   | Proxyhost | Proxypoort |  | Externe host | Externe poort |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   Als uw AEM __alleen__ vereist HTTP/HTTPS-verbindingen met de externe service, laat de `portForwards` array leeg, aangezien deze regels alleen vereist zijn voor niet-HTTP/HTTPS-aanvragen.


1. Voor elke omgeving valideert u de vpn-routeringsregels die worden gebruikt met de API&#39;s van Cloud Manager [getEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getEnvironmentAdvancedNetworkingConfiguration) bewerking.

   __getEnvironmentAdvancedNetworkingConfiguration HTTP-aanvraag__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Proxyconfiguraties van virtuele privénetwerken kunnen worden bijgewerkt met de API&#39;s van Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) bewerking. Herinneren `enableEnvironmentAdvancedNetworkingConfiguration` is een `PUT` bewerking, zodat alle regels moeten worden voorzien van elke aanroep van deze bewerking.

1. Nu kunt u de Virtuele Privé configuratie van de uitgang van het Netwerk in uw douane AEM code en configuratie gebruiken.

## Verbinding maken met externe services via het Virtual Private Network

Met het Virtuele Privé Toegelaten Netwerk, kunnen AEM code en configuratie hen gebruiken om vraag aan externe diensten via VPN te maken. Er zijn twee vlotten van externe vraag die AEM verschillend behandelt:

1. HTTP/HTTPS-aanroepen naar externe services op niet-standaard poorten
   + Omvat HTTP/HTTPS vraag die aan de diensten wordt gemaakt die op havens buiten standaard 80 of 443 havens lopen.
1. niet-HTTP/HTTPS-aanroepen naar externe services
   + Omvat om het even welke niet-HTTP vraag, zoals verbindingen met de servers van de Post, SQL gegevensbestanden, of de diensten die op andere niet-HTTP/HTTPS protocollen lopen.

HTTP/HTTPS-aanvragen van AEM op standaardpoorten (80/443) zijn standaard toegestaan en hebben geen extra configuratie of overwegingen nodig.

### HTTP/HTTPS op niet-standaardpoorten

Wanneer u HTTP/HTTPS-verbindingen maakt met niet-standaardpoorten (niet-80/443) vanaf AEM, moet de verbinding tot stand worden gebracht via speciale host en poorten, die via plaatsaanduidingen worden geleverd.

AEM biedt twee sets speciale Java™-systeemvariabelen die zijn toegewezen aan AEM HTTP/HTTPS-proxy&#39;s.

| Naam variabele | Gebruik | Java™-code | OSGi-configuratie | Mod_proxyconfiguratie voor Apache-webservers | | - | - | - | - | - | | `AEM_HTTP_PROXY_HOST` | Proxyhost voor HTTP-verbindingen | `System.getenv("AEM_HTTP_PROXY_HOST")` | `$[env:AEM_HTTP_PROXY_HOST]` | `${AEM_HTTP_PROXY_HOST}` | | `AEM_HTTP_PROXY_PORT` | Proxypoort voor HTTP-verbindingen | `System.getenv("AEM_HTTP_PROXY_PORT")` | `$[env:AEM_HTTP_PROXY_PORT]` |  `${AEM_HTTP_PROXY_PORT}` | | `AEM_HTTPS_PROXY_HOST` | Proxyhost voor HTTPS-verbindingen | `System.getenv("AEM_HTTPS_PROXY_HOST")` | `$[env:AEM_HTTPS_PROXY_HOST]` | `${AEM_HTTPS_PROXY_HOST}` | | `AEM_HTTPS_PROXY_PORT` | Proxypoort voor HTTPS-verbindingen | `System.getenv("AEM_HTTPS_PROXY_PORT")` | `$[env:AEM_HTTPS_PROXY_PORT]` | `${AEM_HTTPS_PROXY_PORT}` |

Verzoeken naar externe HTTP/HTTPS-services moeten worden gedaan door de proxyconfiguratie van de Java™ HTTP-client via AEM proxyhosts/poortwaarden te configureren.

Bij het aanroepen van HTTP/HTTPS naar externe services op niet-standaardpoorten, is er geen corresponderende `portForwards` moet worden gedefinieerd met de API&#39;s van Cloud Manager `__enableEnvironmentAdvancedNetworkingConfiguration` verrichting, aangezien de haven die &quot;regels&quot;door:sturen &quot;in code&quot;wordt bepaald.

>[!TIP]
>
> Zie AEM as a Cloud Service Virtual Private Network-documentatie voor [de volledige reeks verpletterende regels](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn-traffic-routing).

#### Codevoorbeelden

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports.md"><img alt="HTTP/HTTPS op niet-standaardpoorten" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports.md">HTTP/HTTPS op niet-standaardpoorten</a></strong></div>
    <p>
        Java™-codevoorbeeld waarbij een HTTP/HTTPS-verbinding van AEM as a Cloud Service wordt gemaakt met een externe service op niet-standaard HTTP/HTTPS-poorten.
    </p>
</td>
<td></td>
<td></td>
</tr>
</table>

### Codevoorbeelden van niet-HTTP/HTTPS-verbindingen

Bij het maken van niet-HTTP/HTTPS-verbindingen (bijvoorbeeld SQL, SMTP, etc.) van AEM, moet de verbinding door een speciale gastheernaam worden gemaakt die door AEM wordt verstrekt.

| Naam variabele | Gebruik | Java™-code | OSGi-configuratie | | - | - | - | - | | `AEM_PROXY_HOST` | Proxyhost voor niet-HTTP/HTTPS-verbindingen | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


De verbindingen aan de externe diensten worden dan geroepen door `AEM_PROXY_HOST` en de toegewezen poort (`portForwards.portOrig`), die dan aan toegewezen externe hostname AEM leiden (`portForwards.name`) en poort (`portForwards.portDest`).

| Proxyhost | Proxypoort |  | Externe host | Externe poort |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |


#### Codevoorbeelden

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="SQL-verbinding met JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">SQL-verbinding met JDBC DataSourcePool</a></strong></div>
      <p>
            Java™ codevoorbeeld die met externe SQL gegevensbestanden verbinden door AEM JDBC datasource pool te vormen.
      </p>
    </td>
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="SQL-verbinding met Java API's" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">SQL-verbinding met Java™ API's</a></strong></div>
      <p>
            Java™-codevoorbeeld voor verbinding met externe SQL-databases met SQL API's van Java™.
      </p>
    </td>
   <td>
      <a  href="./examples/email-service.md"><img alt="Virtual Private Network (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">E-mailservice</a></strong></div>
      <p>
        OSGi configuratievoorbeeld dat AEM gebruikt om met externe e-maildiensten te verbinden.
      </p>
    </td>
</tr></table>

### Toegang beperken tot AEM as a Cloud Service via VPN

De virtuele Privé configuratie van het Netwerk staat toegang tot AEM as a Cloud Service milieu&#39;s toe om tot de toegang van VPN worden beperkt.

#### Configuratievoorbeelden

<table><tr>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=en"><img alt="Een IP-lijst van gewenste personen toepassen" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=en">Een IP lijst van gewenste personen toepassen</a></strong></div>
      <p>
            Vorm een IP lijst van gewenste personen dusdanig dat slechts het verkeer van VPN tot AEM kan toegang hebben.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="Op pad gebaseerde VPN-toegangsbeperkingen voor AEM-publicatie" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">Op pad gebaseerde VPN-toegangsbeperkingen voor AEM-publicatie</a></strong></div>
      <p>
            Vereis de toegang van VPN voor specifieke wegen op publiceren AEM.
      </p>
    </td>
   <td></td>
</tr></table>
