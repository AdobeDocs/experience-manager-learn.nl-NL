---
title: API gebruiken om een document met records te genereren met AEM Forms
seo-title: API gebruiken om een document met records te genereren met AEM Forms
description: Document met opnamen (DOR) programmatisch genereren
seo-description: API gebruiken om een document met records te genereren met AEM Forms
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 94ac3b13-01b4-4198-af81-e5609c80324c
discoiquuid: ba91d9df-dc61-47d8-8e0a-e3f66cae6a87
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---


# API gebruiken om het Document van Verslag in AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms} te produceren

Document met opnamen (DOR) programmatisch genereren

Dit artikel illustreert het gebruik van `com.adobe.aemds.guide.addon.dor.DoRService API` om **Document van Verslag** programmatically te produceren. [Document of ](https://docs.adobe.com/content/help/en/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) Recordis een PDF-versie van de gegevens die zijn vastgelegd in adaptief formulier.

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
1. Zorg ervoor dat u de DevelopingWithServiceUser-bundel hebt geïnstalleerd en gestart die deel uitmaakt van [Servicegebruikersartikel maken](service-user-tutorial-develop.md)
1. [Aanmelden bij configMgr](http://localhost:4502/system/console/configMgr)
1. Zoeken naar Apache Sling Service User Mapper Service
1. Zorg ervoor u de volgende ingang _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ in de sectie van de Toewijzingen van de Dienst ervoor
1. [Het formulier openen](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Vul het formulier in en klik op PDF weergeven
1. U moet DOR zien op een nieuw tabblad in uw browser


**Tips voor het oplossen van problemen**

PDF wordt niet weergegeven op het tabblad Nieuwe browser:

1. Let erop dat u pop-ups in uw browser niet blokkeert
1. Maak dat u de stappen hebt gevolgd die in dit [artikel](service-user-tutorial-develop.md) worden beschreven
1. Zorg ervoor dat de bundel &#39;DevelopingWithServiceUser&#39; zich in *actieve status* bevindt
1. Zorg ervoor dat de gegevens &#39; van de systeemgebruiker &#39; lees-, wijzigen- en aanmaakmachtigingen hebben voor het volgende knooppunt `/content/usergenerated/content/aemformsenablement`

