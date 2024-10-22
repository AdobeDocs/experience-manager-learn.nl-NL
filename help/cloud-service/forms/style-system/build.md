---
title: Stijlsysteem gebruiken in AEM Forms
description: Het themaproject samenstellen
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
source-git-commit: a0de7eaa391749b6b0d90e7cf3e363c2d5a232b5
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---


# Wijzigingen testen

Creeer een adaptieve vorm die op **&quot;Leeg met de malplaatje van de Componenten van de Kern&quot;wordt gebaseerd**. Sleep drie knoppen naar het formulier en geef deze het label &#39;&#39;Corporate&#39;&#39;, &#39;&#39;Marketing&#39;&#39; en &#39;&#39;Default&#39;&#39;.
Wijs de juiste stijlvarianten toe aan de bedrijfs- en marketingknoppen door het penseel hieronder te selecteren.

![ stijlen ](assets/marketing-variation.png)

Op de derde knop wordt de standaardstijl toegepast.

## Het themaproject samenstellen

De volgende stap is het themaproject te bouwen. Navigeer aan de wortelomslag van uw themaproject en stel in werking bevel _**npm looppas bouwt**_ zoals aangetoond in het hieronder ontsproten scherm

![ bouwstijl-thema ](assets/build-theme.png)

Zodra het themaproject met succes wordt gebouwd, zijn uw bereid om de veranderingen te testen.

## Snelle en eenvoudige manier om uw CSS te testen

* Open het bestand theme.css in de map dist van uw themaproject. Selecteer en kopieer de volledige bestandsinhoud.
* Bekijk een voorbeeld van het formulier dat u in de vorige stap hebt gemaakt.
* Klik met de rechtermuisknop op een van de knoppen en selecteer Inspect om de console voor ontwikkelaars te openen.
* Klik in de ontwikkelingsconsole op theme.css om theme.css te openen
* Selecteer en schrap de volledige inhoud van theme.css door CTR-A te gebruiken en de schrappingsknoop te drukken.
* Kopieer en plak de inhoud van theme.css die u in de vroegere stap bouwde.
* De knoppen moeten worden bijgewerkt met de juiste stijlen, zoals hieronder wordt weergegeven.

![ definitief-knopen ](assets/final-state-buttons.png)

