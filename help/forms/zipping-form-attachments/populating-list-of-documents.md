---
title: Aangepaste processtap om lijstvariabelen te vullen
description: Leer hoe u een aangepaste processtap maakt om lijstvariabelen van het type document en tekenreeks in Adobe Experience Manager in te vullen.
feature: Workflow
topic: Development
version: 6.5
role: Developer
level: Beginner
kt: kt-8063
exl-id: 09d9eabf-4815-4159-b6c7-cf2ebc8a2df5
duration: 68
source-git-commit: 52b7e6afbfe448fd350e84c3e8987973c87c4718
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---


# Aangepaste processtap

Deze gids zal u door het creëren van een stap van het douaneproces lopen om lijstvariabelen van het typeLijst van de Serie met gehechtheid en gehechtheidsnamen in Adobe Experience Manager te bevolken. Deze variabelen zijn essentieel voor de workflowcomponent E-mail verzenden.

Als u met het creëren van een bundel OSGi onbekend bent, volg deze [ instructies ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=en).

De code in de stap van het douaneproces voert de volgende acties uit:

1. Hiermee wordt gevraagd om alle adaptieve formulierbijlagen in de payload-map. De mapnaam wordt als een procesargument aan de stap doorgegeven.
2. Hiermee wordt de `listOfDocuments` workflowvariabele gevuld.
3. Hiermee wordt de `attachmentNames` workflowvariabele gevuld.
4. Hiermee wordt de waarde van de workflowvariabele `no_of_attachments` ingesteld.

```java
package com.aemforms.formattachments.core;

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
        Constants.SERVICE_DESCRIPTION + "=PopulateListOfDocuments",
        "process.label=PopulateListOfDocuments"
})
public class PopulateListOfDocuments implements WorkflowProcess {

        private static final Logger log = LoggerFactory.getLogger(PopulateListOfDocuments.class);
        @Reference
        QueryBuilder queryBuilder;

        @Override
        public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException
        {
                String payloadPath = workItem.getWorkflowData().getPayload().toString();
                log.debug("The payload path is" + payloadPath);
                MetaDataMap metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
                Session session = workflowSession.adaptTo(Session.class);
                Map < String, String > map = new HashMap < String, String > ();
                map.put("path", workItem.getWorkflowData().getPayload().toString() + "/" + processArguments.get("PROCESS_ARGS", "string").toString());
                map.put("type", "nt:file");
                Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
                query.setStart(0);
                query.setHitsPerPage(20);
                SearchResult result = query.getResult();
                log.debug("Get result hits " + result.getHits().size());
                int no_of_attachments = result.getHits().size();
                Document[] listOfDocuments = new Document[no_of_attachments];
                String[] attachmentNames = new String[no_of_attachments];
                int i = 0;
                for (Hit hit: result.getHits()) {
                        try {
                                String attachmentPath = hit.getPath();
                                log.debug("The hit path is" + hit.getPath());
                                Node attachmentNode = session.getNode(attachmentPath + "/jcr:content");
                                InputStream attachmentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
                                listOfDocuments[i] = new Document(attachmentStream);
                                attachmentNames[i] = new String(hit.getTitle());
                                log.debug("Added " + hit.getTitle() + "to the list");
                                i++;
                        } catch (Exception e) {
                                log.error("Unable to obtain attachment", e);
                        }
                }

                metaDataMap.put("no_of_attachments", no_of_attachments);
                metaDataMap.put("listOfDocuments", listOfDocuments);
                metaDataMap.put("attachmentNames", attachmentNames);

                log.debug("Updated workflow");
        }

}
```

>[!NOTE]
>
> Zorg ervoor dat de code werkt als de volgende variabelen in de workflow zijn gedefinieerd:
> 
> - `listOfDocuments`: variabele van het type ArrayList of Documents
> - `attachmentNames`: variabele van het type ArrayList of String
> - `no_of_attachments`: variabele van het type Double

## Volgende stappen

[Test de oplossing op uw lokale systeem](./test.md)