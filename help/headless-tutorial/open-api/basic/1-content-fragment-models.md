---
title: Modellen voor inhoudsfragmenten maken | AEM Headless OpenAPI Part 1
description: Ga aan de slag met Adobe Experience Manager (AEM) en OpenAPI. Leer hoe u inhoud modelleert en een schema bouwt met Content Fragment Models in AEM. Beoordeel bestaande modellen en maak een model. Leer over de verschillende gegevenstypes die kunnen worden gebruikt om een schema te bepalen.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6712
thumbnail: 22452.jpg
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 430
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '930'
ht-degree: 0%

---

# Modellen voor inhoudsfragmenten definiëren

In dit hoofdstuk, leer hoe te om inhoud te modelleren en een schema met **Modellen van het Fragment van de Inhoud** te bouwen, en over de verschillende gegevenstypes die een Model van het Fragment van de Inhoud bepalen.

In dit leerprogramma, creeert u twee eenvoudige modellen, **Team** en **Persoon**. Het **gegevensmodel van het 0} Team {heeft naam, korte naam, en beschrijving en verwijzingen het** Person **gegevensmodel, dat volledige naam, biodetails, profielbeeld, en bezettingenlijst heeft.**

## Doelstellingen

* Maak een inhoudsfragmentmodel.
* Ontdek de beschikbare gegevenstypen en validatieopties voor het samenstellen van modellen.
* Begrijp hoe de Modellen van het Fragment van de Inhoud **zowel** het gegevensschema als het auteursmalplaatje voor een Fragment van de Inhoud bepalen.

## Een projectconfiguratie maken

Een projectconfiguratie bevat alle Modellen van het Fragment van de Inhoud verbonden aan een bepaald project en verstrekt een middel om modellen te organiseren. Creeer minstens één project **alvorens** het Model van het Fragment van de Inhoud te creëren.

1. Login aan het milieu van de Auteur van AEM **** (zoals `https://author-p<PROGRAM_ID>-e<ENVIRONMENT_ID>.adobeaemcloud.com/`)
1. Van het scherm van het Begin van AEM, navigeer aan **Hulpmiddelen** > **Algemeen** > **Browser van de Configuratie**.
1. Klik **creeer** in de hoogste actiebar en ga de volgende configuratiedetails in:
   * Titel: **Mijn Project**
   * Naam: **my-project**
   * De Modellen van het Fragment van de inhoud: **Gecontroleerde**

   ![ Mijn Configuratie van het Project ](assets/1/create-configuration.png)

1. Selecteer **creeer** om de projectconfiguratie tot stand te brengen.

## Modellen voor inhoudsfragmenten maken

Daarna, creeer de Modellen van het Fragment van de Inhoud voor a **Team** en a **Persoon**. Deze zullen als gegevensmodellen, of schema&#39;s, die een team vertegenwoordigen, en een persoon handelen die deel van een team uitmaakt, en zullen de interface voor auteurs bepalen om de Fragmenten van de Inhoud tot stand te brengen en uit te geven die op deze modellen worden gebaseerd.

### Het fragmentmodel voor persoonlijke inhoud maken

Creeer een Model van het Fragment van de Inhoud voor a **Persoon**, dat het gegevensmodel, of schema is, die een persoon vertegenwoordigen die deel van een team uitmaakt.

1. Van het scherm van het Begin van AEM, navigeer aan **Hulpmiddelen** > **Algemeen** > **Modellen van het Fragment van de Inhoud**.
1. Navigeer in de **Mijn omslag van het Project**.
1. Selecteer **creeer** in de hoger-juiste hoek om **te brengen creeer Model** tovenaar.
1. Maak een inhoudsfragmentmodel met de volgende eigenschappen:

   * ModelTitel: **Persoon**
   * Laat model toe: **Gecontroleerd**

   Selecteer **Maken**. In de resulterende dialoog, uitgezochte **Open**, om het model te bouwen.

1. De belemmering en laat vallen a **Enige lijntekst** element op aan het belangrijkste paneel. Ga de volgende eigenschappen op het **Eigenschappen** lusje in:

   * Het Etiket van het gebied: **Volledige Naam**
   * Eigenschapnaam: `fullName`
   * Controle **Vereiste**

   De **Naam van het Bezit** bepaalt de naam van het bezit waar de authored waarde in AEM wordt opgeslagen. De **Naam van het Bezit** bepaalt ook de **zeer belangrijke** naam voor dit bezit als deel van het gegevensschema, en gebruikt als sleutel in de reactie JSON wanneer het Fragment van de Inhoud via AEM OpenAPIs wordt geleverd.

1. Selecteer het **lusje van de Types van Gegevens 0} {en belemmering en laat vallen a** Multi gebied van de lijntekst **onder het** Volledige gebied van de Naam **.** Voer de volgende eigenschappen in:

   * Het Etiket van het gebied: **Biografie**
   * Eigenschapnaam: `biographyText`
   * Standaardtype: **Rijke Tekst**

1. Klik het **lusje van de Types van Gegevens 0} {en belemmering en laat vallen het gebied van de Verwijzing van de a** Inhoud **.** Voer de volgende eigenschappen in:

   * Het Etiket van het gebied: **Beeld van het Profiel**
   * Eigenschapnaam: `profilePicture`
   * Hoofdpad: `/content/dam`

     Wanneer het vormen van de **Weg van de Weg van de Wortel**, kunt u het **omslag** pictogram klikken om modaal omhoog te brengen om de weg te selecteren. Hierdoor wordt beperkt welke mappen auteurs kunnen gebruiken om het pad te vullen. `/content/dam` is de basis waarin alle AEM Assets (afbeeldingen, video&#39;s, andere inhoudsfragmenten) wordt opgeslagen.

   * Accepteer slechts specifieke inhoudstypes: **Beeld**

     Voeg een bevestiging aan de **Verwijzing van het Beeld** toe zodat slechts inhoudstypes van **Beelden** kunnen worden gebruikt om het gebied te bevolken.

   * Toon Duimnagel: **Gecontroleerd**

1. Klik het **lusje van de Types van Gegevens 0} {en sleep en laat vallen een** Opsomming **gegevenstype onder het** **gebied van de Verwijzing van het Beeld.** Voer de volgende eigenschappen in:

   * Renderen als: **Checkboxes**
   * Het Etiket van het gebied: **Bezetting**
   * Eigenschapnaam: `occupation`
   * Opties:
      * **Kunstenaar**
      * **Influencer**
      * **Fotograaf**
      * **reiziger**
      * **Schrijver**
      * **YouTuber**

   Stel zowel het label als de waarde voor Optie in op dezelfde waarde.

1. Het definitieve **model van de Persoon** zou als het volgende moeten kijken:

   ![ Model van het Fragment van de Inhoud van de Persoon ](assets/1/person-content-fragment-model.png)

1. Klik **sparen** om de veranderingen te bewaren.

### Maak het model voor het fragment met teaminhoud

Creeer een Model van het Fragment van de Inhoud voor a **Team**, dat het gegevensmodel voor een team van mensen is. Het model van het Team verwijzingen de Fragments van de Inhoud van de Persoon, die de leden van het team vertegenwoordigen.

1. In de **Mijn omslag van het Project**, **creeer** in de hogere juiste hoek om **te brengen creeer Model** tovenaar.
1. Op **ModelTitel** gebied, ga **Team** in en selecteer **creeer**.

   Selecteer **Open** in de resulterende dialoog, om het pas gecreëerde model te openen.

1. De belemmering en laat vallen a **Enige lijntekst** element op aan het belangrijkste paneel. Ga de volgende eigenschappen op het **Eigenschappen** lusje in:

   * Het Etiket van het gebied: **Titel**
   * Eigenschapnaam: `title`
   * Controle **Vereiste**

1. Selecteer het **lusje van de Types van Gegevens 0} {en belemmering en laat vallen a** Multi gebied van de lijntekst **onder het** Korte gebied van de Naam **.** Voer de volgende eigenschappen in:

   * Het Etiket van het gebied: **Beschrijving**
   * Eigenschapnaam: `description`
   * Standaardtype: **Rijke Tekst**

1. Klik het **lusje van de Types van Gegevens 0} {en belemmering en laat vallen het gebied van de Verwijzing van het a** Fragment **.** Voer de volgende eigenschappen in:

   * Renderen als: **Veelvoudig Gebied**
   * Minimum Aantal Punten: **2**
   * Het Etiket van het gebied: **Leden van het Team**
   * Eigenschapnaam: `teamMembers`
   * Toegestane Modellen van het Fragment van de Inhoud: Gebruik het omslagpictogram om het **Person** model te selecteren.

1. Het definitieve **model van het Team** zou als het volgende moeten kijken:

   ![ Definitief Model van het Fragment van de Inhoud van het Team ](assets/1/team-content-fragment-model.png)

1. Klik **sparen** om de veranderingen te bewaren.

1. U moet nu twee modellen hebben waaruit u kunt werken:

   ![ Twee Modellen van het Fragment van de Inhoud ](assets/1/two-content-fragment-models.png)

## Gefeliciteerd!

Gefeliciteerd, u hebt zojuist uw eerste modellen van inhoudsfragmenten gemaakt!

## Volgende stappen

In het volgende hoofdstuk, [ het Authoring Modellen van het Fragment van de Inhoud ](2-author-content-fragments.md), creeert en geeft u een nieuw die Fragment van de Inhoud uit op een Model van het Fragment van de Inhoud wordt gebaseerd. U leert ook hoe u variaties van inhoudsfragmenten kunt maken.

## Gerelateerde documentatie

* [Modellen van contentfragmenten](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)

