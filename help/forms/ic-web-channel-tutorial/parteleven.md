---
title: Deelvenster Investeringmix configureren
seo-title: Configuring Investment Mix Panel
description: Dit maakt deel uit van een zelfstudie met meerdere stappen voor het maken van uw eerste interactieve communicatiedocument. In dit deel voegen we schijfgrafieken toe om de huidige en modelinvesteringsmix weer te geven.
seo-description: This is part 11 of multistep tutorial for creating your first interactive communications document.In this part, we will add pie charts to display the current and model investment mix.
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
exl-id: 774d7a6e-2b8f-4a70-98c5-e7712478ff75
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '334'
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
