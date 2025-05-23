---
title: PDF genereren met gegevens uit het op kerncomponenten gebaseerde adaptieve formulier
description: Gegevens uit op kerncomponenten gebaseerde formulierverzending samenvoegen met XDP-sjabloon in workflow
version: Experience Manager 6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-15025
last-substantial-update: 2024-02-26T00:00:00Z
exl-id: cae160f2-21a5-409c-942d-53061451b249
duration: 97
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# PDF genereren met gegevens uit de basiscomponentgebaseerde formulierverzending

Hier is de herziene tekst met &quot;Core Components&quot; met hoofdletters:

Een typisch scenario is dat een PDF wordt gegenereerd op basis van gegevens die zijn ingediend via een adaptief formulier op basis van kerncomponenten. Deze gegevens hebben altijd de JSON-indeling. Als u een PDF wilt genereren met de Render PDF API, moet u de JSON-gegevens converteren naar XML-indeling. De `toString` -methode van `org.json.XML` wordt gebruikt voor deze conversie. Voor meer details, verwijs naar de [ documentatie van `org.json.XML.toString` methode ](https://www.javadoc.io/doc/org.json/json/20171018/org/json/XML.html#toString-java.lang.Object-).

## Adaptief, op basis van JSON-schema

Ga als volgt te werk om een JSON-schema voor uw adaptieve formulier te maken:

### Voorbeeldgegevens genereren voor de XDP

Voer de volgende verfijnde stappen uit om het proces te stroomlijnen:

1. Open het XDP-bestand in AEM Forms Designer.
1. Ga naar &quot;Bestand&quot; > &quot;Formuliereigenschappen&quot; > &quot;Voorbeeld&quot;.
1. Selecteer &quot;Voorbeeldgegevens genereren&quot;.
1. Klik op &quot;Genereren&quot;.
1. Wijs een betekenisvolle bestandsnaam toe, bijvoorbeeld `form-data.xml` .

### JSON-schema genereren op basis van XML-gegevens

U kunt om het even welk vrij online hulpmiddel gebruiken om [ XML in JSON ](https://jsonformatter.org/xml-to-jsonschema) om te zetten gebruikend de gegevens van XML die in de vorige stap worden geproduceerd.

### Aangepast workflowproces voor conversie van JSON naar XML

De opgegeven code converteert JSON naar XML en slaat de resulterende XML op in een werkstroomprocesvariabele met de naam `dataXml` .

```java
import org.slf4j.LoggerFactory;
import com.adobe.granite.workflow.WorkflowException;
import java.io.InputStream;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import javax.jcr.Node;
import javax.jcr.Session;
import org.json.JSONObject;
import org.json.XML;
import org.slf4j.Logger;
import org.osgi.service.component.annotations.Component;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;

@Component(property = {
    "service.description=Convert JSON to XML",
    "process.label=Convert JSON to XML"
})
public class ConvertJSONToXML implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(ConvertJSONToXML.class);

    @Override
    public void execute(final WorkItem workItem, final WorkflowSession workflowSession, final MetaDataMap arg2) throws WorkflowException {
        String processArgs = arg2.get("PROCESS_ARGS", "string");
        log.debug("The process argument I got was " + processArgs);
        
        String submittedDataFile = processArgs;
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        log.debug("The payload in convert json to xml " + payloadPath);
        
        String dataFilePath = payloadPath + "/" + submittedDataFile + "/jcr:content";
        try {
            Session session = workflowSession.adaptTo(Session.class);
            Node submittedJsonDataNode = session.getNode(dataFilePath);
            InputStream jsonDataStream = submittedJsonDataNode.getProperty("jcr:data").getBinary().getStream();
            BufferedReader streamReader = new BufferedReader(new InputStreamReader(jsonDataStream, "UTF-8"));
            StringBuilder stringBuilder = new StringBuilder();
            String inputStr;
            while ((inputStr = streamReader.readLine()) != null) {
                stringBuilder.append(inputStr);
            }
            JSONObject submittedJson = new JSONObject(stringBuilder.toString());
            log.debug(submittedJson.toString());
            
            String xmlString = XML.toString(submittedJson);
            log.debug("The json converted to XML " + xmlString);
            
            MetaDataMap metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
            metaDataMap.put("xmlData", xmlString);
        } catch (Exception e) {
            log.error("Error converting JSON to XML: " + e.getMessage(), e);
        }
    }
}
```

### Workflow maken

Als u formulierverzendingen wilt verwerken, maakt u een workflow die twee stappen omvat:

1. In de eerste stap wordt een aangepast proces gebruikt om de verzonden JSON-gegevens te transformeren in XML.
1. De volgende stap genereert een PDF door de XML-gegevens te combineren met de XDP-sjabloon.

![ json-aan-xml ](assets/json-to-xml-process-step.png)


## De voorbeeldcode implementeren

Voer de volgende gestroomlijnde stappen uit om dit op uw lokale server te testen:

1. [ Download en installeer de douanebundel via de het Webconsole van AEM OSGi ](assets/convertJsonToXML.core-1.0.0-SNAPSHOT.jar).
1. [ voer het werkschemapakket ](assets/workflow_to_render_pdf.zip) in.
1. [ voer de steekproef Aangepaste Vorm en het malplaatje XDP ](assets/adaptive_form_and_xdp_template.zip) in.
1. [ Voorproef de Aangepaste Vorm ](http://localhost:4502/content/dam/formsanddocuments/f23/jcr:content?wcmmode=disabled).
1. Vul een aantal formuliervelden in.
1. Verzend het formulier om de AEM-workflow te starten.
1. Zoek de gerenderde PDF in de payload-map van de workflow.
