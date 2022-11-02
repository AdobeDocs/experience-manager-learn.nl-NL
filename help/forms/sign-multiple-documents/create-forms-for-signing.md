---
title: Forms maken voor ondertekening
description: Maak formulieren die moeten worden opgenomen in het ondertekeningspakket.
feature: Adaptive Forms
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: Development
role: User
level: Beginner
exl-id: 565d81a4-2918-44ea-a3e9-ed79f246f08a
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# Formulieren maken voor ondertekening

De volgende stap bestaat uit het maken van de aangepaste formulieren die u in het pakket wilt opnemen. Houd er rekening mee dat u zich aan de volgende punten houdt wanneer u formulieren maakt voor ondertekening:

* Zorg ervoor dat de formulieren zijn gebaseerd op de **SignMultipleForms** sjabloon. Op deze manier zorgt u ervoor dat de formulieren vooraf worden ingevuld met de gegevens die uit de database worden opgehaald.

* De formulieren moeten zijn geconfigureerd voor het gebruik van Acrobat Sign en het veld signer1 moet zijn gekoppeld aan het veld Customer Email.
* De formulieren moeten ook worden gekoppeld aan clientLib die **getnextform**
* De formulieren moeten de component Signature Step gebruiken.
* Het formulier moet ook de aangepaste **Meerdere formulieren ondertekenen** component. Met deze component kunt u naar het volgende formulier navigeren om u aan te melden bij het pakket.
* De verzending van het formulier moet zo zijn geconfigureerd dat AEM workflow wordt geactiveerd **Handtekeningstatus bijwerken**
* Zorg ervoor dat het pad naar het gegevensbestand is ingesteld op **Data.xml**. Dit is erg belangrijk omdat de voorbeeldcode zoekt naar een bestand met de naam Data.xml in het laadproces van het verzenden van het formulier.

Nadat u het formulier hebt gemaakt, neemt u de opdracht **alleenstaande velden** adaptief formulierfragment in het formulier. Het fragment wordt gemarkeerd als verborgen. Dit fragment bevat de volgende velden.

* **ondertekend** - Het veld dat de status van de handtekening bevat
* **guid** - Unieke id om het formulier in het pakket te identificeren
* **customerEmail** - Dit veld bevat de e-mail van de klant



>[!NOTE]
>Als u gegevens van het ene formulier naar het andere in het pakket wilt verplaatsen, moet u ervoor zorgen dat de formuliervelden in alle formulieren dezelfde naam hebben.

## Alle gereed formulier

Nadat alle formulieren in het pakket zijn ingevuld en ondertekend, moeten we het juiste bericht weergeven. Dit bericht wordt weergegeven met de hulp van het Aldo-formulier. Het Alldone-formulier wordt opgenomen in de voorbeeldformulieren.

## Assets

De voorbeeldformulieren, inclusief de formulieren die in deze zelfstudie worden gebruikt, kunnen [hier gedownload](assets/forms-for-signing.zip)
