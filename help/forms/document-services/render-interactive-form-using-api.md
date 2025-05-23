---
title: Interactieve PDF renderen met Forms Services in AEM Forms
description: Forms Service API in AEM Forms gebruiken om interactieve PDF te renderen
feature: Forms Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
last-substantial-update: 2020-07-07T00:00:00Z
duration: 75
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---

# Interactieve PDF renderen met Forms Services in AEM Forms

Forms Service API in AEM Forms gebruiken om interactieve PDF te renderen

In dit artikel bekijken we de volgende service

* FormsService - Dit is een zeer veelzijdige service waarmee u gegevens kunt exporteren/importeren vanuit en naar een PDF-bestand en waarmee u ook interactieve pdf-bestanden kunt genereren door XML-gegevens samen te voegen in een xdp-sjabloon

Officiële [ javadoc voor AEM Forms API is hier vermeld ](https://helpx.adobe.com/nl/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

Het volgende codefragment geeft interactief pdf terug gebruikend de renderPDFForm verrichting van FormsService. Het bestand schengen.xdp wordt gebruikt om de XML-gegevens samen te voegen.

```java
String uri = "crx:///content/dam/formsanddocuments";
PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
renderOptions.setContentRoot(uri);
Document interactivePDF = null;
try {
interactivePDF = formsService.renderPDFForm("schengen.xdp", xmlData, renderOptions);
} catch (FormsServiceException e) {
 e.printStackTrace();
}
return interactivePDF;
```

Regel 1: Plaats van de omslag die het xdp malplaatje bevat

Regel 2-4: Maak PDFFormRenderOptions en stel de eigenschappen ervan in

Lijn 7: Genereer Interactieve PDF met behulp van de renderPDFForm-servicebewerking van FormsService

Regel 11: Keert geproduceerde interactieve pdf aan de roepende toepassing terug

**om het steekproefpakket op uw systeem** te testen
1. [DevelopingWithServiceUserBundle downloaden en installeren](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Download en installeer de DocumentServices Sample Bundle met de Felix Web Console](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Download en installeer het pakket met behulp van AEM package Manager](assets/downloadinteractivepdffrommobileform.zip)

1. [ Login aan configMgr ](http://localhost:4502/system/console/configMgr)
1. Zoeken naar Adobe Granite CSRF-filter
1. Het volgende pad toevoegen aan de uitgesloten secties en opslaan
1. /bin/generateinteractivepdf
1. Onderzoek naar _de Dienst van het Mapper van de Gebruiker van de Dienst van Apache Sling_ en klik om de eigenschappen te openen
   1. Klik op het pictogram *+* (plus) om de volgende servicetoewijzing toe te voegen
      * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
   1. Klik op Opslaan &#39;
1. [ open de mobiele vorm ](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Vul een paar gebieden in en klik dan de ***Download en vult...*** knop
1. De interactieve pdf moet naar uw lokale systeem worden gedownload


Het voorbeeldpakket bevat het aangepaste profiel dat is gekoppeld aan het mobiele formulier. Gelieve te onderzoeken het {[&#128279;](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) dossier 0} customtoolbar.jsp.  This jsp haalt de gegevens uit de mobiele vorm uit en doet een POST- verzoek aan servlet opgezet op ***/bin/generateinteractivepdf*** weg. De servlet keert interactieve pdf aan de roepende toepassing terug. De code in customtoolbar.jsp downloadt dan het dossier aan uw lokaal systeem
