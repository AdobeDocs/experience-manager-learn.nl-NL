---
title: Geavanceerde gegevensmodellen met fragmentverwijzingen - Aan de slag met AEM zonder kop - GraphQL
description: Ga aan de slag met Adobe Experience Manager (AEM) en GraphQL. Leer hoe u de functie Fragmentverwijzing gebruikt voor geavanceerde gegevensmodellering en om een relatie tussen twee verschillende inhoudsfragmenten te maken. Leer hoe te om een vraag te wijzigen GraphQL om gebied van een referenced model te omvatten.
sub-product: elementen
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6718
thumbnail: KT-6718.jpg
translation-type: tm+mt
source-git-commit: eb2b556c5947b15a31a74a86dadd525fb06bcf14
workflow-type: tm+mt
source-wordcount: '858'
ht-degree: 0%

---


# Geavanceerde gegevensmodellering met fragmentverwijzingen

>[!CAUTION]
>
> De AEM GraphQL API voor de Levering van Inhoudsfragmenten is op verzoek beschikbaar.
> Neem contact op met de Adobe Support om de API voor uw AEM in te schakelen als een Cloud Service-programma.

Het is mogelijk te verwijzen naar een inhoudsfragment vanuit een ander inhoudsfragment. Dit laat een gebruiker toe om complexe gegevensmodellen met verhoudingen tussen Fragments te bouwen.

In dit hoofdstuk werkt u het Adventure-model zo bij dat een verwijzing naar het Contributor-model wordt opgenomen met behulp van het veld **Fragment Reference**. U zult ook leren hoe te om een vraag te wijzigen GraphQL om gebieden van een referenced model te omvatten.

## Vereisten

Dit is een meerdelige zelfstudie en er wordt aangenomen dat de in de vorige onderdelen beschreven stappen zijn voltooid.

## Doelstellingen

In dit hoofdstuk leert u hoe u:

* Een inhoudsfragmentmodel bijwerken voor het veld Fragmentverwijzing
* Creeer een vraag GraphQL die gebieden van een referenced model terugkeert

## Een fragmentverwijzing toevoegen {#add-fragment-reference}

Werk het model van het Fragment van de Fragment van de Inhoud van de Avontuur bij om een verwijzing naar het model van de Medewerker toe te voegen.

1. Open een nieuwe browser en navigeer naar AEM.
1. Navigeer in het menu **AEM Start** naar **Tools** > **Assets** > **Content Fragment Models** > **WKND Site**.
1. Open het **Adventure** Inhoudsfragmentmodel

   ![Open het fragmentmodel voor Adventure-inhoud](assets/fragment-references/adventure-content-fragment-edit.png)

1. Onder **Gegevenstypen** sleept u een veld **Fragmentverwijzing** naar het hoofdvenster.

   ![Veld Fragmentverwijzing toevoegen](assets/fragment-references/add-fragment-reference-field.png)

1. Werk **Eigenschappen** voor dit gebied met het volgende bij:

   * Renderen als - `fragmentreference`
   * Veldlabel - **Adventure Contributor**
   * Eigenschapnaam - `adventureContributor`
   * Model Type - selecteer **Medewerker** model
   * Hoofdpad - `/content/dam/wknd`

   ![Eigenschappen van fragmentverwijzing](assets/fragment-references/fragment-reference-properties.png)

   De eigenschapsnaam `adventureContributor` kan nu worden gebruikt om naar een Contribute-inhoudsfragment te verwijzen.

1. Sla de wijzigingen in het model op.

## Wijs een Medewerker aan een Avontuur toe

Nu het model van het Fragmentmodel van de Inhoud van het Avontuur is bijgewerkt, kunnen wij een bestaand fragment uitgeven en een Medewerker van verwijzingen voorzien. Houd er rekening mee dat het bewerken van het model Inhoudsfragment *invloed heeft op* bestaande inhoudsfragmenten die uit het model zijn gemaakt.

