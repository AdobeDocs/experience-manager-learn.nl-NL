---
title: DocAssurance API gebruiken
description: Voorbeeldcode om DocAssurance API aan te roepen met Apache HTTP Components in Java
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-15508
exl-id: 40617082-4d23-4c91-a016-2d947187052b
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---

# DocAssurance API gebruiken

De [ dienst DocAssurance ](https://developer.adobe.com/experience-manager-forms-cloud-service-developer-reference/api/docassurance/#tag/DocAssurance) verstrekt de capaciteit om diverse digitale handtekening of encryptieverrichtingen met de documenten van PDF uit te voeren, zoals het ondertekenen, certificatie, toevoeging van handtekeningsgebieden, encryptie, decryptie enz.
In dit artikel vindt u Java-codefragmenten waarmee u de API kunt gaan gebruiken. Het codefragment maakt gebruik van toegangstoken. [ Dit artikel verklaart de stappen nodig om toegangstoken ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/doc-gen-formscs/introduction) te produceren


<span class="preview"> Deze eigenschap is beschikbaar onder vroege adopterprogramma. U kunt aan aem-forms-ea@adobe.com van uw officiële e-mailidentiteitskaart schrijven om zich bij het vroege adopterprogramma aan te sluiten en toegang tot deze functionaliteit te verzoeken </span>


## Vereisten

* Ervaring met AEM Forms Cloud Service
* Ervaring in het gebruiken van [ de Componenten van HTTP Apache ](https://hc.apache.org/httpcomponents-client-4.5.x/)
* Toegang tot de omgeving van AEM Forms Cloud Service

## Inspect-document

Gebruik de API voor controle om het type beveiliging op te halen voor een bepaald PDF-document. Het volgende codefragment moet u helpen aan de slag te gaan.

```java
...
File fileToInspect = new File("path_to_your_pdf_file)";
HttpPost httpPost = new HttpPost("<your_aem_forms_instance>/adobe/forms/document/assure/inspect");
httpPost.addHeader("Authorization", "Bearer " + accessToken);
MultipartEntityBuilder builder = MultipartEntityBuilder.create();
byte[] fileContent = FileUtils.readFileToByteArray(fileToInspect);
builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
try
{
    HttpEntity entity = builder.build();
    httpPost.setEntity(entity);
    CloseableHttpClient httpclient = HttpClients.createDefault();
    CloseableHttpResponse response = httpclient.execute(httpPost);
    if (response.getStatusLine().getStatusCode() == 200)   
    {
        String json = EntityUtils.toString(response.getEntity(), StandardCharsets.UTF_8);
        log.info("The mode of encryption is  " + JsonParser.parseString(json).getAsJsonObject().get("mode").getAsString());
    }

} 
catch (Exception e)
{
   log.error(e.getMessage());
}
...
```


## Document versleutelen

Met de API voor versleutelen versleutelt u PDF-documenten met een wachtwoord. Het volgende codefragment van de steekproefcode codeert een bepaalde PDF.

```java
...
File fileToEncrypt = new File("path_to_your_pdf_file");
HttpPost httpPost = new HttpPost(postURL);
httpPost.addHeader("Authorization", "Bearer " + accessToken ");
MultipartEntityBuilder builder = MultipartEntityBuilder.create(); byte[] fileContent = FileUtils.readFileToByteArray(fileToEncrypt); builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
String config = "{\"mode\":\"ENCRYPT_WITH_PASSWORD\",\"params\":{\"openPassword\":\"adobe\",\"permPassword\":\"systems\",\"permissions\":[\"ALL_PERM\"]}}";
 builder.addTextBody("config", config, ContentType.APPLICATION_JSON);
try
 {
    HttpEntity entity = builder.build();
    httpPost.setEntity(entity);
    CloseableHttpClient httpclient = HttpClients.createDefault();
    CloseableHttpResponse response = httpclient.execute(httpPost);
    if (response.getStatusLine().getStatusCode() == 200)
    {
       InputStream generatedPDF = response.getEntity().getContent();
       byte[] bytes = IOUtils.toByteArray(generatedPDF);
       File encryptedFile = new File("c:\\aem_forms_cs_api\\encrypted.pdf");
       FileOutputStream outputStream = new FileOutputStream(encryptedFile);
       outputStream.write(bytes);
       outputStream.close();
    }

}
catch (Exception e)
 {
    log.error(e.getMessage());
 }

...
```

## Handtekeningveld toevoegen aan PDF

Gebruik de handtekeningveld-API om een handtekening toe te voegen aan de geleverde PDF. Het volgende voorbeeldcodefragment voegt een handtekeningveld met de naam SignHere toe op pagina 4 van het document

```java
...
File pdfFile = new File(pdfFile1.getPath());
HttpPost httpPost = new HttpPost(postURL);
httpPost.addHeader("Authorization", "Bearer "+accessToken);
MultipartEntityBuilder builder = MultipartEntityBuilder.create();
byte[] fileContent = FileUtils.readFileToByteArray(pdfFile);
builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
builder.addTextBody("field", "SignHere", ContentType.TEXT_PLAIN);
String rectangle = "{\"lowerLeftX\":1,\"lowerLeftY\":40,\"width\":100,\"height\":100}";
builder.addTextBody("rectangle", rectangle, ContentType.APPLICATION_JSON);
```


## Codering verwijderen

Gebruik de verrichting van de PUT op encrypt API om encryptie uit de geleverde PDF te verwijderen. Het volgende Java-codefragment moet u helpen aan de slag te gaan.

```java
...
File fileToDecrypt = new File("path_to_your_pdf_file");

HttpPut httpPut = new HttpPut(putURL);
httpPut.addHeader("Authorization", "Bearer " + accessToken);

MultipartEntityBuilder builder = MultipartEntityBuilder.create();
byte[] fileContent = FileUtils.readFileToByteArray(fileToDecrypt);
builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
builder.addTextBody("config", "systems", ContentType.TEXT_PLAIN);

try {
    HttpEntity entity = builder.build();
    httpPut.setEntity(entity);
    CloseableHttpClient httpclient = HttpClients.createDefault();
    CloseableHttpResponse response = httpclient.execute(httpPut);

if (response.getStatusLine().getStatusCode() == 200) {
        InputStream generatedPDF = response.getEntity().getContent();
        byte[] bytes = IOUtils.toByteArray(generatedPDF);
        File encryptionRemoved = new File("c:\\aem_forms_cs_api\\encryption_removed.pdf");
        FileOutputStream outputStream = new FileOutputStream(encryptionRemoved);
        outputStream.write(bytes);
        outputStream.close();
        httpclient.close();
    }
} catch (Exception e) {
    log.error(e.getMessage());
}
...
```

### Postman-collectie

Een inzameling van Postman van API kan [ van hier voor het testen doeleinden ](assets/DocAssuranceAPI.postman_collection.json) worden gedownload. U kunt Basisauthentificatie of het Symbolische type van Symbolische authentificatie gebruiken om API aan te halen.
