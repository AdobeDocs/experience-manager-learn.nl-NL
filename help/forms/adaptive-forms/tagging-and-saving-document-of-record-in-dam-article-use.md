---
title: AEM Forms DoR coderen en opslaan in DAM
description: Dit artikel zal door het gebruik geval lopen van het opslaan en etiketteren van DoR die door AEM Forms in AEM DAM wordt geproduceerd. Het document wordt gelabeld op basis van de verzonden formuliergegevens.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 832f04b4-f22f-4cf9-8136-e3c1081de7a9
last-substantial-update: 2019-03-20T00:00:00Z
duration: 195
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# AEM Forms DoR coderen en opslaan in DAM {#tagging-and-storing-aem-forms-dor-in-dam}

Dit artikel zal door het gebruik geval lopen van het opslaan en etiketteren van DoR die door AEM Forms in AEM DAM wordt geproduceerd. Het document wordt gelabeld op basis van de verzonden formuliergegevens.

Een veelvoorkomende vraag van klanten is het opslaan en labelen van het Document of Record (DoR) dat door AEM Forms in AEM DAM wordt gegenereerd. De codering van het document moet gebaseerd zijn op de door Adaptive Forms ingediende gegevens. Als de werkgelegenheidsstatus in de verzonden gegevens bijvoorbeeld &quot;In ruste&quot; is, willen we het document labelen met de tag &quot;In ruste&quot; en het document opslaan in DAM.

Het gebruiksgeval is als volgt:

* Een gebruiker vult het adaptieve formulier in. In het adaptieve formulier worden de huwelijkse staat (ex Single) en de arbeidsstatus (Ex Reensioen) van de gebruiker vastgelegd.
* Bij het verzenden van formulieren wordt een AEM workflow geactiveerd. Met deze workflow wordt het document gelabeld met de burgerlijke staat (Single) en de werkgelegenheidsstatus (In ruste) en wordt het document opgeslagen in DAM.
* Nadat het document is opgeslagen in DAM, kan de beheerder het document op basis van deze codes doorzoeken. Bijvoorbeeld, zou het onderzoek op Enige of In ruste de aangewezen Dor&#39;s halen.

Om aan dit gebruiksgeval te voldoen is een stap van het douaneproces geschreven. In deze stap halen wij de waarden van de aangewezen gegevenselementen uit de voorgelegde gegevens. Vervolgens wordt de labeltegel gemaakt met deze waarde. Als bijvoorbeeld de waarde van het element van de burgerlijke staat &#39;Single&#39; is, wordt de titel van de tag **Peak:EmploymentStatus/Single. **Met behulp van de API TagManager vinden we de tag en passen we de tag toe op de DoR.

Hier volgt de volledige code voor het coderen en opslaan van het Document of Record in AEM DAM.

