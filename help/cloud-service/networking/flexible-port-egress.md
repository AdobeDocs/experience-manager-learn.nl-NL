---
title: Flexibele poortuitgang
description: Leer hoe u flexibele poorttoegang instelt en gebruikt om externe verbindingen van AEM as a Cloud Service naar externe services te ondersteunen.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
duration: 893
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1060'
ht-degree: 0%

---

# Flexibele poortuitgang

Leer hoe u flexibele poorttoegang instelt en gebruikt om externe verbindingen van AEM as a Cloud Service naar externe services te ondersteunen.

## Wat is Flexibele havenuitgang?

De flexibele havenuitgang staat voor douane, specifieke haven toe die regels door:sturen om aan AEM as a Cloud Service worden vastgemaakt, toestaand verbindingen van AEM aan externe diensten worden gemaakt.

Een Cloud Manager-programma kan alleen een __enkel__ type netwerkinfrastructuur. Zorg ervoor dat het IP-adres van de toegewijde uitgang het meest is [geschikt type netwerkinfrastructuur](./advanced-networking.md)  voor uw AEM as a Cloud Service alvorens de volgende bevelen uit te voeren.

>[!MORELIKETHIS]
>
> De as a Cloud Service AEM lezen [geavanceerde documentatie van de netwerkconfiguratie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress) voor meer informatie over flexibele havenuitgang.

## Vereisten

Het volgende is vereist voor het instellen van flexibel poortuitgang:

