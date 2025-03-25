---
title: PDFG gebruiken in AEM Forms
description: Mogelijkheid voor slepen en neerzetten demonstreren om PDF te maken met AEM Forms
feature: PDF Generator
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: bc79fcbf-b8b3-4d3a-9cd6-0bcd9321c7d5
last-substantial-update: 2020-07-07T00:00:00Z
duration: 52
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# PDFG gebruiken in AEM Forms{#using-pdfg-in-aem-forms}

Mogelijkheid voor slepen en neerzetten demonstreren om PDF te maken met AEM Forms

PDFG staat voor PDF Generation. Dit betekent dat u verschillende bestandsindelingen kunt converteren naar PDF. De meest voorkomende zijn Microsoft Office-documenten. PDFG maakt sinds 6.1 deel uit van AEM Forms.
[ javadoc voor PDFG API is hier vermeld ](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)

Met de elementen die aan dit artikel zijn gekoppeld, kunt u MS Office-documenten of JPG-bestanden naar de neerzetzone van de HTML-pagina slepen. Nadat het document is neergezet, wordt de PDFG-service geactiveerd en wordt het document naar PDF geconverteerd en opgeslagen in het bestandssysteem van AEM Server.

Voer de volgende stappen uit om de demo-elementen te installeren

1. Vorm PDFG zoals vermeld in dit document [ hier ](https://helpx.adobe.com/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. Volg de desbetreffende documentatie voor uw AEM Forms-versie.
1. [Importeer en installeer elementen die verwant zijn aan dit artikel met gebruik van pakketbeheer.](assets/createpdfgdemov2.zip)
1. [ navigeer aan post.jsp ](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) in uw CRX
1. De opslaglocatie naar uw voorkeur wijzigen (regel 9)
1. Sla uw wijzigingen op.
1. Open de [ HTML- pagina ](http://localhost:4502/content/DocumentServices/CreatePDFG.html) voor het slepen van en het laten vallen van dossiers voor omzetting.
1. Zet een tekstbestand of JPG neer in de neerzetzone.
1. Het invoerdocument wordt geconverteerd naar PDF en opgeslagen op dezelfde locatie als in punt 4.

Het volgende codefragment toont het gebruik van de PDFG-service voor het converteren van bestanden naar PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```
