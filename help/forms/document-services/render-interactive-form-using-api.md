---
title: Interactieve PDF renderen met Forms Services in AEM Forms
description: Forms Service API in AEM Forms gebruiken om interactieve PDF te renderen
feature: Forms Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# Interactieve PDF renderen met Forms Services in AEM Forms

Forms Service API in AEM Forms gebruiken om interactieve PDF te renderen

In dit artikel bekijken we de volgende service

* FormsService - Dit is een zeer veelzijdige dienst die u toestaat om gegevens uit en in het dossier van PDF uit te voeren/in te voeren en interactieve pdf ook te produceren door xml gegevens in het malplaatje samen te voegen xdp

De ambtenaar [javadoc voor AEM Forms API wordt hier weergegeven](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

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
1. [DevelopingWithServiceUserBundle downloaden en installeren](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Download en installeer de DocumentServices Sample Bundle met de Felix Web Console](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Download en installeer het pakket met AEM pakketbeheer](assets/downloadinteractivepdffrommobileform.zip)

1. [Aanmelden bij configMgr](http://localhost:4502/system/console/configMgr)
1. Zoeken naar Adobe Granite CSRF-filter
1. Het volgende pad toevoegen aan de uitgesloten secties en opslaan
1. /bin/generateinteractivepdf
1. Zoeken naar _Apache Sling Service User Mapper Service_ en klik om de eigenschappen te openen
   1. Klik op de knop *+* pictogram (plus) om de volgende Toewijzing van de Dienst toe te voegen
      * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
   1. Klik op Opslaan &#39;
1. [Het mobiele formulier openen](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Vul een paar gebieden in en klik dan ***Downloaden en vullen....*** knop
1. De interactieve pdf moet naar uw lokale systeem worden gedownload


Het voorbeeldpakket bevat het aangepaste profiel dat is gekoppeld aan het mobiele formulier. Verken de [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) bestand. Deze jsp haalt de gegevens uit het mobiele formulier en doet een verzoek om een POST naar servlet te installeren op ***/bin/generateinteractivepdf*** pad. De servlet keert interactieve pdf aan de roepende toepassing terug. De code in customtoolbar.jsp downloadt dan het dossier aan uw lokaal systeem