+ Adobe Developer Console-project met Cloud Manager-API ingeschakeld en [Machtigingen van Business Owner van Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Toegang tot [Verificatiereferenties van de API van Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + Organisatie-id (ook bekend als IMS Org ID)
   + Client-id (ook bekend als API-sleutel)
   + Toegangstoken (ook bekend als Dragertoken)
+ Programma-id van Cloud Manager
+ De omgevings-id&#39;s van Cloud Manager

Voor meer details bekijk de volgende analyse voor hoe te opstelling, vormen, en verkrijgen de geloofsbrieven van de Manager van de Wolk API, en hoe te om hen te gebruiken om een vraag van de Manager van de Wolk te maken API.

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

Deze zelfstudie gebruikt `curl` om de API-configuraties van Cloud Manager te maken. De verstrekte `curl` veronderstellen een syntaxis van Linux/macOS. Als het gebruiken van de het bevelherinnering van Vensters, vervang `\` regeleindeteken met `^`.

## Flexibele poorttoegang per programma inschakelen

Begin door de flexibele havenuitgang op AEM as a Cloud Service toe te laten.

1. Bepaal eerst in welke regio Geavanceerde netwerken worden ingesteld met behulp van de API van Cloud Manager [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) -bewerking. De `region name` is vereist om volgende API-aanroepen van Cloud Manager uit te voeren. Doorgaans wordt de regio waarin de productieomgeving zich bevindt, gebruikt.

   Zoek het gebied van uw AEM as a Cloud Service omgeving in [Cloud Manager](https://my.cloudmanager.adobe.com) onder de [details van de omgeving](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=en#viewing-environment). De regionaam die wordt weergegeven in Cloud Manager kan [toegewezen aan de regiocode](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) worden gebruikt in de API van Cloud Manager.

   __listRegions HTTP request__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Flexibele poorttoegang voor een Cloud Manager-programma inschakelen met de Cloud Manager-API [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) -bewerking. Gebruik de juiste `region` code die is verkregen via de API voor Cloud Manager `listRegions` -bewerking.

   __createNetworkInfrastructure HTTP request__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   Wacht 15 minuten op het Programma van de Manager van de Wolk om de netwerkinfrastructuur te verstrekken.

1. Controleren of de omgeving gereed is __flexibel poortbereik__ configuratie met de API voor Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) bewerking, gebruiken van de `id` is in de vorige stap geretourneerd van de HTTP-aanvraag createNetworkInfrastructure.

   __getNetworkInfrastructure HTTP-aanvraag__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Controleer of de HTTP-respons een __status__ van __klaar__. Controleer de status om de paar minuten als u nog niet klaar bent.

## Flexibele proxy&#39;s voor poortuitgang per omgeving configureren

1. Schakel de __flexibel poortbereik__ configuratie op elke AEM as a Cloud Service omgeving met gebruik van de API voor Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) -bewerking.

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP request__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   De JSON-parameters definiëren in een `flexible-port-egress.json` en verstrekt via `... -d @./flexible-port-egress.json`.

   [Download het voorbeeld flexibele-port-egress.json](./assets/flexible-port-egress.json). Dit bestand is slechts een voorbeeld. Configureer uw bestand naar wens op basis van de optionele/verplichte velden die zijn gedocumenteerd op [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   ```json
   {
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

   Voor elke `portForwards` afbeelding, bepaalt het geavanceerde voorzien van een netwerk de volgende het door:sturen regel:

   | Proxyhost | Proxypoort |  | Externe host | Externe poort |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   Als uw AEM __alleen__ vereist HTTP/HTTPS-verbindingen (poort 80/443) met de externe service, laat de `portForwards` array leeg, aangezien deze regels alleen vereist zijn voor niet-HTTP/HTTPS-aanvragen.

1. Voor elke omgeving valideert u de egress-regels die van kracht zijn met de API voor Cloud Manager [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) -bewerking.

   __getEnvironmentAdvancedNetworkingConfiguration HTTP-aanvraag__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. Flexibele poortegress-configuraties kunnen worden bijgewerkt met de Cloud Manager-API [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) -bewerking. Herinneren `enableEnvironmentAdvancedNetworkingConfiguration` is een `PUT` bewerking, zodat alle regels moeten worden voorzien van elke aanroep van deze bewerking.

1. Nu kunt u de flexibele configuratie van de havenuitgang in uw douane AEM code en configuratie gebruiken.


## Verbinding maken met externe services via flexibele poortuitgang

Met de flexibele toegelaten volmacht van de havenuitgang, AEM code en configuratie kunnen hen gebruiken om vraag aan externe diensten te maken. Er zijn twee vlotten van externe vraag die AEM verschillend behandelt:

1. HTTP/HTTPS-aanroepen naar externe services op niet-standaard poorten
   + Omvat HTTP/HTTPS vraag die aan de diensten wordt gemaakt die op havens buiten standaard 80 of 443 havens lopen.
1. niet-HTTP/HTTPS-aanroepen naar externe services
   + Omvat om het even welke niet-HTTP vraag, zoals verbindingen met de servers van de Post, SQL gegevensbestanden, of de diensten die op andere niet-HTTP/HTTPS protocollen lopen.

HTTP/HTTPS-aanvragen van AEM op standaardpoorten (80/443) zijn standaard toegestaan en hebben geen extra configuratie of overwegingen nodig.


### HTTP/HTTPS op niet-standaardpoorten

Bij het maken van HTTP/HTTPS-verbindingen met niet-standaardpoorten (niet-80/443) vanaf AEM, moeten de verbindingen tot stand worden gebracht via speciale host en poorten, die via plaatsaanduidingen worden geleverd.

AEM biedt twee sets speciale Java™-systeemvariabelen die zijn toegewezen aan AEM HTTP/HTTPS-proxy&#39;s.

| Naam variabele | Gebruiken | Java™-code | OSGi-configuratie |
| - |  - | - | - |
| `AEM_PROXY_HOST` | Proxyhost voor zowel HTTP/HTTPS-verbindingen | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |
| `AEM_HTTP_PROXY_PORT` | Proxypoort voor HTTPS-verbindingen (fallback instellen op `3128`) | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` |
| `AEM_HTTPS_PROXY_PORT` | Proxypoort voor HTTPS-verbindingen (fallback instellen op `3128`) | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

Bij het aanroepen van HTTP/HTTPS naar externe services op niet-standaardpoorten, is er geen corresponderende `portForwards` moet worden gedefinieerd met de API voor cloud Manager `enableEnvironmentAdvancedNetworkingConfiguration` verrichting, aangezien de haven die &quot;regels&quot;door:sturen &quot;in code&quot;wordt bepaald.

>[!TIP]
>
> Zie de flexibele documentatie van de havenuitgang van AEM as a Cloud Service voor [de volledige reeks verpletterende regels](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress-traffic-routing).

#### Codevoorbeelden

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports-flexible-port-egress.md"><img alt="HTTP/HTTPS op niet-standaardpoorten" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports-flexible-port-egress.md">HTTP/HTTPS op niet-standaardpoorten</a></strong></div>
    <p>
        Java™-codevoorbeeld waarbij een HTTP/HTTPS-verbinding van AEM as a Cloud Service wordt gemaakt met een externe service op niet-standaard HTTP/HTTPS-poorten.
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### Niet-HTTP/HTTPS-verbindingen met externe services

Bij het maken van niet-HTTP/HTTPS-verbindingen (bijvoorbeeld SQL, SMTP, etc.) van AEM, moet de verbinding door een speciale gastheernaam worden gemaakt die door AEM wordt verstrekt.

| Naam variabele | Gebruiken | Java™-code | OSGi-configuratie |
| - |  - | - | - |
| `AEM_PROXY_HOST` | Proxyhost voor niet-HTTP/HTTPS-verbindingen | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |


De verbindingen aan de externe diensten worden dan geroepen door `AEM_PROXY_HOST` en de toegewezen poort (`portForwards.portOrig`), die dan aan toegewezen externe hostname AEM leiden (`portForwards.name`) en poort (`portForwards.portDest`).

| Proxyhost | Proxypoort |  | Externe host | Externe poort |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Codevoorbeelden

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="SQL-verbinding met JDBC DataSourcePool" src="./assets/code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">SQL-verbinding met JDBC DataSourcePool</a></strong></div>
      <p>
            Java™ codevoorbeeld die met externe SQL gegevensbestanden verbinden door AEM JDBC datasource pool te vormen.
      </p>
    </td>   
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="SQL-verbinding met Java API&apos;s" src="./assets/code-examples__sql-java-api.png"/></a>
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
