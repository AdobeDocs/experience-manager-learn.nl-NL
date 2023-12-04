---
title: Knooppunt extraheren uit verzonden gegevens-xml
description: Met de stap Aangepast proces voegt u een schrijven-document dat zich in de payload-map bevindt, toe aan het bestandssysteem
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9860
exl-id: 5282034f-275a-479d-aacb-fc5387da793d
last-substantial-update: 2020-07-07T00:00:00Z
duration: 53
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '179'
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

[U kunt hier aangepaste bundel downloaden](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
