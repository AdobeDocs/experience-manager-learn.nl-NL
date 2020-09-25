---
title: Ontwikkelen met Output and Forms Services in AEM Forms
seo-title: Ontwikkelen met Output and Forms Services in AEM Forms
description: Uitvoer- en Forms Service-API in AEM Forms gebruiken
seo-description: Uitvoer- en Forms Service-API in AEM Forms gebruiken
feature: forms-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 0%

---


# Interactieve PDF-bestanden renderen met Forms Services in AEM Forms

Forms Service API in AEM Forms gebruiken om interactieve PDF-bestanden te renderen

In dit artikel bekijken we de volgende service

* FormsService - Dit is een zeer veelzijdige service waarmee u gegevens kunt exporteren/importeren vanuit en naar een PDF-bestand en waarmee u ook interactieve PDF-bestanden kunt genereren door XML-gegevens samen te voegen tot een xdp-sjabloon

De officiële javadoc voor de AEM Forms API wordt [hier weergegeven](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

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

Regel 1: Locatie van de map die de xdp-sjabloon bevat

Lijn 2-4: PDFFormRenderOptions maken en de eigenschappen ervan instellen

Regel 7: Interactieve PDF genereren met behulp van de renderPDFForm-servicebewerking van FormsService

Regel 11: Hiermee wordt de gegenereerde interactieve PDF naar de opvragende toepassing geretourneerd

**De monsterverpakking op uw systeem testen**
1. [Download en installeer de DocumentServices Sample Bundle met de Felix Web Console](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Download en installeer het pakket met AEM pakketbeheer](assets/downloadinteractivepdffrommobileform.zip)



1. [Aanmelden bij configMgr](http://localhost:4502/system/console/configMgr)
1. Zoeken naar Adobe Granite CSRF-filter
1. Het volgende pad toevoegen aan de uitgesloten secties en opslaan
1. /bin/generateinteractivepdf
1. [Het mobiele formulier openen](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Vul een paar gebieden in en klik dan de ***Download en vult....*** button
1. De interactieve pdf moet naar uw lokale systeem worden gedownload


Het voorbeeldpakket bevat het aangepaste profiel dat is gekoppeld aan het mobiele formulier. Bekijk het bestand [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) . Deze JSP haalt de gegevens uit het mobiele formulier en vraagt een POST om serlet die op het pad ***/bin/generateinteractivepdf*** is gemonteerd. De servlet keert interactieve pdf aan de roepende toepassing terug. De code in customtoolbar.jsp downloadt dan het dossier aan uw lokaal systeem


