---
title: Microservices voor het genereren van documenten in AEM Forms CS
description: De microservices voor het genereren van documenten uit een externe toepassing gebruiken.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: Document Services
topic: Ontwikkeling
kt: 7432
thumbnail: 332439.jpg
source-git-commit: 33cb3d18b744d9a8e54a87152079b42ed09212f2
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 2%

---

# Inleiding

In deze cursus, zullen wij de microdiensten van de documentgeneratie gebruiken om een pdf te produceren door gegevens met een malplaatje samen te voegen XDP. Als u deze microservices vanuit een externe toepassing wilt gebruiken, voert u de volgende stappen uit:

1. Servicereferenties genereren voor een AEM technische account
1. Creeer een Token van het Web JSON (JWT) van de de dienstgeloofsbrieven en ruil het zelfde voor een toegangstoken
1. Toegang voor de technische account configureren in AEM
1. HTTP-aanroepen maken met behulp van het toegangstoken