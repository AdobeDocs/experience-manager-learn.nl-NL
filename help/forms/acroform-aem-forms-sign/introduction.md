---
title: Transformaties met AEM Forms
description: Een zelfstudie die door het maken van een adaptief formulier met Acroform loopt en de gegevens samenvoegt om een PDF te verkrijgen. De PDF met de samengevoegde gegevens kan vervolgens worden verzonden voor ondertekening met behulp van Acrobat Sign.
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 45
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---


# Adaptieve Forms maken van Acrobat-formulieren

Organisaties hebben een grote verscheidenheid aan formulieren. Sommige van deze formulieren worden gemaakt in Microsoft Word en geconverteerd naar PDF. Deze formulieren kunnen standaard niet worden ingevuld met Adobe Reader of Acrobat. Als u deze formulieren invulbaar wilt maken met Acrobat of Reader, moet u deze formulieren converteren naar Acrobat. Acroformulieren zijn formulieren die zijn gemaakt met Acrobat. Dit artikel doorloopt het maken van een adaptief formulier van Acrobat en het samenvoegen van de gegevens in Acrobat om de PDF te verkrijgen. De PDF met de samengevoegde gegevens kan ook worden verzonden voor ondertekening met behulp van Acrobat Sign.

>[!NOTE]
>
>Als u AEM Forms 6.5 gebruikt, moet u de functie voor automatische formulierconversie gebruiken.

## Vereisten

* AEM Forms 6.3 of 6.4 geïnstalleerd en geconfigureerd
* Toegang tot Adobe Acrobat
* Kennis van AEM/AEM Forms.

### Hieronder vindt u de volgende vereisten om deze functie op uw systeem te laten werken

* Download en stel de bundels op gebruikend de [&#x200B; Console van het Web van de Felix &#x200B;](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [&#x200B; AcroFormsToAEMFormsBundle &#x200B;](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [&#x200B; Download en voer dit pakket in AEM &#x200B;](assets/acro-form-aem-form.zip) in. Dit pakket bevat de voorbeeldworkflow en de HTML-pagina om XSD te maken van acroform
* Open [&#x200B; configMgr &#x200B;](http://localhost:4502/system/console/configMgr)
   * Zoek naar &#39;Apache Sling Service User Mapper Service&#39; en klik om de eigenschappen te openen
   * Klik op het pictogram `+` (plus) om de volgende servicetoewijzing toe te voegen
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Klik op Opslaan
