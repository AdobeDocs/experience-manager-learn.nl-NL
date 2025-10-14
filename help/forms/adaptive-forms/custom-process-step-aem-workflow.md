---
title: Aangepaste processtap implementeren
description: Aangepaste formulierbijlagen naar het bestandssysteem schrijven met behulp van een aangepaste processtap
feature: Workflow
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 879518db-3f05-4447-86e8-5802537584e5
last-substantial-update: 2021-06-09T00:00:00Z
duration: 203
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# Aangepaste processtap

Deze zelfstudie is bedoeld voor AEM Forms-klanten die een aangepaste processtap moeten implementeren. Een processtap kan een ECMA-script uitvoeren of aangepaste Java™-code aanroepen om bewerkingen uit te voeren. Dit leerprogramma zal de stappen verklaren nodig om WorkflowProcess uit te voeren dat door de processtap wordt uitgevoerd.

De belangrijkste reden voor het implementeren van een stap voor een aangepast proces is het uitbreiden van de AEM-workflow. Als u bijvoorbeeld AEM Forms-componenten in uw workflowmodel gebruikt, kunt u de volgende bewerkingen uitvoeren

* De adaptieve formulierbijlage(s) opslaan naar het bestandssysteem
* De ingediende gegevens bewerken

Om het bovengenoemde gebruikscase te verwezenlijken, zult u typisch een dienst OSGi schrijven die door de processtap wordt uitgevoerd.

## Maven Project maken

De eerste stap bestaat uit het maken van een gemaven project met behulp van het juiste Adobe Maven Archetype. De gedetailleerde stappen worden vermeld in dit [&#x200B; artikel &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=nl-NL). Zodra u uw Geweven project hebt die in Eclipse wordt ingevoerd, bent u klaar beginnen uw eerste component te schrijven OSGi die in uw processtap kan worden gebruikt.


### Klasse maken die WorkflowProcess implementeert

Open het Geweven project in uw winde van de Verduistering. Breid **projectnaam** > **kern** omslag uit. Vouw de map `src/main/java` uit. Er wordt een pakket weergegeven dat eindigt met `core` . Maak een Java™-klasse die WorkflowProcess in dit pakket implementeert. U moet de uitvoeringsmethode overschrijven. De uitvoeringsmethode is als volgt ondertekend:

```java
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException 
```

De methode execute geeft toegang tot de volgende drie variabelen:

**WorkItem**: De variabele workItem zal toegang tot gegevens verlenen met betrekking tot werkschema. De openbare API documentatie is beschikbaar [&#x200B; hier.](https://helpx.adobe.com/nl/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**: Deze variabele workflowSession zal u de capaciteit geven om het werkschema te controleren. De openbare API documentatie is beschikbaar [&#x200B; hier &#x200B;](https://helpx.adobe.com/nl/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html).

**MetaDataMap**: Alle meta-gegevens verbonden aan het werkschema. Alle procesargumenten die aan de processtap worden doorgegeven, zijn beschikbaar via het object MetaDataMap.[&#x200B; API Documentatie &#x200B;](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

In deze zelfstudie schrijven we de bijlagen die zijn toegevoegd aan Adaptief formulier naar het bestandssysteem als onderdeel van de AEM-workflow.

Voor dit gebruik is de volgende Java™-klasse geschreven

Laten we deze code bekijken

```java
package com.learningaemforms.adobe.core;

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
    Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
    Constants.SERVICE_VENDOR + "=Adobe Systems",
    "process.label" + "=Save Adaptive Form Attachments to File System"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
    @Reference
    QueryBuilder queryBuilder;

    @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)
    throws WorkflowException {
        // TODO Auto-generated method stub
        log.debug("The string I got was ..." + processArguments.get("PROCESS_ARGS", "string").toString());
        String[] params = processArguments.get("PROCESS_ARGS", "string").toString().split(",");
        String attachmentsPath = params[0];
        String saveToLocation = params[1];
        log.debug("The seperator is" + File.separator);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        Map<String, String> map = new HashMap<String, String> ();
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
                log.debug("Error saving file " + e.getMessage());
            }
```

Regel 1 - bepaalt de eigenschappen voor onze component. De eigenschap `process.label` is wat u ziet wanneer u een OSGi-component aan de processtap koppelt, zoals in een van de onderstaande schermafbeeldingen wordt getoond.

Lijnen 13-15 - De procesargumenten die tot deze component OSGi worden overgegaan zijn verdeeld gebruikend &quot;,&quot;separator. De waarden voor attachPath en saveToLocation worden dan gehaald uit de koordserie.

* BijlagePath - Dit is de zelfde plaats die u in het Aangepaste Vorm hebt gespecificeerd toen u de verzendactie van Aangepast Vorm vormde om de Workflow van AEM aan te halen. Dit is een naam van de map waarvan u wilt dat de bijlagen in AEM worden opgeslagen ten opzichte van de lading van de workflow.

* saveToLocation - dit is de plaats die u de gehechtheid wilt worden bewaard op het dossiersysteem van uw AEM server.

Deze twee waarden worden doorgegeven als procesargumenten, zoals in de onderstaande schermafbeelding wordt getoond.

![&#x200B; ProcessStep &#x200B;](assets/implement-process-step.gif)

De dienst QueryBuilder wordt gebruikt aan vraagknopen van type `nt:file` onder de omslag attachmentsPath. De rest van de code doorloopt de zoekresultaten om een object Document te maken en op te slaan in het bestandssysteem.


>[!NOTE]
>
>Aangezien wij een voorwerp van het Document gebruiken dat voor AEM Forms specifiek is, wordt het vereist dat u de aemfd-cliënt-sdk gebiedsdeel in uw gegeven project omvat. De groep-id is `com.adobe.aemfd` en de artefacten-id is `aemfd-client-sdk` .

#### Samenstellen en implementeren

[&#x200B; bouwt de bundel zoals hier beschreven &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=nl-NL)
[&#x200B; zorg ervoor de bundel wordt opgesteld en in actieve staat &#x200B;](http://localhost:4502/system/console/bundles)

Maak een workflowmodel. De stap van het proces van de belemmering en van het dalingsproces in het werkschemamodel. Koppel de processtap aan &quot;Aangepaste formulierbijlagen opslaan naar bestandssysteem&quot;.

Geef de benodigde procesargumenten op, gescheiden door een komma. Bijvoorbeeld Bijlagen,c:\\scrappp\\. Het eerste argument is de map waarin uw adaptieve formulierbijlagen worden opgeslagen ten opzichte van de lading van de workflow. Dit moet dezelfde waarde zijn als u hebt opgegeven bij het configureren van de verzendactie van Adaptief formulier. Het tweede argument is de locatie waar u de bijlagen wilt opslaan.

Maak een adaptief formulier. Sleep de component Bestandsbijlagen naar het formulier. Configureer de verzendactie van het formulier om de workflow aan te roepen die in de eerdere stappen is gemaakt. Geef het juiste pad voor de bijlage op.

Sla de instellingen op.

Bekijk een voorbeeld van het formulier. Voeg een aantal bijlagen toe en verzend het formulier. De bijlagen moeten in het bestandssysteem worden opgeslagen op de locatie die u in de workflow hebt opgegeven.
