---
title: PDF-documenten genereren met behulp van uitvoerservice
description: Gegevens samenvoegen met XDP-sjabloon om niet-interactieve PDF te genereren
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-16384
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 8a5a4d11-12a2-462d-8684-a0c6ec0cac0e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# PDF-documenten genereren met behulp van uitvoerservice

De [&#x200B; dienst van de Output &#x200B;](https://javadoc.io/static/com.adobe.aem/aem-forms-sdk-api/2024.07.31.00-240800/com/adobe/fd/output/api/OutputService.html) is de dienst OSGi die deel van de Diensten van het Document van AEM uitmaakt. Deze biedt ondersteuning voor verschillende uitvoerindelingen en ontwerpfuncties van AEM Forms Designer. De service Uitvoer converteert XFA-sjablonen en XML-gegevens om afdrukdocumenten in verschillende indelingen te genereren.

De Output-service in AEM Forms as a Cloud Service lijkt sterk op die in AEM Forms 6.5. Als u dus bekend bent met de Output-service in AEM Forms 6.5, is de overgang naar AEM Forms as a Cloud Service eenvoudig.

Met de service Uitvoer kunt u toepassingen maken waarmee u:

+ Definitieve formulierdocumenten genereren door sjabloonbestanden te vullen met XML-gegevens.
+ Uitvoerformulieren genereren in verschillende indelingen, waaronder niet-interactieve PDF-, PostScript-, PCL- en ZPL-afdrukstromen.
+ Afdruk-PDF&#39;s genereren van XFA-formulier-PDF&#39;s.
+ Genereer grote hoeveelheden PDF-, PostScript-, PCL- en ZPL-documenten door meerdere gegevenssets samen te voegen met de meegeleverde sjablonen.

Deze service is ontworpen voor gebruik binnen de context van een AEM Forms as a Cloud Service-instantie. Het volgende codefragment genereert een PDF-document in een servlet met behulp van `OutputService` .

```java
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.PDFOutputOptions;
import com.adobe.fd.output.api.AcrobatVersion;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.api.servlets.SlingServletPaths;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.nio.charset.StandardCharsets;

@Component(service = Servlet.class,
           property = {
               "sling.servlet.methods=" + HttpConstants.METHOD_POST,
               "sling.servlet.paths=/bin/generateStatement"
           })
public class GenerateStatementServlet extends SlingAllMethodsServlet {

    @Reference
    private OutputService outputService;

    @Override
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Access the submitted form data
        String formData = request.getParameter("formData");

        // Define the XDP template document
        String templateName = "/content/dam/formsanddocuments/adobe/statement.xdp";
        Document xdpDocument = new Document(templateName);

        // Set the PDF output options
        PDFOutputOptions pdfOutputOptions = new PDFOutputOptions();
        pdfOutputOptions.setAcrobatVersion(AcrobatVersion.Acrobat_10);

        // Create the submitted data document from the form data
        InputStream inputStream = new ByteArrayInputStream(formData.getBytes(StandardCharsets.UTF_8));
        Document submittedData = new Document(inputStream);

        // Use the output service to generate the PDF output
        Document generatedPDF = outputService.generatePDFOutput(xdpDocument, submittedData, pdfOutputOptions);

        // Process the generated PDF as per your use case        
    }
}
```
