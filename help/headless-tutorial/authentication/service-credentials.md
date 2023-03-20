---
title: Servicegegevens
description: Leer hoe te om de Referenties van de Dienst te gebruiken die worden gebruikt om externe toepassingen, systemen, en de diensten te vergemakkelijken om programmatically met de auteur of de Publish diensten over HTTP in wisselwerking te staan.
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
exl-id: e2922278-4d0b-4f28-a999-90551ed65fb4
source-git-commit: 1401710c19ae6ee6a2822ae06286bef4f92cda45
workflow-type: tm+mt
source-wordcount: '1937'
ht-degree: 0%

---

# Servicegegevens

Integraties met as a Cloud Service Adobe Experience Manager (AEM) moeten veilig kunnen worden geverifieerd voor AEM service. AEM Developer Console biedt toegang tot servicerecertificaten, die worden gebruikt om externe toepassingen, systemen en services te helpen programmatisch te communiceren met AEM Author Publish-services via HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

Servicereferenties kunnen er ongeveer zo uitzien [Tokens voor toegang tot lokale ontwikkeling](./local-development-access-token.md) maar zijn op een aantal belangrijke punten verschillend :

+ Servicekredieten zijn gekoppeld aan technische accounts. De veelvoudige de dienstgeloofsbrieven kunnen voor een Technische Rekening actief zijn.
+ Servicekredieten zijn _niet_ toegangstokens, eerder zijn zij geloofsbrieven die worden gebruikt aan _verkrijgen_ toegang tot tokens.
+ De geloofsbrieven van de dienst zijn duurder (hun certificaat verloopt om de 365 dagen), en veranderen niet tenzij ingetrokken, terwijl de Lokale Tokens van de Toegang van de Ontwikkeling dagelijks verlopen.
+ De Verantwoordelijkheden van de dienst voor een AEM as a Cloud Service milieukaart aan één enkele AEM technische rekeningsgebruiker, terwijl de Tokens van de Toegang van de Toegang van de Lokale Ontwikkeling voor authentiek verklaren als AEM gebruiker die het toegangstoken produceerde.
+ Een AEM as a Cloud Service omgeving kan maximaal tien technische accounts hebben, elk met hun eigen servicekredieten, elke toewijzing aan een afzonderlijke technische rekening AEM gebruiker.

Zowel dienen de Geloofsbrieven van de Dienst als de toegangstoken zij produceren, en de Lokale Tokens van de Toegang van de Ontwikkeling, geheim worden gehouden. Aangezien alle drie kunnen worden gebruikt om, toegang tot hun respectieve AEM as a Cloud Service milieu te verkrijgen.

## Servicekredieten genereren

Het genereren van servicekredieten wordt in twee stappen opgedeeld:

1. Een eenmalige technische account aanmaken door een Adobe IMS Org-beheerder
1. Het downloaden en gebruiken van JSON (Service Credentials) van de technische account

### Een technische account maken

In tegenstelling tot Local Development Access Tokens moeten servicerecertificaten door een Adobe Org IMS-beheerder worden aangemaakt voordat ze kunnen worden gedownload. De afzonderlijke Technische Rekeningen zouden voor elke cliënt moeten worden gecreeerd die programmatic toegang tot AEM vereist.

![Een technische account maken](assets/service-credentials/initialize-service-credentials.png)

De technische Rekeningen worden gecreeerd eens, nochtans het gebruik van Privé Sleutels om de Verantwoordelijkheden van de Dienst verbonden aan de Technische Rekening te beheren kan in tijd worden beheerd. Bijvoorbeeld, moeten de nieuwe Persoonlijke Sleutel/de Credentials van de Dienst vóór het verstrijken van de huidige Privé Sleutel worden geproduceerd, om voor ononderbroken toegang door een gebruiker van de Credentials van de Dienst toe te staan.

1. Controleer of u bent aangemeld als een:
   + __Beheerder van Adobe IMS Org__
   + Lid van de __AEM__ IMS-productprofiel op __AEM-auteur__
