---
title: Transformaties met AEM Forms
seo-title: Adaptieve formuliergegevens samenvoegen met Acrobat
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 5eeeb197f9a2ee4216e1f9220c830751c36f01ab
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---


# Adaptieve Forms maken van Acrobat-formulieren

Organisaties hebben een grote verscheidenheid aan formulieren. Sommige van deze formulieren worden gemaakt in Microsoft Word en geconverteerd naar PDF. Deze formulieren kunnen standaard niet worden ingevuld met Adobe Reader of Acrobat. Om deze formulieren invulbaar te maken met Acrobat of Reader, moeten we deze formulieren omzetten in Acroform. Acroformulieren zijn formulieren die zijn gemaakt met Acrobat. In dit artikel wordt door Acrobat een adaptief formulier gemaakt en worden de gegevens weer in Acrobat samengevoegd om de PDF te verkrijgen. De PDF met de samengevoegde gegevens kan ook worden verzonden voor ondertekening met behulp van Adobe Sign.

>[!NOTE]
>
>Als u AEM Forms 6.5 gebruikt, moet u de functie voor automatische Forms-omzetting gebruiken.

## Vereisten

* AEM Forms 6.3 of 6.4 geïnstalleerd en geconfigureerd
* Toegang tot Adobe Acrobat
* Kennis van AEM/AEM Forms.

### Hieronder vindt u de volgende vereisten om deze functie op uw systeem te laten werken

* De bundels downloaden en implementeren met de [Felix-webconsole](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [Download en importeer dit pakket in AEM](assets/acro-form-aem-form.zip). Dit pakket bevat de voorbeeldworkflow en de HTML-pagina om XSD te maken op basis van acroform
* Open [configMgr](http://localhost:4502/system/console/configMgr)
   * Zoek naar &#39;Apache Sling Service User Mapper Service&#39; en klik om de eigenschappen te openen
   * Klik op het `+` pictogram (plus) om de volgende servicetoewijzing toe te voegen
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Klik op Opslaan
