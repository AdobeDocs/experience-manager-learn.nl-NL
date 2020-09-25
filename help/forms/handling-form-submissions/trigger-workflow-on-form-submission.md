---
title: Trigger AEM workflow voor het verzenden van formulieren
description: Configureer adaptief formulier om AEM workflow te activeren.
sub-product: formulieren
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 5407
thumbnail: kt-5407.jpg
translation-type: tm+mt
source-git-commit: 738e356c4e72e0c3518bb5fd4ad6a076522e9f5c
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---


# Aangepast formulier configureren om AEM workflow te activeren

* Adaptief [formulier downloaden](assets/time-off-application.zip)
* Bladeren naar [formulier en documenten](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klik op Maken -> Bestand uploaden. Selecteer het formulier dat u in stap 1 hebt gedownload
* Open [Adaptief formulier in bewerkingsmodus](http://localhost:4502/editor.html/content/forms/af/timeofapplication.html).
* De verkenner van de inhoud openen
   ![Inhoud verkennen](assets/af-workflow-submission.PNG)
* Selecteer het knooppunt Form Container en open de configuratie-eigenschappen ervan
   ![Indiening](assets/af-workflow-submission1.PNG)
* Het deelvenster Verzending uitvouwen
* Stel de verzendactie van het formulier in zoals opgegeven in de bovenstaande schermafbeelding.
   _Zorg ervoor dat u een notitie maakt van de waarde die is opgegeven in het veld Pad gegevensbestand. Deze waarde moet overeenkomen met de waarde die u opgeeft in het gedeelte dat vooraf wordt ingevuld van de component Taak toewijzen van uw workflow._

Wanneer u het adaptieve formulier nu invult en verzendt, wordt de workflow geactiveerd die is gekoppeld aan de verzendactie van het formulier.
