---
title: De handtekeningstatus van het formulier in de database bijwerken
description: De handtekeningstatus van het ondertekende formulier in de database bijwerken met behulp van de AEM workflow
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6888
thumbnail: 6888.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 75852a4b-7008-4c65-bab1-cc5dbf525e20
duration: 42
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 0%

---

# Handtekeningstatus bijwerken

De workflow UpdateSignatureStatus wordt geactiveerd wanneer de gebruiker de ondertekeningsceremonie heeft voltooid. Hier volgt de workflow

![ main-workflow ](assets/update-signature.PNG)

De status van de handtekening bijwerken is een aangepaste processtap.
De belangrijkste reden voor het uitvoeren van de stap van het douaneproces is een AEMWerkschema uit te breiden. Hieronder ziet u de aangepaste code die wordt gebruikt om de status van de handtekening bij te werken.
De code in deze stap van het douaneproces verwijst naar de dienst SignMultipleForms.


```java
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Update Signature Status in DB",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Update Signature Status in DB"
})

public class UpdateSignatureStatusWorkflowStep implements WorkflowProcess {
  private static final Logger log = LoggerFactory.getLogger(UpdateSignatureStatusWorkflowStep.class);@Reference
  SignMultipleForms signMultipleForms;@Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap args) throws WorkflowException {
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    String dataFilePath = payloadPath + "/Data.xml/jcr:content";
    Session session = workflowSession.adaptTo(Session.class);
    DocumentBuilderFactory factory = null;
    DocumentBuilder builder = null;
    Document xmlDocument = null;
    Node xmlDataNode = null;
    try {
      xmlDataNode = session.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      factory = DocumentBuilderFactory.newInstance();
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlDataStream);
      XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
      org.w3c.dom.Node node = (org.w3c.dom.Node) xPath.compile("/afData/afUnboundData/data/guid").evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
      String guid = node.getTextContent();
      StringWriter writer = new StringWriter();
      IOUtils.copy(xmlDataStream, writer, StandardCharsets.UTF_8);
      System.out.println("After ioutils copy" + writer.toString());
      signMultipleForms.updateSignatureStatus(writer.toString(), guid);
    }
    catch(Exception e) {
      log.debug(e.getMessage());
    }

  }

}
```

## Assets

De werkschema van de de statusstatus van de updatehandtekening kan [ van hier worden gedownload ](assets/update-signature-status-workflow.zip)

## Volgende stappen

[Samenvattingsstap aanpassen om het volgende formulier voor ondertekening weer te geven](./customize-summary-component.md)
