---
title: Document inline van record weergeven
description: Aangepaste formuliergegevens samenvoegen met XDP-sjabloon en de PDF inline weergeven met de PDF-API van de documentcloud.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-9411
exl-id: 327ffe26-e88e-49f0-9f5a-63e2a92e1c8a
last-substantial-update: 2021-07-07T00:00:00Z
duration: 165
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 0%

---

# Inline DoR weergeven

Een veelvoorkomend geval is het weergeven van een PDF-document met de gegevens die de invuller van het formulier heeft ingevoerd.

Om dit gebruiksgeval te verwezenlijken hebben wij [ Adobe PDF ingebed API ](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) gebruikt.

De volgende stappen zijn uitgevoerd om de integratie te voltooien

## Aangepaste component maken om de PDF inline weer te geven

Er is een aangepaste component (embed-pdf) gemaakt om de PDF in te sluiten die door de POST-aanroep wordt geretourneerd.

## Clientbibliotheek

De volgende code wordt uitgevoerd wanneer op de knop voor het selectievakje `viewPDF` wordt geklikt. We geven de adaptieve formuliergegevens, sjabloonnaam, door aan het eindpunt om de PDF te genereren. Het gegenereerde PDF-bestand wordt vervolgens aan de gebruiker weergegeven met de PDF-bibliotheek van het formulier.

```javascript
$(document).ready(function() {

    $(".viewPDF").click(function() {
        console.log("view pdfclicked");
        window.guideBridge.getDataXML({
            success: function(result) {
                var obj = new FormData();
                obj.append("data", result.data);
                obj.append("template", document.querySelector("[data-template]").getAttribute("data-template"));
                const fetchPromise = fetch(document.querySelector("[data-endpoint]").getAttribute("data-endpoint"), {
                        method: "POST",
                        body: obj,
                        contentType: false,
                        processData: false,

                    })
                    .then(response => {

                        var adobeDCView = new AdobeDC.View({
                            clientId: document.querySelector("[data-apikey]").getAttribute("data-apikey"),
                            divId: "adobe-dc-view"
                        });
                        console.log("In preview file");
                        adobeDCView.previewFile(

                            {
                                content: {
                                    promise: response.arrayBuffer()
                                },
                                metaData: {
                                    fileName: document.querySelector("[data-filename]").getAttribute("data-filename")
                                }
                            }
                        );


                        console.log("done")
                    })


            }
        });
    });



});
```

## Voorbeeldgegevens genereren voor de XDP

* Open XDP in de ontwerper van AEM Forms.
* Klikken op bestand | Formuliereigenschappen | Voorvertoning
* Klik op Voorbeeldgegevens genereren
* Klik op Genereren
* Geef een betekenisvolle bestandsnaam op, bijvoorbeeld &quot;form-data.xml&quot;

## XSD genereren op basis van de XML-gegevens

