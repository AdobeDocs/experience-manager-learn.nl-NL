---
title: Maak een React-app die query's uitvoert AEM GraphQL API gebruiken - Aan de slag met AEM headless - GraphQL
description: Ga aan de slag met Adobe Experience Manager (AEM) en GraphQL. Maak een React-app waarmee inhoud/gegevens worden opgehaald van AEM GraphQL API. Zie ook hoe AEM Headless JS SDK wordt gebruikt.
version: Cloud Service
mini-toc-levels: 1
jira: KT-6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
duration: 538
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1181'
ht-degree: 0%

---


# Een React-app ontwikkelen die AEM GraphQL API&#39;s gebruikt

In dit hoofdstuk verkent u hoe AEM GraphQL API&#39;s de ervaring in een externe toepassing kunnen aansturen.

Een eenvoudige React-app wordt gebruikt voor query&#39;s en weergave **Team** en **Persoon** inhoud die wordt weergegeven door AEM GraphQL API&#39;s. Het gebruik van React is grotendeels onbelangrijk, en de verbruikende externe toepassing zou in om het even welk kader voor om het even welk platform kunnen worden geschreven.

## Vereisten

Aangenomen wordt dat de stappen die in de vorige onderdelen van deze meerdelige zelfstudie zijn beschreven, zijn voltooid, of [basic-tutorial-solution.content.zip](assets/explore-graphql-api/basic-tutorial-solution.content.zip) is geïnstalleerd op uw AEM as a Cloud Service auteur- en publicatieservices.

