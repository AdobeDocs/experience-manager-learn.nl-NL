---
title: IP-adres van specifiek egress
description: Leer hoe te opstelling en gebruik specifiek uitgangIP adres, dat uitgaande verbindingen van AEM toestaat om van specifieke IP afkomstig te zijn.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9351
thumbnail: KT-9351.jpeg
exl-id: 311cd70f-60d5-4c1d-9dc0-4dcd51cad9c7
last-substantial-update: 2024-04-26T00:00:00Z
duration: 891
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1370'
ht-degree: 0%

---

# IP-adres van specifiek egress

Leer hoe te opstelling en gebruik specifiek uitgangIP adres, dat uitgaande verbindingen van AEM toestaat om van specifieke IP afkomstig te zijn.

## Wat is specifiek IP adres van de uitgang?

Het specifieke IP adres van de uitgang staat verzoeken van AEM as a Cloud Service toe om een specifiek IP adres te gebruiken, dat de externe diensten toestaat om inkomende verzoeken door dit IP adres te filtreren. Als [&#x200B; flexibele uitgang havens &#x200B;](./flexible-port-egress.md), specifieke uitgang IP laat u op niet-standaardhavens binnendringen.

Een programma van Cloud Manager kan het type van a __enige__ netwerkinfrastructuur slechts hebben. Verzeker specifiek adres van uitgangIP het meest [&#x200B; aangewezen type van netwerkinfrastructuur &#x200B;](./advanced-networking.md) voor uw AEM as a Cloud Service alvorens de volgende bevelen uit te voeren.

>[!MORELIKETHIS]
>
> Lees de AEM as a Cloud Service [&#x200B; geavanceerde documentatie van de netwerkconfiguratie &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking) voor meer details op specifiek uitgangIP adres.

## Vereisten

Het volgende is vereist voor het instellen van een toegewezen IP-adres voor toegang via Cloud Manager API&#39;s:

+ Cloud Manager API met [&#x200B; de toestemmingen van de BedrijfsEigenaar van Cloud Manager &#x200B;](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Toegang tot [&#x200B; de authentificatiegeloofsbrieven van Cloud Manager API &#x200B;](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/create-api-integration/)
   + Organisatie-id (ook bekend als IMS Org ID)
   + Client-id (ook bekend als API-sleutel)
   + Toegangstoken (ook bekend als Dragertoken)
+ Cloud Manager-programma-id
+ De milieu-id&#39;s van Cloud Manager

Voor meer details [&#x200B; overzicht hoe te opstelling, vorm, en verkrijg de geloofsbrieven van de Manager van de Wolk API &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/extensibility/app-builder/server-to-server-auth), om hen te gebruiken om een Cloud Manager API vraag te maken.

Deze zelfstudie gebruikt `curl` om de Cloud Manager API-configuraties te maken. De opgegeven `curl` -opdrachten nemen een Linux/macOS-syntaxis aan. Als u de Windows-opdrachtregel gebruikt, vervangt u het teken voor het `\` regeleinde door `^` .

## IP-adres voor speciale uitgang inschakelen voor het programma

Begin door het specifieke uitgang IP adres op AEM as a Cloud Service toe te laten en te vormen.

>[!BEGINTABS]

>[!TAB Cloud Manager]

Het specifieke IP-adres van de uitgang kan worden ingeschakeld met Cloud Manager. In de volgende stappen wordt beschreven hoe u toegewezen IP-adres voor egress op AEM as a Cloud Service kunt inschakelen met behulp van de Cloud Manager.

1. Login aan [&#x200B; Adobe Experience Manager Cloud Manager &#x200B;](https://experience.adobe.com/cloud-manager/) als Bedrijfseigenaar van Cloud Manager.
1. Navigeer naar het gewenste programma.
1. In het linkermenu, navigeer aan __Diensten > de Infrastructuur van het Netwerk__.
1. Selecteer __toevoegen netwerkinfrastructuur__ knoop.

   ![&#x200B; voeg netwerkinfrastructuur &#x200B;](./assets/cloud-manager__add-network-infrastructure.png) toe

1. In __voeg de dialoog van de netwerkinfrastructuur__ toe, selecteer de __Specifieke IP van het uitgang adres__ optie, en selecteer het __Gebied__ om het specifieke uitgangIP adres tot stand te brengen.

   ![&#x200B; voeg specifiek uitgangIP adres &#x200B;](./assets/dedicated-egress-ip-address/select-type.png) toe

1. Selecteer __sparen__ om de toevoeging van het specifieke uitgangIP adres te bevestigen.

   ![&#x200B; bevestig specifieke uitgangIP adresverwezenlijking &#x200B;](./assets/dedicated-egress-ip-address/confirmation.png)

1. Wacht op de netwerkinfrastructuur die moet worden gecreeerd en als __Klaar__ worden gemerkt. Dit proces kan tot 1 uur duren.

   ![&#x200B; Dedicated egress IP status van de adresverwezenlijking &#x200B;](./assets/dedicated-egress-ip-address/ready.png)

Met het speciale IP-adres van de uitgang gemaakt, kunt u het nu configureren met de Cloud Manager API&#39;s, zoals hieronder beschreven.

>[!TAB  Cloud Manager APIs ]

Een specifiek IP-adres voor egress kan worden ingeschakeld met Cloud Manager API&#39;s. In de volgende stappen wordt beschreven hoe u het IP-adres van een specifiek egress-adres op AEM as a Cloud Service kunt inschakelen met de Cloud Manager API.


1. Eerst, bepaal het gebied waarin het Geavanceerde Voorzien van een netwerk nodig is, door de Cloud Manager API [&#x200B; listRegions &#x200B;](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) verrichting te gebruiken. `region name` is vereist om volgende Cloud Manager API-aanroepen te kunnen uitvoeren. Doorgaans wordt de regio waarin de productieomgeving zich bevindt, gebruikt.

   Vind het gebied van uw milieu van AEM as a Cloud Service in [&#x200B; Cloud Manager &#x200B;](https://my.cloudmanager.adobe.com) onder de [&#x200B; details van het milieu &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments). De gebiedsnaam die in Cloud Manager wordt getoond kan [&#x200B; aan de gebiedscode &#x200B;](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) worden in kaart gebracht die in Cloud Manager API wordt gebruikt.

   __listRegions HTTP- verzoek__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

2. Laat specifiek uitgangIP adres voor een Programma van Cloud Manager toe gebruikend Cloud Manager API [&#x200B; createNetworkInfrastructure &#x200B;](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) verrichting. Gebruik de juiste `region` -code die is verkregen via de Cloud Manager API `listRegions` -bewerking.

   __createNetworkInfrastructure HTTP- verzoek__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "dedicatedEgressIp", "region": "va7" }'
   ```

   Wacht 15 minuten op het Cloud Manager-programma om de netwerkinfrastructuur te voorzien.

3. Controleer dat het programma __specifieke uitgangIP adres__ configuratie gebruikend de Cloud Manager API [&#x200B; getNetworkInfrastructure &#x200B;](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) verrichting heeft gebeëindigd, gebruikend `id` teruggekeerd van het `createNetworkInfrastructure` HTTP- verzoek in de vorige stap.

   __getNetworkInfrastructure HTTP- verzoek__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Verifieer dat de reactie van HTTP a __status__ van __klaar__ bevat. Controleer de status om de paar minuten als u dat nog niet hebt gedaan.

Met het speciale IP-adres van de uitgang gemaakt, kunt u het nu configureren met de Cloud Manager API&#39;s, zoals hieronder beschreven.

>[!ENDTABS]


## Vorm specifieke IP van de uitgang adresvolmachten per milieu

1. Vorm de __specifieke IP adres van de uitgang__ configuratie op elk milieu van AEM as a Cloud Service gebruikend Cloud Manager API [&#x200B; enableEnvironmentAdvancedNetworkingConfiguration &#x200B;](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) verrichting.

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP- verzoek__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   Definieer de JSON-parameters in een `dedicated-egress-ip-address.json` en opgegeven voor krullen via `... -d @./dedicated-egress-ip-address.json` .

   [&#x200B; Download het voorbeeld specifiek-egress-ip-address.json &#x200B;](./assets/dedicated-egress-ip-address.json). Dit bestand is slechts een voorbeeld. Vorm uw dossier zoals vereist gebaseerd op de facultatieve/vereiste gebieden die bij [&#x200B; worden gedocumenteerd enableEnvironmentAdvancedNetworkingConfiguration &#x200B;](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org",
       ],
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.net",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   De specifieke handtekening van HTTP van de IP adresconfiguratie van uitgang verschilt slechts van [&#x200B; flexibele uitgang haven &#x200B;](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment) in die het ook de facultatieve `nonProxyHosts` configuratie steunt.

   `nonProxyHosts` verklaart een reeks gastheren waarvoor haven 80 of 443 door de standaard gedeelde IP adreswaaiers eerder dan specifieke uitgang IP zou moeten worden verpletterd. `nonProxyHosts` kan nuttig zijn aangezien het verkeer dat door gedeelde IPs wordt behandeld automatisch door Adobe wordt geoptimaliseerd.

   Voor elke `portForwards` afbeelding, bepaalt het geavanceerde voorzien van een netwerk de volgende het door:sturen regel:

   | Proxyhost | Proxypoort |  | Externe host | Externe poort |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. Voor elk milieu, bevestig de spelregels in feite gebruikend Cloud Manager API [&#x200B; getEnvironmentAdvancedNetworkingConfiguration &#x200B;](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) verrichting.

   __getEnvironmentAdvancedNetworkingConfiguration HTTP- verzoek__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. De specifieke IP van de uitgang adresconfiguraties kunnen worden bijgewerkt gebruikend Cloud Manager API [&#x200B; enableEnvironmentAdvancedNetworkingConfiguration &#x200B;](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) verrichting. Vergeet niet dat `enableEnvironmentAdvancedNetworkingConfiguration` een `PUT` -bewerking is. Alle regels moeten daarom bij elke aanroep van deze bewerking worden opgegeven.

1. Verkrijg het __specifieke adres van uitgang IP__ door een DNS Resolver (zoals [&#x200B; DNSChecker.org &#x200B;](https://dnschecker.org/)) op de gastheer te gebruiken: `p{programId}.external.adobeaemcloud.com`, of door `dig` van de bevellijn in werking te stellen.

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   Hostnaam kan niet zijn `pinged`, aangezien het een uitgang en _niet_ en toegang is.

   Merk op dat het specifieke IP adres van de uitgang door alle milieu&#39;s van AEM as a Cloud Service in het programma wordt gedeeld.

1. Nu kunt u het speciale IP-adres van de uitgang gebruiken in uw aangepaste AEM-code en -configuratie. Vaak wanneer het gebruiken van specifiek uitgangIP adres, de externe diensten AEM as a Cloud Service verbindt met worden gevormd om verkeer van dit specifieke IP adres slechts toe te staan.

## Verbinding maken met externe services via toegewezen IP-adres voor uitgang

Met het specifieke toegelaten adres van uitgang IP, kan de code en de configuratie van AEM specifieke uitgang IP gebruiken om vraag aan externe diensten te maken. Er zijn twee flavors van externe vraag die AEM verschillend behandelt:

1. HTTP/HTTPS-aanroepen naar externe services
   + Omvat HTTP/HTTPS vraag die aan de diensten wordt gemaakt die op havens buiten standaard 80 of 443 havens lopen.
1. niet-HTTP/HTTPS-aanroepen naar externe services
   + Omvat om het even welke niet-HTTP vraag, zoals verbindingen met de servers van de Post, SQL gegevensbestanden, of de diensten die op andere niet-HTTP/HTTPS protocollen lopen.

HTTP/HTTPS-aanvragen van AEM op standaardpoorten (80/443) zijn standaard toegestaan, maar gebruiken het toegewezen IP-adres van de uitgang niet als dit niet op de hieronder beschreven manier is geconfigureerd.

>[!TIP]
>
> Zie AEM as a Cloud Service het specifieke IP van de uitgang adresdocumentatie voor [&#x200B; de volledige reeks het verpletteren van regels &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking).


### HTTP/HTTPS

Wanneer u HTTP/HTTPS-verbindingen maakt vanuit AEM, worden bij gebruik van een toegewezen IP-adres voor toegang, HTTP/HTTPS-verbindingen automatisch vanuit AEM opgehaald met behulp van het toegewezen IP-adres voor toegang. Er is geen aanvullende code of configuratie vereist voor ondersteuning van HTTP/HTTPS-verbindingen, behalve het instellen van een specifiek IP-adres voor toegang tot geavanceerde netwerken.

#### Codevoorbeelden

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md"> HTTP/HTTPS </a></strong></div>
    <p>
        Java™-codevoorbeeld waarbij via het HTTP/HTTPS-protocol een HTTP/HTTPS-verbinding van AEM as a Cloud Service naar een externe service wordt gemaakt.
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
| `AEM_PROXY_HOST` | Proxyhost voor niet-HTTP/HTTPS-verbindingen | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


Verbindingen met externe diensten worden dan geroepen door `AEM_PROXY_HOST` en de in kaart gebrachte haven (`portForwards.portOrig`), die AEM dan aan toegewezen externe hostname (`portForwards.name`) en haven (`portForwards.portDest`) leidt.

| Proxyhost | Proxypoort |  | Externe host | Externe poort |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Codevoorbeelden

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="SQL-verbinding met JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md"> SQL verbinding gebruikend JDBC DataSourcePool </a></strong></div>
      <p>
            Java™ codevoorbeeld die met externe SQL gegevensbestanden verbinden door AEM te vormen JDBC datasource pool.
      </p>
    </td>   
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="SQL-verbinding met Java API&apos;s" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md"> SQL verbinding gebruikend Java™ APIs </a></strong></div>
      <p>
            Java™-codevoorbeeld voor verbinding met externe SQL-databases met SQL API's van Java™.
      </p>
    </td>   
   <td>
      <a  href="./examples/email-service.md"><img alt="Virtual Private Network (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md"> E-maildienst </a></strong></div>
      <p>
        OSGi-configuratievoorbeeld dat AEM gebruikt om verbinding te maken met externe e-mailservices.
      </p>
    </td>   
</tr></table>
