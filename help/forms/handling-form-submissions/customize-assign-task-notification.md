---
title: Taakmelding toewijzen aanpassen
description: Formuliergegevens opnemen in de e-mails met taakmeldingen toewijzen
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
jira: KT-6279
thumbnail: KT-6279.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 0cb74afd-87ff-4e79-a4f4-a4634ac48c51
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 0%

---

# Taakmelding toewijzen aanpassen

De taakcomponent toewijzen wordt gebruikt om taken toe te wijzen aan workflowdeelnemers. Wanneer een taak aan een gebruiker of een groep wordt toegewezen, wordt een e-mailbericht verzonden naar de bepaalde gebruiker of groepsleden.
Dit e-mailbericht bevat meestal dynamische gegevens die betrekking hebben op de taak. Deze dynamische gegevens worden opgehaald met het gegenereerde systeem [eigenschappen van metagegevens](https://experienceleague.adobe.com/docs/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html#using-system-generated-metadata-in-an-email-notification).
Als u waarden uit de verzonden formuliergegevens wilt opnemen in het e-mailbericht, moet u een eigenschap voor aangepaste metagegevens maken en deze eigenschappen voor aangepaste metagegevens in de e-mailsjabloon gebruiken



## Aangepaste metagegevenseigenschap maken

De geadviseerde benadering is een component te creëren OSGI die de getUserMetadata methode van [WorkitemUserMetadataService](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html#getUserMetadataMap--)

De volgende code maakt 4 eigenschappen van metagegevens (_firstName_,_lastName_,_reden_ en _amountRequested_) en stelt de waarde ervan in op basis van de ingediende gegevens. Bijvoorbeeld de eigenschap metadata _firstName_ De waarde wordt ingesteld op de waarde van het element met de naam firstName in de verzonden gegevens. De volgende code gaat ervan uit dat de verzonden gegevens van het adaptieve formulier de XML-indeling hebben. Adaptieve Forms op basis van JSON-schema of formuliergegevensmodel genereert gegevens in JSON-indeling.


```java
package com.aemforms.workitemuserservice.core;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.*;


import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;
@Component(property={Constants.SERVICE_DESCRIPTION+"=A sample implementation of a user metadata service.",
Constants.SERVICE_VENDOR+"=Adobe Systems",
"process.label"+"=Sample Custom Metadata Service"})


public class WorkItemUserServiceImpl implements WorkitemUserMetadataService {
private static final Logger log = LoggerFactory.getLogger(WorkItemUserServiceImpl.class);

@Override
public Map<String, String> getUserMetadata(WorkItem workItem, WorkflowSession workflowSession,MetaDataMap metadataMap)
{
HashMap<String, String> customMetadataMap = new HashMap<String, String>();
String payloadPath = workItem.getWorkflowData().getPayload().toString();
String dataFilePath = payloadPath + "/Data.xml/jcr:content";
Session session = workflowSession.adaptTo(Session.class);
DocumentBuilderFactory factory = null;
DocumentBuilder builder = null;
Document xmlDocument = null;
javax.jcr.Node xmlDataNode = null;
try
{
    xmlDataNode = session.getNode(dataFilePath);
    InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
    XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
    factory = DocumentBuilderFactory.newInstance();
    builder = factory.newDocumentBuilder();
    xmlDocument = builder.parse(xmlDataStream);
    Node firstNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/firstName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    log.debug("The value of first name element  is " + firstNameNode.getTextContent());
    Node lastNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/lastName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node amountRequested = (org.w3c.dom.Node) xPath
            .compile("afData/afUnboundData/data/amountRequested")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node reason = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/reason")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    customMetadataMap.put("firstName", firstNameNode.getTextContent());
    customMetadataMap.put("lastName", lastNameNode.getTextContent());
    customMetadataMap.put("amountRequested", amountRequested.getTextContent());
    customMetadataMap.put("reason", reason.getTextContent());
    log.debug("Created  " + customMetadataMap.size() + " metadata  properties");

}
catch (Exception e)
{
    log.debug(e.getMessage());
}
return customMetadataMap;
}

}
```

## De aangepaste eigenschappen voor metagegevens in de e-mailsjabloon voor taakmeldingen gebruiken

In de e-mailsjabloon kunt u de eigenschap metadata opnemen door de volgende syntaxis te gebruiken waarbij amountRequested de eigenschap metadata is `${amountRequested}`

## Taak toewijzen om eigenschap voor aangepaste metagegevens te gebruiken

Nadat de component OSGi wordt gebouwd en in AEM server wordt opgesteld, vorm de Assign component van de Taak zoals hieronder getoond om de eigenschappen van douanemetagegevens te gebruiken.


![Taakmelding](assets/task-notification.PNG)

## Het gebruik van aangepaste eigenschappen van metagegevens inschakelen

![Eigenschappen van Aangepaste metagegevens](assets/custom-meta-data-properties.PNG)

## Om dit op uw server te proberen

* [CQ-mailservice op dag configureren](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html#configuring-the-mail-service)
* Een geldige e-mailid koppelen aan [beheerder](http://localhost:4502/security/users.html)
* Download en installeer de [Workflow-and-notification-template](assets/workflow-and-task-notification-template.zip) gebruiken [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* Downloaden [Adaptief formulier](assets/request-travel-authorization.zip) en importeren in AEM uit de [formulieren en documenten ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Implementeer en start de [Aangepast pakket](assets/work-items-user-service-bundle.jar) met de [webconsole](http://localhost:4502/system/console/bundles)
* [Een voorbeeld van het formulier bekijken en het formulier verzenden](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

Bij het verzenden van een formulier wordt het toewijzingsbericht verzonden naar de e-mailid die is gekoppeld aan de beheerder. De volgende schermafbeelding laat u het voorbeeld van een taaktoewijzingsmelding zien

![Melding](assets/task-nitification-email.png)

>[!NOTE]
>De e-mailsjabloon voor het taakbericht toewijzen moet de volgende indeling hebben.
>
> subject=Task toegewezen - `${workitem_title}`
>
> message=String die uw e-mailmalplaatje zonder nieuwe lijnkarakters vertegenwoordigen.

## Taakopmerkingen in E-mailbericht taak toewijzen

In sommige gevallen wilt u wellicht de opmerkingen van de vorige taakeigenaar opnemen in volgende taakmeldingen. De code die de laatste opmerking van de taak moet vastleggen, wordt hieronder weergegeven:

```java
package samples.aemforms.taskcomments.core;

import org.osgi.service.component.annotations.Component;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.HistoryItem;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;

import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=A sample implementation of a user metadata service.",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Capture Workflow Comments"
})

public class CaptureTaskComments implements WorkitemUserMetadataService {
  private static final Logger log = LoggerFactory.getLogger(CaptureTaskComments.class);
  @Override
  public Map <String, String> getUserMetadata(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metadataMap) {
    HashMap < String, String > customMetadataMap = new HashMap < String, String > ();
    workflowSession.adaptTo(Session.class);
    try {
      List <HistoryItem> workItemsHistory = workflowSession.getHistory(workItem.getWorkflow());
      int listSize = workItemsHistory.size();
      HistoryItem lastItem = workItemsHistory.get(listSize - 1);
      String reviewerComments = (String) lastItem.getWorkItem().getMetaDataMap().get("workitemComment");
      log.debug("####The comment I got was ...." + reviewerComments);
      customMetadataMap.put("comments", reviewerComments);
      log.debug("Created  " + customMetadataMap.size() + " metadata  properties");

    } catch (Exception e) {
      log.debug(e.getMessage());
    }
    return customMetadataMap;
  }

}
```

De bundel met de bovenstaande code kan [hier gedownload](assets/samples.aemforms.taskcomments.taskcomments.core-1.0-SNAPSHOT.jar)
