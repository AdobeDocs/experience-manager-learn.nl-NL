---
title: Deelvenster Investeringmix configureren
description: Dit maakt deel uit van een zelfstudie met meerdere stappen voor het maken van uw eerste interactieve communicatiedocument. In dit deel voegen we schijfgrafieken toe om de huidige en modelinvesteringsmix weer te geven.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
exl-id: 774d7a6e-2b8f-4a70-98c5-e7712478ff75
duration: 97
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---

# Deelvenster Investeringmix configureren

In dit deel, zullen wij cirkelgrafieken toevoegen om de huidige en modelinvesteringsmengeling te tonen.

* Meld u aan bij AEM Forms en navigeer naar Adobe Experience Manager > Forms > Forms &amp; Documents.

* Open de map 401KStatement.

* Open de 401KStatement in bewerkingsmodus.

* We voegen twee schijfgrafieken toe die de huidige en modelinvesteringsmix van de rekeninghouder weergeven.

## Huidige middelenmix {#current-asset-mix}

* Tik op het deelvenster &quot;CurrentAssetMix&quot; aan de rechterkant en selecteer het pictogram &quot;+&quot; en voeg tekstcomponent in. Wijzig de standaardtekst in &quot;Huidige assetmix&quot;.

* Tik op het deelvenster &quot;CurrentAssetMix&quot; en selecteer het pictogram &quot;+&quot; en voeg diagramcomponent in. Tik op de nieuw ingevoegde diagramcomponent en klik op het pictogram &quot;moersleutel&quot; om de pagina met configuratie-eigenschappen voor het diagram te openen.

* Stel de eigenschappen in zoals in de onderstaande afbeelding. Zorg ervoor dat het diagramtype schijfdiagram is.

* Let op het gegevensmodelobject dat aan de X- en Y-as is gebonden. U moet het hoofdelement van het formuliergegevensmodel selecteren en vervolgens naar beneden gaan om het juiste element te selecteren.

* ![currentassetmix](assets/currentassetmixchart.png)

## Model Asset Mix {#model-asset-mix}

* Tik op het deelvenster &quot;RecommendedAssetMix&quot; aan de rechterkant en selecteer het pictogram &quot;+&quot; en voeg tekstcomponent in. Wijzig de standaardtekst in &quot;Model Asset Mix&quot;.

* Tik op het deelvenster &quot;RecommendedAssetMix&quot; en selecteer het pictogram &quot;+&quot; en voeg diagramcomponent in. Tik op de nieuw ingevoegde diagramcomponent en klik op het pictogram &quot;moersleutel&quot; om de pagina met configuratie-eigenschappen voor het diagram te openen.

* Stel de eigenschappen in zoals in de onderstaande afbeelding. Zorg ervoor dat het diagramtype schijfdiagram is.

* Let op het gegevensmodelobject dat aan de X- en Y-as is gebonden. U moet het hoofdelement van het formuliergegevensmodel selecteren en vervolgens naar beneden gaan om het juiste element te selecteren.

* ![assettype](assets/modelassettypechart.png)

## Volgende stappen

[Voorbereiden op levering van webkanaaldocument](./parttwelve.md)
