---
title: Het ladingsdocument naar het bestandssysteem schrijven
description: Met de stap Aangepast proces voegt u een schrijven-document dat zich in de payload-map bevindt, toe aan het bestandssysteem
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
source-git-commit: 160471fdc34439da6c312d65b252eaa941b7c7a2
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Document naar het bestandssysteem schrijven

Het is gebruikelijk dat de gegenereerde documenten in de workflow naar het bestandssysteem worden geschreven.
Deze stap van het douanewerkschemaproces maakt het gemakkelijk om de werkschemadocumenten aan dossiersysteem te schrijven.
Voor het aangepaste proces worden de volgende door komma&#39;s gescheiden argumenten gebruikt

```java
ChangeBeneficiary.pdf,c:\confirmation
```

Het eerste argument is de naam van het document dat u wilt opslaan in het bestandssysteem. Het tweede argument is de maplocatie waar u het document wilt opslaan. In het bovenstaande geval van gebruik wordt het document bijvoorbeeld geschreven naar c:\confirmation\ChangeBeneficiary.pdf

Het volgende het schermschot toont u de argumenten die u tot de stap van het douaneproces moet overgaan
![write-payload-file-system](assets/write-payload-file-system.png)

[Aangepaste bundel kan hier worden gedownload](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)