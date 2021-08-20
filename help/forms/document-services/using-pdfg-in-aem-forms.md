---
title: PDFG gebruiken in AEM Forms
description: Mogelijkheid voor slepen en neerzetten demonstreren om een PDF te maken met AEM Forms
feature: PDF Generator
version: 6.4,6.5
topic: Ontwikkeling
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%

---


# PDFG gebruiken in AEM Forms{#using-pdfg-in-aem-forms}

Mogelijkheid voor slepen en neerzetten demonstreren om een PDF te maken met AEM Forms

PDFG staat voor PDF Generation. Dit betekent dat u verschillende bestandsindelingen kunt converteren naar PDF. De gemeenschappelijkste degenen zijn de documenten van Microsoft Office. PDFG maakt sinds 6.1 deel uit van AEM Forms.
[De javadoc voor PDFG API wordt hier weergegeven](https://helpx.adobe.com/experience-manager/6-3/forms/using/aem-document-services-programmatically.html#PDFGeneratorService)

Met de elementen die aan dit artikel zijn gekoppeld, kunt u MS Office-documenten of JPG-bestanden naar de neerzetzone van de HTML-pagina slepen. Nadat het document is neergezet, wordt de PDFG-service geactiveerd en wordt het document naar PDF geconverteerd en opgeslagen in het bestandssysteem van AEM Server.

Voer de volgende stappen uit om de demo-elementen te installeren

1. Configureer PDFG zoals vermeld in dit document [hier](https://helpx.adobe.com/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. Volg de desbetreffende documentatie voor uw AEM Forms-versie.
1. [Importeer en installeer elementen die verwant zijn aan dit artikel met gebruik van pakketbeheer.](assets/createpdfgdemov2.zip)
1. [Navigeer naar post.](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) jspin uw CRX
1. De opslaglocatie naar uw voorkeur wijzigen (regel 9)
1. Sla uw wijzigingen op.
1. Open de HTML-pagina [ voor het slepen en neerzetten van bestanden voor conversie.](http://localhost:4502/content/DocumentServices/CreatePDFG.html)
1. Zet een tekstbestand of JPG neer in de neerzetzone.
1. Het invoerdocument wordt geconverteerd naar PDF en opgeslagen op dezelfde locatie als opgegeven in punt 4.

Het volgende codefragment toont het gebruik van de PDFG-service voor het converteren van bestanden naar PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

