---
title: Kanaaldocument afdrukken genereren door gegevens samen te voegen
seo-title: Kanaaldocument afdrukken genereren door gegevens samen te voegen
description: Leer hoe u een document met een afdrukkanaal genereert door de gegevens in de invoerstroom samen te voegen
seo-description: Leer hoe u een document met een afdrukkanaal genereert door de gegevens in de invoerstroom samen te voegen
feature: Interactieve communicatie
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Ontwikkeling
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 0%

---

# Kanaaldocumenten afdrukken met behulp van verzonden gegevens

Afdrukkanaaldocumenten worden doorgaans gegenereerd door gegevens op te halen van een gegevensbron op de achtergrond via de ophalen-service van het formuliergegevensmodel. In sommige gevallen moet u mogelijk afdrukkanaaldocumenten genereren met de verschafte gegevens. De klant vult bijvoorbeeld de wijziging van het formulier in en u wilt mogelijk een document voor het afdrukkanaal genereren met gegevens uit het verzonden formulier. Om dit te bereiken moet deze gebruikscase de volgende stappen volgen

## Prefill-service maken

De de dienstnaam &quot;ccm-druk-test&quot;zal worden gebruikt om tot deze dienst toegang te hebben. Zodra deze voorvulservice is gedefinieerd, kunt u deze service openen in uw servlet of in de implementatie van de stap voor workflowproces om het document voor het afdrukkanaal te genereren.

```java
import java.io.InputStream;
import org.osgi.service.component.annotations.Component;

import com.adobe.forms.common.service.ContentType;
import com.adobe.forms.common.service.DataOptions;
import com.adobe.forms.common.service.DataProvider;
import com.adobe.forms.common.service.FormsException;
import com.adobe.forms.common.service.PrefillData;

@Component(immediate = true, service = {DataProvider.class})
public class ICPrefillService implements DataProvider {

@Override
public String getServiceDescription() {
    // TODO Auto-generated method stub
    return "Prefill Service for IC Print Channel";
}

@Override
public String getServiceName() {
    // TODO Auto-generated method stub
    return "ccm-print-test";
}

@Override
public PrefillData getPrefillData(DataOptions options) throws FormsException {
    // TODO Auto-generated method stub
        PrefillData data = null;
        if (options != null && options.getExtras() != null && options.getExtras().get("data") != null) {
            InputStream is = (InputStream) options.getExtras().get("data");
            data = new PrefillData(is, options.getContentType() != null ? options.getContentType() : ContentType.JSON);
        }
        return data;
    }
}
```

### WorkflowProcess-implementatie maken

Het codefragment voor workflowimplementatie wordt hieronder weergegeven. Deze code wordt uitgevoerd wanneer processtap in de AEM Workflow aan deze implementatie is gekoppeld. Deze implementatie verwacht drie procesargumenten die hieronder worden beschreven:

* Naam van het pad DataFile dat is opgegeven bij het configureren van het adaptieve formulier
* Naam van de afdruksjabloon
* Naam van het gegenereerde afdrukkanaaldocument

Regel 98 - Aangezien het Aangepaste Vorm op het Model van de Gegevens van het Vorm wordt gebaseerd, worden de gegevens die in de gegevensknoop van afBoundData verblijven gehaald.
Regel 128 - De de dienstnaam van de Opties van Gegevens wordt geplaatst. Noteer de servicenaam. Deze moet overeenkomen met de naam die is geretourneerd in regel 45 van de vorige code.
Regel 135 - Het document wordt geproduceerd gebruikend de teruggevende methode van het voorwerp PrintChannel


