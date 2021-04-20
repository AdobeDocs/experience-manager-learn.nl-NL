---
title: Taakmelding toewijzen aanpassen
description: Formuliergegevens opnemen in de e-mails met taakmeldingen toewijzen
sub-product: formulieren
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 6279
thumbnail: KT-6279.jpg
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 0%

---


# Taakmelding toewijzen aanpassen

De taakcomponent toewijzen wordt gebruikt om taken toe te wijzen aan workflowdeelnemers. Wanneer een taak aan een gebruiker of een groep wordt toegewezen, wordt een e-mailbericht verzonden naar de bepaalde gebruiker of groepsleden.
Dit e-mailbericht bevat meestal dynamische gegevens die betrekking hebben op de taak. Deze dynamische gegevens worden opgehaald met het systeem [metagegevenseigenschappen](https://docs.adobe.com/content/help/en/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html#using-system-generated-metadata-in-an-email-notification) worden gegenereerd.
Als u waarden uit de verzonden formuliergegevens wilt opnemen in het e-mailbericht, moet u een eigenschap voor aangepaste metagegevens maken en deze eigenschappen voor aangepaste metagegevens in de e-mailsjabloon gebruiken



## Aangepaste metagegevenseigenschap maken

De aanbevolen aanpak is het maken van een OSGI-component die de methode getUserMetadata van de [WorkitemUserMetadataService](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html#getUserMetadataMap--) implementeert

De volgende code maakt 4 metagegevenseigenschappen (_firstName_,_lastName_,_reason_ en _amountRequested_) en stelt de waarde ervan in op basis van de verzonden gegevens. Bijvoorbeeld, wordt het meta-gegevensbezit _firstName_ waarde geplaatst aan de waarde van het element genoemd firstName van de voorgelegde gegevens. De volgende code gaat ervan uit dat de verzonden gegevens van het adaptieve formulier de XML-indeling hebben. Adaptieve Forms op basis van JSON-schema of formuliergegevensmodel genereert gegevens in JSON-indeling.


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

In de e-mailsjabloon kunt u de eigenschap metadata opnemen door de volgende syntaxis te gebruiken waarbij amountRequested de eigenschap metadata `${amountRequested}` is

## Taak toewijzen om eigenschap voor aangepaste metagegevens te gebruiken

Nadat de component OSGi wordt gebouwd en in AEM server wordt opgesteld, vorm de Assign component van de Taak zoals hieronder getoond om de eigenschappen van douanemetagegevens te gebruiken.


![Taakmelding](assets/task-notification.PNG)

## Het gebruik van aangepaste eigenschappen van metagegevens inschakelen

![Eigenschappen van Aangepaste metagegevens](assets/custom-meta-data-properties.PNG)

## Om dit op uw server te proberen

* [CQ-mailservice op dag configureren](https://docs.adobe.com/content/help/en/experience-manager-65/administering/operations/notification.html#configuring-the-mail-service)
* Een geldige e-mailid koppelen aan [admin-gebruiker](http://localhost:4502/security/users.html)
* Download en installeer [Workflow-and-notification-template](assets/workflow-and-task-notification-template.zip) met behulp van [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* Download [Adaptief formulier](assets/request-travel-authorization.zip) en importeer in AEM van de [formulieren en documenten ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* De [Aangepaste bundel](assets/work-items-user-service-bundle.jar) implementeren en starten met de [webconsole](http://localhost:4502/system/console/bundles)
* [Een voorbeeld van het formulier bekijken en het formulier verzenden](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

Bij het verzenden van een formulier wordt het toewijzingsbericht verzonden naar de e-mailid die is gekoppeld aan de beheerder. De volgende schermafbeelding laat u het voorbeeld van een taaktoewijzingsmelding zien

![Melding](assets/task-nitification-email.png)

>[!NOTE]
>De e-mailsjabloon voor het taakbericht toewijzen moet de volgende indeling hebben.
>
> subject=Task toegewezen - `${workitem_title}`
>
> message=String die uw e-mailmalplaatje zonder nieuwe lijnkarakters vertegenwoordigen.
