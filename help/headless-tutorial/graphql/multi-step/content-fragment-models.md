---
title: Modellen voor inhoudsfragmenten definiëren - Aan de slag met AEM headless - GraphQL
description: Ga aan de slag met Adobe Experience Manager (AEM) en GraphQL. Leer hoe u inhoud modelleert en een schema samenstelt met Content Fragment Models in AEM. Beoordeel bestaande modellen en maak een model. Leer over de verschillende gegevenstypes die kunnen worden gebruikt om een schema te bepalen.
version: Cloud Service
mini-toc-levels: 1
jira: KT-6712
thumbnail: 22452.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 9400d9f2-f828-4180-95a7-2ac7b74cd3c9
duration: 228
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1110'
ht-degree: 0%

---

# Modellen voor inhoudsfragmenten definiëren {#content-fragment-models}

In dit hoofdstuk, leer hoe te om inhoud te modelleren en een schema met **Modellen van het Fragment van de Inhoud te bouwen**. U leert over de verschillende gegevenstypen die kunnen worden gebruikt om een schema als deel van het model te bepalen.

Wij creëren twee eenvoudige modellen, **Team** en **Persoon**. Het **gegevensmodel van het 0} Team {heeft naam, korte naam, en beschrijving en verwijzingen het** Person **gegevensmodel, dat volledige naam, biodetails, profielbeeld, en bezettingenlijst heeft.**

U kunt ook uw eigen model maken aan de hand van de basisstappen en de respectieve stappen aanpassen, zoals GraphQL-query&#39;s, en de App-code Reageren of gewoon de stappen volgen die in deze hoofdstukken worden beschreven.

## Vereisten {#prerequisites}

Dit is een meerdelig leerprogramma en het wordt verondersteld dat een [ AEM auteursmilieu ](./overview.md#prerequisites) beschikbaar is.

## Doelstellingen {#objectives}

* Maak een inhoudsfragmentmodel.
* Beschikbare gegevenstypen en validatieopties identificeren voor het samenstellen van modellen.
* Begrijp hoe het Model van het Fragment van de Inhoud **zowel** het gegevensschema als het auteursmalplaatje voor een Fragment van de Inhoud bepaalt.

## Een projectconfiguratie maken

Een projectconfiguratie bevat alle modellen van het Fragment van de Inhoud verbonden aan een bepaald project en verstrekt een middel om modellen te organiseren. Minstens één project moet worden gecreeerd **alvorens** het creëren van het Model van het Fragment van de Inhoud.

1. Login aan het AEM **milieu van de Auteur** (zoals `https://author-pYYYY-eXXXX.adobeaemcloud.com/`)
1. Van het AEM scherm van het Begin, navigeer aan **Hulpmiddelen** > **Algemeen** > **Browser van de Configuratie**.

   ![ ga aan Browser van de Configuratie ](assets/content-fragment-models/navigate-config-browser.png)
1. Klik **creëren**, in de hoger-juiste hoek
1. In het resulterende dialoogvenster voert u in:

   * Titel*: **Mijn Project**
   * Naam*: **mijn-project** (verkies om alle kleine letters te gebruiken gebruikend koppeltekens aan afzonderlijke woorden. Deze tekenreeks beïnvloedt het unieke GraphQL-eindpunt waarop clienttoepassingen aanvragen uitvoeren.)
   * De Modellen van het Fragment van de controle **Inhoud**
   * Controle **de Blijvende Vragen van GraphQL**

   ![ Mijn Configuratie van het Project ](assets/content-fragment-models/my-project-configuration.png)

## Modellen voor inhoudsfragmenten maken

Daarna, creeer twee modellen voor a **Team** en a **Persoon**.

### Het personenmodel maken

Creeer een model voor a **Persoon**, dat het gegevensmodel is dat een persoon vertegenwoordigt die deel van een team uitmaakt.

1. Van het AEM scherm van het Begin, navigeer aan **Hulpmiddelen** > **Algemeen** > **Modellen van het Fragment van de Inhoud**.

   ![ ga aan de Modellen van het Fragment van de Inhoud ](assets/content-fragment-models/navigate-cf-models.png)

1. Navigeer in de **Mijn omslag van het Project**.
1. Tik **creeer** in de hoger-juiste hoek om **te brengen creeer Model** tovenaar.
1. Op **ModelTitel** gebied, ga **Persoon** in en tik **creeer**. In de resulterende dialoog, tik **Open**, om het model te bouwen.

1. De belemmering en laat vallen a **Enige lijntekst** element op aan het belangrijkste paneel. Ga de volgende eigenschappen op het **Eigenschappen** lusje in:

   * **Etiket van het Gebied**: **Volledige Naam**
   * **de Naam van het Bezit**: `fullName`
   * Controle **Vereiste**

   ![ Volledige het bezitsgebied van de Naam ](assets/content-fragment-models/full-name-property-field.png)

   De **Naam van het Bezit** bepaalt de naam van het bezit dat aan AEM wordt voortgeduurd. De **Naam van het Bezit** bepaalt ook de **zeer belangrijke** naam voor dit bezit als deel van het gegevensschema. Deze **sleutel** wordt gebruikt wanneer de gegevens van het Fragment van de Inhoud via GraphQL APIs worden blootgesteld.

1. Tik het **lusje van de Types van Gegevens 0} {en sleep en laat vallen a** Meerdere lijntekst **gebied onder het** Volledige gebied van de Naam **.** Voer de volgende eigenschappen in:

   * **Etiket van het Gebied**: **Biografie**
   * **de Naam van het Bezit**: `biographyText`
   * **StandaardType**: **Rijke Tekst**

1. Klik het **lusje van de Types van Gegevens 0} {en belemmering en laat vallen het gebied van de Verwijzing van de a** Inhoud **.** Voer de volgende eigenschappen in:

   * **Etiket van het Gebied**: **Beeld van het Profiel**
   * **de Naam van het Bezit**: `profilePicture`
   * **Weg van de wortel**: `/content/dam`

   Wanneer het vormen van de **Weg van de Weg van de Wortel**, kunt u het **omslag** pictogram klikken om modaal omhoog te brengen om de weg te selecteren. Hierdoor wordt beperkt welke mappen auteurs kunnen gebruiken om het pad te vullen. `/content/dam` is de basis waarin alle AEM Assets (afbeeldingen, video&#39;s, andere inhoudsfragmenten) wordt opgeslagen.

1. Voeg een bevestiging aan de **Verwijzing van het Beeld** toe zodat slechts inhoudstypes van **Beelden** kunnen worden gebruikt om het gebied te bevolken.

   ![ Beperk tot Beelden ](assets/content-fragment-models/picture-reference-content-types.png)

1. Klik het **lusje van de Types van Gegevens 0} {en sleep en laat vallen een** Opsomming **gegevenstype onder het** **gebied van de Verwijzing van het Beeld.** Voer de volgende eigenschappen in:

   * **geeft terug als**: **checkboxes**
   * **Etiket van het Gebied**: **Bezetting**
   * **de Naam van het Bezit**: `occupation`

1. Voeg verscheidene **Opties** toe gebruikend **voeg een optie** knoop toe. Gebruik de zelfde waarde voor **Etiket van de Optie** en **Waarde van de Optie**:

   **Kunstenaar**, **Influencer**, **Fotograaf**, **Reiziger**, **Schrijver**, **YouTuber**

1. Het definitieve **model van de Persoon** zou als het volgende moeten kijken:

   ![ Eind Person Model ](assets/content-fragment-models/final-author-model.png)

1. Klik **sparen** om de veranderingen te bewaren.

### Het teammodel maken

Creeer een model voor a **Team**, dat het gegevensmodel voor een team van mensen is. Het model van het Team verwijzingen het model van de Persoon om de leden van het team te vertegenwoordigen.

1. In de **Mijn omslag van het Project**, leidt de kraan **** tot in de hogere juiste hoek om **te brengen tot modeltovenaar**.
1. Op **ModelTitel** gebied, ga **Team** in en tik **creeer**.

   Tik **Open** in de resulterende dialoog, om het pas gecreëerde model te openen.

1. De belemmering en laat vallen a **Enige lijntekst** element op aan het belangrijkste paneel. Ga de volgende eigenschappen op het **Eigenschappen** lusje in:

   * **Etiket van het Gebied**: **Titel**
   * **de Naam van het Bezit**: `title`
   * Controle **Vereiste**

1. Tik het **lusje van de Types van Gegevens 0} {en sleep en versleep a** Enige lijntekst **element op het belangrijkste paneel.** Ga de volgende eigenschappen op het **Eigenschappen** lusje in:

   * **Etiket van het Gebied**: **Korte Naam**
   * **de Naam van het Bezit**: `shortName`
   * Controle **Vereiste**
   * Controle **Uniek**
   * Onder, **Type van Bevestiging** > kies **Douane**
   * Onder, **Regex van de Bevestiging van de Douane** > ga `^[a-z0-9\-_]{5,40}$` in - dit zorgt ervoor dat slechts de waarden en de streepjes van kleine letters van 5 door 40 karakters kunnen worden ingegaan.

   De eigenschap `shortName` biedt ons een manier om een query uit te voeren voor een afzonderlijk team op basis van een verkort pad. Het **Unieke** plaatsen zorgt ervoor dat de waarde altijd uniek per het Fragment van de Inhoud van dit model is.

1. Tik het **lusje van de Types van Gegevens 0} {en sleep en laat vallen a** Meerdere lijntekst **gebied onder het** Korte Naam **gebied.** Voer de volgende eigenschappen in:

   * **Etiket van het Gebied**: **Beschrijving**
   * **de Naam van het Bezit**: `description`
   * **StandaardType**: **Rijke Tekst**

1. Klik het **lusje van de Types van Gegevens 0} {en belemmering en laat vallen het gebied van de Verwijzing van het a** Fragment **.** Voer de volgende eigenschappen in:

   * **geeft terug als**: **Veelvoudig Gebied**
   * **Etiket van het Gebied**: **Leden van het Team**
   * **de Naam van het Bezit**: `teamMembers`
   * **Toegestane Modellen van het Fragment van de Inhoud**: Gebruik het omslagpictogram om het **Person** model te selecteren.

1. Het definitieve **model van het Team** zou als het volgende moeten kijken:

   ![ Definitief Model van het Team ](assets/content-fragment-models/final-team-model.png)

1. Klik **sparen** om de veranderingen te bewaren.

1. U moet nu twee modellen hebben waaruit u kunt werken:

   ![ Twee Modellen ](assets/content-fragment-models/two-new-models.png)

## Publish-modellen voor projectconfiguratie en inhoudsfragmenten

Publiceer na revisie en verificatie de `Project Configuration` &amp; `Content Fragment Model`

1. Van het AEM scherm van het Begin, navigeer aan **Hulpmiddelen** > **Algemeen** > **Browser van de Configuratie**.

1. Tik checkbox naast **Mijn Project** en tik **Publish**

   ![ Config van het Project van Publish ](assets/content-fragment-models/publish-project-config.png)

1. Van het AEM scherm van het Begin, navigeer aan **Hulpmiddelen** > **Algemeen** > **Modellen van het Fragment van de Inhoud**.

1. Navigeer in de **Mijn omslag van het Project**.

1. Tik **Persoon** en **Team** modellen en tik **Publish**

   ![ Modellen van het Fragment van de Inhoud van Publish ](assets/content-fragment-models/publish-content-fragment-model.png)

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, u hebt zojuist uw eerste modellen van inhoudsfragmenten gemaakt!

## Volgende stappen {#next-steps}

In het volgende hoofdstuk, [ het Authoring Modellen van het Fragment van de Inhoud ](author-content-fragments.md), creeert en geeft u een nieuw die Fragment van de Inhoud uit op een Model van het Fragment van de Inhoud wordt gebaseerd. U leert ook hoe u variaties van inhoudsfragmenten kunt maken.

## Verwante documentatie

* [Modellen van contentfragmenten](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)

