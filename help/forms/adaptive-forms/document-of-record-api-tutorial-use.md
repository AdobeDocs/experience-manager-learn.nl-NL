---
title: API gebruiken om een document met records te genereren met AEM Forms
seo-title: API gebruiken om een document met records te genereren met AEM Forms
description: Document met opnamen (DOR) programmatisch genereren
seo-description: API gebruiken om een document met records te genereren met AEM Forms
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 94ac3b13-01b4-4198-af81-e5609c80324c
discoiquuid: ba91d9df-dc61-47d8-8e0a-e3f66cae6a87
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---


# API gebruiken om een document met records te genereren in AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Document met opnamen (DOR) programmatisch genereren

Dit artikel illustreert het gebruik van het `com.adobe.aemds.guide.addon.dor.DoRService API` om **Document van Verslag** programmatically te produceren. [Document of Record](https://docs.adobe.com/content/help/en/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) is een PDF-versie van de gegevens die zijn vastgelegd in Adaptief formulier.

1. Hier volgt het codefragment. De eerste regel krijgt de DOR-service.
1. Stel de DoROptions in.
1. Roep de teruggevende methode van DoRService aan en ga het voorwerp DoROptions tot de teruggevende methode over

```java
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions =  new com.adobe.aemds.guide.addon.dor.DoROptions();
 dorOptions.setData(dataXml);
 dorOptions.setFormResource(resource);
 java.util.Locale locale = new java.util.Locale("en");
 dorOptions.setLocale(locale);
 com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
 byte[] fileBytes = dorResult.getContent();
 com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
```

Voer de volgende stappen uit om dit op uw lokale systeem te proberen

1. [Artikelelementen downloaden en installeren met pakketbeheer](assets/dor-with-api.zip)
1. Controleer of u de bundel DevelopingWithServiceUser hebt geïnstalleerd en gestart die als onderdeel van het [artikel Servicegebruiker maken is meegeleverd](service-user-tutorial-develop.md)
1. [Aanmelden bij configMgr](http://localhost:4502/system/console/configMgr)
1. Zoeken naar Apache Sling Service User Mapper Service
1. Zorg ervoor u de volgende ingang _DevelopingWithServiceUser.core:getformsresourceresolver=fd-dienst_ in de sectie van de Toewijzingen van de Dienst ervoor
1. [Het formulier openen](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Vul het formulier in en klik op PDF weergeven
1. U moet DOR zien op een nieuw tabblad in uw browser


**Tips voor het oplossen van problemen**

PDF wordt niet weergegeven op het tabblad Nieuwe browser:

1. Let erop dat u pop-ups in uw browser niet blokkeert
1. Zorg dat u de stappen hebt uitgevoerd die in dit [artikel worden beschreven](service-user-tutorial-develop.md)
1. Zorg ervoor de bundel &#39;DevelopingWithServiceUser&#39; in *actieve staat is*
1. Zorg ervoor dat de gegevens &#39; van de systeemgebruiker &#39; lees-, wijzigen- en aanmaakmachtigingen hebben voor het volgende knooppunt `/content/usergenerated/content/aemformsenablement`

