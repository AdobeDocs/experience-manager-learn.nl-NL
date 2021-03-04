---
title: Meerdere PDF's genereren op basis van één gegevensbestand
seo-title: Meerdere PDF's genereren op basis van één gegevensbestand
feature: Uitvoerservice
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Ontwikkeling
role: Developer
level: Ervaren
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 0%

---


# Een set PDF-documenten genereren op basis van één XML-gegevensbestand

OutputService biedt een aantal methoden om documenten te maken met behulp van een formulierontwerp en gegevens om samen te voegen met het formulierontwerp. In het volgende artikel wordt uitgelegd hoe u meerdere pdf&#39;s kunt genereren op basis van één groot XML-bestand met meerdere afzonderlijke records.
Hier volgt een schermafbeelding van een XML-bestand dat meerdere records bevat.

![multi-record-xml](assets/multi-record-xml.PNG)

Data xml heeft 2 records. Elke record wordt vertegenwoordigd door het form1-element. Deze xml wordt doorgegeven aan de OutputService [generatePDFOutputBatch-methode](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/OutputService.html) we krijgen een lijst met PDF-documenten (één per record)
Voor de handtekening van de methode generatePDFOutputBatch worden de volgende parameters gebruikt

* sjablonen - kaart met de sjabloon, geïdentificeerd door een sleutel
* data - Map met XML-gegevensdocumenten, geïdentificeerd door sleutel
* pdfOutputOptions - opties om pdf-generatie te configureren
* batchOptions - opties voor het configureren van batch

>[!NOTE]
>
>Dit gebruiksgeval is beschikbaar als levend voorbeeld op deze [website](https://forms.enablementadobe.com/content/samples/samples.html?query=0).

## Hoofdletterdetails gebruiken{#use-case-details}

In dit geval zullen we een eenvoudige webinterface beschikbaar stellen om de sjabloon en het bestand data(xml) te uploaden. Nadat het uploaden van de bestanden is voltooid en het verzoek van de POST naar AEM servlet is verzonden. Deze servlet haalt de documenten uit en roept de methode generatePDFOutputBatch van OutputService aan. De gegenereerde PDF&#39;s worden gecomprimeerd in een ZIP-bestand en beschikbaar gesteld aan de eindgebruiker om te downloaden vanuit de webbrowser.

## Servlet Code{#servlet-code}

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

### Code interfaceimplementatie{#Interface-Implementation-Code}

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

### Implementeren op uw server{#Deploy-on-your-server}

Volg onderstaande instructies om deze mogelijkheid op uw server te testen:

* [Download en extraheer de ZIP-bestandsinhoud naar uw bestandssysteem](assets/mult-records-template-and-xml-file.zip). Dit ZIP-bestand bevat de sjabloon en het XML-gegevensbestand.
* [De browser naar de Felix-webconsole sturen](http://localhost:4502/system/console/bundles)
* [Implementeer DevelopingWithServiceUser Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).
* [Implementeer Aangepaste AEMFormsDocumentServices-bundel](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).Aangepaste bundel die de PDF&#39;s genereert met de OutputService-API
* [De browser naar pakketbeheer verwijzen](http://localhost:4502/crx/packmgr/index.jsp)
* [Importeer en installeer het pakket](assets/generate-multiple-pdf-from-xml.zip). Dit pakket bevat HTML-pagina waarmee u de sjabloon en gegevensbestanden kunt neerzetten.
* [Wijs uw browser naar MultiRecords.html](http://localhost:4502/content/DocumentServices/Multirecord.html?)
* Sleep de sjabloon en het XML-gegevensbestand samen
* Download het gemaakte zip-bestand. Dit ZIP-bestand bevat de PDF-bestanden die door de uitvoerservice zijn gegenereerd.

>[!NOTE]
>Er zijn meerdere manieren om deze mogelijkheid te activeren. In dit voorbeeld hebben we een webinterface gebruikt om de sjabloon en het gegevensbestand neer te zetten om de mogelijkheid aan te tonen.

