---
title: Formulierverzending opslaan in Azure Storage
description: Formuliergegevens opslaan in Azure Storage met REST API
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-10-23T00:00:00Z
jira: KT-14238
duration: 78
exl-id: 77f93aad-0cab-4e52-b0fd-ae5af23a13d0
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---

# Gegevens ophalen uit Azure-opslag

In dit artikel wordt uitgelegd hoe u een adaptief formulier kunt vullen met de gegevens die in Azure-opslag zijn opgeslagen.
Aangenomen wordt dat u de adaptieve formuliergegevens in Azure-opslag hebt opgeslagen en nu uw adaptieve formulier met die gegevens wilt vooraf invullen.
>[!NOTE]
>De code in dit artikel werkt niet met op kerncomponenten gebaseerde adaptieve formulieren.[ het gelijkwaardige artikel voor kern op component gebaseerde adaptieve vorm is hier beschikbaar ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/prefill-form-with-data-attachments/introduction.html?lang=nl-NL)


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

* [ specificeer de aangewezen waarden in de Azure PortalConfiguratie gebruikend de OSGi configuratieconsole.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/some-useful-integrations/store-form-data-in-azure-storage.html?lang=nl-NL#provide-the-blob-sas-token-and-storage-uri)

* [ Voorproef en voorlegt de vorm BankAccount ](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* Controleer of de gegevens zijn opgeslagen in de Azure-opslagcontainer van uw keuze. Kopieer de blob-id.

* [ Voorproef de vorm BankAccount ](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) en specificeer identiteitskaart van de Blob als begeleide parameter in URL voor de vorm die met de gegevens van Azure opslag moet worden vooraf bevolkt
