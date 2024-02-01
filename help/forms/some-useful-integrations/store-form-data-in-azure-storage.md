---
title: Formulierverzending opslaan in Azure Storage
description: Formuliergegevens opslaan in Azure Storage met REST API
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-08-14T00:00:00Z
jira: KT-13781
exl-id: 2bec5953-2e0c-4ae6-ae98-34492d4cfbe4
duration: 159
source-git-commit: b1734f75bdda174788d880be28fa19f8e787af0a
workflow-type: tm+mt
source-wordcount: '601'
ht-degree: 0%

---

# Formulierverzendingen opslaan in Azure Storage

Dit artikel laat zien hoe u REST-oproepen kunt maken om verzonden AEM Forms-gegevens op te slaan in Azure Storage.
Als u verzonden formuliergegevens wilt opslaan in Azure Storage, moet u de volgende stappen uitvoeren.

>[!NOTE]
>De code in dit artikel werkt niet met op kerncomponenten gebaseerde adaptieve formulieren. [Het equivalente artikel voor het op kerncomponenten gebaseerde adaptieve formulier is hier beschikbaar](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/prefill-form-with-data-attachments/introduction.html?lang=en)


## Azure Storage-account maken

[Meld u aan bij uw Azure Portal-account en maak een opslagaccount](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#create-a-storage-account-1). Geef uw opslagaccount een betekenisvolle naam, klik op Revisie en klik vervolgens op Maken. Hiermee maakt u uw opslagaccount met alle standaardwaarden. Voor dit artikel hebben we onze opslagaccount benoemd `aemformstutorial`.


## Container maken

Het volgende wat we moeten doen is een container maken om de gegevens van formulierverzendingen op te slaan.
Van de pagina van de opslagrekening, klik op het het menupunt van Containers op de linkerzijde en creeer een container genoemd `formssubmissions`. Zorg ervoor dat het openbare toegangsniveau aan privé wordt geplaatst
![container](./assets/new-container.png)

## SAS maken op de container

Wij zullen ons van de Gedeelde Handtekening van de Toegang of SAS Methode van vergunning maken om met de Azure container van de Opslag in wisselwerking te staan.
Navigeer naar de container in de opslagaccount, klik op de ellips en selecteer de optie Generate SAS (SAS genereren), zoals weergegeven in de schermafbeelding
![sas-on-container](./assets/sas-on-container.png)
Zorg ervoor dat u de juiste machtigingen en de juiste einddatum opgeeft, zoals in de onderstaande schermafbeelding wordt weergegeven, en klik op SAS-token en URL genereren. Kopieer de Blob SAS-token en Blob SAS url. Wij zullen deze twee waarden gebruiken om onze HTTP- vraag te maken
![gedeelde toegang-sleutels](./assets/shared-access-signature.png)


## De BLOB SAS-token en opslag-URI opgeven

Om de code generischer te maken, kunnen de twee eigenschappen worden gevormd gebruikend de configuratie OSGi zoals hieronder getoond. De _**vormgeving**_ de naam van de opslagrekening is, _**formules**_ is de container waarin de gegevens worden opgeslagen.
Zorg dat / aan het einde van de opslaguri staat en dat het SAS-token begint met?
![osgi-configuratie](./assets/azure-portal-osgi-configuration.png)


## Aanvraag PUT maken

De volgende stap bestaat uit het maken van een verzoek van een PUT om de ingediende formuliergegevens op te slaan in Azure Storage. Elke formulierverzending moet worden geïdentificeerd met een unieke BLOB-id. De unieke BLOB-id wordt gewoonlijk in de code gemaakt en ingevoegd in de URL van het verzoek om PUT.
Hier volgt de gedeeltelijke URL van het verzoek om PUT. De `aemformstutorial` is de naam van de opslagaccount, formverzendingen is de container waarin de gegevens met een unieke BLOB-id worden opgeslagen. De rest van de URL blijft ongewijzigd.
https://aemformstutorial.blob.core.windows.net/formsubmissions/blobid/sastoken De volgende functie is geschreven om de ingediende formuliergegevens op te slaan in Azure Storage met behulp van een aanvraag voor een PUT. Let op het gebruik van de containernaam en de uuid in de URL. U kunt een OSGi-service of een verkoopserver maken met de onderstaande voorbeeldcode en de formulierverzendingen opslaan in Azure Storage.

```java
 public String saveFormDatainAzure(String formData) {
    log.debug("in SaveFormData!!!!!" + formData);
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    log.debug("The SAS Token is " + sasToken);
    log.debug("The Storage URL is " + storageURI);
    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    UUID uuid = UUID.randomUUID();
    String putRequestURL = storageURI + uuid.toString();
    putRequestURL = putRequestURL + sasToken;
    HttpPut httpPut = new HttpPut(putRequestURL);
    httpPut.addHeader("x-ms-blob-type", "BlockBlob");
    httpPut.addHeader("Content-Type", "text/plain");

    try {
        httpPut.setEntity(new StringEntity(formData));

        CloseableHttpResponse response = httpClient.execute(httpPut);
        log.debug("Response code " + response.getStatusLine().getStatusCode());
        if (response.getStatusLine().getStatusCode() == 201) {
            return uuid.toString();
        }
    } catch (IOException e) {
        log.error("Error: " + e.getMessage());
        throw new RuntimeException(e);
    }
    return null;

}
```

## Opgeslagen gegevens in de container verifiëren

![form-data-in-container](./assets/form-data-in-container.png)

## De oplossing testen

* [De aangepaste OSGi-bundel implementeren](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [Het aangepaste aangepaste formuliersjabloon en de paginacomponent die aan de sjabloon zijn gekoppeld importeren](./assets/store-and-fetch-from-azure.zip)

* [Het adaptieve voorbeeldformulier importeren](./assets/bank-account-sample-form.zip)

* [Specificeer de aangewezen waarden in de Azure Portal Configuratie gebruikend de OSGi configuratieconsole](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/some-useful-integrations/store-form-data-in-azure-storage.html?lang=en#provide-the-blob-sas-token-and-storage-uri)

* [Het bankrekeningformulier bekijken en verzenden](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* Controleer of de gegevens zijn opgeslagen in de Azure-opslagcontainer van uw keuze. Kopieer de blob-id.
* [Een voorbeeld weergeven van het formulier Bankrekening](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) en geef de Blob-id op als een guid-parameter in de URL voor het formulier dat vooraf moet worden ingevuld met de gegevens uit Azure-opslag