1. Navigeer naar **Middelen** > **Bestanden** > **WKND Site** > **Engels** > **Adventures** > **[Bali Surf Camp](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**.

   ![De map Bali Surf Camp](assets/setup/bali-surf-camp-folder.png)

1. Klik op het inhoudsfragment **Bali Surf Camp** om de Content Fragment Editor te openen.
1. Werk het veld **Adventure Contributor** bij en selecteer een Medewerker door op het mappictogram te klikken.

   ![Stacey Roswells selecteren als contribuant](assets/fragment-references/stacey-roswell-contributor.png)

   *Een pad naar een Contribute-fragment selecteren*

   ![gevuld pad naar contribuant](assets/fragment-references/populated-path.png)

   U ziet dat alleen fragmenten die zijn gemaakt met het model **Contributor** kunnen worden geselecteerd.

1. Sla de wijzigingen in het fragment op.

1. Herhaal de bovenstaande stappen om een contribuant toe te wijzen aan avonturen zoals [Yosemite Backpackaging](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking) en [Colorado Rotsen Beklimmen](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/colorado-rock-climbing/colorado-rock-climbing)

## Geneste inhoudsfragment zoeken met GraphiQL

Daarna, voer een vraag voor een Avontuur uit en voeg genestelde eigenschappen van het referenced model van de Medewerker toe. Wij zullen het hulpmiddel gebruiken GraphiQL om de syntaxis van de vraag snel te verifiëren.

1. Navigeer naar het gereedschap GraphiQL in AEM: [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html)

1. Voer de volgende query in:

   ```graphql
   {
     adventureByPath(_path:"/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp") {
        item {
          _path
          adventureTitle
          adventureContributor {
            fullName
            occupation
            pictureReference {
           ...on ImageRef {
             _path
           }
         }
       }
     }
    }
   }
   ```

   De bovenstaande vraag is voor één enkel avontuur door het weg van het. De `adventureContributor` bezitsverwijzingen het model van de Medewerker en wij kunnen eigenschappen van het genestelde Fragment van de Inhoud dan verzoeken.

1. Voer de vraag uit en u zou een resultaat als het volgende moeten krijgen:

   ```json
   {
     "data": {
       "adventureByPath": {
           "item": {
               "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
               "adventureTitle": "Bali Surf Camp",
               "adventureContributor": {
                   "fullName": "Stacey Roswells",
                   "occupation": "Photographer",
                   "pictureReference": {
                       "_path": "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
                   }
               }
           }
        }
     }
   }
   ```

1. Experimenteer met andere query&#39;s zoals `adventureList` en voeg eigenschappen toe voor het gerefereerde inhoudsfragment onder `adventureContributor`.

## De React-app bijwerken om de inhoud van de Contribute-server weer te geven

Werk vervolgens de query&#39;s bij die door de React-toepassing worden gebruikt om de nieuwe Contribute op te nemen en geef informatie over de Contribute weer als onderdeel van de weergave met Adventure-details.

1. Open de WKND GraphQL React-app in uw IDE.

1. Open het bestand `src/components/AdventureDetail.js`.

   ![IDE van de component van het Detail van de avontuur](assets/fragment-references/adventure-detail-ide.png)

1. Zoek de functie `adventureDetailQuery(_path)`. De functie `adventureDetailQuery(..)` verpakt eenvoudig een het filtreren vraag GraphQL, die AEM `<modelName>ByPath` syntaxis gebruikt om één enkel die Fragment van de Inhoud te vragen door zijn weg JCR wordt geïdentificeerd.

1. Werk de vraag bij om informatie over referenced Medewerker te omvatten:

   ```javascript
   function adventureDetailQuery(_path) {
       return `{
           adventureByPath (_path: "${_path}") {
           item {
               _path
               adventureTitle
               adventureActivity
               adventureType
               adventurePrice
               adventureTripLength
               adventureGroupSize
               adventureDifficulty
               adventurePrice
               adventurePrimaryImage {
                   ... on ImageRef {
                   _path
                   mimeType
                   width
                   height
                   }
               }
               adventureDescription {
                   html
               }
               adventureItinerary {
                   html
               }
               adventureContributor {
                   fullName
                   occupation
                   pictureReference {
                       ...on ImageRef {
                           _path
                       }
                   }
               }
             }
          }
        }
       `;
   }
   ```

   Met deze update worden aanvullende eigenschappen over `adventureContributor`, `fullName`, `occupation` en `pictureReference` opgenomen in de query.

1. Inspect de `Contributor`-component die is ingesloten in het `AdventureDetail.js`-bestand op `function Contributor(...)`. Deze component geeft de naam, het beroep en het beeld van de medewerker weer als de eigenschappen bestaan.

   Naar de component `Contributor` wordt verwezen in de methode `AdventureDetail(...)` `return`:

   ```javascript
   function AdventureDetail(props) {
       ...
       return (
           ...
            <h2>Itinerary</h2>
           <hr />
           <div className="adventure-detail-itinerary"
                dangerouslySetInnerHTML={{__html: adventureData.adventureItinerary.html}}></div>
           {/* Contributor component is instaniated and 
               is passed the adventureContributor object from the GraphQL Query results */}
           <Contributer {...adventureData.adventureContributor} />
           ...
       )
   }
   ```

1. Sla de wijzigingen in het bestand op.
1. Start de React-app, indien deze nog niet wordt uitgevoerd:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. Navigeer naar [http://localhost:3000](http://localhost:3000/) en klik op een avontuur met een Contribute-medewerker waarnaar wordt verwezen. U moet nu de informatie van de Medewerker onder **Traject** zien:

   ![Medewerker toegevoegd in de app](assets/fragment-references/contributor-added-detail.png)

## Aanvullende bronnen

Voor meer details over de Fragmenten van de Inhoud en GraphQL zie de volgende middelen:

* [Aflevering van inhoud zonder kop met gebruik van inhoudsfragmenten met GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html)
* [AEM GraphQL API voor gebruik met Content Fragments](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html)

## Gefeliciteerd!{#congratulations}

Gefeliciteerd! U hebt een bestaand model van het Fragment van de Inhoud bijgewerkt om naar een genestelde Fragment van de Inhoud te verwijzen gebruikend het **gebied van de Verwijzing van het Fragment**. U leerde ook hoe te om een vraag te wijzigen GraphQL om gebieden van een referenced model te omvatten.