```java
String params = arg2.get("PROCESS_ARGS","string").toString();
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    String dataFile = params.split(",")[0];
    final String icFileName = params.split(",")[1];
    String dataFilePath = payloadPath + "/"+dataFile+"/jcr:content";
    Session session = workflowSession.adaptTo(Session.class);
    Node xmlDataNode = null;
    try {
        xmlDataNode = session.getNode(dataFilePath);
        InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
        JsonParser jsonParser = new JsonParser();
        BufferedReader streamReader = null;
        try {
            streamReader = new BufferedReader(new InputStreamReader(xmlDataStream, "UTF-8"));
        } catch (UnsupportedEncodingException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        StringBuilder responseStrBuilder = new StringBuilder();
        String inputStr;
        try {
            while ((inputStr = streamReader.readLine()) != null)
                responseStrBuilder.append(inputStr);
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        String submittedDataXml = responseStrBuilder.toString();
        JsonObject jsonObject = jsonParser.parse(submittedDataXml).getAsJsonObject().get("afData").getAsJsonObject()
                .get("afBoundData").getAsJsonObject().get("data").getAsJsonObject();
        logger.info("Successfully Parsed gson" + jsonObject.toString());
        InputStream targetStream = IOUtils.toInputStream(jsonObject.toString());
        //InputStream targetStream = new ByteArrayInputStream(jsonObject.toString().getBytes());
        
        // Node dataNode = session.getNode(formName);
        logger.info("Got resource using resource resolver"
                );
        resHelper.callWith(getResolver.getFormsServiceResolver(), new Callable<Void>() {
            @Override
            public Void call() throws Exception {
                System.out.println("The target stream is "+targetStream.available());
                // TODO Auto-generated method stub
                com.adobe.fd.ccm.channels.print.api.model.PrintChannel printChannel = null;
                String formName = params.split(",")[2];
                logger.info("The form name I got was "+formName);
                printChannel = printChannelService.getPrintChannel(formName);
                logger.info("Did i get print channel?");
                com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions options = new com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
                options.setMergeDataOnServer(true);
                options.setRenderInteractive(false);
                com.adobe.forms.common.service.DataOptions dataOptions = new com.adobe.forms.common.service.DataOptions();
                dataOptions.setServiceName(printChannel.getPrefillService());
                // dataOptions.setExtras(map);
                dataOptions.setContentType(ContentType.JSON);
                logger.info("####Set the content type####");
                dataOptions.setFormResource(getResolver.getFormsServiceResolver().getResource(formName));
                dataOptions.setServiceName("ccm-print-test");
                dataOptions.setExtras(new HashMap<String, Object>());
                dataOptions.getExtras().put("data", targetStream);
                options.setDataOptions(dataOptions);
                logger.info("####Set the data options");
                com.adobe.fd.ccm.channels.print.api.model.PrintDocument printDocument = printChannel
                .render(options);
                logger.info("####Generated the document");
                com.adobe.aemfd.docmanager.Document uploadedDocument = new com.adobe.aemfd.docmanager.Document(
                    printDocument.getInputStream());
                logger.info("Generated the document");
                Binary binary = session.getValueFactory().createBinary(printDocument.getInputStream());
                Session jcrSession = workflowSession.adaptTo(Session.class);
                String dataFilePath = workItem.getWorkflowData().getPayload().toString();
                
                Node dataFileNode = jcrSession.getNode(dataFilePath);
                Node icPdf = dataFileNode.addNode(icFileName, "nt:file");
                Node contentNode = icPdf.addNode("jcr:content", "nt:resource");
                contentNode.setProperty("jcr:data", binary);
                jcrSession.save();
                logger.info("Copied the generated document");
                uploadedDocument.close();
                
                return null;
            }
```

Voer de volgende stappen uit om dit op uw server te testen:

* [Configureer de Day CQ Mail Service.](https://helpx.adobe.com/experience-manager/6-5/communities/using/email.html) Dit is nodig om e-mail te verzenden met het document dat als bijlage is gegenereerd.
* [Implementeer de Developing with Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Controleer of u de volgende vermelding hebt toegevoegd in de configuratie van de service Gebruikerskaart van de Apache Sling Service
* **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [De aan dit artikel gerelateerde elementen downloaden en uitpakken naar uw bestandssysteem](assets/prefillservice.zip)
* [De volgende pakketten importeren met AEM pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
   1. beneficiaryconfirmationic.zip
   2. changeofbeneficiaryform.zip
   3. generatebeneficiaryworkflow.zip
* [Het volgende implementeren met AEM Felix Web Console](http://localhost:4502/system/console/bundles)

   * GenerateIC.GenerateIC.core-1.0-SNAPSHOT.jar. Deze bundel bevat de code die in dit artikel wordt vermeld.

* [Open ChangeOfBeneficiaryForm](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled)
* Zorg ervoor dat het adaptieve formulier is geconfigureerd voor verzending naar AEM workflow, zoals hieronder wordt weergegeven
   ![afbeelding](assets/generateic.PNG)
* [Configureer het workflowmodel.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ChangesToBeneficiary.html)Zorg ervoor dat de processtap en het verzenden van e-mailcomponenten zijn geconfigureerd volgens uw omgeving
* [Geef een voorvertoning weer van het formulier ChangeOfBeneficiary.](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled) Vul enkele details in en verzend
* De workflow moet worden aangeroepen en het document met het IC-afdrukkanaal moet worden verzonden naar de ontvanger die is opgegeven in het onderdeel E-mail verzenden als bijlage
