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
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
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
