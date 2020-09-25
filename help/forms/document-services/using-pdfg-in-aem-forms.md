---
title: PDFG gebruiken in AEM Forms
seo-title: PDFG gebruiken in AEM Forms
description: Mogelijkheid voor slepen en neerzetten demonstreren om een PDF te maken met AEM Forms
seo-description: Mogelijkheid voor slepen en neerzetten demonstreren om een PDF te maken met AEM Forms
uuid: ee54edb4-a7b1-42ed-81ea-cb6bef6cf97f
feature: pdf-generator
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 7f570f12-ce43-4da7-a249-ef6bd0fe48e5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '281'
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
1. [Navigeer naar post.jsp](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) in uw CRX
1. De opslaglocatie naar uw voorkeur wijzigen (regel 9)
1. Sla uw wijzigingen op.
1. Open de [ HTML-pagina](http://localhost:4502/content/DocumentServices/CreatePDFG.html) voor het slepen en neerzetten van bestanden voor conversie.
1. Zet een tekstbestand of JPG neer in de neerzetzone.
1. Het invoerdocument wordt geconverteerd naar PDF en opgeslagen op dezelfde locatie als opgegeven in punt 4.

Het volgende codefragment toont het gebruik van de PDFG-service voor het converteren van bestanden naar PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

