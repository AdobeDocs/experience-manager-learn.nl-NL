---
title: Aangepast formulier vooraf invullen met gegevens uit aandelenpuntlijst
description: Leer hoe u een adaptief formulier vooraf instelt met een formuliergegevensmodel dat wordt ondersteund door een lijst met deelpunten
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14795
duration: 46
exl-id: 9abe9f9d-8fb3-4e01-a830-1dad1c27274d
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 0%

---

# Aangepast formulier vooraf invullen met gegevens uit de deelpuntlijst

In de vorige versie van AEM formulier(6.5) moest aangepaste code worden geschreven naar het vooraf ingevulde formuliergegevensmodel met ondersteuning voor adaptief formulier met behulp van aanvraagkenmerk. In AEM Forms als cloudservice is het niet langer nodig aangepaste code te schrijven.

In dit artikel worden de stappen beschreven die nodig zijn om een adaptief formulier vooraf in te vullen of in te vullen met gegevens die uit de SharePoint-lijst zijn opgehaald met de service voor het vooraf invullen van het formuliergegevensmodel.

Dit artikel veronderstelt u [ met succes gevormd adaptieve vorm hebt om gegevens naar de lijst van SharePoint voor te leggen.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=en#connect-af-sharepoint-list)

Hier volgt een overzicht van de gegevens in de SharePoint-lijst
![ sharepoint-list ](assets/list-data.png)

Als u een adaptief formulier vooraf wilt invullen met de gegevens die bij een bepaalde hulplijn horen, moeten de volgende stappen worden uitgevoerd

## Vorm de get dienst

* Een get-service maken voor het object op het hoogste niveau van het formuliergegevensmodel met het kenmerk guid
  ![ get-service ](assets/mapping-request-attribute.png)

In deze schermafbeelding is de hulplijnkolom gebonden via een aanvraagkenmerk met de naam `submissionid` .

De get dienst volledig gevormd kijkt als dit

![ get-service ](assets/fdm-request-attribute.png)

## Het aangepaste formulier configureren voor het gebruik van de vooraf ingevulde service van het formuliergegevensmodel

* Open een adaptief formulier op basis van het formuliergegevensmodel van de lijst met deelpunten. De service Prefill-up formuliergegevensmodel koppelen
  ![ vorm-prefill-dienst ](assets/form-prefill-service.png)

## Het formulier testen

Geef een voorbeeld van het formulier weer door de `submissionid` op te nemen in de URL, zoals hieronder wordt weergegeven

```html
http://localhost:4502/content/dam/formsanddocuments/contactusform/jcr:content?wcmmode=disabled&submissionid=57e12249-751a-4a38-a81f-0a4422b24412
```
