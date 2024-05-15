---
title: Interactieve DoR genereren met adaptieve formuliergegevens
description: Aangepaste formuliergegevens samenvoegen met XDP-sjabloon om downloadbare pdf te genereren
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-9226
exl-id: d9618cc8-d399-4850-8714-c38991862045
last-substantial-update: 2020-02-07T00:00:00Z
duration: 177
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '521'
ht-degree: 0%

---

# Interactieve DoR downloaden

Een veelvoorkomend geval is het downloaden van een interactieve DoR met de Adaptief-formuliergegevens. Het gedownloade DoR wordt dan voltooid met Adobe Acrobat of Adobe Reader.

## Adaptief formulier is niet gebaseerd op XSD-schema

Als uw XDP- en Adaptief formulier niet zijn gebaseerd op een schema, voert u de volgende stappen uit om een interactief document met records te genereren.

### Adaptief formulier maken

Maak een adaptief formulier en zorg ervoor dat de adaptieve namen van formuliervelden identiek zijn aan veldnamen in uw xdp-sjabloon.
Noteer de naam van het hoofdelement van uw xdp-sjabloon.
![root-element](assets/xfa-root-element.png)

### Clientbibliotheek

De volgende code wordt geactiveerd wanneer de knop PDF downloaden wordt geactiveerd

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","two.xdp")
                postParameters.append("formBasedOnSchema", "false");
                postParameters.append("xfaRootElement","form1");
                console.log(guideResultObject.data);
                req.send(postParameters);
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

## Adaptief formulier gebaseerd op XSD-schema

Als uw xdp niet is gebaseerd op XSD, voert u de volgende stappen uit om XSD(schema) te maken waarop u het adaptieve formulier baseert

### Voorbeeldgegevens genereren voor de XDP

* Open XDP in de ontwerper van AEM Forms.
* Klikken op bestand | Formuliereigenschappen | Voorvertoning
* Klik op Voorbeeldgegevens genereren
* Klik op Genereren
* Geef een betekenisvolle bestandsnaam op, bijvoorbeeld &quot;form-data.xml&quot;

### XSD genereren op basis van de XML-gegevens

U kunt alle gratis onlinegereedschappen gebruiken om [XSD genereren](https://www.freeformatter.com/xsd-generator.html) uit de XML-gegevens die in de vorige stap zijn gegenereerd.

### Adaptief formulier maken

Maak een adaptief formulier op basis van de XSD van de vorige stap. Koppel het formulier aan het gebruik van de clientbibliotheek &quot;irs&quot;. Deze clientbibliotheek heeft de code om een POST aan te roepen naar de servlet die de PDF naar de aanroepende toepassing terugkeert. De volgende code wordt geactiveerd wanneer de _PDF downloaden_ is aangeklikt

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","f8918-r14e_redo-barcode_3 2.xdp")
                postParameters.append("formBasedOnSchema", "true");
                postParameters.append("dataNodeToExtract","afData/afBoundData/topmostSubform");
                console.log(guideResultObject.data);
                req.send(postParameters);
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

Maak een aangepaste servlet die de gegevens samenvoegt met een XDP-sjabloon en de PDF retourneert. De code om dit te verwezenlijken is hieronder vermeld. De aangepaste servlet maakt deel uit van de [AEMFormsDocumentServices.core-1.0-SNAPSHOT-bundel](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)).

```java
public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
    private static final long serialVersionUID = 1 L;
    @Reference
    DocumentServices documentServices;
    @Reference
    FormsService formsService;
    private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        doPost(request, response);
    }
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        String xdpName = request.getParameter("xdpName");

        boolean formBasedOnXSD = Boolean.parseBoolean(request.getParameter("formBasedOnSchema"));

        XPathFactory xfact = XPathFactory.newInstance();
        XPath xpath = xfact.newXPath();
        String dataXml = request.getParameter("dataXml");
        log.debug("The data xml is " + dataXml);
        org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
        Document renderedPDF = null;
        try {
            if (!formBasedOnXSD) {
                String xfaRootElement = request.getParameter("xfaRootElement");
                DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();
                DocumentBuilder dBuilder = dbFactory.newDocumentBuilder();
                org.w3c.dom.Document newXMLDocument = dBuilder.newDocument();
                Element rootElement = newXMLDocument.createElement(xfaRootElement);
                String unboundData = "afData/afUnboundData/data";
                Node dataNode = (Node) xpath.evaluate(unboundData, xmlDataDoc, XPathConstants.NODE);
                NodeList dataChildNodes = dataNode.getChildNodes();
                for (int i = 0; i<dataChildNodes.getLength(); i++) {
                    Node childNode = dataChildNodes.item(i);
                    if (childNode.getNodeType() == 1) {
                        Element newElement = newXMLDocument.createElement(childNode.getNodeName());
                        newElement.setTextContent(childNode.getTextContent());
                        rootElement.appendChild(newElement);
                        log.debug("the node name is  " + childNode.getNodeName() + " and its value is " + childNode.getTextContent());
                    }
                }
                newXMLDocument.appendChild(rootElement);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(newXMLDocument);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);

            } else {
                // form is based on xsd
                // get the actual xml data that needs to be merged with the template. This can be made more generic
                String nodeToExtract = request.getParameter("dataNodeToExtract");
                Node dataNode = (Node) xpath.evaluate(nodeToExtract, xmlDataDoc, XPathConstants.NODE);
                StringWriter writer = new StringWriter();
                Transformer transformer = TransformerFactory.newInstance().newTransformer();
                transformer.transform(new DOMSource(dataNode), new StreamResult(writer));
                String xml = writer.toString();
                System.out.println(xml);
                xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
            }
            InputStream fileInputStream = renderedPDF.getInputStream();
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

        } catch (XPathExpressionException | TransformerException | FormsServiceException | IOException | ParserConfigurationException e) {
            log.debug(e.getMessage());
        }

    }

}
```

In de voorbeeldcode extraheren we de xdp-naam en andere parameters uit het aanvraagobject. Als het formulier niet is gebaseerd op XSD, wordt het XML-document gemaakt dat met de xdp moet worden samengevoegd. Als het formulier is gebaseerd op XSD, extraheren we gewoon het juiste knooppunt uit de aangepaste formulierverzendgegevens om XML-document te genereren voor samenvoeging met de xdp-sjabloon.

## Het voorbeeld op de server implementeren

Voer de volgende stappen uit om dit op uw lokale server te testen:

1. [Download en installeer de DevelopingWithServiceUser Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. Voeg de volgende vermelding toe in de Apache Sling Service User Mapper Service DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
1. [De aangepaste DocumentServices-bundel downloaden en installeren](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Dit heeft servlet om de gegevens met het malplaatje XDP samen te voegen en pdf terug te stromen
1. [De clientbibliotheek importeren](assets/generate-interactive-dor-client-lib.zip)
1. [De artikelelementen importeren (Adaptief formulier,XDP-sjablonen en XSD)](assets/generate-interactive-dor-sample-assets.zip)
1. [Voorbeeld van adaptief formulier](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. Vul slechts enkele formuliervelden in.
1. Klik op PDF downloaden om de PDF op te halen. U moet mogelijk een paar seconden wachten voordat de PDF kan worden gedownload.

>[!NOTE]
>
>U kunt hetzelfde gebruiksgeval proberen met [adaptief formulier op basis van niet-xsd](http://localhost:4502/content/dam/formsanddocuments/two/jcr:content?wcmmode=disabled). Zorg ervoor dat u de juiste parameters doorgeeft aan het posteindpunt in streampdf.js in de irs clientlib.
