---
title: Servicereferenties
description: AEM Service Credentials worden gebruikt om externe toepassingen, systemen en services te helpen programmatisch te communiceren met AEM Author of Publish services via HTTP.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: API's
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '1825'
ht-degree: 0%

---


# Servicereferenties

Integraties met AEM als Cloud Service moeten veilig kunnen worden geverifieerd op AEM. AEM Developer Console biedt toegang tot servicerecertificaten, die worden gebruikt om externe toepassingen, systemen en services te helpen programmatisch te communiceren met AEM Author Publish-services via HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

De Referenties van de dienst kunnen gelijkaardig [Lokale Tokens van de Toegang van de Ontwikkeling ](./local-development-access-token.md) verschijnen maar zijn verschillend op een paar zeer belangrijke manieren:

+ Servicedetecties zijn _geen_ toegangstokens, maar zijn referenties die worden gebruikt om _access tokens te verkrijgen_.
+ De Verantwoordelijkheden van de dienst zijn duurder (verlopen om de 365 dagen), en veranderen niet tenzij ingetrokken, terwijl de Lokale Tokens van de Toegang van de Ontwikkeling dagelijks verlopen.
+ De Verantwoordelijkheden van de dienst voor een AEM als milieukaart van de Cloud Service aan één enkele AEM technische rekeningsgebruiker, terwijl de Lokale Tokens van de Toegang van de Ontwikkeling voor authentiek verklaren als AEM gebruiker die het toegangstoken produceerde.

Zowel dienen de Verantwoordelijkheden van de Dienst en de toegangstoken zij produceren, evenals de Tokens van de Toegang van de Lokale Ontwikkeling geheim worden gehouden, aangezien alle drie kunnen worden gebruikt om toegang tot hun respectieve AEM als Cloud Service milieu&#39;s te verkrijgen

## Servicekredieten genereren

Het genereren van servicekredieten wordt in twee stappen opgedeeld:

1. Een eenmalige initialisatie van servicekredieten door een Adobe IMS-beheerder
1. Het downloaden en gebruiken van de Service Credentials JSON

### Initialisatie van servicekredieten

De Verantwoordelijkheden van de dienst, in tegenstelling tot de Tokens van de Toegang van de Lokale Ontwikkeling, vereisen een _eenmalige initialisering_ door uw beheerder van Adobe Org IMS alvorens zij kunnen worden gedownload.

![Servicekredieten initialiseren](assets/service-credentials/initialize-service-credentials.png)

__Dit is een eenmalige initialisatie per AEM als een Cloud Service-omgeving__

