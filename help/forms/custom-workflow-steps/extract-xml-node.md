---
title: Het ladingsdocument naar het bestandssysteem schrijven
description: Met de stap Aangepast proces voegt u een schrijven-document dat zich in de payload-map bevindt, toe aan het bestandssysteem
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9860
source-git-commit: 160471fdc34439da6c312d65b252eaa941b7c7a2
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# Knooppunt extraheren uit verzonden gegevens-xml

Deze stap voor aangepast proces bestaat uit het maken van een nieuw XML-document door knooppunten uit een ander XML-document te extraheren. Dit moet u gebruiken wanneer u de verzonden gegevens wilt samenvoegen met de XDP-sjabloon om PDF te genereren. Wanneer u bijvoorbeeld een adaptief formulier verzendt, bevinden de gegevens die u wilt samenvoegen met de xdp-sjabloon zich in het gegevenselement. In dat geval moet u een ander XML-document maken door het juiste gegevenselement uit te pakken.

Het volgende het schermschot toont u de argumenten die u tot de stap van het douaneproces moet overgaan
![processtap](assets/create-xml-process-step.png)
Hieronder volgen de parameters
* Data.xml - Het XML-bestand waaruit u het knooppunt wilt extraheren
* datatomerge.xml - De nieuwe xml die is gemaakt met het geëxtraheerde knooppunt
* /afData/afUnboundData/data - Het knooppunt dat moet worden geëxtraheerd


Het volgende schermschot toont u datamerge.xml die onder de ladingsomslag wordt gecreeerd
![create-xml](assets/create-xml.png)

[Aangepaste bundel kan hier worden gedownload](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)