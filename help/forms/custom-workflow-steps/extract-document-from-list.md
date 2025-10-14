---
title: Document extraheren uit lijst met documenten in een AEM-workflow
description: Aangepaste workflowcomponent om een specifiek document uit een lijst met documenten te extraheren
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-13918
last-substantial-update: 2023-09-12T00:00:00Z
exl-id: b0baac71-3074-49d5-9686-c9955b096abb
duration: 56
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# Document uit lijst met documenten extraheren

Een veelvoorkomend geval is het verzenden van de formuliergegevens en de formulierbijlage naar een extern systeem met behulp van de stap Formuliergegevensmodel aanroepen in een AEM-workflow. Bijvoorbeeld, wanneer het creëren van een geval in ServiceNow u case details met een ondersteunend document zou willen voorleggen. De bijlagen die aan het adaptieve formulier worden toegevoegd, worden opgeslagen in een variabele van het type arraylist van documenten. Als u een specifiek document uit deze arraylijst wilt extraheren, moet u aangepaste code schrijven.

In dit artikel worden de stappen doorlopen voor het gebruik van de aangepaste workflowcomponent voor het uitpakken en opslaan van het document in een documentvariabele.

## Workflow maken

Er moet een workflow worden gemaakt voor het verzenden van formulieren. Voor de workflow moeten de volgende variabelen worden gedefinieerd

* Een variabele van het type ArrayList of Document (deze variabele bevat de formulierbijlagen die door de gebruiker zijn toegevoegd)
* Een variabele van het type Document.(Deze variabele bevat het document dat uit de ArrayList is geëxtraheerd)

* De aangepaste component aan uw workflow toevoegen en de eigenschappen ervan configureren
  ![&#x200B; extract-punt-werkschema &#x200B;](assets/extract-document-array-list.png)

## Adaptief formulier configureren

* De verzendactie van het adaptieve formulier configureren om de AEM-workflow te activeren
  ![&#x200B; voorleggen-actie &#x200B;](assets/store-attachments.png)

## De oplossing testen

[Implementeer de aangepaste bundel met de OSGi-webconsole](assets/ExtractItemsFromArray.core-1.0.0-SNAPSHOT.jar)

[De workflowcomponent importeren met pakketbeheer](assets/Extract-item-from-documents-list.zip)

[De voorbeeldworkflow importeren](assets/extract-item-sample-workflow.zip)

[Het adaptieve formulier importeren](assets/test-attachment-extractions-adaptive-form.zip)

[&#x200B; Voorproef de vorm &#x200B;](http://localhost:4502/content/dam/formsanddocuments/testattachmentsextractions/jcr:content?wcmmode=disabled)

Voeg een bijlage aan het formulier toe en verzend het.

>[!NOTE]
>
>Het geëxtraheerde document kan vervolgens worden gebruikt in elke andere workflowstap, zoals E-mail verzenden of FDM-stap aanroepen
