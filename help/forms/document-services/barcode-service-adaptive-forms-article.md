---
title: Streepjescodeservice met adaptieve Forms
seo-title: Streepjescodeservice met adaptieve Forms
description: Streepjescodeservice gebruiken om streepjescode te decoderen en formuliervelden te vullen met de geëxtraheerde gegevens
seo-description: Streepjescodeservice gebruiken om streepjescode te decoderen en formuliervelden te vullen met de geëxtraheerde gegevens
uuid: 42568b81-cbcd-479e-8d9a-cc0b244da4ae
feature: barcoded-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 1224de6d-7ca1-4e9d-85fe-cd675d03e262
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---


# Streepjescodeservice met adaptieve Forms{#barcode-service-with-adaptive-forms}

Dit artikel toont het gebruik van de streepjescodeservice voor het invullen van een adaptief formulier. Het gebruiksgeval is als volgt:

1. De gebruiker voegt PDF toe met streepjescode als adaptieve formulierbijlage
1. Het pad van de bijlage wordt naar de servlet verzonden
1. De servlet decodeerde de streepjescode en retourneert de gegevens in JSON-indeling
1. Het adaptieve formulier wordt vervolgens gevuld met behulp van de gedecodeerde gegevens

De volgende code decodeert de streepjescode en vult een JSON-object met de gedecodeerde waarden. De servlet retourneert vervolgens het JSON-object in zijn reactie op de aanroepende toepassing.

U kunt deze mogelijkheid live zien, bezoek dan de [portal samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) en zoek naar de demo van de streepjescodeservice

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

De volgende code maakt deel uit van de clientbibliotheek waarnaar wordt verwezen door Adaptief formulier. Wanneer een gebruiker de bijlage aan het aangepaste formulier toevoegt, wordt deze code geactiveerd. De code doet een GET vraag aan servlet met de weg van de gehechtheid die in de verzoekparameter wordt overgegaan. De gegevens die worden ontvangen van de servlet-aanroep worden vervolgens gebruikt om het aangepaste formulier in te vullen.

```
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

Regel 12 - de code van de douane om de dienstoplosser te krijgen. Deze bundel maakt deel uit van deze artikelen.

Regel 23 - Roep de methode DocumentServices extractBarCode aan om het JSON-object te krijgen dat is gevuld met gedecodeerde gegevens

Voer de volgende stappen uit om dit op uw systeem uit te voeren

1. [Download BarcodeService.](assets/barcodeservice.zip) zip en importeer in AEM met behulp van pakketbeheer
1. [De aangepaste DocumentServices-bundel downloaden en installeren](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Download en installeer de DevelopingWithServiceUser Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Het PDF-voorbeeldformulier downloaden](assets/barcode.pdf)
1. Wijs uw browser naar het [voorbeeldadaptieve formulier](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)
1. Het voorbeeld-PDF uploaden
1. De formulieren worden gevuld met gegevens

