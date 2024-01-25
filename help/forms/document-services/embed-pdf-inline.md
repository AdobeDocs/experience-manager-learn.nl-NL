---
title: Document inline van record weergeven
description: Voeg adaptieve formuliergegevens samen met XDP-sjabloon en geef de PDF inline weer met gebruik van PDF-API in de documentcloud.
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-9411
exl-id: 327ffe26-e88e-49f0-9f5a-63e2a92e1c8a
last-substantial-update: 2021-07-07T00:00:00Z
duration: 202
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 0%

---

# Inline DoR weergeven

Een veelvoorkomend geval is het weergeven van een PDF-document met de gegevens die de invuller van het formulier heeft ingevoerd.

Voor dit gebruiksgeval hebben we de [Adobe PDF Embed-API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

De volgende stappen zijn uitgevoerd om de integratie te voltooien

## Aangepaste component maken om de PDF inline weer te geven

Er is een aangepaste component (embed-pdf) gemaakt om de PDF in te sluiten die door de aanroep van de POST wordt geretourneerd.

## Clientbibliotheek

De volgende code wordt uitgevoerd wanneer de `viewPDF` op de knop Selectievakje wordt geklikt. We geven de adaptieve formuliergegevens, sjabloonnaam, door aan het eindpunt om de PDF te genereren. Het gegenereerde PDF-bestand wordt vervolgens weergegeven aan de invuller van het formulier met de PDF JavaScript-bibliotheek ingesloten.

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

U kunt alle gratis onlinegereedschappen gebruiken om [XSD genereren](https://www.freeformatter.com/xsd-generator.html) uit de XML-gegevens die in de vorige stap zijn gegenereerd.

## De sjabloon uploaden

Zorg ervoor dat u de xdp-sjabloon uploadt naar [AEM Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) de knop Maken gebruiken


## Adaptief formulier maken

Maak een adaptief formulier op basis van de XSD van de vorige stap.
Voeg een nieuw tabblad toe aan het aanpassingsbestand. Voeg een component CheckBox en een component embed-pdf toe aan dit tabblad Geef het selectievakje viewPDF op.
De component embed-pdf configureren, zoals hieronder in de schermafbeelding wordt weergegeven
![embed-pdf](assets/embed-pdf-configuration.png)

**API-sleutel voor PDF insluiten** - Dit is de sleutel die u kunt gebruiken om pdf in te sluiten. Deze sleutel werkt alleen met localhost. U kunt [uw eigen sleutel](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) en deze aan een ander domein koppelen.

**Eindpunt dat de pdf retourneert** - Dit is de aangepaste servlet die de gegevens samenvoegt met de xdp-sjabloon en de PDF retourneert.

**Sjabloonnaam** - Dit is het pad naar de xdp. Doorgaans wordt het bestand opgeslagen in de map formsanddocuments.

**PDF-bestandsnaam** - Dit is de tekenreeks die wordt weergegeven in de ingesloten pdf-component.

## Aangepaste servlet maken

Er is een aangepaste servlet gemaakt om de gegevens samen te voegen met de XDP-sjabloon en de PDF te retourneren. De code om dit te verwezenlijken is hieronder vermeld. De aangepaste servlet maakt deel uit van de [insluiten, pdf-bundel](assets/embedpdf.core-1.0-SNAPSHOT.jar)

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

1. [De ingesloten pdf-bundel downloaden en installeren](assets/embedpdf.core-1.0-SNAPSHOT.jar).
Dit heeft servlet om de gegevens met het malplaatje XDP samen te voegen en pdf terug te stromen.
1. Voeg het pad /bin/getPDFToEmbed toe in de uitgesloten padsectie van het Adobe Granite CSRF-filter met behulp van de [AEM ConfigMgr](http://localhost:4502/system/console/configMgr). In uw productieomgeving is het raadzaam de [Beschermingskader van de GVTO](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=en)
1. [De clientbibliotheek en de aangepaste component importeren](assets/embed-pdf.zip)
1. [Het adaptieve formulier en de sjabloon importeren](assets/embed-pdf-form-and-xdp.zip)
1. [Voorbeeld van adaptief formulier](http://localhost:4502/content/dam/formsanddocuments/from1040/jcr:content?wcmmode=disabled)
1. Enkele formuliervelden invullen
1. Tab naar het tabblad PDF weergeven. Schakel het selectievakje PDF weergeven in. Er wordt een PDF weergegeven in het formulier dat is gevuld met de aangepaste formuliergegevens
