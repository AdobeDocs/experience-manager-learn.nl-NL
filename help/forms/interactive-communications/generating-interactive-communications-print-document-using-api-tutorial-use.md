---
title: Interactief communicatiedocument genereren voor afdrukkanaal met behulp van controlemap
description: Gecontroleerde map gebruiken om kanaaldocumenten voor afdrukken te genereren
feature: Interactive Communication
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f5ab4801-cde5-426d-bfe4-ce0a985e25e8
last-substantial-update: 2019-07-07T00:00:00Z
duration: 115
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---

# Interactief communicatiedocument genereren voor afdrukkanaal met behulp van controlemap

Nadat u het afdrukkanaaldocument hebt ontworpen en getest, moet u het document meestal genereren door een REST-aanroep te maken of afdrukdocumenten te genereren aan de hand van het controlemap.

In dit artikel wordt uitgelegd hoe u documenten met afdrukkanalen kunt genereren aan de hand van het mechanisme voor gecontroleerde mappen.

Wanneer u een bestand neerzet in de controlemap, wordt een script uitgevoerd dat is gekoppeld aan een controlemap. Dit script wordt uitgelegd in het onderstaande artikel.

Het bestand dat naar de gecontroleerde map is neergezet, heeft de volgende structuur. De code genereert instructies voor alle accountnummers die in het XML-document worden vermeld.

&lt;accountnummers>

&lt;accountnummer>509840&lt;/accountnummer>

&lt;accountnummer>948576&lt;/accountnummer>

&lt;accountnummer>398762&lt;/accountnummer>

&lt;accountnummer>291723&lt;/accountnummer>

&lt;/accountnummers>

In de onderstaande code ziet u het volgende:

Lijn 1 - Weg aan InteractiveCommunicationsDocument

Regels 15-20: hiermee wordt de lijst met accountnummers uit het XML-document in de gecontroleerde map geplaatst

Lijnen 24 -25: krijg PrintChannelService en het Kanaal van de Druk verbonden aan het document.

Regel 30: Geef het accountnummer als het sleutelelement door aan het Model van de Gegevens van het Vorm.

Lijnen 32-36: plaats de Opties van Gegevens voor het Document dat moet worden geproduceerd.

Regel 38: Render het document.

Lijnen 39-40 - Slaat het geproduceerde document aan het dossiersysteem op.

Het REST-eindpunt van het Form Data Model verwacht een id als invoerparameter. deze id wordt in kaart gebracht aan een Attribuut van het Verzoek genoemd accountnummer zoals aangetoond in het hieronder schroevingsschot.

![ requestAttribute ](assets/requestattributeprintchannel.gif)

```java
var interactiveCommunicationsDocument = "/content/forms/af/retirementstatementprint/channels/print/";
var saveLocation =  new Packages.java.io.File("c:\\scrap\\loadtesting");

if(!saveLocation.exists())
{
 saveLocation.mkdirs();
}

var inputMap = processorContext.getInputMap();
var entry = inputMap.entrySet().iterator().next();
var inputDocument = inputMap.get(entry.getKey());
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
var resourceResolver = aemDemoListings.getServiceResolver();
var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var dbFactory = Packages.javax.xml.parsers.DocumentBuilderFactory.newInstance();
var dBuilder = dbFactory.newDocumentBuilder();
var xmlDoc = dBuilder.parse(inputDocument.getInputStream());
var nList = xmlDoc.getElementsByTagName("accountnumber");
for(var i=0;i<nList.getLength();i++)
{
 var accountnumber = nList.item(i).getTextContent();
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
   var printChannelService = sling.getService(Packages.com.adobe.fd.ccm.channels.print.api.service.PrintChannelService);
   var printChannel = printChannelService.getPrintChannel(interactiveCommunicationsDocument);
   var options = new Packages.com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
   options.setMergeDataOnServer(true);
   options.setRenderInteractive(false);
   var map = new Packages.java.util.HashMap();
   map.put("accountnumber",accountnumber);
    // Required Data Options
   var dataOptions = new Packages.com.adobe.forms.common.service.DataOptions(); 
   dataOptions.setServiceName(printChannel.getPrefillService()); 
   dataOptions.setExtras(map); 
   dataOptions.setContentType(Packages.com.adobe.forms.common.service.ContentType.JSON);
   dataOptions.setFormResource(resourceResolver.resolve(interactiveCommunicationsDocument));
            options.setDataOptions(dataOptions); 
    var printDocument = printChannel.render(options);
   var statement = new Packages.com.adobe.aemfd.docmanager.Document(printDocument.getInputStream());
            statement.copyToFile(new Packages.java.io.File(saveLocation+"\\"+accountnumber+".pdf"));

      }
   });
}
```


**om dit op uw lokaal systeem te testen gelieve de volgende instructies te volgen:**

* Opstelling Tomcat zoals die in dit [ artikel wordt beschreven.](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcat heeft het oorlogsdossier dat de steekproefgegevens produceert.
* De dienst van de opstelling alias systeemgebruiker zoals die in dit [ wordt beschreven artikel ](/help/forms/adaptive-forms/service-user-tutorial-develop.md).
Zorg ervoor dat deze systeemgebruiker lees toestemmingen op de volgende knoop heeft. Om de toestemmingenlogin aan [ gebruiker te geven admin ](https://localhost:4502/useradmin) en onderzoek naar de systeemgebruiker &quot;gegevens&quot;en de gelezen toestemmingen op de volgende knoop te geven door aan het toestemmingenlusje van labels te voorzien
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* Importeer de volgende pakketten in AEM met de pakketmanager. Dit pakket bevat het volgende:


* [Voorbeeld van interactief communicatiedocument](assets/retirementstatementprint.zip)
* [Script voor gecontroleerde mappen](assets/printchanneldocumentusingwatchedfolder.zip)
* [Source-configuratie van gegevens](assets/datasource.zip)

* Open het bestand /etc/fd/watchfolder/scripts/PrintPDF.ecma. Zorg ervoor dat het pad naar het interactiveCommunicationsDocument in regel 1 naar het juiste document verwijst dat u wilt afdrukken

* De opslaglocatie wijzigen volgens uw voorkeur op regel 2

* Het bestand accountnumbers.xml maken met de volgende inhoud

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```


* Zet de accountnumbers.xml neer in de map C:\RenderPrintChannel\input.

* De gegenereerde PDF-bestanden worden naar saveLocation geschreven, zoals in het ecma-script is opgegeven.

>[!NOTE]
>
>Als u dit wilt gebruiken op een ander besturingssysteem dan Windows, navigeert u naar
>
>/etc/fd/watchfolder /config/PrintChannelDocument en verander de folderPath volgens uw voorkeur
