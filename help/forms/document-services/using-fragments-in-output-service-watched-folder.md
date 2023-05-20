---
title: Fragmenten in uitvoerservice gebruiken met gecontroleerde map
description: PDF-documenten genereren met fragmenten die zich in de crx-opslagplaats bevinden
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-09-07T00:00:00Z
thumbnail: ecma-fragments.jpg
exl-id: 6b0bd2f1-b8ee-4f96-9813-8c11aedd3621
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# PDF-document met fragmenten genereren met behulp van ECMA-script{#developing-with-output-and-forms-services-in-aem-forms}


In dit artikel gebruiken we de uitvoerservice om PDF-bestanden te genereren met behulp van xdp-fragmenten. De hoofd xdp en de fragmenten verblijven in de crx bewaarplaats. Het is belangrijk dat u de mapstructuur van het bestandssysteem in AEM nabootst. Als u bijvoorbeeld een fragment in de fragmentmap in uw xdp gebruikt, moet u een map maken met de naam **fragmenten** onder de basismap in AEM. De basismap bevat uw basis-xdp-sjabloon. Bijvoorbeeld, als u de volgende structuur op uw dossiersysteem hebt
* c:\xdptemplates - This will contain your base xdp template
* c:\xdptemplates\fragments - This folder will contain fragments and the main template will reference the fragment as shown below
   ![fragment-xdp](assets/survey-fragment.png).
* De map xdpdocuments bevat uw basissjabloon en de fragmenten in **fragmenten** map

U kunt de vereiste structuur maken met de [formulieren en document-ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

Hier volgt de mapstructuur voor de voorbeeld-xdp die 2 fragmenten gebruikt
![formulieren&amp;document](assets/fragment-folder-structure-ui.png)


* Uitvoerservice - Deze service wordt doorgaans gebruikt om XML-gegevens samen te voegen met de xdp-sjabloon of pdf om samengevoegde pdf te genereren. Raadpleeg voor meer informatie de [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) voor de service Uitvoer. In dit voorbeeld gebruiken we fragmenten die zich in de crx-opslagplaats bevinden.


Het volgende ECMA-script is gebruikt om PDF te genereren. Merk het gebruik van ResourceResolver en ResourceResolverHelper in de code op. ResourceReolver is nodig aangezien deze code buiten om het even welke gebruikerscontext loopt.

```java
var inputMap = processorContext.getInputMap();
var itr = inputMap.entrySet().iterator();
var entry = inputMap.entrySet().iterator().next();
var xmlData = inputMap.get(entry.getKey());
log.info("Got XML Data File");

var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
log.info("Got service resolver");
var resourceResolver = aemDemoListings.getFormsServiceResolver();
//The ResourceResolverHelper execute's the following code within the context of the resourceResolver 
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
             //var statement = new Packages.com.adobe.aemfd.docmanager.Document("/content/dam/formsanddocuments/xdpdocuments/main.xdp",resourceResolver);
               var outputService = sling.getService(Packages.com.adobe.fd.output.api.OutputService);
            var pdfOutputOptions = new Packages.com.adobe.fd.output.api.PDFOutputOptions();
            pdfOutputOptions.setContentRoot("crx:///content/dam/formsanddocuments/xdpdocuments");
            pdfOutputOptions.setAcrobatVersion(Packages.com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
            var dataMergedDocument = outputService.generatePDFOutput("main.xdp",xmlData,pdfOutputOptions);
               //var dataMergedDocument = outputService.generatePDFOutput(statement,xmlData,pdfOutputOptions);
            processorContext.setResult("mergeddocument.pdf",dataMergedDocument);
            log.info("Generated the pdf document with fragments");
      }

 });
```

**De monsterverpakking op uw systeem testen**
* [Implementeer de DevelopingWithServiceUSer-bundel](assets/DevelopingWithServiceUser.jar)
* De vermelding toevoegen **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** in de gebruikerstoewijzingswijziging zoals weergegeven in onderstaande screenshot
   ![wijziging gebruikershandtekening](assets/user-mapper-service-amendment.png)
* [Download en importeer de voorbeeld-xdp-bestanden en de ECMA-scripts](assets/watched-folder-fragments-ecma.zip).
Hiermee wordt een gecontroleerde mapstructuur gemaakt in de map c:/fragmentsandoutputservice

* [Het bestand met voorbeeldgegevens extraheren](assets/usingFragmentsSampleData.zip) en deze in de installatiemap van uw controlemap plaatsen (c:\fragmentsandoutputservice\install)

* Controleer de resultatenmap van de configuratie van de gecontroleerde map op het gegenereerde PDF-bestand
