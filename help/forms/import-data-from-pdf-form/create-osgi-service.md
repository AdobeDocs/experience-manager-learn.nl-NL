---
title: OSGi-service maken om gegevens uit een PDF-formulier te exporteren
description: De gegevens vanuit een PDF-formulier exporteren met FormsService API
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: c3032669-154c-4565-af6e-32d94e975e37
duration: 52
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# Gegevens exporteren

De eerste stap voor het invullen van een adaptief formulier uit een PDF-bestand is het exporteren van de gegevens uit het opgegeven PDF-bestand en het opslaan in de AEM-opslagplaats.

De volgende code is geschreven om de gegevens uit de geüploade pdf te extraheren en om de juiste indeling te krijgen dan kan worden gebruikt om het adaptieve formulier te vullen.

```java
public String getFormData(Document pdfForm) {
   DocumentBuilderFactory factory = null;
   DocumentBuilder builder = null;
   org.w3c.dom.Document xmlDocument = null;

   try {
      Document xmlData = formsService.exportData(pdfForm, DataFormat.Auto);
      xmlData.copyToFile(new File("xmlData.xml"));
      factory = DocumentBuilderFactory.newInstance();
      factory.setNamespaceAware(true);
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlData.getInputStream());

      org.w3c.dom.Node xdpNode = xmlDocument.getDocumentElement();
      log.debug("Got xdp " + xdpNode.getNodeName());
      org.w3c.dom.Node datasets = getChildByTagName(xdpNode, "xfa:datasets");
      log.debug("Got datasets " + datasets.getNodeName());
      org.w3c.dom.Node data = getChildByTagName(datasets, "xfa:data");
      log.debug("Got data " + data.getNodeName());
      org.w3c.dom.Node topmostSubform = getChildByTagName(data, "topmostSubform");

      if (topmostSubform != null) {

         org.w3c.dom.Document newXmlDocument = builder.newDocument();
         org.w3c.dom.Node importedNode = newXmlDocument.importNode(topmostSubform, true);
         newXmlDocument.appendChild(importedNode);
         Document aemFDXmlDocument = documentServices.orgw3cDocumentToAEMFDDocument(newXmlDocument);
         aemFDXmlDocument.copyToFile(new File("aemFDXmlDocument.xml"));
         // saveDocumentInCrx is an utility method of the custom DocumentServices service. 
         return documentServices.saveDocumentInCrx("/content/exporteddata", ".xml", aemFDXmlDocument);
      }

   } catch (Exception e) {
      log.debug("Error:  " + e.getMessage());

   }
   return null;
}
```

Het volgende is de nutsfunctie die wordt geschreven om _&#x200B;**topSubForm**&#x200B;_ met aangewezen namespaces te halen

```java
private static org.w3c.dom.Node getChildByTagName(org.w3c.dom.Node parent, String tagName) {
   NodeList nodeList = parent.getChildNodes();
   for (int i = 0; i < nodeList.getLength(); i++) {
      org.w3c.dom.Node node = nodeList.item(i);
      if (node.getNodeType() == org.w3c.dom.Node.ELEMENT_NODE && node.getNodeName().equals(tagName)) {
         return node;
      }
   }
   return null;
}
```

De geëxtraheerde gegevens worden opgeslagen onder het gegevensknooppunt /content/export in de crx-opslagplaats. Het bestandspad van de geëxporteerde gegevens wordt vervolgens teruggestuurd naar de opvragende toepassing om het aangepaste formulier in te vullen.

## Volgende stappen

[Gegevens importeren uit PDF-bestand](./populate-adaptive-form.md)
