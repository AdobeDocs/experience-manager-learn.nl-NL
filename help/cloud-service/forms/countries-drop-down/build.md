---
title: De landencomponent bouwen, implementeren en testen
description: Samenstellen, implementeren en testen
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: ab9bd406-e25e-4e3c-9f67-ad440a8db57e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# De landencomponent bouwen, implementeren en testen

Als u alle modules wilt bouwen en het `all` -pakket wilt implementeren in een lokale instantie van AEM, voert u de volgende opdracht uit in de hoofdmap van het project:

```mvn clean install -PautoInstallSinglePackage```

## De component testen

Voer de volgende stappen uit om de component Landen te integreren in uw AEM Forms Cloud Ready-exemplaar en deze te configureren:

* Extraheer de inhoud van het [ landen ](assets/countries.zip) zip dossier. Elk bestand moet de gegevens voor een bepaald continent bevatten.
* Upload de JSON-bestanden onder content/dam/corecomponent.This is de locatie waar de code naar de JSON-bestanden zoekt. Als u de JSON-bestanden op een andere locatie wilt opslaan, moet u de Java-code in de klasse CountriesDropDownImpl bijwerken. Werk met name het pad bij in de methode init() waar de JSON-bestanden worden geladen. Als u bijvoorbeeld de JSON-bestanden wilt opslaan in content/dam/mydata/, werkt u het pad als volgt bij

```java
String jsonPath = "/content/dam/mydata/" + getContinent() + ".json"; // Update path accordingly
```

* Aanmelden bij uw AEM Forms Cloud Ready Instance
* Een adaptief formulier maken en de component Landen op het formulier plaatsen
* De component Landen configureren met behulp van de dialoogeditor en de verschillende eigenschappen instellen, waaronder het continent
  ![ inhoud ](assets/select-continent.png)
* Een voorbeeld van het formulier bekijken en controleren of de vervolgkeuzelijst met landen naar behoren werkt
