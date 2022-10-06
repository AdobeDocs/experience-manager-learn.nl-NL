---
title: Inhoudsfragmenten ontwerpen - Aan de slag met AEM zonder kop - GraphQL
description: Ga aan de slag met Adobe Experience Manager (AEM) en GraphQL. Maak en bewerk een nieuw inhoudsfragment op basis van een inhoudsfragmentmodel. Leer hoe u variaties van inhoudsfragmenten maakt.
version: Cloud Service
mini-toc-levels: 1
kt: 6713
thumbnail: 22451.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 701fae92-f740-4eb6-8133-1bc45a472d0f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '817'
ht-degree: 1%

---

# Inhoudsfragment ontwerpen {#authoring-content-fragments}

In dit hoofdstuk maakt en bewerkt u een nieuw inhoudsfragment op basis van de [nieuw gedefinieerd inhoudsfragmentmodel](./content-fragment-models.md). U leert ook hoe u variaties van inhoudsfragmenten kunt maken.

## Vereisten {#prerequisites}

Dit is een meerdelige zelfstudie en er wordt aangenomen dat de stappen die worden beschreven in het dialoogvenster [Modellen voor inhoudsfragmenten definiëren](./content-fragment-models.md) zijn voltooid.

## Doelstellingen {#objectives}

* Een inhoudsfragment maken op basis van een model van een inhoudsfragment
* Een variatie in een inhoudsfragment maken

## Een map met middelen maken

Inhoudsfragmenten worden opgeslagen in mappen in AEM Assets. Als u inhoudsfragmenten wilt maken op basis van de modellen die u in het vorige hoofdstuk hebt gemaakt, moet u een map maken waarin u de fragmenten kunt opslaan. Er is een configuratie vereist voor de map om het maken van fragmenten van specifieke modellen mogelijk te maken.

1. Navigeer van het scherm AEM Start naar **Activa** > **Bestanden**.

   ![Navigeren naar bestanden met elementen](assets/author-content-fragments/navigate-assets-files.png)

1. Tikken **Maken** in de hoek en tik **Map**. In het resulterende dialoogvenster voert u in:

   * Titel*: **Mijn project**
   * Naam: **mijn-project**

   ![Map maken, dialoogvenster](assets/author-content-fragments/create-folder-dialog.png)

1. Selecteer **Mijn map** map en tik **Eigenschappen**.

   ![Mapeigenschappen openen](assets/author-content-fragments/open-folder-properties.png)

1. Tik op de knop **Cloud Services** tab. Onder **Cloud Configuration** gebruik de wegfinder om te selecteren **Mijn project** configuratie. De waarde moet `/conf/my-project`.

   ![Cloudconfiguratie instellen](assets/author-content-fragments/set-cloud-config-my-project.png)

   Als u deze eigenschap instelt, kunnen Content Fragments worden gemaakt met behulp van de modellen die in het vorige hoofdstuk zijn gemaakt.

1. Tik op de knop **Beleid** tab. Onder **Modellen voor toegestane inhoudsfragmenten** gebruik de wegfinder om te selecteren **Persoon** en **Team** model dat eerder is gemaakt.

   ![Modellen voor inhoudsfragmenten toestaan](assets/author-content-fragments/allowed-content-fragment-models.png)

   Dit beleid wordt automatisch door submappen overgeërfd en kan worden overschreven. Merk op dat u modellen door markeringen kunt ook toestaan of modellen van andere projectconfiguraties toelaten. Dit mechanisme biedt een krachtige manier om uw inhoudshiërarchie te beheren.

1. Tikken **Opslaan en sluiten** om de wijzigingen in de mapeigenschappen op te slaan.

1. Navigeren in het deelvenster **Mijn project** map.

1. Maak een andere map met de volgende waarden:

   * Titel*: **Engels**
   * Naam: **en**

   De beste praktijken zijn het opzetten van projecten voor meertalige ondersteuning. Zie [de volgende documentpagina voor meer informatie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html).


## Een inhoudsfragment maken {#create-content-fragment}

Er worden nu verschillende inhoudsfragmenten gemaakt op basis van de **Team** en **Persoon** modellen.

1. Tik vanaf het scherm AEM starten **Inhoudsfragmenten** om de interface voor inhoudsfragmenten te openen.

   ![UI voor inhoudsfragment](assets/author-content-fragments/cf-fragment-ui.png)

