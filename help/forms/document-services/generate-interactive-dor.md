---
title: Interactieve DoR genereren met adaptieve formuliergegevens
description: Aangepaste formuliergegevens samenvoegen met XDP-sjabloon om downloadbare pdf te genereren
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
source-git-commit: 72a9edb3edc73cf14f13bb53355a37e707ed4c79
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---


# Interactieve DoR downloaden

Een veelvoorkomend geval is het downloaden van een interactieve DoR met de Adaptief-formuliergegevens. Het gedownloade DoR wordt dan voltooid met Adobe Acrobat of Adobe Reader.

Voor dit gebruiksgeval moeten we het volgende doen:

## Voorbeeldgegevens voor de xdp genereren

Open XDP in de ontwerper van AEM Forms.
Klikken op bestand | Formuliereigenschappen | Klik op Voorvertoning genereren Gegevens voorvertoning genereren Klik op Voorvertoning genereren Geef een betekenisvolle bestandsnaam op, bijvoorbeeld &quot;form-data.xml&quot;

## XSD genereren op basis van de XML-gegevens

U kunt alle gratis online gereedschappen gebruiken om [xsd genereren](https://www.freeformatter.com/xsd-generator.html) uit de XML-gegevens die in de vorige stap zijn gegenereerd.

## Adaptief maken

Maak een adaptief formulier op basis van de xsd van de vorige stap. Koppel het formulier aan het gebruik van de clientbibliotheek &quot;irs&quot;. Deze clientbibliotheek heeft de code om een POST aan te roepen naar de servlet die de PDF naar de aanroepende toepassing terugkeert. De volgende code wordt geactiveerd wanneer de _PDF downloaden_ is aangeklikt

```javascript
$(document).ready(function() {
    $(".downloadpdf").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();

                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";

                var formData = new FormData();
                formData.append("dataXml", guideResultObject.data);
                console.log(guideResultObject.data);
                req.send(formData);

                req.onreadystatechange = function() {

                    if (req.readyState == 4 && req.status == 200) {



                        download(this.response, "report.pdf", "application/pdf");


                    }


                }
            }
        });

    });
});
```



## Aangepaste servlet maken

Maak een aangepaste servlet die de gegevens samenvoegt met de xdp-sjabloon en de PDF retourneert. De code om dit te verwezenlijken is hieronder vermeld. De aangepaste servlet maakt deel uit van de [AEMFormsDocumentServices.core-1.0-SNAPSHOT-bundel](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)).

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringWriter;

import javax.servlet.Servlet;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathExpressionException;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Node;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.adobe.fd.forms.api.FormsServiceException;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = {
        Servlet.class
}, property = {
        "sling.servlet.methods=post",
        "sling.servlet.paths=/bin/generateinteractivedor"
})

public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
        @Reference
        DocumentServices documentServices;
        @Reference
        FormsService formsService;

        private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

        protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                doPost(request, response);
        }

        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                // The xdp name can be passed to this servlet. For now it have been hardocded.

                String xdpName = "f8918-r14e_redo-barcode_3 2.xdp";

                XPathFactory xfact = XPathFactory.newInstance();
                XPath xpath = xfact.newXPath();
                //String dataXml = request.getParameter("formData");
                String dataXml = request.getParameter("dataXml");
                System.out.println("The data xml is " + dataXml);
                org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
                System.out.println("The af bound data is " + xmlDataDoc.getElementsByTagName("topmostSubform").getLength());
                try {
                        // get the actual xml data that needs to be merged with the template. This can be made more generic
                        Node res = (Node) xpath.evaluate("afData/afBoundData/topmostSubform", xmlDataDoc, XPathConstants.NODE);
                        StringWriter writer = new StringWriter();
                        Transformer transformer = TransformerFactory.newInstance().newTransformer();
                        transformer.transform(new DOMSource(res), new StreamResult(writer));
                        String xml = writer.toString();
                        System.out.println(xml);
                        xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                        Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                        String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                        com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                        renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                        renderOptions.setContentRoot(xdpTemplatePath);
                        renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                        Document xdpPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
                        InputStream fileInputStream = xdpPDF.getInputStream();
                        System.out.println("Got xdp PDF" + fileInputStream.available());
                        response.setContentType("application/pdf");
                        
                        response.addHeader("Content-Disposition", "attachment; filename=" + xdpName.replace("xdp", "pdf"));
                        response.setContentLength((int) fileInputStream.available());
                        OutputStream responseOutputStream = response.getOutputStream();
                        int bytes;
                        while ((bytes = fileInputStream.read()) != -1) {
                                responseOutputStream.write(bytes);
                        }
                        responseOutputStream.flush();
                        responseOutputStream.close();

                } catch (XPathExpressionException e) {
                        log.debug(e.getMessage());

                } catch (TransformerException e) {

                        log.debug(e.getMessage());
                } catch (FormsServiceException e) {

                        log.debug(e.getMessage());
                } catch (IOException e) {

                        log.debug(e.getMessage());
                }

        }

}
```

In de voorbeeldcode is de sjabloonnaam (f8918-r14e_redo-barcode_3 2.xdp) hard gecodeerd. U kunt de malplaatjenaam aan servlet gemakkelijk overgaan om deze code algemeen te maken om tegen alle malplaatjes te werken.


Voer de volgende stappen uit om dit op uw lokale server te testen:
1. [Download en installeer de DevelopingWithServiceUser Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. Voeg de volgende vermelding toe in de Apache Sling Service User Mapper Service DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
1. [De aangepaste DocumentServices-bundel downloaden en installeren](/hep/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Dit heeft servlet om de gegevens met het malplaatje XDP samen te voegen en pdf terug te stromen
1. [De clientbibliotheek importeren](assets/irs.zip)
1. [Het adaptieve formulier importeren](assets/f8918complete.zip)
!. [De XDP-sjabloon en het XDP-schema importeren](assets/xdp-template-and-xsd.zip)
1. [Voorbeeld van adaptief formulier](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. Enkele formuliervelden invullen
1. Klik op PDF downloaden om de PDF te downloaden