1. Zorg ervoor dat u bent aangemeld als beheerder van uw Adobe IMS Org
1. Aanmelden bij [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Open het Programma dat de AEM als omgeving van de Cloud Service bevat om opstelling de Referenties van de Dienst voor te integreren
1. Tik op de ellips naast de omgeving in de sectie __Omgevingen__ en selecteer __Developer Console__
1. Tik op de tab __Integraties__
1. Tik __Servicedesentials ophalen__-knop
1. De servicekredieten worden geïnitialiseerd en weergegeven als JSON

![AEM Developer Console - Integratie - Inschrijvingen voor service](./assets/service-credentials/developer-console.png)

Zodra de AEM als de Referentials van de Dienst van het milieu van de Cloud Service zijn geïnitialiseerd, kunnen andere AEM ontwikkelaars in uw Adobe IMS Org hen downloaden.

### Referenties van service downloaden

![Referenties van service downloaden](assets/service-credentials/download-service-credentials.png)

Voor het downloaden van de servicekredieten gelden dezelfde stappen als voor de initialisatie. Als de initialisatie nog niet is opgetreden, wordt een fout weergegeven met het tikken op de knop __Servicereferenties ophalen__.

1. Zorg ervoor dat u lid bent van het __Cloud Manager - Developer__ IMS-productprofiel (dat toegang biedt tot AEM Developer Console)
   + Voor AEM als Cloud Service is alleen lidmaatschap vereist in de __AEM Beheerders__ of __AEM Gebruikers__ Productprofiel
1. Aanmelden bij [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Open het programma dat de AEM bevat als een Cloud Service-omgeving om te integreren met
1. Tik op de ellips naast de omgeving in de sectie __Omgevingen__ en selecteer __Developer Console__
1. Tik op de tab __Integraties__
1. Tik __Servicedesentials ophalen__-knop
1. Tik op de downloadknop in de linkerbovenhoek om het JSON-bestand met de waarde Servicekredieten te downloaden en het bestand op een veilige locatie op te slaan.
   + _Als de Credentials van de Dienst worden gecompromitteerd, contacteer onmiddellijk de Steun van Adobe om hen te hebben herroepen_

## De servicereferenties installeren

De geloofsbrieven van de Dienst verstrekken de details nodig om een JWT te produceren, die voor een toegangstoken wordt geruild dat wordt gebruikt om met AEM als Cloud Service voor authentiek te verklaren. De servicekredieten moeten worden opgeslagen op een beveiligde locatie die toegankelijk is voor externe toepassingen, systemen of services die deze gebruiken voor toegang tot AEM. Hoe en waar de Credentials van de Dienst worden beheerd zal uniek per klant zijn.

Voor eenvoud, gaat dit leerprogramma de Credentials van de Dienst binnen via de bevellijn over, echter, werk met uw team van de Veiligheid van IT om te begrijpen hoe te om tot deze geloofsbrieven in overeenstemming met de veiligheidsrichtlijnen van uw organisatie op te slaan en toegang te hebben.

1. Kopieer [de Service Credentials JSON](#download-service-credentials) naar een bestand met de naam `service_token.json` in de hoofdmap van het project
   + Maar vergeet niet dat u nooit aanmeldgegevens aan Git wilt toewijzen!

## Servicereferenties gebruiken

De servicekredieten, een volledig samengesteld JSON-object, zijn niet hetzelfde als de JWT of het toegangstoken. In plaats daarvan worden de Referenties van de Dienst (die een privé sleutel bevatten) gebruikt om JWT te produceren, die met Adobe IMS APIs voor een toegangstoken wordt geruild.

![Servicereferenties - externe toepassing](assets/service-credentials/service-credentials-external-application.png)

1. Download de servicedesentials van AEM Developer Console naar een beveiligde locatie
1. Een externe toepassing moet programmatisch communiceren met AEM als een Cloud Service-omgeving
1. De externe toepassing leest in de Referenties van de Dienst van een veilige plaats
1. De externe toepassing gebruikt informatie van de Referenties van de Dienst om een Token te construeren JWT
1. De JWT Token wordt verzonden naar Adobe IMS om voor een toegangstoken te ruilen
1. Adobe IMS keert een toegangstoken terug dat kan worden gebruikt om tot AEM als Cloud Service toegang te hebben
   + Voor toegangstokens kan een vervaldatum worden aangevraagd. Het is best om het leven van het toegangstoken kort te houden, en te verfrissen wanneer nodig.
1. De externe toepassing doet HTTP-verzoeken om als Cloud Service te AEM, waarbij het toegangstoken als een token Drager wordt toegevoegd aan de header HTTP-aanvragen voor autorisatie.
1. AEM als Cloud Service ontvangt het HTTP- verzoek, verklaart het verzoek voor authentiek, en voert het werk uit dat door het HTTP- verzoek wordt gevraagd, en keert een reactie van HTTP terug naar de Externe Toepassing

### Updates voor de externe toepassing

Om tot AEM als Cloud Service toegang te hebben die de Referenties van de Dienst gebruiken moet onze externe toepassing op drie manieren worden bijgewerkt:

1. Lees in de Referenties van de Dienst
   + Voor eenvoud, zullen wij deze van het gedownloade JSON dossier lezen, nochtans in real-use scenario&#39;s, moeten de Referenties van de Dienst veilig volgens de veiligheidsrichtlijnen van uw organisatie worden opgeslagen
1. Een JWT genereren op basis van de servicerecertificaten
1. Uitwisseling JWT voor een toegangstoken
   + Wanneer de Referenties van de Dienst aanwezig zijn, gebruikt onze externe toepassing dit toegangstoken in plaats van het Lokale Token van de Toegang van de Ontwikkeling, wanneer de toegang tot van AEM als Cloud Service

In deze zelfstudie wordt de Adobe `@adobe/jwt-auth` npm module gebruikt om (1) JWT van de Referenties van de Dienst te produceren, en (2) het voor een toegangstoken, in één enkele functievraag te ruilen. Als uw toepassing niet op JavaScript is gebaseerd, te herzien [steekproefcode in andere talen](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) voor hoe te om JWT van de Referenties van de Dienst tot stand te brengen, en het voor een toegangstoken met Adobe IMS uit te wisselen.

## Lees de Referenties van de Dienst

Controleer `getCommandLineParams()` en zie dat wij in de dossiers kunnen lezen van de Credentials JSON van de Dienst gebruikend de zelfde code die wordt gebruikt om in het Token JSON van de Toegang van de Lokale Ontwikkeling te lezen.

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

Zodra de Credentials van de Dienst worden gelezen, worden zij gebruikt om JWT te produceren die dan met Adobe IMS APIs voor een toegangstoken wordt geruild, die dan kan worden gebruikt om tot AEM als Cloud Service toegang te hebben.

Deze voorbeeldtoepassing is gebaseerd op Node.js, dus is het beter om [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm module te gebruiken om de (1) generatie van JWT en (20 uitwisseling met Adobe IMS te vergemakkelijken. Als uw toepassing wordt ontwikkeld gebruikend een andere taal gelieve [de aangewezen codesteekproeven](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) op hoe te om de HTTP- aanvraag aan Adobe IMS te construeren gebruikend andere programmeertalen.

1. Werk `getAccessToken(..)` bij om de JSON dossierinhoud te inspecteren en te bepalen als het een Lokaal Token van de Toegang van de Ontwikkeling of de Referenties van de Dienst vertegenwoordigt. Dit kan gemakkelijk worden bereikt door het bestaan van het `.accessToken` bezit te controleren, dat slechts voor de Token JSON van de Toegang van de Lokale Ontwikkeling bestaat.

   Als Service Credentials is opgegeven, genereert de toepassing een JWT en ruilt deze met Adobe IMS voor een toegangstoken. We gebruiken de [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) functie die een JWT genereert en deze in één functieaanroep ruilt voor een toegangstoken.  `auth(...)`  De parameters voor `auth(..)` zijn een [JSON-object dat bestaat uit specifieke informatie](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) die beschikbaar is via de Service Credentials JSON, zoals hieronder in de code wordt beschreven.

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

   Afhankelijk van welk JSON-bestand (het Local Development Access Token JSON of de Service Credentials JSON) wordt doorgegeven via die opdrachtregelparameter `file`, leidt de toepassing een toegangstoken af.

   Herinner me, dat terwijl de Referenties van de Dienst niet verlopen, JWT en het overeenkomstige toegangstoken doen, en moeten worden verfrist alvorens zij verlopen. Dit kan worden gedaan door `refresh_token` [te gebruiken die door Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens) wordt verstrekt.

1. Als deze wijzigingen zijn aangebracht en de Service Credentials JSON is gedownload van de AEM Developer Console (en voor de eenvoud is deze opgeslagen als `service_token.json` dezelfde map als deze `index.js`), voert u de toepassing uit waarbij de opdrachtregelparameter `file` wordt vervangen door `service_token.json` en werkt u `propertyValue` bij naar een nieuwe waarde zodat de effecten zichtbaar zijn in AEM.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   De output aan de terminal zal als kijken:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   De regels __403 - Forbidden__ geven fouten aan in de HTTP API-aanroepen die als Cloud Service moeten worden AEM. Deze 403 Verboden fouten treden op wanneer wordt geprobeerd de metagegevens van de elementen bij te werken.

   De reden voor dit is het de toegangstoken van Credentials-Afgeleide van de Dienst verklaart het verzoek om AEM gebruikend een auto-gecreeerde technische rekening AEM gebruiker voor authentiek, die door gebrek, slechts gelezen toegang heeft. Om de toepassing te verstrekken schrijf toegang tot AEM, moet de technische rekening AEM gebruiker verbonden aan het toegangstoken toestemming in AEM worden verleend.

## Toegang configureren in AEM

Het toegangstoken op basis van servicekredieten gebruikt een technische account AEM gebruiker die lidmaatschap heeft in de gebruikersgroep Medewerkers AEM.

![Servicekredieten - Technische account AEM gebruiker](./assets/service-credentials/technical-account-user.png)

Zodra de technische rekening AEM gebruiker in AEM (na eerste HTTP- verzoek met het toegangstoken) bestaat, kunnen de toestemmingen van deze AEM gebruiker het zelfde als andere AEM gebruikers worden beheerd.

1. Zoek eerst de aanmeldnaam van de AEM van de technische account door de servicerecertificaten te openen die JSON van AEM Developer Console heeft gedownload en zoek de waarde `integration.email`, die er ongeveer als volgt moet uitzien: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Meld u als AEM beheerder aan bij de bijbehorende AEM-service Auteur
1. Navigeer naar __Gereedschappen__ > __Beveiliging__ > __Gebruikers__
1. Zoek de AEM gebruiker met de __Aanmeldingsnaam__ in Stap 1 en open zijn __Eigenschappen__
1. Navigeer naar het tabblad __Groepen__ en voeg de groep __DAM Users__ toe (die als schrijftoegang tot elementen)
1. Tik __Opslaan en sluiten__

Voer de toepassing opnieuw uit terwijl de technische account in AEM schrijfmachtigingen voor elementen heeft:

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

De output aan de terminal zal als kijken:

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## Wijzigingen controleren

1. Log in de AEM als een Cloud Service-omgeving die is bijgewerkt (met dezelfde hostnaam die is opgegeven in de opdrachtregelparameter `aem`)
1. Navigeer naar __Middelen__ > __Bestanden__
1. Navigeer in de elementenmap die wordt aangegeven door de opdrachtregelparameter `folder`, bijvoorbeeld __WKND__ > __English__ > __Adventures__ > __Napa Wine Tasting__
1. Open __Eigenschappen__ voor om het even welk element in de omslag
1. Navigeer naar het tabblad __Geavanceerd__
1. Controleer de waarde van de bijgewerkte eigenschap, bijvoorbeeld __Copyright__ die is toegewezen aan de bijgewerkte JCR-eigenschap `metadata/dc:rights`, die nu de waarde weerspiegelt die is opgegeven in de parameter `propertyValue`, bijvoorbeeld __Beperkt gebruik WKND__

![Update van metagegevens voor beperkt gebruik van WKND](./assets/service-credentials/asset-metadata.png)

## Gefeliciteerd!

Nu wij programmatically AEM als Cloud Service gebruikend een lokaal toegangstoken van de ontwikkelingstoegang, evenals een productie-klaar dienst-aan-dienst toegangstoken hebben betreden!

