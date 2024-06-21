---
title: Adrescomponent maken
description: Nieuwe adreskern-component maken in AEM Forms Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
source-git-commit: a8fc8fa19ae19e27b07fa81fc931eca51cb982a1
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---


# Adrescomponent maken

Meld u aan bij CRXDE van uw lokale, voor de cloud geschikte exemplaar van AEM Forms.

Maak een kopie van het dialoogvenster ``/apps/bankingapplication/components/adaptiveForm/button`` en hernoemen in adresblok. Selecteer de adressblock knoop en plaats zijn eigenschappen zoals hieronder getoond.

>[!NOTE]
>
> ``bankingapplication`` is appId die werd verstrekt toen het creëren van het Maven project. Deze appID kan in uw omgeving anders zijn. U kunt een exemplaar van om het even welke component maken, toevallig maakte ik enkel een exemplaar van de knoopcomponent


![adresblok](assets/address-properties.png)

## eigenschappen van cq-sjabloonknooppunten

Selecteer de ``cq-template`` knooppunt onder ``addressblock`` en stelt de eigenschappen ervan in zoals hieronder weergegeven. Het veldType is ingesteld op deelvenster
![cq-template](assets/cq-template.png)

## Knooppunten toevoegen onder cq-sjabloon

De volgende knooppunten van het type toevoegen ``nt:unstructured`` krachtens ``cq-template``

* streetaddress
* stad
* zip
* state

Deze knopen vertegenwoordigen de gebieden van de component van het adresblok. De velden streetaddress, city en zip zijn een tekstinvoerveld en het statusveld is een vervolgkeuzeveld.

## Eigenschappen van streetadresknooppunt instellen

>[!NOTE]
>
> De **_banktoepassing_** in het pad verwijst naar de appId van het gemaakte project. Dit kan in uw omgeving anders zijn

Selecteer de ``streetaddress`` en stelt de eigenschappen ervan in zoals hieronder weergegeven.
![straatadres](assets/streetaddress.png)

## Eigenschappen van stadsknooppunt instellen

Selecteer de ``city`` en stelt de eigenschappen ervan in zoals hieronder weergegeven.
![stad](assets/city.png)

## Eigenschappen van ZIP-knooppunt instellen

Selecteer de ``zip`` en stelt de eigenschappen ervan in zoals hieronder weergegeven.
![zip](assets/zip.png)

## Eigenschappen van statusknooppunt instellen

Selecteer de ``state`` en stelt de eigenschappen ervan in zoals hieronder weergegeven. Let op het fieldType van de status - deze is ingesteld op een vervolgkeuzelijst
![state](assets/state.png)

De uiteindelijke adresblokcomponent ziet er als volgt uit

![eindadres](assets/crx-address-block.png)

## Volgende stappen

[Het project implementeren](./deploy-your-project.md)




