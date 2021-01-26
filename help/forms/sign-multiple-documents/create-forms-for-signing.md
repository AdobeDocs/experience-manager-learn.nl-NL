---
title: Forms maken voor ondertekening
description: Maak formulieren die moeten worden opgenomen in het ondertekeningspakket.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%

---


# Formulieren maken voor ondertekening

De volgende stap bestaat uit het maken van de aangepaste formulieren die u in het pakket wilt opnemen. Houd er rekening mee dat u zich aan de volgende punten houdt wanneer u formulieren maakt voor ondertekening:

* Zorg ervoor dat de formulieren zijn gebaseerd op de sjabloon **SignMultipleForms**. Op deze manier zorgt u ervoor dat de formulieren vooraf worden ingevuld met de gegevens die uit de database worden opgehaald.

* De formulieren moeten zijn geconfigureerd voor het gebruik van Adobe Sign en het veld signer1 moet zijn gekoppeld aan het veld Customer Email.
* De formulieren moeten ook worden gekoppeld aan clientLib met de naam **getnextform**
* De formulieren moeten de component Signature Step gebruiken.
* Het formulier moet ook de aangepaste **Meerdere formulieren ondertekenen**-component gebruiken. Met deze component kunt u naar het volgende formulier navigeren om u aan te melden bij het pakket.
* De verzending van het formulier moet zo zijn geconfigureerd dat AEM workflow **Handtekeningstatus bijwerken** wordt geactiveerd
* Zorg ervoor dat het pad naar gegevensbestand is ingesteld op **Data.xml**. Dit is erg belangrijk omdat de voorbeeldcode zoekt naar een bestand met de naam Data.xml in het laadproces van het verzenden van het formulier.

Nadat u het formulier hebt gemaakt, neemt u het aangepaste formulierfragment **commonfields** op in het formulier. Het fragment wordt gemarkeerd als verborgen. Dit fragment bevat de volgende velden.

* **ondertekend**  - Het veld waarin de status van de handtekening wordt opgeslagen
* **guid**  - Unieke id om het formulier in het pakket te identificeren
* **customerEmail**  - Dit veld bevat de e-mail van de klant



>[!NOTE]
>Als u gegevens van het ene formulier naar het andere in het pakket wilt verplaatsen, moet u ervoor zorgen dat de formuliervelden in alle formulieren dezelfde naam hebben.

## Alle gereed formulier

Nadat alle formulieren in het pakket zijn ingevuld en ondertekend, moeten we het juiste bericht weergeven. Dit bericht wordt weergegeven met de hulp van het Aldo-formulier. Het Alldone-formulier wordt opgenomen in de voorbeeldformulieren.

## Assets

De voorbeeldformulieren, inclusief de formulieren die in deze zelfstudie worden gebruikt, kunnen [hier worden gedownload](assets/forms-for-signing.zip)