_IDE screenshots in dit hoofdstuk komen van [Visual Studio-code](https://code.visualstudio.com/)_

De volgende software moet worden geïnstalleerd:

- [Node.js v18](https://nodejs.org/en)
- [Visual Studio-code](https://code.visualstudio.com/)

## Doelstellingen

Leer hoe u:

- De voorbeeldtoepassing React downloaden en starten
- De eindpunten van de vraag AEM GraphQL gebruikend [AEM Headless JS SDK](https://github.com/adobe/aem-headless-client-js)
- De AEM van de vraag voor een lijst van teams, en hun referenced leden
- De AEM van de vraag voor de details van een teamlid

## De voorbeeldtoepassing React ophalen

In dit hoofdstuk wordt een uitgelijnde voorbeeldtoepassing React geïmplementeerd met de code die vereist is voor interactie met AEM GraphQL API en de gegevens van het weergaveteam en de persoon die van hen zijn verkregen.

De voorbeeldbroncode van de React-app is beschikbaar op Github.com op <https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial>

Zo krijgt u de React-app:

1. De voorbeeldtoepassing WKND GraphQL React klonen vanuit [Github.com](https://github.com/adobe/aem-guides-wknd-graphql).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Navigeren naar `basic-tutorial` en open het in uw winde.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![App Reageren in VSCode](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. Bijwerken `.env.development` om verbinding te maken met AEM as a Cloud Service publicatieservice.

   - Set `REACT_APP_HOST_URI`De waarde die de publicatie-URL van uw AEM as a Cloud Service moet zijn (bijvoorbeeld `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`) en `REACT_APP_AUTH_METHOD`s waarde aan `none`

   >[!NOTE]
   >
   > Zorg ervoor u projectconfiguratie, de modellen van het Fragment van de Inhoud, authored de Fragmenten van de Inhoud, GraphQL eindpunten en voortgeduurde vragen van vorige stappen hebt gepubliceerd.
   >
   > Als u bovenstaande stappen hebt uitgevoerd op de lokale AEM Auteur-SDK, kunt u naar `http://localhost:4502` en `REACT_APP_AUTH_METHOD`s waarde aan `basic`.


1. Ga vanaf de opdrachtregel naar de `aem-guides-wknd-graphql/basic-tutorial` map

1. De React-app starten

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. De React-app start in de ontwikkelmodus op [http://localhost:3000/](http://localhost:3000/). Wijzigingen die tijdens de zelfstudie in de React-app worden aangebracht, worden direct doorgevoerd.

![Gedeeltelijk geïmplementeerde React App](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   Deze React-app is gedeeltelijk geïmplementeerd. Volg de stappen in deze zelfstudie om de implementatie te voltooien. De JavaScript-bestanden die implementatiewerk nodig hebben, hebben de volgende opmerking. Controleer of u de code in die bestanden toevoegt/bijwerkt met de code die in deze zelfstudie is opgegeven.
>
>
> //************************************
>
>  // TODO Implementeer dit door de stappen van AEM zelfstudie voor headless uit te voeren.
>
>  //************************************
>

## Anatomie van de React-app

De voorbeeldtoepassing React bestaat uit drie hoofdonderdelen:

1. De `src/api` Deze map bevat bestanden die worden gebruikt om GraphQL-query&#39;s naar AEM te maken.
   - `src/api/aemHeadlessClient.js` initialiseert en exporteert de AEM Headless Client die wordt gebruikt om te communiceren met AEM
   - `src/api/usePersistedQueries.js` implements [aangepaste React-haken](https://react.dev/learn/reusing-logic-with-custom-hooks#custom-hooks-sharing-logic-between-components) gegevens van AEM GraphQL aan de `Teams.js` en `Person.js` weergavecomponenten.

1. De `src/components/Teams.js` het dossier toont een lijst van teams en hun leden, door een lijstvraag te gebruiken.
1. De `src/components/Person.js` het dossier toont de details van één enkele persoon, gebruikend een parameter, enig-resultaatvraag.

## Het object AEMHeadless controleren

Controleer de `aemHeadlessClient.js` bestand voor het maken van de `AEMHeadless` -object dat wordt gebruikt om te communiceren met AEM.

1. Open `src/api/aemHeadlessClient.js`.

1. Herzie de lijnen 1-40:

   - Het importeren `AEMHeadless` verklaring van de [AEM headless-client voor JavaScript](https://github.com/adobe/aem-headless-client-js), lijn 11.

   - De samenstelling van de vergunning op basis van de in `.env.development`, regel 14-22 en de expressie van de pijlfunctie `setAuthorization`, lijn 31-40.

   - De `serviceUrl` instellen voor de opgenomen [ontwikkelingsproxy](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app#proxy-api-requests) configuratie, lijn 27.

1. Lijnen 42-49 zijn het belangrijkst, aangezien zij concretiseren `AEMHeadless` en exporteer deze voor gebruik in de React-app.

```javascript
// Initialize the AEM Headless Client and export it for other files to use
const aemHeadlessClient = new AEMHeadless({
  serviceURL: serviceURL,
  endpoint: REACT_APP_GRAPHQL_ENDPOINT,
  auth: setAuthorization(),
});

export default aemHeadlessClient;
```

## Implementeren om AEM GraphQL doorlopende query&#39;s uit te voeren

Om generieke uit te voeren `fetchPersistedQuery(..)` De functie om de AEM GraphQL voortgezette query&#39;s uit te voeren opent de `usePersistedQueries.js` bestand. De `fetchPersistedQuery(..)` functie gebruikt de `aemHeadlessClient` object `runPersistedQuery()` functie voor het asynchroon uitvoeren van query, op belofte gebaseerd gedrag.

Later, aangepast Reageren `useEffect` De haken roept deze functie om specifieke gegevens van AEM terug te winnen.

1. In `src/api/usePersistedQueries.js` **update** `fetchPersistedQuery(..)`, regel 35, met de onderstaande code.

```javascript
/**
 * Private, shared function that invokes the AEM Headless client.
 *
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  // Return the GraphQL and any errors
  return { data, err };
}
```

## Teams implementeren

Vervolgens ontwikkelt u de functionaliteit om de Teams en hun leden weer te geven in de hoofdweergave van de React-app. Deze functionaliteit vereist:

- Een nieuwe [aangepaste React useEffect-haak](https://react.dev/reference/react/useEffect#useeffect) in `src/api/usePersistedQueries.js` dat de `my-project/all-teams` voortgezette vraag, terugkerend een lijst van de Fragmenten van de Inhoud van het Team in AEM.
- Een component Reageren op `src/components/Teams.js` die de nieuwe aangepaste Reactie aanroept `useEffect` haak, en geeft de teamgegevens terug.

Als de hoofdweergave van de app is voltooid, worden de teamgegevens uit AEM ingevuld.

![Teamweergave](./assets/graphql-and-external-app/react-app__teams-view.png)

### Stappen

1. Open `src/api/usePersistedQueries.js`.

1. De functie zoeken `useAllTeams()`

1. Een `useEffect` haak die de voortgezette vraag aanhaalt `my-project/all-teams` via `fetchPersistedQuery(..)`voegt u de volgende code toe. De haak retourneert ook alleen de relevante gegevens van de reactie van AEM GraphQL op `data?.teamList?.items`, zodat de React-weergavecomponenten niet op de hoogte zijn van de bovenliggende JSON-structuren.

   ```javascript
   /**
    * Custom hook that calls the 'my-project/all-teams' persisted query.
    *
    * @returns an array of Team JSON objects, and array of errors
    */
   export function useAllTeams() {
     const [teams, setTeams] = useState(null);
     const [error, setError] = useState(null);
   
     // Use React useEffect to manage state changes
     useEffect(() => {
       async function fetchData() {
         // Call the AEM GraphQL persisted query named "my-project/all-teams"
         const { data, err } = await fetchPersistedQuery(
           "my-project/all-teams"
         );
         // Sets the teams variable to the list of team JSON objects
         setTeams(data?.teamList?.items);
         // Set any errors
         setError(err);
       }
       // Call the internal fetchData() as per React best practices
       fetchData();
     }, []);
   
     // Returns the teams and errors
     return { teams, error };
   }
   ```

1. Openen `src/components/Teams.js`

1. In de `Teams` Reageer component, haal de lijst van teams van AEM gebruikend `useAllTeams()` haak.

   ```javascript
   import { useAllTeams } from "../api/usePersistedQueries";
   ...
   function Teams() {
     // Get the Teams data from AEM using the useAllTeams
     const { teams, error } = useAllTeams();
     ...
   }
   ```



1. Voer de op weergave gebaseerde gegevensvalidatie uit en geef een foutbericht of ladingsindicator weer op basis van de geretourneerde gegevens.

   ```javascript
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       // If an error ocurred while executing the GraphQL query, display an error message
       return <Error errorMessage={error} />;
     } else if (!teams) {
       // While the GraphQL request is executing, show the Loading indicator
       return <Loading />;
     }
     ...
   }
   ```

1. Tot slot geef de teamgegevens terug. Elk team dat wordt geretourneerd uit de GraphQL-query, wordt weergegeven met de opgegeven `Team` Reageer subcomponent.

   ```javascript
   import React from "react";
   import { Link } from "react-router-dom";
   import { useAllTeams } from "../api/usePersistedQueries";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Teams.scss";
   
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!teams) {
       return <Loading />;
     }
   
     // Teams have been populated by AEM GraphQL query. Display the teams.
     return (
       <div className="teams">
         {teams.map((team, index) => {
           return <Team key={index} {...team} />;
         })}
       </div>
     );
   }
   
   // Render single Team
   function Team({ title, shortName, description, teamMembers }) {
     // Must have title, shortName and at least 1 team member
     if (!title || !shortName || !teamMembers) {
       return null;
     }
   
     return (
       <div className="team">
         <h2 className="team__title">{title}</h2>
         <p className="team__description">{description.plaintext}</p>
         <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
             {/* Render the referenced Person models associated with the team */}
             {teamMembers.map((teamMember, index) => {
               return (
                 <li key={index} className="team__member">
                   <Link to={`/person/${teamMember.fullName}`}>
                     {teamMember.fullName}
                   </Link>
                 </li>
               );
             })}
           </ul>
         </div>
       </div>
     );
   }
   
   export default Teams;
   ```


## Functie Personen implementeren

Met de [Functie Teams](#implement-teams-functionality) volledig, voeren de functionaliteit uit om de vertoning op de details van een teamlid, of van een persoon te behandelen.

Deze functionaliteit vereist:

- Een nieuwe [aangepaste React useEffect-haak](https://react.dev/reference/react/useEffect#useeffect) in `src/api/usePersistedQueries.js` dat de parameters activeert `my-project/person-by-name` persisted query en retourneert één persoonrecord.

- Een component Reageren op `src/components/Person.js` dat de volledige naam van een persoon als vraagparameter gebruikt, haalt nieuwe douaneReact aan `useEffect` en geeft de gegevens van de persoon weer.

Als u de naam van een persoon hebt geselecteerd in de weergave Teams, wordt de weergave van die persoon weergegeven.

![Persoon](./assets/graphql-and-external-app/react-app__person-view.png)

1. Open `src/api/usePersistedQueries.js`.

1. De functie zoeken `usePersonByName(fullName)`

1. Een `useEffect` haak die de voortgezette vraag aanhaalt `my-project/all-teams` via `fetchPersistedQuery(..)`voegt u de volgende code toe. De haak retourneert ook alleen de relevante gegevens van de reactie van AEM GraphQL op `data?.teamList?.items`, zodat de React-weergavecomponenten niet op de hoogte zijn van de bovenliggende JSON-structuren.

   ```javascript
   /**
    * Calls the 'my-project/person-by-name' and provided the {fullName} as the persisted query's `name` parameter.
    *
    * @param {String!} fullName the full
    * @returns a JSON object representing the person
    */
   export function usePersonByName(fullName) {
     const [person, setPerson] = useState(null);
     const [errors, setErrors] = useState(null);
   
     useEffect(() => {
       async function fetchData() {
         // The key is the variable name as defined in the persisted query, and may not match the model's field name
         const queryParameters = { name: fullName };
   
         // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
         const { data, err } = await fetchPersistedQuery(
           "my-project/person-by-name",
           queryParameters
         );
   
         if (err) {
           // Capture errors from the HTTP request
           setErrors(err);
         } else if (data?.personList?.items?.length === 1) {
           // Set the person data after data validation
           setPerson(data.personList.items[0]);
         } else {
           // Set an error if no person could be found
           setErrors(`Cannot find person with name: ${fullName}`);
         }
       }
       fetchData();
     }, [fullName]);
   
     return { person, errors };
   }
   ```

1. Openen `src/components/Person.js`
1. In de `Person` Reageren, component, parseren `fullName` routeparameter, en haal de persoongegevens van AEM gebruikend `usePersonByName(fullName)` haak.

   ```javascript
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   ...
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
     ...
   }
   ```

1. Voer op weergave gebaseerde gegevensvalidatie uit en geef een foutbericht of ladingsindicator weer op basis van de geretourneerde gegevens.

   ```javascript
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
     ...
   }
   ```

1. Tot slot geef de persoongegevens terug.

   ```javascript
   import React from "react";
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   import { mapJsonRichText } from "../utils/renderRichText";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Person.scss";
   
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
   
     // Render the person data
     return (
       <div className="person">
         <img
           className="person__image"
           src={process.env.REACT_APP_HOST_URI+person.profilePicture._path}
           alt={person.fullName}
         />
         <div className="person__occupations">
           {person.occupation.map((occupation, index) => {
             return (
               <span key={index} className="person__occupation">
                 {occupation}
               </span>
             );
           })}
         </div>
         <div className="person__content">
           <h1 className="person__full-name">{person.fullName}</h1>
           <div className="person__biography">
             {/* Use this utility to transform multi-line text JSON into HTML */}
             {mapJsonRichText(person.biographyText.json)}
           </div>
         </div>
       </div>
     );
   }
   
   export default Person;
   ```

## Probeer de app

De app controleren [http://localhost:3000/](http://localhost:3000/) en klik op _Leden_ koppelingen. Ook kunt u meer teams en/of leden aan de Alpha van het Team toevoegen door de Fragmenten van de Inhoud in AEM toe te voegen.

>[!IMPORTANT]
>
>Als u de wijzigingen in de implementatie wilt controleren of als u de toepassing niet kunt laten werken nadat de bovenstaande wijzigingen zijn aangebracht, raadpleegt u de [basiszelfstudie](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial) oplossingsvertakking.

## Onder de hood

De browser openen **Gereedschappen voor ontwikkelaars** > **Netwerk** en _Filter_ for `all-teams` verzoek. Let op de GraphQL API-aanvraag `/graphql/execute.json/my-project/all-teams` is gemaakt tegen `http://localhost:3000` en **NOT** tegen de waarde van `REACT_APP_HOST_URI`bijvoorbeeld `<https://publish-pxxx-exxx.adobeaemcloud.com`. De aanvragen worden ingediend op het domein van de React-app omdat [proxyinstelling](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) is ingeschakeld met `http-proxy-middleware` -module.


![GraphQL API-verzoek via Proxy](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


De belangrijkste `../setupProxy.js` bestand en binnen `../proxy/setupProxy.auth.**.js` bestanden zien hoe `/content` en `/graphql` paden zijn proxy en geven aan dat het geen statisch element is.

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

Het gebruik van de lokale proxy is geen geschikte optie voor productieimplementatie en meer details kunt u vinden op _Implementatie van productie_ sectie.

## Gefeliciteerd!{#congratulations}

Gefeliciteerd! U hebt de React-app gemaakt om gegevens van AEM GraphQL API&#39;s te verbruiken en weer te geven als onderdeel van de basiszelfstudie.
