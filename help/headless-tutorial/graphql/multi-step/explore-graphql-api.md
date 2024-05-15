---
title: Ontdek GraphQL API's - Aan de slag met AEM headless - GraphQL
description: Ga aan de slag met Adobe Experience Manager (AEM) en GraphQL. Ontdek AEM GraphQL API's met de ingebouwde GrapiQL IDE. Leer hoe AEM automatisch een GraphQL-schema genereert op basis van een model voor inhoudsfragmenten. Experimenteer met het samenstellen van basisquery's met de GraphQL-syntaxis.
version: Cloud Service
mini-toc-levels: 1
jira: KT-6714
thumbnail: KT-6714.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 508b0211-fa21-4a73-b8b4-c6c34e3ba696
duration: 332
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1416'
ht-degree: 0%

---

# GraphQL API&#39;s verkennen {#explore-graphql-apis}

De GraphQL API van AEM biedt een krachtige querytaal om gegevens van Content Fragments toegankelijk te maken voor downstreamtoepassingen. De modellen van het Fragment van de inhoud bepalen het gegevensschema dat door de Fragmenten van de Inhoud wordt gebruikt. Wanneer een Content Fragment Model wordt gemaakt of bijgewerkt, wordt het schema vertaald en toegevoegd aan de &quot;grafiek&quot; die de GraphQL API vormt.

