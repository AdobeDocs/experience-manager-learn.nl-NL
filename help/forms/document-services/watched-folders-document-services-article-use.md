---
title: Gecontroleerde mappen gebruiken in AEM Forms
seo-title: Gecontroleerde mappen gebruiken in AEM Forms
description: Gecontroleerde mappen configureren en gebruiken in AEM Forms
seo-description: Gecontroleerde mappen configureren en gebruiken in AEM Forms
uuid: 32c4bda2-363d-4294-925e-405a176f7f8d
feature: output-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a40e2381-0dc8-4784-9b80-15e27b244035
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---


# Gecontroleerde mappen gebruiken in AEM Forms{#using-watched-folders-in-aem-forms}

Een beheerder kan een netwerkmap, ook wel een gecontroleerde map genoemd, zo configureren dat wanneer een gebruiker een bestand (zoals een PDF-bestand) in de Gecontroleerde map plaatst, een vooraf geconfigureerde workflow, service of scriptbewerking wordt gestart om het toegevoegde bestand te verwerken. Nadat de service de opgegeven bewerking heeft uitgevoerd, wordt het resultaatbestand opgeslagen in een opgegeven uitvoermap. Voor meer informatie over werkschema, de dienst, en manuscript.

Klik hier voor meer informatie over het maken van een controlemap. [](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Gecontroleerde mappen worden gebruikt om documenten te genereren in de batchmodus. Met het gecontroleerde mapmechanisme kunt u interactieve communicatie voor het afdrukkanaal genereren of de uitvoerservice gebruiken om gegevens samen te voegen met de sjabloon.

In dit artikel wordt beschreven hoe gegevens via het mechanisme voor gecontroleerde mappen kunnen worden samengevoegd met een sjabloon.

De dienst van de output is de dienst OSGi die deel van AEM Diensten van het Document uitmaakt. De uitvoerservice ondersteunt verschillende uitvoerindelingen en functies voor het ontwerpen van uitvoer van AEM Forms Designer. De uitvoerservice kan XFA-sjablonen en XML-gegevens converteren om afdrukdocumenten in verschillende indelingen te genereren.

Klik [hier](https://helpx.adobe.com/aem-forms/6/output-service.html)voor meer informatie over de uitvoerservice.
Volg onderstaande stappen om een gecontroleerde map op uw systeem in te stellen:
* [Download en extraheer de inhoud van het ZIP-bestand](assets/outputservicewatchedfolderkt.zip). Dit ZIP-bestand bevat een pakket voor het maken van gecontroleerde mappen en voorbeeldbestanden om de uitvoerservice te testen met behulp van het mechanisme voor gecontroleerde mappen
   * Windows-systeem

      * Importeer de uitvoerservicewatchedfolder.zip in AEM met behulp van pakketbeheer
      * Hiermee maakt u een gecontroleerde map met de naam outputservice watchedfolder op het station C.
   * Niet Windows-systeem
      * [De configuratie-instelling van de controlemap openen](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Stel de eigenschap voor het mappad van het serviceknooppunt zo in dat deze naar een geschikte locatie wijst
      * Uw wijzigingen opslaan
      * De bovenstaande locatie is de gecontroleerde map.

Zet de mappen SamplePdfFile en SampleXdpFile neer in de invoermap van de controlemap. Wanneer de bestanden met succes zijn verwerkt, worden de resultaten in de map met resultaten van de gecontroleerde map geplaatst.


>[!NOTE]
>
>Als het script dat aan een gecontroleerde map is gekoppeld meer dan één bestand nodig heeft, moet u een map maken en alle vereiste bestanden in de map plaatsen en de map neerzetten in de invoermap van de gecontroleerde map.
>
>Als het script dat is gekoppeld aan een gecontroleerde map maar één invoerbestand nodig heeft, kunt u het bestand rechtstreeks neerzetten in de invoermap van de controlemap

