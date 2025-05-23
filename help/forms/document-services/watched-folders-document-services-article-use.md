---
title: Gecontroleerde mappen gebruiken in AEM Forms
description: Gecontroleerde mappen configureren en gebruiken in AEM Forms
feature: Output Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: abb74d44-d1b9-44d6-a49f-36c01acfecb4
last-substantial-update: 2020-07-07T00:00:00Z
duration: 86
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# Gecontroleerde mappen gebruiken in AEM Forms{#using-watched-folders-in-aem-forms}

Een beheerder kan een netwerkmap, ook wel een gecontroleerde map genoemd, zo configureren dat wanneer een gebruiker een bestand (zoals een PDF-bestand) in de Gecontroleerde map plaatst, een vooraf geconfigureerde workflow, service of scriptbewerking wordt gestart om het toegevoegde bestand te verwerken. Nadat de service de opgegeven bewerking heeft uitgevoerd, wordt het resulterende bestand opgeslagen in een opgegeven uitvoermap. Voor meer informatie over werkschema, de dienst, en manuscript.

Meer over het creëren van een gelete op omslag leren, [ klik hier ](https://helpx.adobe.com/nl/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Gecontroleerde mappen worden gebruikt om documenten te genereren in de batchmodus. Met het gecontroleerde mapmechanisme kunt u interactieve communicatie voor het afdrukkanaal genereren of de uitvoerservice gebruiken om gegevens samen te voegen met de sjabloon.

In dit artikel wordt beschreven hoe gegevens via het mechanisme voor gecontroleerde mappen kunnen worden samengevoegd met een sjabloon.

De dienst van de output is de dienst OSGi die deel van de Diensten van het Document van AEM uitmaakt. De uitvoerservice ondersteunt verschillende uitvoerindelingen en functies voor uitvoerontwerp van AEM Forms Designer. De uitvoerservice kan XFA-sjablonen en XML-gegevens converteren om afdrukdocumenten in verschillende indelingen te genereren.

Meer over de outputdienst leren, [ gelieve te klikken hier ](https://helpx.adobe.com/nl/aem-forms/6/output-service.html).
Volg onderstaande stappen om een gecontroleerde map op uw systeem in te stellen:
* [ Download en haal de inhoud van het ZIP dossier ](assets/outputservicewatchedfolderkt.zip) uit.Dit ZIP dossier bevat pakket voor het creëren van gecontroleerde omslag en steekproefdossiers om de outputdienst te testen gebruikend gecontroleerd omslagmechanisme
   * Windows-systeem

      * Importeer de uitvoerservicewatchedfolder.zip naar AEM via pakketbeheer
      * Hiermee maakt u een gecontroleerde map met de naam outputservice watchedfolder op het station C.
   * Niet-Windows-systeem
      * [ open de configuratie die van de gelete op omslag ](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice) plaatst
      * Stel de eigenschap voor het mappad van het serviceknooppunt zo in dat deze naar een geschikte locatie wijst
      * Uw wijzigingen opslaan
      * De bovenstaande locatie is uw controlemap.

Zet de mappen SamplePdfFile en SampleXdpFile neer in de invoermap van de controlemap. Wanneer de bestanden met succes zijn verwerkt, worden de resultaten in de map met resultaten van de gecontroleerde map geplaatst.


>[!NOTE]
>
>Als het script dat aan een gecontroleerde map is gekoppeld meer dan één bestand nodig heeft, moet u een map maken en alle vereiste bestanden in de map plaatsen en de map neerzetten in de invoermap van de gecontroleerde map.
>
>Als het script dat is gekoppeld aan een gecontroleerde map maar één invoerbestand nodig heeft, kunt u het bestand rechtstreeks neerzetten in de invoermap van de controlemap
