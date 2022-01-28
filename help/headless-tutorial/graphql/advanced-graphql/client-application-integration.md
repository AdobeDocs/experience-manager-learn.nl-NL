---
title: De Integratie van de Toepassing van de cliënt - Geavanceerde Concepten van AEM Zwaartepunt - GraphQL
description: Voer voortgezette vragen uit en integreer het in app WKND.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
source-git-commit: 83e16ea87847182139982ea2378d8ff9f079c968
workflow-type: tm+mt
source-wordcount: '1228'
ht-degree: 0%

---


# Integratie van clienttoepassingen

In het vorige hoofdstuk, creeerde en bijwerkte persisted vragen gebruikend de verzoeken van de PUT en van de POST van HTTP.

In dit hoofdstuk worden de stappen doorlopen waarmee u die aanslepende query&#39;s kunt integreren met de WKND-app met HTTP-GET-aanvragen binnen vijf React-componenten:

* Locatie
* Adres
* Instructeurs
* Beheerder
* Team

## Vereisten {#prerequisites}

Dit document is onderdeel van een zelfstudie met meerdere onderdelen. Controleer of de vorige hoofdstukken zijn voltooid voordat u verdergaat met dit hoofdstuk. Voltooiing van de [basiszelfstudie](/help/headless-tutorial/graphql/multi-step/overview.md) aanbevolen.

