---
title: Formulierverzending opslaan in Azure Storage
description: Formuliergegevens opslaan in Azure Storage met REST API
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-08-14T00:00:00Z
kt: 13781
source-git-commit: 17f6148ce6f897052d9d13f23e3f1792646eb958
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# Formulierverzendingen opslaan in Azure Storage

Dit artikel laat zien hoe u REST-oproepen kunt maken om verzonden AEM Forms-gegevens op te slaan in Azure Storage.
Als u verzonden formuliergegevens wilt opslaan in Azure Storage, moet u de volgende stappen uitvoeren.

## Azure Storage-account maken

[Meld u aan bij uw Azure Portal-account en maak een opslagaccount](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#create-a-storage-account-1). Geef uw opslagaccount een betekenisvolle naam, klik op Revisie en klik vervolgens op Maken. Hiermee maakt u uw opslagaccount met alle standaardwaarden. Voor dit artikel hebben we onze opslagaccount benoemd `aemformstutorial`.

## Gedeelde toegang maken

Wij zullen ons van de Gedeelde Handtekening van de Toegang of SAS Methode van vergunning maken om met de Azure container van de Opslag in wisselwerking te staan.
Klik op de pagina met opslagaccounts in de portal op het menu-item Handtekening voor gedeelde toegang aan de linkerkant om de nieuwe pagina met instellingen voor de gedeelde toegangshandtekening te openen. Controleer of u de instellingen en de juiste einddatum opgeeft, zoals in de onderstaande schermafbeelding wordt getoond, en klik op de knop SAS genereren en de verbindingstekenreeks. Kopieer de SAS-url van de Blob-service. We gebruiken deze URL om onze HTTP-aanroepen te maken
![gedeelde toegang-sleutels](./assets/shared-access-signature.png)

## Container maken

Het volgende wat we moeten doen is een container maken om de gegevens van formulierverzendingen op te slaan.
Van de pagina van de opslagrekening, klik op het het menupunt van Containers op de linkerzijde en creeer een container genoemd `formssubmissions`. Zorg ervoor dat het openbare toegangsniveau aan privé wordt geplaatst
![container](./assets/new-container.png)

## Aanvraag PUT maken

De volgende stap bestaat uit het maken van een verzoek van een PUT om de ingediende formuliergegevens op te slaan in Azure Storage. We moeten de SAS-URL van de Blob-service wijzigen om de containernaam en de BLOB-id op te nemen in de URL. Elke formulierverzending moet worden geïdentificeerd met een unieke BLOB-id. De unieke BLOB-id wordt gewoonlijk in de code gemaakt en ingevoegd in de URL van het verzoek om PUT.
Hier volgt de gedeeltelijke URL van het verzoek om PUT. De `aemformstutorial` is de naam van de opslagaccount, formverzendingen is de container waarin de gegevens met een unieke BLOB-id worden opgeslagen. De rest van de URL blijft ongewijzigd.
https://aemformstutorial.blob.core.windows.net/formsubmissions/00cb1a0c-a891-4dd7-9bd2-67a22bef3b8b?...............

Hieronder vindt u een functie die wordt geschreven om de verzonden formuliergegevens op te slaan in Azure Storage met behulp van een aanvraag voor een PUT. Let op het gebruik van de containernaam en de uuid in de URL. U kunt een OSGi-service of een verkoopserver maken met de onderstaande voorbeeldcode en de formulierverzendingen opslaan in Azure Storage.

```java
 public String saveFormDatainAzure(String formData) {
        System.out.println("in SaveFormData!!!!!"+formData);
        org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        UUID uuid = UUID.randomUUID();
        
        String url = "https://aemformstutorial.blob.core.windows.net/formsubmissions/"+uuid.toString();
        url = url+"?sv=2022-11-02&ss=bf&srt=o&sp=rwdlaciytfx&se=2024-06-28T00:42:59Z&st=2023-06-27T16:42:59Z&spr=https&sig=v1MR%2FJuhEledioturDFRTd9e2fIDVSGJuAiUt6wNlkLA%3D";
        HttpPut httpPut = new HttpPut(url);
        httpPut.addHeader("x-ms-blob-type","BlockBlob");
        httpPut.addHeader("Content-Type","text/plain");
        try {
            httpPut.setEntity(new StringEntity(formData));
            CloseableHttpResponse response = httpClient.execute(httpPut);
            log.debug("Response code "+response.getStatusLine().getStatusCode());
        } catch (IOException e) {
            log.error("Error: "+e.getMessage());
            throw new RuntimeException(e);
        }
        return uuid.toString();


    }
```

## Opgeslagen gegevens in de container verifiëren

![form-data-in-container](./assets/form-data-in-container.png)





