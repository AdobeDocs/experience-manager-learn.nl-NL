---
title: Fragmenten in uitvoerservice gebruiken
description: PDF-documenten genereren met fragmenten die zich in de crx-opslagplaats bevinden
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-07-09T00:00:00Z
exl-id: d7887e2e-c2d4-4f0c-b117-ba7c41ea539a
duration: 147
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# PDF-documenten genereren met fragmenten{#developing-with-output-and-forms-services-in-aem-forms}


In dit artikel gebruiken we de uitvoerservice om PDF-bestanden te genereren met xdp-fragmenten. De hoofd xdp en de fragmenten verblijven in de crx bewaarplaats. Het is belangrijk dat u de mapstructuur van het bestandssysteem in AEM nabootst. Als u bijvoorbeeld een fragment in de fragmentmap in uw xdp gebruikt, moet u een map maken met de naam **fragmenten** onder de basismap in AEM. De basismap bevat uw basis-xdp-sjabloon. Bijvoorbeeld, als u de volgende structuur op uw dossiersysteem hebt
* c:\xdptemplates - Deze bevat uw basis-xdp-sjabloon
* c:\xdptemplates\fragments - Deze map bevat fragmenten en de hoofdsjabloon verwijst naar het fragment zoals hieronder wordt weergegeven
  ![fragment-xdp](assets/survey-fragment.png).
* De map xdpdocuments bevat uw basissjabloon en de fragmenten in **fragmenten** map

U kunt de vereiste structuur maken met de [formulieren en document-ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

Hier volgt de mapstructuur voor de voorbeeld-xdp die 2 fragmenten gebruikt
![formulieren&amp;document](assets/fragment-folder-structure-ui.png)


* Uitvoerservice - Deze service wordt doorgaans gebruikt om XML-gegevens samen te voegen met de xdp-sjabloon of pdf om samengevoegde pdf te genereren. Raadpleeg voor meer informatie de [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) voor de service Uitvoer. In dit voorbeeld gebruiken we fragmenten die zich in de crx-opslagplaats bevinden.


De volgende code is gebruikt om fragmenten in het PDF-bestand op te nemen

```java
System.out.println("I am in using fragments POST.jsp");
// contentRootURI is the base folder. All fragments are relative to this folder
String contentRootURI = request.getParameter("contentRootURI");
String xdpName = request.getParameter("xdpName");
javax.servlet.http.Part xmlDataPart = request.getPart("xmlDataFile");
System.out.println("Got xml file");
String filePath = request.getParameter("saveLocation");
java.io.InputStream xmlIS = xmlDataPart.getInputStream();
com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlIS);
com.adobe.fd.output.api.OutputService outputService = sling.getService(com.adobe.fd.output.api.OutputService.class);

if (outputService == null) {
  System.out.println("The output service is  null.....");
} else {
  System.out.println("The output service is  not null.....");

}
com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);

pdfOptions.setContentRoot(contentRootURI);

com.adobe.aemfd.docmanager.Document generatedDocument = outputService.generatePDFOutput(xdpName, xmlDocument, pdfOptions);
generatedDocument.copyToFile(new java.io.File(filePath));
out.println("Document genreated and saved to " + filePath);
```

**De monsterverpakking op uw systeem testen**

* [Download en importeer de voorbeeld-xdp-bestanden in AEM](assets/xdp-templates-fragments.zip)
* [Download en installeer het pakket met AEM pakketbeheer](assets/using-fragments-assets.zip)
* [De voorbeeld-xdp en -fragmenten kunnen hier worden gedownload](assets/xdptemplates.zip)

**Nadat u het pakket installeert, zult u volgende URLs in de Filter van de Adobe moeten lijsten van gewenste personen Granite CSRF.**

1. Volg de onderstaande stappen om de hierboven vermelde paden te lijsten van gewenste personen.
1. [Aanmelden bij configMgr](http://localhost:4502/system/console/configMgr)
1. Zoeken naar graniet-CSRF-filter voor Adobe
1. Het volgende pad toevoegen aan de uitgesloten secties en opslaan
1. /content/AemFormsSamples/usingfragments

U kunt de voorbeeldcode op verschillende manieren testen. De snelste en eenvoudigste manier is om Postman-app te gebruiken. Met Postman kunt u POSTEN aanvragen bij uw server. Installeer de Postman-toepassing op uw systeem.
Start de app en voer de volgende URL in om de API voor exportgegevens te testen

Controleer of u &quot;POST&quot; hebt geselecteerd in de vervolgkeuzelijst http://localhost:4502/content/AemFormsSamples/usingfragments.html Controleer of u &quot;Autorisatie&quot; opgeeft als &quot;Basisauth&quot;. Geef de gebruikersnaam en het wachtwoord voor AEM server op en ga naar het tabblad Body en geef de aanvraagparameters op zoals in de onderstaande afbeelding wordt getoond
![export](assets/using-fragment-postman.png)
Klik vervolgens op Verzenden

[U kunt deze postmanverzameling importeren om de API te testen](assets/usingfragments.postman_collection.json)
