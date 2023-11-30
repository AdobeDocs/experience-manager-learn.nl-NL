---
title: Formulierverzending opslaan in Azure Storage
description: Formuliergegevens opslaan in Azure Storage met REST API
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-10-23T00:00:00Z
kt: 14238
source-git-commit: 23459de98420d2a489288df4a1b992c17d42972e
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 0%

---

# Gegevens ophalen uit Azure-opslag

In dit artikel wordt uitgelegd hoe u een adaptief formulier kunt vullen met de gegevens die in Azure-opslag zijn opgeslagen.
Aangenomen wordt dat u de adaptieve formuliergegevens in Azure-opslag hebt opgeslagen en nu het aangepaste formulier wilt vullen met die gegevens.

## GET-aanvraag maken

De volgende stap bestaat uit het schrijven van de code om de gegevens van de Azure Storage op te halen met behulp van de blobID. De volgende code is geschreven om de gegevens op te halen. De url werd geconstrueerd gebruikend de waarden sasToken en storageURI van de configuratie OSGi en blobID die tot de functie getBlobData wordt overgegaan

```java
 @Override
public String getBlobData(String blobID) {
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    log.debug("The SAS Token is " + sasToken);
    log.debug("The Storage URL is " + storageURI);
    String httpGetURL = storageURI + blobID;
    httpGetURL = httpGetURL + sasToken;
    HttpGet httpGet = new HttpGet(httpGetURL);

    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    CloseableHttpResponse httpResponse = null;
    try {
        httpResponse = httpClient.execute(httpGet);
        HttpEntity httpEntity = httpResponse.getEntity();
        String blobData = EntityUtils.toString(httpEntity);
        log.debug("The blob data I got was " + blobData);
        return blobData;

    } catch (ClientProtocolException e) {

        log.debug("Got Client Protocol Exception " + e.getMessage());
    } catch (IOException e) {

        log.debug("Got IOEXception " + e.getMessage());
    }

    return null;
}
```

Wanneer een adaptief formulier wordt weergegeven met een hulplijnparameter in de URL, haalt de aangepaste pagina-component die aan de sjabloon is gekoppeld het adaptieve formulier op en vult deze met de gegevens van Azure-opslag.
Hier volgt de code in de jsp van de pagina-component die aan de sjabloon is gekoppeld

```java
com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage azureStorage = sling.getService(com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage.class);


String guid = request.getParameter("guid");

if(guid!=null&&!guid.isEmpty())
{
    String dataXml = azureStorage.getBlobData(guid);
    slingRequest.setAttribute("data",dataXml);

}
```

## De oplossing testen

* [De aangepaste OSGi-bundel implementeren](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [Het aangepaste aangepaste formuliersjabloon en de paginacomponent die aan de sjabloon zijn gekoppeld importeren](./assets/store-and-fetch-from-azure.zip)

* [Het adaptieve voorbeeldformulier importeren](./assets/bank-account-sample-form.zip)

* Specificeer de aangewezen waarden in de Azure Portal Configuratie gebruikend de OSGi configuratieconsole.

* [Het bankrekeningformulier bekijken en verzenden](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* Controleer of de gegevens zijn opgeslagen in de Azure-opslagcontainer van uw keuze. Kopieer de blob-id.

* [Een voorbeeld weergeven van het formulier Bankrekening](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) en geef de Blob-id op als een guid-parameter in de URL voor het formulier dat vooraf moet worden ingevuld met de gegevens uit Azure-opslag

