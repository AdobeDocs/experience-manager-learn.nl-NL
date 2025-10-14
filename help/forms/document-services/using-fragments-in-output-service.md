---
title: Fragmenten in uitvoerservice gebruiken
description: PDF-documenten genereren met fragmenten die zich in de crx-opslagplaats bevinden
feature: Output Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-07-09T00:00:00Z
exl-id: d7887e2e-c2d4-4f0c-b117-ba7c41ea539a
duration: 106
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# PDF-documenten genereren met fragmenten{#developing-with-output-and-forms-services-in-aem-forms}


In dit artikel gebruiken we de uitvoerservice om PDF-bestanden te genereren met xdp-fragmenten. De hoofd xdp en de fragmenten verblijven in de crx bewaarplaats. Het is belangrijk om de mapstructuur van het bestandssysteem in AEM na te bootsen. Bijvoorbeeld als u een fragment in fragmentomslag in uw xdp gebruikt moet u een omslag creëren genoemd **fragmenten** onder uw basisomslag in AEM. De basismap bevat uw basis-xdp-sjabloon. Bijvoorbeeld, als u de volgende structuur op uw dossiersysteem hebt
* c:\xdptemplates - Deze bevat uw basis-xdp-sjabloon
* c:\xdptemplates\fragments - Deze map bevat fragmenten en de hoofdsjabloon verwijst naar het fragment zoals hieronder wordt weergegeven
  ![&#x200B; fragment-xdp &#x200B;](assets/survey-fragment.png).
* De omslag xdpdocuments zal uw basissjabloon en de fragmenten in **fragments** omslag bevatten

U kunt de vereiste structuur tot stand brengen gebruikend [&#x200B; vormen en document ui &#x200B;](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

Hier volgt de mapstructuur voor de voorbeeld-xdp die 2 fragmenten gebruikt
![&#x200B; vormen&amp;document &#x200B;](assets/fragment-folder-structure-ui.png)


* Uitvoerservice - Deze service wordt doorgaans gebruikt om XML-gegevens samen te voegen met de xdp-sjabloon of pdf om samengevoegde pdf te genereren. Voor meer details, gelieve te verwijzen naar [&#x200B; javadoc &#x200B;](https://helpx.adobe.com/nl/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) voor de dienst van de Output. In dit voorbeeld gebruiken we fragmenten die zich in de crx-opslagplaats bevinden.


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

**om het steekproefpakket op uw systeem** te testen

* [Download en importeer de xdp-voorbeeldbestanden naar AEM](assets/xdp-templates-fragments.zip)
* [Download en installeer het pakket met behulp van AEM package Manager](assets/using-fragments-assets.zip)
* [De voorbeeld-xdp en -fragmenten kunnen hier worden gedownload](assets/xdptemplates.zip)

**nadat u het pakket installeert zult u volgende URLs in de Filter van Adobe moeten lijsten van gewenste personen Granite CSRF.**

1. Volg de onderstaande stappen om de hierboven vermelde paden te lijsten van gewenste personen.
1. [&#x200B; Login aan configMgr &#x200B;](http://localhost:4502/system/console/configMgr)
1. Zoeken naar Adobe Granite CSRF-filter
1. Het volgende pad toevoegen aan de uitgesloten secties en opslaan
1. /content/AemFormsSamples/usingfragments

U kunt de voorbeeldcode op verschillende manieren testen. De snelste en eenvoudigste manier is om Postman-app te gebruiken. Met Postman kunt u POST-aanvragen indienen bij uw server. Installeer de Postman-toepassing op uw systeem.
Start de app en voer de volgende URL in om de API voor exportgegevens te testen

Controleer of u &quot;POST&quot; hebt geselecteerd in de vervolgkeuzelijst
http://localhost:4502/content/AemFormsSamples/usingfragments.html
Zorg ervoor dat u &quot;Autorisatie&quot; opgeeft als &quot;Basic Auth&quot;. Geef de gebruikersnaam en het wachtwoord voor de AEM Server op
Navigeer naar het tabblad Body en geef de aanvraagparameters op, zoals in de onderstaande afbeelding wordt getoond
![&#x200B; uitvoer &#x200B;](assets/using-fragment-postman.png)
Klik vervolgens op Verzenden

[U kunt deze postmanverzameling importeren om de API te testen](assets/usingfragments.postman_collection.json)