1. Aanmelden bij [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Open het programma dat de AEM as a Cloud Service omgeving bevat om de instelling van de servicekredieten voor
1. Tik op de ellips naast de omgeving in het dialoogvenster __Omgevingen__ en selecteert u __Ontwerpconsole__
1. Tik in het dialoogvenster __Integraties__ tab
1. Tik op de knop __Technische rekeningen__ tab
1. Tikken __Nieuwe technische account maken__ knop
1. De servicekredieten van de technische account worden geïnitialiseerd en weergegeven als JSON

![AEM Developer Console - Integratie - Inschrijvingen voor service](./assets/service-credentials/developer-console.png)

Nadat de AEM als servicegegevens van de Cloud Service-omgeving zijn geïnitialiseerd, kunnen andere AEM ontwikkelaars van uw Adobe IMS-organisatie deze downloaden.

### Referenties van service downloaden

![Referenties van service downloaden](assets/service-credentials/download-service-credentials.png)

Het downloaden van de Referentieformals van de Dienst volgt de gelijkaardige stappen zoals de initialisering.

1. Controleer of u bent aangemeld als een:
   + __Beheerder van Adobe IMS Org__
   + Lid van de __AEM__ IMS-productprofiel op __AEM-auteur__
1. Aanmelden bij [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Open het programma dat de AEM as a Cloud Service omgeving bevat om mee te integreren
1. Tik op de ellips naast de omgeving in het dialoogvenster __Omgevingen__ en selecteert u __Ontwerpconsole__
1. Tik in het dialoogvenster __Integraties__ tab
1. Tik op de knop __Technische rekeningen__ tab
1. Breid uit __Technische rekening__ te gebruiken
1. Breid uit __Persoonlijke sleutel__ wiens servicekredieten worden gedownload en controleren of de status __Actief__
1. Tik op de knop __...__ > __Weergave__ in verband met de __Persoonlijke sleutel__, die de Service Credentials JSON weergeeft
1. Tik op de downloadknop in de linkerbovenhoek om het JSON-bestand met de waarde Servicekredieten te downloaden en het bestand op een veilige locatie op te slaan

## De servicereferenties installeren

De referenties van de Dienst verstrekken de details nodig om een JWT te produceren, die voor een toegangstoken wordt geruild dat wordt gebruikt om met AEM as a Cloud Service voor authentiek te verklaren. De servicekredieten moeten worden opgeslagen op een beveiligde locatie die toegankelijk is voor externe toepassingen, systemen of services die deze gebruiken voor toegang tot AEM. Hoe en waar de Credentials van de Dienst worden beheerd zijn uniek per klant.

Voor eenvoud, gaat dit leerprogramma de Credentials van de Dienst binnen via de bevellijn over. Werk echter samen met uw IT-beveiligingsteam om te begrijpen hoe deze gegevens moeten worden opgeslagen en geopend in overeenstemming met de beveiligingsrichtlijnen van uw organisatie.

1. Kopieer de [De Service Credentials JSON downloaden](#download-service-credentials) naar een bestand met de naam `service_token.json` in de basis van het project
   + Onthouden, nooit vastleggen _om het even welke geloofsbrieven_ naar Git!

## Servicereferenties gebruiken

De servicekredieten, een volledig samengesteld JSON-object, zijn niet hetzelfde als de JWT of het toegangstoken. In plaats daarvan worden de servicekredieten (die een persoonlijke sleutel bevatten) gebruikt om een JWT te genereren, die wordt uitgewisseld met Adobe IMS API&#39;s voor een toegangstoken.

![Servicereferenties - externe toepassing](assets/service-credentials/service-credentials-external-application.png)

1. Download de servicedesentials van AEM Developer Console naar een beveiligde locatie
1. De externe toepassing moet programmatisch communiceren met AEM as a Cloud Service omgeving
1. De externe toepassing leest in de Referenties van de Dienst van een veilige plaats
1. De externe toepassing gebruikt informatie van de Referenties van de Dienst om een Token te construeren JWT
1. De JWT Token wordt naar Adobe IMS verzonden voor uitwisseling voor een toegangstoken
1. Adobe IMS retourneert een toegangstoken dat kan worden gebruikt voor toegang tot AEM as a Cloud Service
   + Voor toegangstokens kan een vervaldatum worden aangevraagd. Het is best om het leven van het toegangstoken kort te houden, en te verfrissen wanneer nodig.
1. De externe toepassing maakt HTTP-aanvragen om as a Cloud Service te AEM, waarbij het toegangstoken als een token voor Drager wordt toegevoegd aan de header voor HTTP-aanvragen.
1. AEM as a Cloud Service ontvangt het HTTP- verzoek, verklaart het verzoek voor authentiek, en voert het werk uit dat door het HTTP- verzoek wordt gevraagd en keert een reactie van HTTP terug naar de Externe Toepassing

### Updates voor de externe toepassing

Om tot AEM as a Cloud Service toegang te hebben gebruikend de Referenties van de Dienst, moet de externe toepassing op drie manieren worden bijgewerkt:

1. Lees in de Referenties van de Dienst

+ Voor eenvoud, worden de Credentials van de Dienst gelezen van het gedownloade JSON dossier, nochtans in real-use scenario&#39;s, moeten de Referenties van de Dienst veilig worden opgeslagen in overeenstemming met de veiligheidsrichtlijnen van uw organisatie

1. Een JWT genereren op basis van de servicerecertificaten
1. Uitwisseling JWT voor een toegangstoken

+ Wanneer de Referenties van de Dienst aanwezig zijn, gebruikt de externe toepassing dit toegangstoken in plaats van het Lokale Token van de Toegang van de Ontwikkeling, wanneer de toegang tot AEM as a Cloud Service

In deze zelfstudie kiest u Adobe `@adobe/jwt-auth` De npm module wordt gebruikt aan allebei, (1) produceert JWT van de Referenties van de Dienst, en (2) ruilt het voor een toegangstoken, in één enkele functievraag. Als uw toepassing niet is gebaseerd op JavaScript, raadpleegt u de [voorbeeldcode in andere talen](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/) voor hoe u een JWT maakt van de servicekredieten en deze uitwisselt voor een toegangstoken met Adobe IMS.

## Lees de Referenties van de Dienst

Controleer de `getCommandLineParams()` Zo zie hoe het dossier van de Verantwoordelijkheid JSON van de Dienst gebruikend de zelfde code wordt gelezen die wordt gebruikt om in het Lokale Token JSON van de Toegang van de Ontwikkeling te lezen.

```javascript
function getCommandLineParams() {
    ...

    // Read in the credentials from the provided JSON file
    // Since both the Local Development Access Token and Service Credentials files are JSON, this same approach can be re-used
    if (parameters.file) {
        parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
    }

    ...
    return parameters;
}
```

## Creeer JWT en ruil voor een Token van de Toegang

Nadat de servicekredieten zijn gelezen, worden deze gebruikt om een JWT te genereren die vervolgens wordt uitgewisseld met Adobe IMS API&#39;s voor een toegangstoken. Dit toegangstoken kan dan worden gebruikt om tot AEM as a Cloud Service toegang te hebben.

Deze voorbeeldtoepassing is gebaseerd op Node.js, dus is het beter om te gebruiken [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm-module om de (1) JWT-generatie en (20-uitwisseling met Adobe IMS te vergemakkelijken. Als uw toepassing is ontwikkeld in een andere taal, kunt u het beste [de passende codevoorbeelden](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/) over het samenstellen van de HTTP-aanvraag bij Adobe IMS met behulp van andere programmeertalen.

1. Werk de `getAccessToken(..)` om de inhoud van het JSON-bestand te inspecteren en vast te stellen of dit een token of servicekrediet voor lokale ontwikkelingstoegang vertegenwoordigt. Dit kan eenvoudig worden bereikt door na te gaan of het `.accessToken` eigenschap, die alleen bestaat voor Local Development Access Token JSON.

   Als Service Credentials is opgegeven, genereert de toepassing een JWT en wisselt deze uit met Adobe IMS voor een toegangstoken. Gebruik de [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)s `auth(...)` functie die een JWT genereert en deze in één functieaanroep ruilt voor een toegangstoken. De parameters die `auth(..)` methode is [JSON-object met specifieke informatie](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) beschikbaar bij Service Credentials JSON, zoals hieronder in code wordt beschreven.

```javascript
 async function getAccessToken(developerConsoleCredentials) {

     if (developerConsoleCredentials.accessToken) {
         // This is a Local Development access token
         return developerConsoleCredentials.accessToken;
     } else {
         // This is the Service Credentials JSON object that must be exchanged with Adobe IMS for an access token
         let serviceCredentials = developerConsoleCredentials.integration;

         // Use the @adobe/jwt-auth library to pass the service credentials generated a JWT and exchange that with Adobe IMS for an access token.
         // If other programming languages are used, please see these code samples: https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md
         let { access_token } = await auth({
             clientId: serviceCredentials.technicalAccount.clientId, // Client Id
             technicalAccountId: serviceCredentials.id,              // Technical Account Id
             orgId: serviceCredentials.org,                          // Adobe IMS Org Id
             clientSecret: serviceCredentials.technicalAccount.clientSecret, // Client Secret
             privateKey: serviceCredentials.privateKey,              // Private Key to sign the JWT
             metaScopes: serviceCredentials.metascopes.split(','),   // Meta Scopes defining level of access the access token should provide
             ims: `https://${serviceCredentials.imsEndpoint}`,       // IMS endpoint used to obtain the access token from
         });

         return access_token;
     }
 }
```

    Afhankelijk van welk JSON-bestand (de Local Development Access Token JSON of de Service Credentials JSON) wordt doorgegeven via de opdrachtregelparameter &#39;file`, leidt de toepassing een toegangstoken af.
    
    Herinner me, dat terwijl de Referenties van de Dienst elke 365 dagen verlopen, JWT en het overeenkomstige toegangstoken vaak verlopen, en moeten worden verfrist alvorens zij verlopen. Dit kan worden gedaan door ` te gebruiken verfrist_token ` [verstrekt door Adobe IMS] (https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens).

1. Met deze wijzigingen is JSON-servicerecertificaten gedownload van de AEM Developer Console en voor eenvoud opgeslagen als `service_token.json` in dezelfde map als deze `index.js`. Laten we nu de toepassing uitvoeren die de opdrachtregelparameter vervangt `file` with `service_token.json`en de `propertyValue` aan een nieuwe waarde zodat de gevolgen duidelijk in AEM zijn.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   De output aan de terminal kijkt als:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   De __403 - Verboden__ lijnen, wijzen op fouten in de HTTP API vraag aan AEM as a Cloud Service. Deze 403 Verboden fouten treden op wanneer wordt geprobeerd de metagegevens van de elementen bij te werken.

   De reden voor dit is het de toegangstoken van Credentials-Afgeleide van de Dienst verklaart het verzoek om AEM gebruikend een auto-gecreeerde technische rekening AEM gebruiker voor authentiek, die door gebrek, slechts gelezen toegang heeft. Om de toepassing te verstrekken schrijf toegang tot AEM, moet de technische rekening AEM gebruiker verbonden aan het toegangstoken toestemming in AEM worden verleend.

## Toegang configureren in AEM

Het toegangstoken op basis van servicerecertificaten gebruikt een technische account AEM gebruiker die lid is van de __Medewerkers__ AEM gebruikersgroep.

![Servicekredieten - Technische account AEM gebruiker](./assets/service-credentials/technical-account-user.png)

Zodra de technische rekening AEM gebruiker in AEM (na eerste HTTP- verzoek met het toegangstoken) bestaat, kunnen de toestemmingen van deze AEM gebruiker het zelfde als andere AEM gebruikers worden beheerd.

1. Zoek eerst de aanmeldnaam van de AEM van de technische account door de Service Credentials te openen die JSON van AEM Developer Console heeft gedownload, en zoek de `integration.email` waarde, die er ongeveer als volgt uitziet: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Meld u als AEM beheerder aan bij de bijbehorende AEM-service Auteur
1. Navigeren naar __Gereedschappen__ > __Beveiliging__ > __Gebruikers__
1. Zoek de AEM gebruiker met de __Aanmeldingsnaam__ die in Stap 1 worden geïdentificeerd, en opent zijn __Eigenschappen__
1. Ga naar de __Groepen__ en voegt u de __DAM-gebruikers__ groep (die als schrijf toegang tot activa)
1. Tikken __Opslaan en sluiten__

Voer de toepassing opnieuw uit terwijl de technische account in AEM schrijfmachtigingen voor elementen mag hebben:

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd-shared/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

De output aan de terminal kijkt als:

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## Wijzigingen controleren

1. Meld u aan bij de AEM as a Cloud Service omgeving die is bijgewerkt (met dezelfde hostnaam die in het dialoogvenster `aem` opdrachtregelparameter)
1. Ga naar de __Activa__ > __Bestanden__
1. Navigeren in de elementenmap die is opgegeven in het dialoogvenster `folder` opdrachtregelparameter, bijvoorbeeld __WKND__ > __Engels__ > __avonturen__ > __Napa Wine Tasting__
1. Open de __Eigenschappen__ voor elk element in de map
1. Ga naar de __Geavanceerd__ tab
1. Controleer de waarde van de bijgewerkte eigenschap, bijvoorbeeld __Copyright__ die is toegewezen aan de bijgewerkte `metadata/dc:rights` JCR-eigenschap, die nu de waarde weerspiegelt die wordt verschaft in het dialoogvenster `propertyValue` parameter, bijvoorbeeld __Beperkt gebruik WKND__

![Update van metagegevens voor beperkt gebruik van WKND](./assets/service-credentials/asset-metadata.png)

## Gefeliciteerd!

Nu wij programmatically hebben betreden AEM as a Cloud Service gebruikend een lokaal toegangstoken van de ontwikkelingstoegang, en een productie-klaar dienst-aan-dienst toegangstoken!
