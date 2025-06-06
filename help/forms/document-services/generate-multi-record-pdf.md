---
title: Meerdere PDF's genereren op basis van één gegevensbestand
description: OutputService biedt een aantal methoden om documenten te maken met behulp van een formulierontwerp en gegevens om samen te voegen met het formulierontwerp. Leer om veelvoudige pdf's van één grote xml te produceren die veelvoudige individuele verslagen bevatten.
feature: Output Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 58582acd-cabb-4e28-9fd3-598d3cbac43c
last-substantial-update: 2020-01-07T00:00:00Z
duration: 138
jira: KT-16142
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---

# Een set PDF-documenten genereren op basis van één XML-gegevensbestand

OutputService biedt een aantal methoden om documenten te maken met behulp van een formulierontwerp en gegevens om samen te voegen met het formulierontwerp. In het volgende artikel wordt uitgelegd hoe u meerdere pdf&#39;s kunt genereren op basis van één groot XML-bestand met meerdere afzonderlijke records.
Hier volgt een schermafbeelding van een XML-bestand dat meerdere records bevat.

![ multi-record-xml ](assets/multi-record-xml.PNG)

Data xml heeft 2 records. Elke record wordt vertegenwoordigd door het form1-element. Dit xml wordt overgegaan tot de methode OutputService [ generatePDFOutputBatch ](https://helpx.adobe.com/nl/aem-forms/6/javadocs/com/adobe/fd/output/api/OutputService.html) wij krijgen lijst van pdf- documenten (één per verslag)
Voor de handtekening van de methode generatePDFOutputBatch worden de volgende parameters gebruikt

* sjablonen - kaart met de sjabloon, geïdentificeerd door een sleutel
* data - Map met XML-gegevensdocumenten, geïdentificeerd door sleutel
* pdfOutputOptions - opties om pdf-generatie te configureren
* batchOptions - opties voor het configureren van batch



## Details kwestie gebruiken{#use-case-details}

In dit geval zullen we een eenvoudige webinterface beschikbaar stellen om de sjabloon en het bestand data(xml) te uploaden. Zodra het uploaden van de bestanden is voltooid en het POST-verzoek naar AEM servlet wordt verzonden. Deze servlet haalt de documenten uit en roept de methode generatePDFOutputBatch van OutputService aan. De gegenereerde PDF&#39;s worden gecomprimeerd in een ZIP-bestand en beschikbaar gesteld aan de eindgebruiker om te downloaden vanuit de webbrowser.

## Servlet-code{#servlet-code}

Hier volgt het codefragment van het servlet. Code extraheert de sjabloon(xdp) en het gegevensbestand(xml) uit de aanvraag. Sjabloonbestand wordt opgeslagen in het bestandssysteem. Twee kaarten worden gecreeerd - templateMap en dataFileMap die het malplaatje en xml(gegevens) dossiers respectievelijk bevatten. Een vraag wordt dan gemaakt aan generateMultipleRecords methode van de dienst DocumentServices.

```java
for (final java.util.Map.Entry < String, org.apache.sling.api.request.RequestParameter[] > pairs: params
.entrySet()) {
final String key = pairs.getKey();
final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
final org.apache.sling.api.request.RequestParameter param = pArr[0];
try {
if (!param.isFormField()) {

if (param.getFileName().endsWith("xdp")) {
    final InputStream xdpStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xdpDocument = new com.adobe.aemfd.docmanager.Document(xdpStream);

    xdpDocument.copyToFile(new File(saveLocation + File.separator + "fromui.xdp"));
    templateMap.put("key1", "file://///" + saveLocation + File.separator + "fromui.xdp");
    System.out.println("####  " + param.getFileName());

}
if (param.getFileName().endsWith("xml")) {
    final InputStream xmlStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlStream);
    dataFileMap.put("key1", xmlDocument);
}
}

Document zippedDocument = documentServices.generateMultiplePdfs(templateMap, dataFileMap,saveLocation);
.....
.....
....
```

### Interfaceimplementatiecode{#Interface-Implementation-Code}

De volgende code genereert meerdere pdf&#39;s met behulp van generatePDFOutputBatch van de OutputService en retourneert een ZIP-bestand met de pdf-bestanden naar het aanroepende servlet

```java
public Document generateMultiplePdfs(HashMap < String, String > templateMap, HashMap < String, Document > dataFileMap, String saveLocation) {
    log.debug("will save generated documents to " + saveLocation);
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    com.adobe.fd.output.api.BatchOptions batchOptions = new com.adobe.fd.output.api.BatchOptions();
    batchOptions.setGenerateManyFiles(true);
    com.adobe.fd.output.api.BatchResult batchResult = null;
    try {
        batchResult = outputService.generatePDFOutputBatch(templateMap, dataFileMap, pdfOptions, batchOptions);
        FileOutputStream fos = new FileOutputStream(saveLocation + File.separator + "zippedfile.zip");
        ZipOutputStream zipOut = new ZipOutputStream(fos);
        FileInputStream fis = null;

        for (int i = 0; i < batchResult.getGeneratedDocs().size(); i++) {
              com.adobe.aemfd.docmanager.Document dataMergedDoc = batchResult.getGeneratedDocs().get(i);
            log.debug("Got document " + i);
            dataMergedDoc.copyToFile(new File(saveLocation + File.separator + i + ".pdf"));
            log.debug("saved file " + i);
            File fileToZip = new File(saveLocation + File.separator + i + ".pdf");
            fis = new FileInputStream(fileToZip);
            ZipEntry zipEntry = new ZipEntry(fileToZip.getName());
            zipOut.putNextEntry(zipEntry);
            byte[] bytes = new byte[1024];
            int length;
            while ((length = fis.read(bytes)) >= 0) {
                zipOut.write(bytes, 0, length);
            }
            fis.close();
        }
        zipOut.close();
        fos.close();
        Document zippedDocument = new Document(new File(saveLocation + File.separator + "zippedfile.zip"));
        log.debug("Got zipped file from file system");
        return zippedDocument;


    } catch (OutputServiceException | IOException e) {

        e.printStackTrace();
    }
    return null;


}
```

### Distribueren op uw server{#Deploy-on-your-server}

Volg onderstaande instructies om deze mogelijkheid op uw server te testen:

* [ Download de steekproefactiva ](assets/mult-records-template-and-xml-file.zip).Dit zip dossier bevat het malplaatje en xml- gegevensdossier.
* [ Punt uw browser aan het Webconsole van Felix ](http://localhost:4502/system/console/bundles)
* [ stelt DevelopingWithServiceUser Bundel ](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar) op.
* Voeg de volgende ingang in de Dienst van het Mapper van de Gebruiker van de Dienst van Apache Sling toe gebruikend configMgr.

```java
DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
```



![ gebruiker-in kaart:brengen-dienst ](assets/user-mapper-service-fd-service.png)

* [ stelt de Bundel van AEMFormsDocumentServices van de Douane AEMForms ](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar) op.De bundel van de Douane die pdf produceert gebruikend OutputService API
* [ Punt uw browser aan pakketmanager ](http://localhost:4502/crx/packmgr/index.jsp)
* [ de Invoer en installeert het pakket ](assets/generate-multiple-pdf-from-xml.zip). Dit pakket bevat HTML-pagina waarmee u de sjabloon en gegevensbestanden kunt neerzetten.
* [ Punt uw browser aan MultiRecords.html ](http://localhost:4502/content/DocumentServices/Multirecord.html?)
* Sleep de sjabloon en het XML-gegevensbestand samen
* Download het gemaakte zip-bestand. Dit ZIP-bestand bevat de PDF-bestanden die door de uitvoerservice zijn gegenereerd.

>[!NOTE]
>Er zijn meerdere manieren om deze mogelijkheid te activeren. In dit voorbeeld hebben we een webinterface gebruikt om de sjabloon en het gegevensbestand neer te zetten om de mogelijkheid aan te tonen.
