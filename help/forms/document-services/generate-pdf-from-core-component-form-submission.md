---
title: PDF genereren met gegevens uit het op kerncomponenten gebaseerde adaptieve formulier
description: Gegevens uit op kerncomponenten gebaseerde formulierverzending samenvoegen met XDP-sjabloon in workflow
version: 6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-15025
last-substantial-update: 2024-02-26T00:00:00Z
exl-id: cae160f2-21a5-409c-942d-53061451b249
duration: 97
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# PDF genereren met gegevens uit de op kerncomponenten gebaseerde formulierverzending

Hier is de herziene tekst met &quot;Core Components&quot; met hoofdletters:

Een typisch scenario is het genereren van een PDF op basis van gegevens die zijn ingediend via een adaptief formulier op basis van kerncomponenten. Deze gegevens hebben altijd de JSON-indeling. Als u een PDF wilt genereren met de API voor het renderen van de PDF, is het nodig dat u de JSON-gegevens omzet in XML-indeling. De `toString` methode `org.json.XML` wordt gebruikt voor deze conversie. Raadpleeg voor meer informatie de [documentatie van `org.json.XML.toString` methode](https://www.javadoc.io/doc/org.json/json/20171018/org/json/XML.html#toString-java.lang.Object-).

## Adaptief, op basis van JSON-schema

Ga als volgt te werk om een JSON-schema voor uw adaptieve formulier te maken:

### Voorbeeldgegevens genereren voor de XDP

Voer de volgende verfijnde stappen uit om het proces te stroomlijnen:

1. Open het XDP-bestand in AEM Forms Designer.
1. Ga naar &quot;Bestand&quot; > &quot;Formuliereigenschappen&quot; > &quot;Voorbeeld&quot;.
1. Selecteer &quot;Voorbeeldgegevens genereren&quot;.
1. Klik op &quot;Genereren&quot;.
1. Wijs een betekenisvolle bestandsnaam toe, zoals `form-data.xml`.

### JSON-schema genereren op basis van XML-gegevens

U kunt elke gratis online tool gebruiken om [XML converteren naar JSON](https://jsonformatter.org/xml-to-jsonschema) de XML-gegevens gebruiken die in de vorige stap zijn gegenereerd.

### Aangepast workflowproces voor conversie van JSON naar XML

De opgegeven code converteert JSON naar XML en slaat de resulterende XML op in een variabele van het workflowproces met de naam `dataXml`.

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

![json-to-xml](assets/json-to-xml-process-step.png)


## De voorbeeldcode implementeren

Voer de volgende gestroomlijnde stappen uit om dit op uw lokale server te testen:

1. [Download en installeer de aangepaste bundel via de AEM OSGi-webconsole](assets/convertJsonToXML.core-1.0.0-SNAPSHOT.jar).
1. [Het workflowpakket importeren](assets/workflow_to_render_pdf.zip).
1. [De voorbeeldsjabloon Adaptief formulier en XDP importeren](assets/adaptive_form_and_xdp_template.zip).
1. [Voorbeeld van adaptief formulier bekijken](http://localhost:4502/content/dam/formsanddocuments/f23/jcr:content?wcmmode=disabled).
1. Vul een aantal formuliervelden in.
1. Verzend het formulier om de AEM te starten.
1. Zoek de gerenderde PDF in de payload-map van de workflow.
