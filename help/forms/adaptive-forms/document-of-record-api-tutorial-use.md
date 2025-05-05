---
title: API gebruiken om een document met records te genereren met AEM Forms
description: Document met opnamen (DOR) programmatisch genereren
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2023-01-26T00:00:00Z
duration: 67
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# API gebruiken om een document met records te genereren in AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Document met opnamen (DOR) programmatisch genereren

Dit artikel illustreert het gebruik van `com.adobe.aemds.guide.addon.dor.DoRService API` om **Document van Verslag** programmatically te produceren. [ Document van Verslag ](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html?lang=nl-NL) is een versie van PDF van de gegevens die in AanpassingsVorm worden gevangen.

1. Hier volgt het codefragment. De eerste regel krijgt de DOR-service.
1. Stel de DoROptions in.
1. Roep de teruggevende methode van DoRService aan en ga het voorwerp DoROptions tot de teruggevende methode over

```java
String dataXml = request.getParameter("data");
System.out.println("Got " + dataXml);
Session session;
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
System.out.println("Got ... DOR Service");
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
System.out.println("Got aem DemoListings");
resourceResolver = aemDemoListings.getFormsServiceResolver();
session = resourceResolver.adaptTo(Session.class);
resource = resourceResolver.getResource("/content/forms/af/sandbox/1201-borrower-payments");
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions = new com.adobe.aemds.guide.addon.dor.DoROptions();
dorOptions.setData(dataXml);
dorOptions.setFormResource(resource);
java.util.Locale locale = new java.util.Locale("en");
dorOptions.setLocale(locale);
com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
byte[] fileBytes = dorResult.getContent();
com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
resource = resourceResolver.getResource("/content/usergenerated/content/aemformsenablement");
Node paydotgov = resource.adaptTo(Node.class);
java.util.Random r = new java.util.Random();
String nodeName = Long.toString(Math.abs(r.nextLong()), 36);
Node fileNode = paydotgov.addNode(nodeName + ".pdf", "nt:file");

System.out.println("Created file Node...." + fileNode.getPath());
Node contentNode = fileNode.addNode("jcr:content", "nt:resource");
Binary binary = session.getValueFactory().createBinary(dorDocument.getInputStream());
contentNode.setProperty("jcr:data", binary);
JSONWriter writer = new JSONWriter(response.getWriter());
writer.object();
writer.key("filePath");
writer.value(fileNode.getPath());
writer.endObject();
session.save();
```

Voer de volgende stappen uit om dit op uw lokale systeem te proberen

1. [Artikelelementen downloaden en installeren met pakketbeheer](assets/dor-with-api.zip)
1. Zorg ervoor u geïnstalleerd en begonnen de bundel DevelopingWithServiceUser als deel van [ wordt verstrekt creeer het artikel van de Gebruiker van de Dienst ](service-user-tutorial-develop.md)
1. [ Login aan configMgr ](http://localhost:4502/system/console/configMgr)
1. Zoeken naar Apache Sling Service User Mapper Service
1. Zorg ervoor u de volgende ingang _DevelopingWithServiceUser.core:getformsresourceresolver=fd-dienst_ in de sectie van de Toewijzingen van de Dienst
1. [ open de vorm ](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Vul het formulier in en klik op &#39; View PDF &#39;
1. U moet DOR zien op een nieuw tabblad in uw browser


**Tips van het Oplossen van problemen**

PDF wordt niet weergegeven op het tabblad Nieuwe browser:

1. Let erop dat u pop-ups in uw browser niet blokkeert
1. Zorg ervoor dat u de AEM-server als beheerder start (tenminste in vensters)
1. Zorg ervoor de &quot;DevelopingWithServiceUser&quot;bundel in *actieve staat* is
1. [ zorg ervoor de systeemgebruiker ](http://localhost:4502/useradmin) &quot; fd-dienst&quot;gelezen heeft, wijzigt, en leidt toestemmingen op de volgende knoop `/content/usergenerated/content/aemformsenablement`
