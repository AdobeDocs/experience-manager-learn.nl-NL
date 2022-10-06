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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Document naar het bestandssysteem schrijven

Het is gebruikelijk dat de gegenereerde documenten in de workflow naar het bestandssysteem worden geschreven.
Deze stap van het douanewerkschemaproces maakt het gemakkelijk om de werkschemadocumenten aan dossiersysteem te schrijven.
Voor het aangepaste proces worden de volgende door komma&#39;s gescheiden argumenten gebruikt

```java
ChangeBeneficiary.pdf,c:\confirmation
```

Het eerste argument is de naam van het document dat u wilt opslaan in het bestandssysteem. Het tweede argument is de maplocatie waar u het document wilt opslaan. In het bovenstaande geval kunt u het document bijvoorbeeld gebruiken waarnaar `c:\confirmation\ChangeBeneficiary.pdf`

Het volgende het schermschot toont u de argumenten die u tot de stap van het douaneproces moet overgaan
![write-payload-file-system](assets/write-payload-file-system.png)

[Aangepaste bundel kan hier worden gedownload](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
