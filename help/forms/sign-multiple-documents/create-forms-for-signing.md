---
title: Forms maken voor ondertekening
description: Maak formulieren die moeten worden opgenomen in het ondertekeningspakket.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6893
thumbnail: 6893.jpg
topic: Development
role: User
level: Beginner
exl-id: 565d81a4-2918-44ea-a3e9-ed79f246f08a
duration: 71
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# Formulieren maken voor ondertekening

De volgende stap bestaat uit het maken van de aangepaste formulieren die u in het pakket wilt opnemen. Houd er rekening mee dat u zich aan de volgende punten houdt wanneer u formulieren maakt voor ondertekening:

* Zorg ervoor de vormen op het **SignMultipleForms** malplaatje gebaseerd zijn. Op deze manier zorgt u ervoor dat de formulieren vooraf worden ingevuld met de gegevens die uit de database worden opgehaald.

* De formulieren moeten zijn geconfigureerd voor het gebruik van Acrobat Sign en het veld signer1 moet zijn gekoppeld aan het veld Customer Email.
* De vormen moeten ook met clientLib worden geassocieerd geroepen **getnextform**
* De formulieren moeten de component Signature Step gebruiken.
* De vorm moet de douane **Meerdere component van de Vorm van het Teken {ook gebruiken 1}.** Met deze component kunt u naar het volgende formulier navigeren om u aan te melden bij het pakket.
* De voorlegging van de vorm moet worden gevormd om het werkschema van AEM **de Status van de Ondertekening van de Update** teweeg te brengen
* Zorg ervoor de Weg van het Dossier van Gegevens aan **Data.xml** wordt geplaatst. Dit is erg belangrijk omdat de voorbeeldcode zoekt naar een bestand met de naam Data.xml tijdens het laden van het formulier.

Zodra u uw vorm hebt authored, omvat **gemeenschappelijke gebieden** adaptief vormfragment in de vorm. Het fragment wordt gemarkeerd als verborgen. Dit fragment bevat de volgende velden.

* **ondertekend** - het gebied om het statuut van de handtekening te houden
* **leidraad** - Unieke herkenningsteken om de vorm in het pakket te identificeren
* **customerEmail** - Dit gebied houdt e-mail van de klant



>[!NOTE]
>Als u gegevens van het ene formulier naar het andere in het pakket wilt verplaatsen, moet u ervoor zorgen dat de formuliervelden in alle formulieren dezelfde naam hebben.

## Alle gereed formulier

Nadat alle formulieren in het pakket zijn ingevuld en ondertekend, moeten we het juiste bericht weergeven. Dit bericht wordt weergegeven met de hulp van het Aldo-formulier. Het Alldone-formulier wordt opgenomen in de voorbeeldformulieren.

## Assets

De steekproefvormen met inbegrip van het gebruikte in dit leerprogramma kunnen [ van hier worden gedownload ](assets/forms-for-signing.zip)

## Volgende stappen

[Test de oplossing op uw lokale systeem](./testing-and-trouble-shooting.md)