```java
package com.aemforms.setvalue.core;
import java.io.InputStream;
import javax.jcr.Node;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowData;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.adobe.granite.workflow.model.WorkflowModel;
import com.day.cq.tagging.Tag;
import com.day.cq.tagging.TagManager;

@Component(property = {
   Constants.SERVICE_DESCRIPTION + "=Tag and Store Dor in DAM",
   Constants.SERVICE_VENDOR + "=Adobe Systems",
   "process.label" + "=Tag and Store Dor in DAM"
})
@Designate(ocd = TagDorServiceConfiguration.class)
public class TagAndStoreDoRinDAM implements WorkflowProcess
{
   private static final Logger log = LoggerFactory.getLogger(TagAndStoreDoRinDAM.class);

   private TagDorServiceConfiguration serviceConfig;
   @Activate
   public void activate(TagDorServiceConfiguration config)
   {
      this.serviceConfig = config;
   }
   @Override
   public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException
   {
       log.debug("The process arguments passed ..." + arg2.get("PROCESS_ARGS", "string").toString());
      String params = arg2.get("PROCESS_ARGS", "string").toString();
      WorkflowModel wfModel = workflowSession.getModel("/var/workflow/models/dam/update_asset");
      // Read the Tag DoR service configuration
      String damFolder = serviceConfig.damFolder();
      String dorPDFName = serviceConfig.dorPath();
      String dataXmlFile = serviceConfig.dataFilePath();
      log.debug("The Data Xml File is ..." + dataXmlFile + "DorPDFName" + dorPDFName);
      // Read the arguments passed to this workflow step
      String parameters[] = params.split(",");
      log.debug("The %%%% length of parameters is " + parameters.length);
      Tag[] tagArray = new Tag[parameters.length];
      WorkflowData wfData = workItem.getWorkflowData();
      String dorFileName = (String) wfData.getMetaDataMap().get("filename");
      log.debug("The dorFileName is ..." + dorFileName);
      String payloadPath = workItem.getWorkflowData().getPayload().toString();
      String dataFilePath = payloadPath + "/" + dataXmlFile + "/jcr:content";
      String dorDocumentPath = payloadPath + "/" + dorPDFName + "/jcr:content";
      log.debug("Data File Path" + dataFilePath);
      log.debug("Dor File Path" + dorDocumentPath);
      Session session = workflowSession.adaptTo(Session.class);
      ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
      com.day.cq.dam.api.AssetManager assetMgr = resourceResolver.adaptTo(com.day.cq.dam.api.AssetManager.class);
      DocumentBuilderFactory factory = null;
      DocumentBuilder builder = null;
      Document xmlDocument = null;
      Node xmlDataNode = null;
      Node dorDocumentNode = null;

      try
      {
         // create org.w3c.dom.Document object from submitted form data
         xmlDataNode = session.getNode(dataFilePath);
         log.debug("xml Data Node" + xmlDataNode.getName());
         dorDocumentNode = session.getNode(dorDocumentPath);
         log.debug("DOR Document Node is " + dorDocumentNode.getName());
         InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
         InputStream dorInputStream = dorDocumentNode.getProperty("jcr:data").getBinary().getStream();
         XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
         factory = DocumentBuilderFactory.newInstance();
         builder = factory.newDocumentBuilder();
         xmlDocument = builder.parse(xmlDataStream);
         String newFile = "/content/dam/" + damFolder + "/" + dorFileName;
         log.debug("the new file is ..." + newFile);
         // Store the DoR in DAM
         assetMgr.createAsset(newFile, dorInputStream, "application/pdf", true);
         WorkflowData wfDataLoad = workflowSession.newWorkflowData("JCR_PATH", newFile);
         log.debug("Wrote the document to DAM" + newFile);
         TagManager tagManager = resourceResolver.adaptTo(TagManager.class);
         Resource pdfDocumentNode = resourceResolver.getResource(newFile);
         Resource metadata = pdfDocumentNode.getChild("jcr:content/metadata");
         // Fetch the xml elements from the xml document
         for (int i = 0; i < parameters.length; i++)
            {
                String tagTitle = parameters[i].split("=")[0];
                log.debug("The tag title is" + tagTitle);
                String nameOfNode = parameters[i].split("=")[1];
                org.w3c.dom.Node xmlElement = (org.w3c.dom.Node) xPath.compile(nameOfNode).evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
                log.debug("###The value data node is " + xmlElement.getTextContent());
                Tag tagFound = tagManager.resolveByTitle(tagTitle + xmlElement.getTextContent());
                log.debug("The tag found was ..." + tagFound.getPath());
                tagArray[i] = tagFound;
            }
         tagManager.setTags(metadata, tagArray, true);
         workflowSession.startWorkflow(wfModel, wfDataLoad);
         log.debug("Workflow started");
         log.debug("Done setting tags");
         xmlDataStream.close();
         dorInputStream.close();
      } catch (Exception e)
            {
                 log.debug("The error message is " + e.getMessage());
            }

   }

}
```

Volg onderstaande stappen om dit voorbeeld op uw systeem te laten werken:
* [De gebruikersbundel DevelopingWithService implementeren](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [De setvalue-bundel downloaden en implementeren](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dit is de aangepaste OSGI-bundel die de codes instelt van de verzonden formuliergegevens.

* [Download het voorbeeldadaptieve formulier](assets/tag-and-store-in-dam-adaptive-form.zip)

* [Ga naar Forms en Documenten](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Klik op Maken | Bestand uploaden en uploaden van de tag-and-store-in-dam-adaptive-form.zip

* [Artikelelementen importeren](assets/tag-and-store-in-dam-assets.zip) met AEM pakketbeheer
* Open de [voorbeeldformulier in voorbeeldmodus](http://localhost:4502/content/dam/formsanddocuments/tagandstoreindam/jcr:content?wcmmode=disabled). **Alle velden invullen** en het formulier verzenden.
* [Navigeren naar de map Peak in DAM](http://localhost:4502/assets.html/content/dam/Peak). U zou DoR in de Piekomslag moeten zien. Controleer de eigenschappen van het document. Het moet op passende wijze worden gelabeld.
Gefeliciteerd! U hebt het voorbeeld op uw systeem geïnstalleerd

* Laten we de [werkstroom](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) die wordt geactiveerd bij het verzenden van het formulier.
* De eerste stap in de workflow maakt een unieke bestandsnaam door de naam van de aanvrager en het land van verblijf samen te voegen.
* De tweede stap van de workflow gaat over de taghiërarchie en de formulierveldelementen die moeten worden gecodeerd. De processtap extraheert de waarde uit de verzonden gegevens en bouwt de codetitel die het document moet labelen.
* Als u DoR in een verschillende omslag in DAM wilt opslaan, specificeert u de omslagplaats gebruikend de configuratieeigenschappen zoals die in het hieronder screenshot worden gespecificeerd.

De andere twee parameters zijn specifiek voor DoR en de Weg van het Dossier van Gegevens zoals die in de Adaptieve de indieningsopties van de Vorm worden gespecificeerd. Zorg ervoor dat de waarden die u hier opgeeft, overeenkomen met de waarden die u hebt opgegeven in de verzendopties voor Adaptief formulier.

![Tag-dor](assets/tag_dor_service_configuration.gif)