U kunt om het even welke vrije online hulpmiddelen gebruiken om [ XSD ](https://www.freeformatter.com/xsd-generator.html) van de xmlgegevens te produceren die in de vorige stap worden geproduceerd.

## De sjabloon uploaden

Zorg ervoor u het xdp malplaatje in [ AEM Forms ](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) uploadt gebruikend creeer knoop


## Adaptief formulier maken

Maak een adaptief formulier op basis van de XSD van de vorige stap.
Voeg een nieuw tabblad toe aan het aanpassingsbestand. Een component selectievakje en een component embed-pdf toevoegen aan dit tabblad
Geef het selectievakje de naam van de weergave-PDF.
De component embed-pdf configureren, zoals hieronder in de schermafbeelding wordt weergegeven
![ embed-pdf ](assets/embed-pdf-configuration.png)

**bedt Sleutel PDF API** in - dit is de sleutel die u kunt gebruiken om pdf in te bedden. Deze sleutel werkt alleen met localhost. U kunt [ uw eigen sleutel ](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) tot stand brengen en het associëren met ander domein.

**Eindpunt die pdf** terugkeren - dit is douaneserlet die de gegevens met het xdp malplaatje zal samenvoegen en pdf terugkeren.

**naam van het Malplaatje** - dit is de weg aan xdp. Doorgaans wordt het bestand opgeslagen in de map formsanddocuments.

**de Naam van het Dossier van PDF** - dit is het koord dat in ingebed pdf component zal verschijnen.

## Aangepaste servlet maken

Er is een aangepaste servlet gemaakt om de gegevens samen te voegen met de XDP-sjabloon en de PDF te retourneren. De code om dit te verwezenlijken is hieronder vermeld. De douaneserlet maakt deel uit van de [ ingebedde pdf- bundel ](assets/embedpdf.core-1.0-SNAPSHOT.jar)

```java
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.io.StringWriter;
import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.InputSource;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;

package com.embedpdf.core.servlets;
@Component(service = {
   Servlet.class
}, property = {
   "sling.servlet.methods=post",
   "sling.servlet.paths=/bin/getPDFToEmbed"
})
public class StreamPDFToEmbed extends SlingAllMethodsServlet {
   @Reference
   OutputService outputService;
   private static final long serialVersionUID = 1 L;
   private static final Logger log = LoggerFactory.getLogger(StreamPDFToEmbed.class);

   protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
      String xdpName = request.getParameter("template");
      String formData = request.getParameter("data");
      log.debug("in doPOST of Stream PDF Form Data is >>> " + formData + " template is >>> " + xdpName);

      try {

         XPathFactory xfact = XPathFactory.newInstance();
         XPath xpath = xfact.newXPath();
         DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
         DocumentBuilder builder = factory.newDocumentBuilder();

         org.w3c.dom.Document xmlDataDoc = builder.parse(new InputSource(new StringReader(formData)));

         // get the data to merge with template

         Node afBoundData = (Node) xpath.evaluate("afData/afBoundData", xmlDataDoc, XPathConstants.NODE);
         NodeList afBoundDataChildren = afBoundData.getChildNodes();
         String afDataNodeName = afBoundDataChildren.item(0).getNodeName();
         Node nodeWithDataToMerge = (Node) xpath.evaluate("afData/afBoundData/" + afDataNodeName, xmlDataDoc, XPathConstants.NODE);
         StringWriter writer = new StringWriter();
         Transformer transformer = TransformerFactory.newInstance().newTransformer();
         transformer.transform(new DOMSource(nodeWithDataToMerge), new StreamResult(writer));
         String xml = writer.toString();
         InputStream targetStream = new ByteArrayInputStream(xml.getBytes());
         Document xmlDataDocument = new Document(targetStream);
         // get the template
         Document xdpTemplate = new Document(xdpName);
         log.debug("got the  xdp Template " + xdpTemplate.length());

         // use output service the merge data with template
         com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
         pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
         com.adobe.aemfd.docmanager.Document documentToReturn = outputService.generatePDFOutput(xdpTemplate, xmlDataDocument, pdfOptions);

         // stream pdf to the client

         InputStream fileInputStream = documentToReturn.getInputStream();
         response.setContentType("application/pdf");
         response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
         response.setContentLength((int) fileInputStream.available());
         OutputStream responseOutputStream = response.getOutputStream();
         int bytes;
         while ((bytes = fileInputStream.read()) != -1) {
            responseOutputStream.write(bytes);
         }
         responseOutputStream.flush();
         responseOutputStream.close();

      } catch (Exception e) {

         System.out.println("Error " + e.getMessage());
      }

   }

}
```


## Het voorbeeld op de server implementeren

Voer de volgende stappen uit om dit op uw lokale server te testen:

1. [ Download en installeer de ingebedde pdf- bundel ](assets/embedpdf.core-1.0-SNAPSHOT.jar).
Dit heeft servlet om de gegevens met het malplaatje XDP samen te voegen en pdf terug te stromen.
1. Voeg de weg /bin/getPDFToEmbed in de uitgesloten wegensectie van de Filter toe van Adobe Granite CSRF gebruikend [ AEM ConfigMgr ](http://localhost:4502/system/console/configMgr). In uw productiemilieu wordt het geadviseerd om het [ CSRF beschermingskader ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=en) te gebruiken
1. [De clientbibliotheek en de aangepaste component importeren](assets/embed-pdf.zip)
1. [Het adaptieve formulier en de sjabloon importeren](assets/embed-pdf-form-and-xdp.zip)
1. [ Voorproef Aangepaste Vorm ](http://localhost:4502/content/dam/formsanddocuments/from1040/jcr:content?wcmmode=disabled)
1. Enkele formuliervelden invullen
1. Tab naar het tabblad View PDF. Schakel het selectievakje PDF weergeven in. Er wordt een PDF weergegeven in het formulier dat is gevuld met de aangepaste formuliergegevens