_IDE screenshots in dit hoofdstuk komen van [Visual Studio-code](https://code.visualstudio.com/)_

### Hoofdstuk 1-4 Oplossingspakket (optioneel) {#solution-package}

Er is een oplossingspakket beschikbaar om te worden geïnstalleerd dat de stappen in AEM UI voor hoofdstukken 1-4 voltooit. Dit pakket is **niet nodig** indien de vorige hoofdstukken zijn afgesloten.

1. Downloaden [Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip).
1. Navigeer in AEM naar **Gereedschappen** > **Implementatie** > **Pakketten** toegang **Pakketbeheer**.
1. Upload en installeer het pakket (ZIP-bestand) dat u in de vorige stap hebt gedownload.

## Doelstellingen {#objectives}

In deze zelfstudie leert u hoe u de aanvragen voor voortgezette query&#39;s kunt integreren in de voorbeeldtoepassing WKND GraphQl React met behulp van JavaScript zonder AEM kop [SDK](https://github.com/adobe/aem-headless-client-js).

## De voorbeeldclienttoepassing installeren en uitvoeren {#install-client-app}

Om de zelfstudie te versnellen, wordt een startapp van React JS geleverd.

>[!NOTE]
> 
> Hieronder ziet u hoe u de React-app koppelt aan een **Auteur** omgeving in AEM as a Cloud Service met behulp van een [toegangstoken voor lokale ontwikkeling](/help/headless-tutorial/authentication/local-development-access-token.md). Het is ook mogelijk om de app te verbinden met een [lokale instantie van Auteur die AEMaaCS SDK gebruikt](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) basisverificatie gebruiken.

1. Downloaden **[aem-guides-wknd-headless-start-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-start-tutorial.zip)**.
1. Pak het bestand uit en open het project in uw IDE.
1. Vraag een [token voor lokale ontwikkeling](/help/headless-tutorial/authentication/local-development-access-token.md) voor uw AEM.
1. Open het bestand in het project `.env.development`.
   1. Set `REACT_APP_DEV_TOKEN` gelijk aan `accessToken` waarde uit het token voor lokale ontwikkeling. (Niet het gehele JSON-bestand)
   1. Set `REACT_APP_HOST_URI` naar de URL van uw AEM **Auteur** milieu.

   ![Lokaal omgevingsbestand bijwerken](assets/client-application-integration/update-environment-file.png)
1. Open een nieuwe terminal en navigeer in de projectomslag. Voer de volgende opdrachten uit:

   ```shell
   $ npm install
   $ npm start
   ```

1. Een nieuwe browser moet worden geopend op `http://localhost:3000/aem-guides-wknd-pwa`.
1. Tikken **Kamperen** > **Yosemite-achtergrondverpakking** om de Yosemite Backpackaging adventure details te bekijken.

   ![Yosemite-achtergrondscherm](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. Open de ontwikkelaarsgereedschappen van de browser en controleer de `XHR` verzoek

   ![POST GraphQL](assets/client-application-integration/post-query-graphql.png)

   U dient een `POST` aan het eindpunt GraphQL. De `Payload`, kunt u de volledige vraag zien GraphQL die werd verzonden. In de volgende secties wordt de app bijgewerkt voor gebruik **blijvend** query&#39;s.


## Aan de slag

In het basisleerprogramma, wordt een parameterized vraag GraphQl gebruikt om één enkel Fragment van de Inhoud te verzoeken en de avontuurdetails terug te geven. Werk vervolgens de `adventureDetailQuery` om nieuwe gebieden en gebruik voortgeduurde vragen te omvatten die in het vorige hoofdstuk worden gecreeerd.

Er worden vijf componenten gemaakt:

| Component Reageren | Locatie |
|-------|------|
| Beheerder | `src/components/Administrator.js` |
| Team | `src/components/Team.js` |
| Locatie | `src/components/Location.js` |
| Instructeurs | `src/components/Instructors.js` |
| Adres | `src/components/Address.js` |

## UseGraphQL-haak bijwerken

Een aangepaste [React-haak](https://reactjs.org/docs/hooks-overview.html#effect-hook) is gemaakt die luistert naar wijzigingen in de app `query`en bij wijziging een HTTP-POST aanvragen bij het eindpunt AEM GraphQL en de JSON-reactie op de app retourneren.

Een nieuwe haak maken voor gebruik **blijvend** query&#39;s. De app kan vervolgens HTTP-GET-aanvragen indienen voor Adventure-details. De `runPersistedQuery` van de [AEM Headless Client SDK](https://github.com/adobe/aem-headless-client-js) wordt gebruikt om het uitvoeren van een voortgezette vraag gemakkelijker te maken.

1. Het bestand openen `src/api/useGraphQL.js`
1. Een nieuwe haak toevoegen voor `useGraphQLPersisted`:

   ```javascript
   /**
   * Custom React Hook to perform a GraphQL query to a persisted query endpoint
   * @param persistedPath - the short path to the persisted query
   * @param fragmentPathParam - optional parameters object that can be passed in for parameterized persistent queries
   */
   export function useGraphQLPersisted(persistedPath, fragmentPathVariable) {
       let [data, setData] = useState(null);
       let [errors, setErrors] = useState(null);
   
       useEffect(() => {
           let queryVariables = {};
   
           // we pass in a primitive fragmentPathVariable (String) and then construct the object {fragmentPath: fragmentPathParam} to pass as query params to the persisted query
           // It is simpler to pass a primitive into a React hooks, as comparing the state of a dependent object can be difficult. see https://reactjs.org/docs/hooks-faq.html#can-i-skip-an-effect-on-updates
           if(fragmentPathVariable) {
               queryVariables = {fragmentPath: fragmentPathVariable};
           }
   
           // execute a persisted query using the given path and pass in variables (if needed)
           sdk.runPersistedQuery(persistedPath, queryVariables)
               .then(({ data, errors }) => {
               if (errors) setErrors(mapErrors(errors));
               if (data) setData(data);
           })
           .catch((error) => {
           setErrors(error);
           });
   }, [persistedPath, fragmentPathVariable]);
   
   return { data, errors }
   }
   ```
1. Wijzigingen in het bestand opslaan.

## Adventure-details-component bijwerken

Het bestand `src/api/queries.js` Bevat de vragen GraphQL die worden gebruikt om de toepassing aan te drijven `adventureDetailQuery` Retourneert details voor een individueel avontuur met behulp van het standaard GraphQL-verzoek van de POST. Werk vervolgens de `AdventureDetail` om de blijvend `wknd/all-adventure-details` query.

1. Open `src/screens/AdventureDetail.js`.
1. Wijs eerst de volgende regel toe:

   ```javascript
   export default function AdventureDetail() {
   
       ...
   
       //const { data, errors } = useGraphQL(adventureDetailQuery(adventureFragmentPath));
   ```

   Het bovenstaande gebruik de standaard POST GraphQL om avontuurdetails terug te winnen die op a worden gebaseerd `adventureFragmentPath`

1. Als u de opdracht `useGraphQLPersisted` haak, voeg de volgende lijn toe:

   ```javascript
   export default function AdventureDetail() {
   
      //const { data, errors } = useGraphQL(adventureDetailQuery(adventureFragmentPath));
       const {data, errors} = useGraphQLPersisted("wknd/all-adventure-details", adventureFragmentPath);
   ```

   Het pad waarnemen `wknd/all-adventure-details` is de weg aan de voortgezette vraag die in het vorige hoofdstuk wordt gecreeerd.

   >[!CAUTION]
   >
   > Voor de bijgewerkte query kunt u de opdracht `wknd/all-adventure-details` moet worden voortgezet in de AEM. Bekijk de stappen in [Blijvende query&#39;s voor GraphQL](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md#cache-control-all-adventures) of installeer de [AEM oplossingspakket](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip)

1. Ga terug naar de app die in de browser wordt uitgevoerd en gebruik de ontwikkelaarsgereedschappen van uw browser om de aanvraag te inspecteren nadat u naar een **Adventure-details** pagina.

   ![Verzoek ophalen](assets/client-application-integration/get-request-persisted-query.png)

   ```
   http://localhost:3000/graphql/execute.json/wknd/all-adventure-details;fragmentPath=/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking
   ```

   U moet nu een `GET` verzoek dat de voortgezette vraag bij gebruikt `wknd/all-adventure-details`.

1. Navigeer naar andere avontuurdetails en merk op dat het zelfde `GET` aanvraag wordt gedaan maar met verschillende fragmentpaden. De toepassing blijft werken zoals voorheen.

Zie `AdventureDetail.js` in de [aem-guides-wknd-headless-oplossing-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) voor een volledig voorbeeld van de bijgewerkte component.

Maak vervolgens de **Locatie**, **Beheerder**, en **Instructeurs** componenten om locatiegegevens te renderen. De **Adres** naar component wordt verwezen binnen de **Team** component.

## De component Locatie ontwikkelen

1. In de `AdventureDetail.js` bestand, een verwijzing naar de `<Location>` component die de locatiegegevens doorgeeft van de `adventure` gegevensobject:

   ```javascript
   export default function AdventureDetail() {
       ...
   
       return (
           ...
   
           <Location data={adventure.location} />
   ```

1. Bestand bekijken op `src/components/Location.js`. De `Location` de component geeft de gegevens voor waar te ontmoeten, contactinfo, informatie over het weer, en een plaatsbeeld van terug **Locatie** Inhoudsfragmentmodel. De `Location` component verwacht een `address` door te geven object.
1. Zie `Location.js` in de [aem-guides-wknd-headless-oplossing-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) voor een volledig voorbeeld van de bijgewerkte component.

Nadat de updates zijn uitgevoerd, ziet de weergegeven detailpagina er als volgt uit:

![location-component](assets/client-application-integration/location-component.png)

## Ontwikkel de component van het Team

1. In de `AdventureDetail.js` bestand, een verwijzing naar de `<Team>` component (onder de component `<Location>` component) het doorgeven van `instructorTeam` gegevens van de `adventure` gegevensobject:

   ```javascript
   <Location data={adventure.location} />
   <Team data={adventure.instructorTeam} />
   ```

1. Bestand bekijken op `src/components/Team.js`. De `Team` geeft gegevens over de oprichtingsdatum, de afbeelding en de beschrijving van het team weer van de **Team** Inhoudsfragment.

1. In `Team.js` nota nemen van de opneming van de `Address` component.

   ```javascript
   export default function Team({data}) {
       ...
       {teamPath && <Address _path={teamPath}/>}
   ```

   Hier wordt een pad naar het huidige team doorgegeven aan het `Address` component, die beurtelings een vraag uitvoert om het adres te krijgen dat op het team wordt gebaseerd.

1. Zie `Team.js` in de [aem-guides-wknd-headless-oplossing-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) voor een volledig voorbeeld van de component.

Zodra de vraag wordt geïntegreerd, zou het als dit moeten kijken:

![teamcomponent](assets/client-application-integration/address-component.png)

## Ontwikkel de component van het Adres

1. Bestand bekijken op `src/components/Address.js`. De `Address` de component geeft adresinformatie zoals straatadres, plaats, staat, postcode, land van **Adres** Inhoudsfragment en telefoon en e-mail van de **Contactinfo** fragmentverwijzing.
1. De `Address` de component is vergelijkbaar met `AdventureDetails` in die zin dat het een voortgezette vraag maakt om gegevens terug te winnen die op een weg worden gebaseerd. Het verschil is dat het gebruikt `/wknd/team-location-by-location-path` om het verzoek in te dienen.
1. Zie `Address.js` in de [aem-guides-wknd-headless-oplossing-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) voor een volledig voorbeeld van de component.

## De component Beheerder ontwikkelen

1. In de `AdventureDetail.js` bestand, een verwijzing naar de `<Adminstrator>` component (onder de component `<Team>` component) het doorgeven van `administrator` gegevens van de `adventure` gegevensobject:

   ```javascript
   <Location data={adventure.location} />
   <Team data={adventure.instructorTeam} />
   <Administrator data={adventure.administrator} /> 
   ```

1. Bestand bekijken op `src/components/Administrator.js`. De `Administrator` de component geeft details terug zoals hun volledige naam van **Beheerder** Inhoudsfragment en telefoon en e-mail renderen vanuit de **Contactinfo** fragmentverwijzing.
1. Zie `Administrator.js` in [aem-guides-wknd-headless-oplossing-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) voor een volledig voorbeeld van de component.

Nadat u de beheerderscomponent hebt gemaakt, kunt u de toepassing renderen. De uitvoer moet overeenkomen met de onderstaande afbeelding:

![administrator-component](assets/client-application-integration/administrator-component.png)

## De component Instructors ontwikkelen

1. In de `AdventureDetail.js` bestand, een verwijzing naar de `<Instructors>` component (onder de component `<Administrator>` component) het doorgeven van `instructorTeam` gegevens van de `adventure` gegevensobject:

   ```javascript
   <Location data={adventure.location} />
   <Team data={adventure.instructorTeam}/>
   <Administrator data={adventure.administrator} />             
   <Instructors data={adventure.instructorTeam} />
   ```

1. Bestand bekijken op `src/components/Instructors.js`. De `Instructors` geeft gegevens over elk van de teamleden, met inbegrip van volledige naam, biografie, beeld, telefoonaantal, ervaringsniveau, en vaardigheden terug. De component herhaalt zich over een serie om elk lid te tonen.
1. Zie `Instructors.js` in [aem-guides-wknd-headless-oplossing-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip) voor een volledig voorbeeld van de component.

Nadat u de toepassing hebt gerenderd, moet de uitvoer overeenkomen met de onderstaande afbeelding:

![instructeur-component](assets/client-application-integration/instructors-component.png)

## Voorbeeld van WKND-app voltooid

De voltooide app moet er als volgt uitzien:

![AEM-headless-definitieve ervaring](assets/client-application-integration/aem-headless-final-experience.gif)

### Uiteindelijke clienttoepassing

De definitieve versie van de app kan worden gedownload en gebruikt:
**[aem-guides-wknd-headless-oplossing-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip)**

## Gefeliciteerd

Gefeliciteerd! U hebt de voortgezette query&#39;s nu geïntegreerd en geïmplementeerd in de WKND-voorbeeldapp.