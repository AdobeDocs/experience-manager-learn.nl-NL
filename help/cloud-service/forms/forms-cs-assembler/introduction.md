---
title: PDF-bewerking in Forms CS met DDX-bewerking aanroepen
description: PDF-bestanden samenstellen met DDX aanroepen.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 713c4e9e-95ac-48e1-a7fc-2b3ec0b145e5
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 18
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 0%

---

# Inleiding

In deze cursus gebruiken we de PDF-manipulatie en archivering van PDF-documenten met Forms CS. Als u deze microservices vanuit een externe toepassing wilt gebruiken, voert u de volgende stappen uit:

1. Servicereferenties genereren voor een technische AEM-account
1. Creeer een Token van het Web JSON (JWT) van de de dienstgeloofsbrieven en ruil het zelfde voor een toegangstoken
1. Toegang voor de technische account in AEM configureren
1. Maak HTTP- vraag gebruikend het toegangstoken om PDF dossiers te manipuleren/PDF/A dossiers te produceren en te bevestigen
