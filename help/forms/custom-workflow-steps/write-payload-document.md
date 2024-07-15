---
title: Het ladingsdocument naar het bestandssysteem schrijven
description: Met de stap Aangepast proces voegt u een schrijven-document dat zich in de payload-map bevindt, toe aan het bestandssysteem
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
exl-id: bab7c403-ba42-4a91-8c86-90b43ca6026c
last-substantial-update: 2020-07-07T00:00:00Z
duration: 27
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Document naar het bestandssysteem schrijven

Het is gebruikelijk dat de gegenereerde documenten in de workflow naar het bestandssysteem worden geschreven.
Deze stap van het douanewerkschemaproces maakt het gemakkelijk om de werkschemadocumenten aan dossiersysteem te schrijven.
Het aangepaste proces heeft de volgende door komma&#39;s gescheiden argumenten

```java
ChangeBeneficiary.pdf,c:\confirmation
```

Het eerste argument is de naam van het document dat u wilt opslaan in het bestandssysteem. Het tweede argument is de maplocatie waar u het document wilt opslaan. In het bovenstaande geval moet u het document bijvoorbeeld naar `c:\confirmation\ChangeBeneficiary.pdf` schrijven

Het volgende het schermschot toont u de argumenten die u tot de stap van het douaneproces moet overgaan
![ schrijven-lading-dossier-systeem ](assets/write-payload-file-system.png)

[U kunt hier aangepaste bundel downloaden](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
