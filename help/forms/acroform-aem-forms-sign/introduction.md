---
title: Transformaties met AEM Forms
description: Een zelfstudie die door het maken van een adaptief formulier met Acroform loopt en de gegevens samenvoegt om een PDF te verkrijgen. De PDF met de samengevoegde gegevens kunnen vervolgens worden verzonden voor ondertekening met behulp van Acrobat Sign.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 45
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---


# Adaptieve Forms maken van Acrobat-formulieren

Organisaties hebben een grote verscheidenheid aan formulieren. Sommige van deze formulieren worden gemaakt in Microsoft Word en omgezet in PDF. Deze formulieren kunnen standaard niet worden ingevuld met Adobe Reader of Acrobat. Om deze formulieren invulbaar te maken met Acrobat of Reader, moeten we deze formulieren omzetten in Acroform. Acroformulieren zijn formulieren die zijn gemaakt met Acrobat. Dit artikel doorloopt het maken van een adaptief formulier van Acrobat en het samenvoegen van de gegevens in Acroform om de PDF te verkrijgen. De PDF met de samengevoegde gegevens kunnen ook worden verzonden voor ondertekening met behulp van Acrobat Sign.

>[!NOTE]
>
>Als u AEM Forms 6.5 gebruikt, moet u de Automatede form conversion gebruiken.

## Vereisten

* AEM Forms 6.3 of 6.4 geïnstalleerd en geconfigureerd
* Toegang tot Adobe Acrobat
* Kennis van AEM/AEM Forms.

### Hieronder vindt u de volgende vereisten om deze functie op uw systeem te laten werken

* Download en stel de bundels op gebruikend de [ Console van het Web van de Felix ](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [ AcroFormsToAEMFormsBundle ](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [ Download en voer dit pakket in AEM ](assets/acro-form-aem-form.zip) in. Dit pakket bevat de voorbeeldworkflow en de HTML-pagina om XSD te maken van acroform
* Open [ configMgr ](http://localhost:4502/system/console/configMgr)
   * Zoek naar &#39;Apache Sling Service User Mapper Service&#39; en klik om de eigenschappen te openen
   * Klik op het pictogram `+` (plus) om de volgende servicetoewijzing toe te voegen
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Klik op Opslaan
