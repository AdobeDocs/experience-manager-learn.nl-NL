---
title: Aangepaste processtap implementeren met dialoogvenster
description: Aangepaste formulierbijlagen naar bestandssysteem schrijven met behulp van een stap voor aangepast proces
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-06-09T00:00:00Z
exl-id: 149d2c8c-bf44-4318-bba8-bec7e25da01b
duration: 135
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# Aangepaste processtap

Deze zelfstudie is bedoeld voor AEM Forms-klanten die een aangepaste workflowcomponent moeten implementeren. De eerste stap bij het maken van een workflowcomponent bestaat uit het schrijven van uw Java-code die aan de workflowcomponent wordt gekoppeld. In deze zelfstudie schrijven we eenvoudige Java-klasse voor het opslaan van de adaptieve formulierbijlagen in het bestandssysteem. Deze Java-code leest de argumenten die in de workflowcomponent zijn opgegeven.

De volgende stappen worden vereist om de klasse te schrijven java en de klasse als bundel op te stellen OSGi

## Maven Project maken

De eerste stap bestaat uit het maken van een gemodelleerd project met de juiste Adobe Maven Archetype. De gedetailleerde stappen worden in dit [artikel](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html). Zodra u uw beproefd die project hebt in eclipse wordt ingevoerd, bent u klaar beginnen uw eerste component te schrijven OSGi die in uw processtap kan worden gebruikt.


### Klasse maken die WorkflowProcess implementeert

Open het beproefde project in uw eclipse winde. Uitbreiden **projectnaam** > **kern** map. Vouw de map src/main/java uit. U moet een pakket zien dat eindigt met &quot;core&quot;. Maak een Java-klasse die WorkflowProcess in dit pakket implementeert. U moet de uitvoeringsmethode overschrijven. De handtekening van de uitvoeringsmethode is als volgt openbare void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) genereert WorkflowException

In deze zelfstudie gaan we de bijlagen die zijn toegevoegd aan Adaptief formulier naar het bestandssysteem schrijven als onderdeel van de AEM workflow.

Voor dit gebruik is de volgende Java-klasse geschreven

Laten we eens naar deze code kijken

```java
package com.mysite.core;
import java.io.File;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;
import javax.jcr.Node;
import javax.jcr.Session;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.cq.search.PredicateGroup;
import com.day.cq.search.Query;
import com.day.cq.search.QueryBuilder;
import com.day.cq.search.result.Hit;
import com.day.cq.search.result.SearchResult;
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Custom component to wrtie form attachments to file system",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Custom component to wrtie form attachments to file system"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

  private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
  @Reference
  QueryBuilder queryBuilder;

  @Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metaDataMap)
  throws WorkflowException {

    String attachmentsPath = metaDataMap.get("attachmentsPath", String.class);

    log.debug("Got attachments path: " + attachmentsPath);
    String saveToLocation = metaDataMap.get("SaveToLocation", String.class);
    log.debug("Got save location: " + saveToLocation);

    log.debug("The seperator is" + File.separator);
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    Map < String, String > map = new HashMap < String, String > ();
    map.put("path", payloadPath + "/" + attachmentsPath);
    File saveLocationFolder = new File(saveToLocation);
    if (!saveLocationFolder.exists()) {
      saveLocationFolder.mkdirs();
    }

    map.put("type", "nt:file");
    Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
    query.setStart(0);
    query.setHitsPerPage(20);

    SearchResult result = query.getResult();
    log.debug("Got  " + result.getHits().size() + " attachments ");
    Node attachmentNode = null;
    for (Hit hit: result.getHits()) {
      try {
        String path = hit.getPath();
        log.debug("The attachment title is  " + hit.getTitle() + " and the attachment path is  " + path);
        attachmentNode = workflowSession.adaptTo(Session.class).getNode(path + "/jcr:content");
        InputStream documentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
        Document attachmentDoc = new Document(documentStream);
        attachmentDoc.copyToFile(new File(saveLocationFolder + File.separator + hit.getTitle()));
        attachmentDoc.close();
      } catch (Exception e) {
        log.error("Error saving file " + e.getMessage());
      }
    }
  }
}
```


* attachmentsPath - Dit is de zelfde plaats die u in het Aangepaste Vorm hebt gespecificeerd toen u de verzendactie van Aangepast Vorm vormde om AEM Workflow aan te halen. Dit is een naam van de map waarvan u wilt dat de bijlagen worden opgeslagen in AEM ten opzichte van de lading van de workflow.

* saveToLocation - dit is de plaats die u de gehechtheid wilt worden bewaard op het dossiersysteem van uw AEM server.

Deze twee waarden worden als procesargumenten doorgegeven via het dialoogvenster van de workflowcomponent

![ProcessStep](assets/custom-workflow-component.png)

De dienst QueryBuilder wordt gebruikt aan vraagknopen van type nt:dossier onder de omslag attachmentsPath. De rest van de code doorloopt de zoekresultaten om een object Document te maken en op te slaan in het bestandssysteem


>[!NOTE]
>
>Aangezien wij het voorwerp van het Document gebruiken dat voor AEM Forms specifiek is, wordt het vereist dat u aemfd-cliënt-sdk gebiedsdeel in uw beven project omvat.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.772</version>
</dependency>
```

#### Samenstellen en implementeren

[De bundel maken zoals hier beschreven](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[Zorg ervoor dat de bundel is geïmplementeerd en actief is](http://localhost:4502/system/console/bundles)

## Volgende stappen

Maak uw [aangepaste workflowcomponent](./custom-workflow-component.md)