1. In de linkerspoorstaaf breidt zich uit **Mijn project** en tikken **Engels**.
1. Tikken **Maken** om **Nieuw inhoudsfragment** en voert u de volgende waarden in:

   * Locatie: `/content/dam/my-project/en`
   * Inhoudsfragmentmodel: **Persoon**
   * Titel: **John Doe**
   * Naam: `john-doe`

   ![Nieuw inhoudsfragment](assets/author-content-fragments/new-content-fragment-john-doe.png)
1. Tikken **Maken**.
1. Herhaal bovenstaande stappen om een nieuw fragment te maken dat **Alison Smith**:

   * Locatie: `/content/dam/my-project/en`
   * Inhoudsfragmentmodel: **Persoon**
   * Titel: **Alison Smith**
   * Naam: `alison-smith`

   Tikken **Maken** om het nieuwe Person-fragment te maken.

1. Herhaal de stappen om een nieuwe **Team** fragment dat **Team Alpha**:

   * Locatie: `/content/dam/my-project/en`
   * Inhoudsfragmentmodel: **Team**
   * Titel: **Team Alpha**
   * Naam: `team-alpha`

   Tikken **Maken** om het nieuwe fragment van het Team tot stand te brengen.

1. Er moeten nu drie Content Fragments onder zijn **Mijn project** > **Engels**:

   ![Nieuwe inhoudsfragmenten](assets/author-content-fragments/new-content-fragments.png)

## Inhoudsfragmenten voor personen bewerken {#edit-person-content-fragments}

Vervolgens vult u de nieuwe fragmenten met gegevens.

1. Tik op het selectievakje naast **John Doe** en tikken **Openen**.

   ![Inhoudsfragment openen](assets/author-content-fragments/open-fragment-for-editing.png)

1. De Inhoudsfragmenteditor bevat een formulier dat is gebaseerd op het model Inhoudsfragment. Vul de verschillende velden in die u aan de **John Doe** fragment. Upload voor Profielafbeelding uw eigen afbeelding naar AEM Assets.

   ![Inhoudsfragmenteditor](assets/author-content-fragments/content-fragment-editor-jd.png)

1. Tikken **Opslaan en sluiten** om de wijzigingen in het fragment Jan Smit op te slaan.
1. Ga terug naar de interface van het inhoudsfragment en open de interface **Alison Smith** bestand voor bewerking.
1. Herhaal de bovenstaande stappen om de **Alison Smith** fragment met inhoud.

## Inhoudsfragment team bewerken {#edit-team-content-fragment}

1. Open de **Team Alpha** Inhoudsfragment met de gebruikersinterface van het inhoudsfragment.
1. Vul de velden in voor **Titel**, **Korte naam**, en **Beschrijving**.
1. Selecteer **John Doe** en **Alison Smith** Inhoudsfragmenten om de **Teamleden** veld:

   ![Teamleden instellen](assets/author-content-fragments/select-team-members.png)

   >[!NOTE]
   >
   >U kunt ook online nieuwe inhoudsfragmenten maken met de opdracht **Nieuw inhoudsfragment** knop.

1. Tikken **Opslaan en sluiten** om de wijzigingen in het Team Alpha- fragment op te slaan.

## Inhoudsfragmenten publiceren

Na revisie en verificatie publiceert u de geautoriseerde `Content Fragments`

1. Tik vanaf het scherm AEM starten **Inhoudsfragmenten** om de interface voor inhoudsfragmenten te openen.

1. In de linkerspoorstaaf breidt zich uit **Mijn project** en tikken **Engels**.

1. Tik op het selectievakje naast de inhoudsfragmenten en tik op **Publiceren**

   ![Inhoudsfragment publiceren](assets/author-content-fragments/publish-content-fragment.png)

## Gefeliciteerd! {#congratulations}

U hebt zojuist meerdere inhoudsfragmenten gemaakt en een variatie gemaakt.

## Volgende stappen {#next-steps}

In het volgende hoofdstuk: [GraphQL API&#39;s verkennen](explore-graphql-api.md), zult u AEM GraphQL APIs gebruikend het ingebouwde hulpmiddel GrapiQL onderzoeken. Leer hoe AEM automatisch een GrafiekQL-schema genereert dat op een model van het Fragment van de Inhoud wordt gebaseerd. U zult het construeren van basisvragen gebruikend de syntaxis GraphQL experimenteren.

## Verwante documentatie

* [Contentfragmenten beheren](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-managing.html)
* [Variaties - Authoring van content voor fragmenten](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html)
