---
title: Streepjescodeservice met adaptieve Forms
description: Streepjescodeservice gebruiken om streepjescode te decoderen en formuliervelden te vullen met de geëxtraheerde gegevens.
feature: Barcoded Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f89cd02d-3ffe-42c6-b547-c0445f912ee8
last-substantial-update: 2020-02-07T00:00:00Z
duration: 141
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 0%

---

# Streepjescodeservice met adaptieve Forms{#barcode-service-with-adaptive-forms}

Dit artikel toont het gebruik van de streepjescodeservice voor het invullen van een adaptief formulier. Het gebruiksgeval is als volgt:

1. De gebruiker voegt PDF met streepjescode toe als adaptieve formulierbijlage
1. Het pad van de bijlage wordt naar de servlet verzonden
1. De servlet decodeerde de streepjescode en retourneert de gegevens in JSON-indeling
1. Het adaptieve formulier wordt vervolgens ingevuld met de gedecodeerde gegevens

De volgende code decodeert de streepjescode en vult een JSON-object met de gedecodeerde waarden. De servlet retourneert vervolgens het JSON-object in zijn reactie op de aanroepende toepassing.



```java
public JSONObject extractBarCode(Document pdfDocument) {
  // TODO Auto-generated method stub
  try {
   org.w3c.dom.Document result = barcodeService.decode(pdfDocument, true, false, false, false, false, false,
     false, false, CharSet.UTF_8);
   List<org.w3c.dom.Document> listResult = barcodeService.extractToXML(result, Delimiter.Carriage_Return,
     Delimiter.Tab, XMLFormat.XDP);
   log.debug("the form1 lenght is " + listResult.get(0).getElementsByTagName("form1").getLength());
   JSONObject decodedData = new JSONObject();
   decodedData.put("name", listResult.get(0).getElementsByTagName("Name").item(0).getTextContent());
   decodedData.put("address", listResult.get(0).getElementsByTagName("Address").item(0).getTextContent());
   decodedData.put("city", listResult.get(0).getElementsByTagName("City").item(0).getTextContent());
   decodedData.put("state", listResult.get(0).getElementsByTagName("State").item(0).getTextContent());
   decodedData.put("zipCode", listResult.get(0).getElementsByTagName("ZipCode").item(0).getTextContent());
   decodedData.put("country", listResult.get(0).getElementsByTagName("Country").item(0).getTextContent());
   log.debug("The JSON Object is " + decodedData.toString());
   return decodedData;
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
 }
```

Hier volgt de servletcode. Dit servlet wordt geroepen wanneer de gebruiker een gehechtheid aan het Aangepaste Vorm toevoegt. De servlet retourneert het JSON-object terug naar de aanroepende toepassing. De aanroepende toepassing vult vervolgens het adaptieve formulier met de waarden die uit het JSON-object zijn geëxtraheerd.

```java
@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/decodebarcode"

})
public class DecodeBarCode extends SlingSafeMethodsServlet {
 @Reference
 DocumentServices documentServices;
 @Reference
 GetResolver getResolver;
 private static final Logger log = LoggerFactory.getLogger(DecodeBarCode.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  ResourceResolver fd = getResolver.getFormsServiceResolver();
  Node pdfDoc = fd.getResource(request.getParameter("pdfPath")).adaptTo(Node.class);
  Document pdfDocument = null;
  log.debug("The path of the pdf I got was " + request.getParameter("pdfPath"));
  try {
   pdfDocument = new Document(pdfDoc.getPath());
   JSONObject decodedData = documentServices.extractBarCode(pdfDocument);
   response.setContentType("application/json");
   response.setHeader("Cache-Control", "nocache");
   response.setCharacterEncoding("utf-8");
   PrintWriter out = null;
   out = response.getWriter();
   out.println(decodedData.toString());
  } catch (RepositoryException | IOException e1) {
   // TODO Auto-generated catch block
   e1.printStackTrace();
  }
 }
}
```

De volgende code maakt deel uit van de clientbibliotheek waarnaar wordt verwezen door Adaptief formulier. Wanneer een gebruiker de bijlage aan het adaptieve formulier toevoegt, wordt deze code geactiveerd. De code doet een GET vraag aan servlet met de weg van de gehechtheid die in de verzoekparameter wordt overgegaan. De gegevens die worden ontvangen van de servlet-aanroep worden vervolgens gebruikt om het aangepaste formulier in te vullen.

```javascript
$(document).ready(function()
   {
       guideBridge.on("elementValueChanged",function(event,data){
             if(data.target.name=="fileAttachment")
         {
             window.guideBridge.getFileAttachmentsInfo({
        success:function(list) 
                 {
                     console.log(list[0].name + " "+ list[0].path);
                      const getFormNames = '/bin/decodebarcode?pdfPath='+list[0].path;
      $.getJSON(getFormNames, function (data) {
                            console.log(data);
                            var nameField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Name[0]");
                            nameField.value = data.name;
                            var addressField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Address[0]");
                            addressField.value = data.address;
                            var cityField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].City[0]");
                            cityField.value = data.city;
                            var stateField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].State[0]");
                            stateField.value = data.state;
                             var zipField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Zip[0]");
                            zipField.value = data.zipCode;
                            var countryField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Country[0]");
                            countryField.value = data.country;
                        });
                        }
                  });
             }
         });
        });
```

>[!NOTE]
>
>Het adaptieve formulier dat bij dit pakket is geleverd, is gemaakt met AEM Forms 6.4. Als u dit pakket wilt gebruiken in de AEM Forms 6.3-omgeving, maakt u het adaptieve formulier in AEM formulier 6.3

Regel 12 - de code van de douane om de dienstoplosser te krijgen. Deze bundel is opgenomen als onderdeel van deze artikelen.

Regel 23 - Roep de methode DocumentServices extractBarCode aan om het JSON-object te krijgen dat is gevuld met gedecodeerde gegevens

Voer de volgende stappen uit om dit op uw systeem uit te voeren:

1. [BarcodeService.zip downloaden](assets/barcodeservice.zip) en importeren in AEM met pakketbeheer
1. [De aangepaste DocumentServices-bundel downloaden en installeren](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Download en installeer de DevelopingWithServiceUser Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Het voorbeeldformulier PDF downloaden](assets/barcode.pdf)
1. Wijs uw browser aan [adaptief voorbeeldformulier](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)
1. De meegeleverde PDF van het voorbeeld uploaden
1. De formulieren worden gevuld met gegevens