In dit hoofdstuk, onderzoeken sommige gemeenschappelijke vragen van GraphQL om inhoud te verzamelen gebruikend winde riep [GraphiQL](https://github.com/graphql/graphiql). Met GraphiQL IDE kunt u snel de geretourneerde query&#39;s en gegevens testen en verfijnen. Het biedt ook eenvoudige toegang tot de documentatie, waardoor u gemakkelijk kunt leren en begrijpen welke methoden beschikbaar zijn.

## Vereisten {#prerequisites}

Dit is een meerdelige zelfstudie en er wordt aangenomen dat de stappen die in het dialoogvenster [Inhoudsfragmenten ontwerpen](./author-content-fragments.md) zijn voltooid.

## Doelstellingen {#objectives}

* Leer om het hulpmiddel te gebruiken GraphiQL om een vraag te construeren gebruikend de syntaxis van GraphQL.
* Leer hoe u een lijst met inhoudsfragmenten en één inhoudsfragment kunt opvragen.
* Leer hoe u specifieke gegevenskenmerken kunt filteren en aanvragen.
* Leer hoe u een query uitvoert op meerdere modellen van inhoudsfragmenten
* Leer hoe je GraphQL query kunt voortzetten.

## GraphQL-eindpunt inschakelen {#enable-graphql-endpoint}

Een eindpunt van GraphQL moet worden gevormd om GraphQL API vragen voor de Fragments van de Inhoud toe te laten.

1. Navigeer in het scherm AEM starten naar **Gereedschappen** > **Algemeen** > **GraphQL**.

   ![Navigeren naar GraphQL-eindpunt](assets/explore-graphql-api/navigate-to-graphql-endpoint.png)

1. Tikken **Maken** Voer in de rechterbovenhoek van het dialoogvenster dat wordt weergegeven de volgende waarden in:

   * Naam*: **Mijn eindpunt van project**.
   * GraphQL-schema gebruiken dat is opgegeven door ... *: **Mijn project**

   ![GraphQL-eindpunt maken](assets/explore-graphql-api/create-graphql-endpoint.png)

   Tikken **Maken** om het eindpunt te bewaren.

   De GraphQL eindpunten die op een projectconfiguratie worden gecreeerd laten slechts vragen tegen modellen toe die tot dat project behoren. In dit geval zijn de enige vragen tegen de **Persoon** en **Team** modellen kunnen worden gebruikt.

   >[!NOTE]
   >
   > Een Globaal eindpunt kan ook worden gecreeerd om vragen tegen modellen over veelvoudige configuraties toe te laten. Dit moet met voorzichtigheid worden gebruikt aangezien het het milieu voor extra veiligheidskwetsbaarheid kan openen, en aan algemene ingewikkeldheid bij het beheren van AEM kan toevoegen.

1. Er moet nu één GraphQL-eindpunt worden weergegeven dat op uw omgeving is ingeschakeld.

   ![Grafische eindpunten ingeschakeld](assets/explore-graphql-api/enabled-graphql-endpoints.png)

## GraphiQL IDE gebruiken

De [GraphiQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) kunnen ontwikkelaars query&#39;s maken en testen op inhoud in de huidige AEM. Met het gereedschap GraphiQL kunnen gebruikers ook **aanhouden of opslaan** vragen die door cliënttoepassingen in een productie het plaatsen moeten worden gebruikt.

Verken vervolgens de kracht van AEM GraphQL API met behulp van de ingebouwde GraphiQL IDE.

1. Navigeer in het scherm AEM starten naar **Gereedschappen** > **Algemeen** > **GraphQL Query Editor**.

   ![Navigeer aan GrahiQL winde](assets/explore-graphql-api/navigate-graphql-query-editor.png)

   >[!NOTE]
   >
   > In, kunnen de oudere versies van AEM GrahiQL winde niet worden ingebouwd. Hierna kunt u het programma handmatig installeren [instructies](#install-graphiql).

1. Zorg ervoor dat in de rechterbovenhoek het eindpunt is ingesteld op **Mijn eindpunt van project**.

   ![GraphQL-eindpunt instellen](assets/explore-graphql-api/set-my-project-endpoint.png)

Dit zal alle vragen aan modellen behandelen die in **Mijn project** project.

### Een query uitvoeren op een lijst met inhoudsfragmenten {#query-list-cf}

Een algemene vereiste is om te zoeken naar meerdere inhoudsfragmenten.

1. Plak de volgende query in het hoofdvenster (vervang de lijst met opmerkingen):

   ```graphql
   query allTeams {
     teamList {
       items {
         _path
         title
       }
     }
   } 
   ```

1. Druk op **Afspelen** in het bovenste menu om de query uit te voeren. De resultaten van de inhoudsfragmenten uit het vorige hoofdstuk worden weergegeven:

   ![Resultaten personenlijst](assets/explore-graphql-api/all-teams-list.png)

1. De cursor onder de cursor plaatsen `title` tekst en typ **CTRL+Space** om coderingstips te activeren. Toevoegen `shortname` en `description` naar de query.

   ![Query bijwerken met coderingstips](assets/explore-graphql-api/update-query-codehinting.png)

1. Voer de vraag opnieuw uit door te duwen **Afspelen** en u zou moeten zien de resultaten de extra eigenschappen omvatten van `shortname` en `description`.

   ![korte naam en beschrijvingsresultaten](assets/explore-graphql-api/updated-query-shortname-description.png)

   De `shortname` is een eenvoudige eigenschap en `description` is een tekstveld met meerdere regels en met de GraphQL API kunnen we verschillende indelingen kiezen voor de resultaten, zoals `html`, `markdown`, `json`, of `plaintext`.

### Query voor geneste fragmenten

Experimenteer vervolgens met vragen om geneste fragmenten op te halen. Vergeet niet dat de **Team** model verwijst naar de **Persoon** model.

1. Werk de vraag bij om te omvatten `teamMembers` eigenschap. Er zij aan herinnerd dat dit een **Fragmentverwijzing** aan het Person Model. Eigenschappen van het Person-model kunnen worden geretourneerd:

   ```graphql
   query allTeams {
       teamList {
           items {
               _path
               title
               shortName
               description {
                   plaintext
               }
               teamMembers {
                   fullName
                   occupation
               }
           }
       }
   }
   ```

   JSON-reactie:

   ```json
   {
       "data": {
           "teamList": {
           "items": [
               {
               "_path": "/content/dam/my-project/en/team-alpha",
               "title": "Team Alpha",
               "shortName": "team-alpha",
               "description": {
                   "plaintext": "This is a description of Team Alpha!"
               },
               "teamMembers": [
                   {
                   "fullName": "John Doe",
                   "occupation": [
                       "Artist",
                       "Influencer"
                   ]
                   },
                   {
                   "fullName": "Alison Smith",
                   "occupation": [
                       "Photographer"
                   ]
                   }
                 ]
           }
           ]
           }
       }
   }
   ```

   De mogelijkheid om te zoeken op geneste fragmenten is een krachtige functie van de AEM GraphQL API. In dit eenvoudige voorbeeld is het nesten slechts twee niveaus diep. Maar het is mogelijk om fragmenten nog verder te nesten. Als er bijvoorbeeld een **Adres** model gekoppeld aan **Persoon** het zou mogelijk zijn om gegevens van alle drie modellen in één enkele vraag terug te keren.

### Een lijst met inhoudsfragmenten filteren {#filter-list-cf}

Daarna, kijken hoe het mogelijk is om de resultaten aan een ondergroep van Inhoudsfragmenten te filtreren die op een bezitswaarde worden gebaseerd.

1. Ga de volgende vraag in GraphiQL UI in:

   ```graphql
   query personByName($name:String!){
     personList(
       filter:{
         fullName:{
           _expressions:[{
             value:$name
             _operator:EQUALS
           }]
         }
       }
     ){
       items{
         _path
         fullName
         occupation
       }
     }
   }  
   ```

   De bovenstaande query voert een zoekopdracht uit tegen alle Person-fragmenten in het systeem. Het toegevoegde filter aan het begin van de vraag voert een vergelijking op uit `name` veld en variabele tekenreeks `$name`.

1. In de **Query-variabelen** voert u het volgende in:

   ```json
   {"name": "John Doe"}
   ```

1. Voer de vraag uit, wordt het verwacht dat slechts **Personen** Content Fragment wordt geretourneerd met de waarde `John Doe`.

   ![Query-variabelen gebruiken om te filteren](assets/explore-graphql-api/using-query-variables-filter.png)

   Er zijn veel andere opties voor het filteren en maken van complexe query&#39;s. Zie [GraphQL leren gebruiken met AEM - Voorbeeldinhoud en query&#39;s](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/sample-queries.html).

1. Verbeter bovenstaande query om profielafbeelding op te halen

   ```graphql
   query personByName($name:String!){
     personList(
       filter:{
         fullName:{
           _expressions:[{
             value:$name
             _operator:EQUALS
           }]
         }
       }
     ){
       items{  
         _path
         fullName
         occupation
         profilePicture{
           ... on ImageRef{
             _path
             _authorUrl
             _publishUrl
             height
             width
   
           }
         }
       }
     }
   } 
   ```

   De `profilePicture` is een inhoudsverwijzing en wordt verwacht dat het een afbeelding is, en daarom is ingebouwd `ImageRef` wordt gebruikt. Op deze manier kunnen we aanvullende gegevens opvragen over de afbeelding waarnaar wordt verwezen, zoals de `width` en `height`.

### Een query uitvoeren op één inhoudsfragment {#query-single-cf}

Het is ook mogelijk rechtstreeks een query uit te voeren op één inhoudsfragment. Inhoud in AEM wordt hiërarchisch opgeslagen en de unieke id voor een fragment is gebaseerd op het pad van het fragment.

1. Ga de volgende vraag in de redacteur GraphiQL in:

   ```graphql
   query personByPath($path: String!) {
       personByPath(_path: $path) {
           item {
           fullName
           occupation
           }
       }
   }
   ```

1. Voer het volgende in voor de **Query-variabelen**:

   ```json
   {"path": "/content/dam/my-project/en/alison-smith"}
   ```

1. Voer de vraag uit en neem waar dat het enige resultaat is teruggekeerd.

## Zoekopdrachten behouden {#persist-queries}

Zodra een ontwikkelaar met de vraag en resultaatgegevens tevreden is die van de vraag zijn teruggekeerd, moet de volgende stap de vraag opslaan of voortzetten aan AEM. De [Blijvende query&#39;s](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html) zijn het voorkeursmechanisme voor het toegankelijk maken van de GraphQL API voor clienttoepassingen. Zodra een vraag is voortgeduurd, kan het worden gevraagd gebruikend een verzoek van de GET en in het voorgeheugen ondergebracht bij de lagen van de Verzender en CDN. De prestaties van de gepresteerde vragen zijn veel beter. Naast prestatievoordelen zorgen permanente query&#39;s ervoor dat extra gegevens niet per ongeluk aan clienttoepassingen worden blootgesteld. Meer informatie over [Hier kunt u permanente query&#39;s vinden](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html).

Daarna, persisteert twee eenvoudige vragen, worden zij gebruikt in het volgende hoofdstuk.

1. Ga de volgende vraag in GrahiQL winde in:

   ```graphql
   query allTeams {
       teamList {
           items {
               _path
               title
               shortName
               description {
                   plaintext
               }
               teamMembers {
                   fullName
                   occupation
               }
           }
       }
   }
   ```

   Controleer of de query werkt.

1. Volgende tikken **Opslaan als** en betreden `all-teams` als de **Naam query**.

   De query moet worden weergegeven onder **Blijvende query&#39;s** in het linkerspoor.

   ![Alle teams blijven query uitvoeren](assets/explore-graphql-api/all-teams-persisted-query.png)
1. Tik vervolgens op de ovalen **...** naast de permanente query en tik op **URL kopiëren** om het pad naar het klembord te kopiëren.

   ![URL van permanente query kopiëren](assets/explore-graphql-api/copy-persistent-query-url.png)

1. Open een nieuw tabblad en plak het gekopieerde pad in uw browser:

   ```plain
   https://$YOUR-AEMasCS-INSTANCEID$.adobeaemcloud.com/graphql/execute.json/my-project/all-teams
   ```

   Het moet er ongeveer zo uitzien als het bovenstaande pad. U moet zien dat de JSON-resultaten van de geretourneerde query.

   De bovenstaande URL opsplitsen:

   | Naam | Beschrijving |
   | ---------|---------- |
   | `/graphql/execute.json` | Blijvend zoekeindpunt |
   | `/my-project` | Projectconfiguratie voor `/conf/my-project` |
   | `/all-teams` | Naam van de voortgezette query |

1. Terugkeer naar GrahiQL winde en gebruik de plus knoop **+** om de NIEUWE query voort te zetten

   ```graphql
   query personByName($name: String!) {
     personList(
       filter: {
         fullName:{
           _expressions: [{
             value: $name
             _operator:EQUALS
           }]
         }
       }){
       items {
         _path
         fullName
         occupation
         biographyText {
           json
         }
         profilePicture {
           ... on ImageRef {
             _path
             _authorUrl
             _publishUrl
             width
             height
           }
         }
       }
     }
   }
   ```

1. Sla de query op als: `person-by-name`.
1. Er moeten twee doorlopende query&#39;s worden opgeslagen:

   ![Laatste voortgeduurde vragen](assets/explore-graphql-api/final-persisted-queries.png)


## GraphQL-eindpunt en doorlopende query&#39;s publiceren

Na revisie en verificatie publiceert u de `GraphQL Endpoint` &amp; `Persisted Queries`

1. Navigeer in het scherm AEM starten naar **Gereedschappen** > **Algemeen** > **GraphQL**.

1. Tik op het selectievakje naast **Mijn eindpunt van project** en tikken **Publiceren**

   ![GraphQL-eindpunt publiceren](assets/explore-graphql-api/publish-graphql-endpoint.png)

1. Navigeer in het scherm AEM starten naar **Gereedschappen** > **Algemeen** > **GraphQL Query Editor**

1. Tik op de knop **alle teams** query uit deelvenster Blijvende query&#39;s en tik **Publiceren**

   ![Blijvende query&#39;s publiceren](assets/explore-graphql-api/publish-persisted-query.png)

1. Herhaal bovenstaande stap voor `person-by-name` query

## Oplossingsbestanden {#solution-files}

Download de inhoud, de modellen, en de voortgezette vragen die in de laatste drie hoofdstukken worden gecreeerd: [basic-tutorial-solution.content.zip](assets/explore-graphql-api/basic-tutorial-solution.content.zip)

## Aanvullende bronnen

Meer informatie over GraphQL-query&#39;s vindt u op [GraphQL leren gebruiken met AEM - Voorbeeldinhoud en query&#39;s](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/sample-queries.html).

## Gefeliciteerd! {#congratulations}

U hebt verschillende GraphQL-query&#39;s gemaakt en uitgevoerd.

## Volgende stappen {#next-steps}

In het volgende hoofdstuk: [React-app ontwikkelen](./graphql-and-react-app.md), onderzoekt u hoe een externe toepassing AEM eindpunten van GraphQL kan vragen en deze twee persisted query&#39;s kan gebruiken. Er is ook een aantal standaardfoutafhandeling geïntroduceerd tijdens het uitvoeren van GraphQL-query&#39;s.

## GraphiQL installeren (optioneel) {#install-graphiql}

In sommige versies van AEM (6.X.X) moet het GraphiQL IDE-gereedschap handmatig worden geïnstalleerd, kunt u het [instructies van hier](../how-to/install-graphiql-aem-6-5.md).

