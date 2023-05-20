---
title: PDF-bewerking in Forms CS met DDX-bewerking aanroepen
description: U kunt PDF-bestanden samenstellen met DDX aanroepen.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9980
exl-id: 713c4e9e-95ac-48e1-a7fc-2b3ec0b145e5
source-git-commit: e925b9fa02dc8d4695b85377c5f7f43fbd45ebc8
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 1%

---

# Inleiding

In deze cursus gebruiken we de PDF-bewerking en archivering van PDF-documenten met Forms CS. Als u deze microservices vanuit een externe toepassing wilt gebruiken, voert u de volgende stappen uit:

1. Servicereferenties genereren voor een AEM technische account
1. Creeer een Token van het Web JSON (JWT) van de de dienstgeloofsbrieven en ruil het zelfde voor een toegangstoken
1. Toegang voor de technische account configureren in AEM
1. Maak HTTP- vraag gebruikend het toegangstoken om PDF dossiers te manipuleren/PDF- dossiers te produceren en te bevestigen
