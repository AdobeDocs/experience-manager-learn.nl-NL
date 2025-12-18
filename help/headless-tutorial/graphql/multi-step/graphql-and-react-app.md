---
title: Een React-app maken waarmee AEM wordt gevraagd met GraphQL API - Aan de slag met AEM Headless - GraphQL
description: Ga aan de slag met Adobe Experience Manager (AEM) en GraphQL. Maak een React-app waarmee inhoud/gegevens worden opgehaald van de AEM GraphQL API en bekijk ook hoe AEM Headless JS SDK wordt gebruikt.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
duration: 410
source-git-commit: e7f556737cdf6a92c0503d3b4a52eef1f71c8330
workflow-type: tm+mt
source-wordcount: '1479'
ht-degree: 0%

---


# Een React-app maken die AEM GraphQL API&#39;s gebruikt

In dit hoofdstuk leert u hoe AEM GraphQL API&#39;s de ervaring in een externe toepassing kunnen aansturen.

Een eenvoudige React app wordt gebruikt om **Team** en **Persoon** inhoud te vragen en te tonen die door AEM GraphQL APIs wordt blootgesteld. Het gebruik van React is grotendeels onbelangrijk, en de verbruikende externe toepassing zou in om het even welk kader voor om het even welk platform kunnen worden geschreven.

## Vereisten

Men veronderstelt dat de stappen die in de vorige delen van dit meerdelige leerprogramma worden geschetst zijn voltooid, of [&#x200B; basis-tutorial-solution.content.zip &#x200B;](assets/explore-graphql-api/basic-tutorial-solution.content.zip) is geïnstalleerd op uw Auteur en de Publish diensten van AEM.

_winde screenshots in dit hoofdstuk komen uit [&#x200B; Code van Visual Studio &#x200B;](https://code.visualstudio.com/)_

### AEM omgeving

Als u deze zelfstudie wilt voltooien, kunt u het beste AEM Administrator toegang geven tot een van de volgende mogelijkheden:

+ [&#x200B; AEM as a Cloud Service &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=nl-NL)
+ Lokale opstelling die [&#x200B; de Dienst SDK van de Wolk AEM gebruiken &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=nl-NL)
+ [&#x200B; AEM 6.5 LTS &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-65/content/release-notes/release-notes.html?lang=nl-NL) met [&#x200B; geïnstalleerd Pakket 1.0.5+ van de Index van GraphQL &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-65/content/headless/graphql-api/graphql-endpoint.html)

### Softwarevereisten

De volgende software moet worden geïnstalleerd:

+ [&#x200B; Node.js v18+ &#x200B;](https://nodejs.org/en)
+ [&#x200B; Code van Visual Studio &#x200B;](https://code.visualstudio.com/)
+ [&#x200B; Git &#x200B;](https://git-scm.com/)
+ [&#x200B; Java JDK &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-65/content/implementing/deploying/introduction/technical-requirements) (als het verbinden met lokale AEM SDK of 6.5 instantie)

## Doelstellingen

Leer hoe u:

+ De voorbeeldtoepassing React downloaden en starten
+ De eindpunten van GraphQL van de vraag AEM gebruikend [&#x200B; AEM Headless JS SDK &#x200B;](https://github.com/adobe/aem-headless-client-js)
+ Vraag AEM voor een lijst van teams, en hun referenced leden
+ Vraag AEM voor de details van een teamlid

## De voorbeeldtoepassing React ophalen

In dit hoofdstuk wordt een uitgelijnde voorbeeldtoepassing React geïmplementeerd met de code die vereist is voor interactie met de AEM GraphQL API en de gegevens van het weergaveteam en de persoon die van hen zijn verkregen.

De voorbeeldbroncode van de React-app is beschikbaar op Github.com op <https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial>

Zo krijgt u de React-app:

1. Kloon de steekproefWKND GraphQL React app van [&#x200B; Github.com &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql). Onthoud dat deze Git-opslagplaats meerdere projecten bevat. Blader dus naar de map `basic-tutorial` zoals beschreven in de volgende stap.

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Navigeer naar de map `basic-tutorial` en open deze in uw IDE.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![&#x200B; React App in VSCode &#x200B;](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. Werk `.env.development` bij om verbinding te maken met uw AEM-publicatieservice.

   Voor meer details op de het milieuvariabelen van het voorbeeldproject en hoe te om hen te plaatsen, [&#x200B; herzie README.md &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#update-environment-variables).

   **AEM as a Cloud Service**

   Wanneer u de React-app koppelt aan de AEM as a Cloud Service-publicatieservice, stelt u de waarde van `REACT_APP_HOST_URI` in op de publicatie-URL voor AEM as a Cloud Service (bijvoorbeeld `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com` ) en `REACT_APP_AUTH_METHOD` &#39;&#39;s value to `none`

   **AEM SDK voor AEM as a Cloud Service (lokaal)**

   Wanneer u de React-app koppelt aan een lokale AEM as a Cloud Service SDK-omgeving, stelt u de waarde van `REACT_APP_HOST_URI` in op uw lokale host en publicatiepoort (bijvoorbeeld `REACT_APP_HOST_URI=http://localhost:4503` ) en `REACT_APP_AUTH_METHOD` &#39;&#39;s value to `none`

   **AEM 6.5 LTS ontvangen milieu**

   Wanneer u de React-app koppelt aan de AEM as a Cloud Service-publicatieservice, stelt u de waarde van `REACT_APP_HOST_URI` in op uw AEM 6.5-publicatie-URL (bijvoorbeeld `REACT_APP_HOST_URI=https://dev.mysite.com` ) en `REACT_APP_AUTH_METHOD` &#39;&#39;s value to `none`

   **AEM 6.5 LTS quickstart (lokaal)**

   Wanneer u de React-app koppelt aan een lokale AEM 6.5-snelstartverbinding, stelt u de waarde van `REACT_APP_HOST_URI` in op uw lokale host en publicatiepoort (bijvoorbeeld `REACT_APP_HOST_URI=http://localhost:4503` ) en `REACT_APP_AUTH_METHOD` &#39;&#39;s value to `none`

   >[!NOTE]
   >
   > Zorg ervoor u projectconfiguratie, de modellen van het Fragment van de Inhoud, authored de Fragmenten van de Inhoud, GraphQL eindpunten en voortgeduurde vragen van vorige stappen hebt gepubliceerd.
   >
   > Als u bovenstaande stappen hebt uitgevoerd op de lokale AEM Author SDK, kunt u `http://localhost:4502` en `REACT_APP_AUTH_METHOD` &#39;&#39;s value to `basic` &#39; aanwijzen.


1. Ga vanaf de opdrachtregel naar de map `aem-guides-wknd-graphql/basic-tutorial`

1. De React-app starten

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. React app begint op ontwikkelingswijze op [&#x200B; http://localhost :3000/ &#x200B;](http://localhost:3000/). Wijzigingen die tijdens de zelfstudie in de React-app worden aangebracht, worden direct doorgevoerd.

![&#x200B; gedeeltelijk uitgevoerde Reageer App &#x200B;](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   Deze React-app is gedeeltelijk geïmplementeerd. Volg de stappen in deze zelfstudie om de implementatie te voltooien. De JavaScript-bestanden die implementatiewerk nodig hebben, hebben de volgende opmerking. Zorg ervoor dat u de code in die bestanden toevoegt/bijwerkt met de code die in deze zelfstudie is opgegeven.
>
>
> //**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**
>
>  // TODO Implementeer dit door de stappen van de zelfstudie voor AEM Headless te volgen.
>
>  //**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**
>

## Anatomie van de React-app

De voorbeeldtoepassing React bestaat uit drie hoofdonderdelen:

1. De map `src/api` bevat bestanden waarmee GraphQL-query&#39;s naar AEM kunnen worden uitgevoerd.
   + `src/api/aemHeadlessClient.js` initialiseert en exporteert de AEM Headless-client die wordt gebruikt voor communicatie met AEM
   + `src/api/usePersistedQueries.js` voert [&#x200B; douane React haks &#x200B;](https://react.dev/learn/reusing-logic-with-custom-hooks#custom-hooks-sharing-logic-between-components) terugkeergegevens van AEM GraphQL aan de `Teams.js` en `Person.js` meningscomponenten uit.

1. Het `src/components/Teams.js` dossier toont een lijst van teams en hun leden, door een lijstvraag te gebruiken.
1. In het bestand `src/components/Person.js` worden de details van één persoon weergegeven, waarbij een query met één resultaat als parameter wordt gebruikt.

## Het object AEMHeadless controleren

Bekijk het `aemHeadlessClient.js` -bestand voor informatie over het maken van het `AEMHeadless` -object dat wordt gebruikt om te communiceren met AEM.

1. Open `src/api/aemHeadlessClient.js`.

1. Herzie de lijnen 1-40:

   + De invoer `AEMHeadless` verklaring van de [&#x200B; Hoofdloze Cliënt van AEM voor JavaScript &#x200B;](https://github.com/adobe/aem-headless-client-js), lijn 11.

   + De configuratie van de autorisatie op basis van de variabelen in `.env.development`, regel 14-22 en, de expressie van de pijlfunctie `setAuthorization`, regel 31-40.

   + De `serviceUrl` opstelling voor de inbegrepen [&#x200B; configuratie van de ontwikkelingsvolmacht &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#proxy-api-requests), lijn 27.

1. De regels 42-49 zijn het belangrijkst, aangezien zij de `AEMHeadless` cliënt concretiseren en het voor gebruik door React app uitvoeren.

```javascript
// Initialize the AEM Headless Client and export it for other files to use
const aemHeadlessClient = new AEMHeadless({
  serviceURL: serviceURL,
  endpoint: REACT_APP_GRAPHQL_ENDPOINT,
  auth: setAuthorization(),
});

export default aemHeadlessClient;
```

## Implementeren om AEM GraphQL-query&#39;s uit te voeren

Als u de algemene functie `fetchPersistedQuery(..)` wilt implementeren om de AEM GraphQL-query&#39;s die u hebt uitgevoerd, uit te voeren, opent u het `usePersistedQueries.js` -bestand. De functie `fetchPersistedQuery(..)` gebruikt de functie `aemHeadlessClient` object `runPersistedQuery()` om query asynchroon, op belofte gebaseerd gedrag uit te voeren.

Later wordt deze functie aangeroepen door de aangepaste React `useEffect` -haak om specifieke gegevens van AEM op te halen.

1. In `src/api/usePersistedQueries.js` **update** `fetchPersistedQuery(..)`, lijn 35, met de hieronder code.

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

+ Een nieuwe [&#x200B; haak React useEffect &#x200B;](https://react.dev/reference/react/useEffect#useeffect) in `src/api/usePersistedQueries.js` die `my-project/all-teams` voortgezette vraag aanhaalt, terugkerend een lijst van de Fragmenten van de Inhoud van het Team in AEM.
+ Een React component bij `src/components/Teams.js` die de nieuwe aangepaste React `useEffect` haak aanhaalt, en de teamgegevens rendert.

Zodra de app is voltooid, wordt de hoofdweergave van de app gevuld met de teamgegevens uit AEM.

![&#x200B; mening van Teams &#x200B;](./assets/graphql-and-external-app/react-app__teams-view.png)

### Stappen

1. Open `src/api/usePersistedQueries.js`.

1. De functie zoeken `useAllTeams()`

1. Voeg de volgende code toe om een `useEffect` -haak te maken die de voortgezette query `my-project/all-teams` via `fetchPersistedQuery(..)` aanroept. De haak retourneert ook alleen de relevante gegevens van de AEM GraphQL-respons op `data?.teamList?.items`, zodat de React-weergavecomponenten niet op de hoogte zijn van de bovenliggende JSON-structuren.

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

1. Haal in de component `Teams` React de lijst met teams van AEM op met de `useAllTeams()` -haak.

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

1. Tot slot geef de teamgegevens terug. Elk team dat door de GraphQL-query wordt geretourneerd, wordt gerenderd met de meegeleverde subcomponent `Team` React.

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

Met de [&#x200B; functionaliteit van Teams &#x200B;](#implement-teams-functionality) volledig, voeren wij de functionaliteit uit om de vertoning op de details van een teamlid, of persoon te behandelen.

Deze functionaliteit vereist:

+ Een nieuwe [&#x200B; haak React useEffect &#x200B;](https://react.dev/reference/react/useEffect#useeffect) in `src/api/usePersistedQueries.js` die de geparameterized `my-project/person-by-name` voortgeduurde vraag aanhaalt, en één enkel persoonverslag terugkeert.

+ Een component React bij `src/components/Person.js` die de volledige naam van een persoon als vraagparameter gebruikt, haalt de nieuwe douane React `useEffect` haak aan, en geeft de persoongegevens terug.

Als u de naam van een persoon hebt geselecteerd in de weergave Teams, wordt de weergave van die persoon weergegeven.

![&#x200B; Persoon &#x200B;](./assets/graphql-and-external-app/react-app__person-view.png)

1. Open `src/api/usePersistedQueries.js`.

1. De functie zoeken `usePersonByName(fullName)`

1. Voeg de volgende code toe om een `useEffect` -haak te maken die de voortgezette query `my-project/all-teams` via `fetchPersistedQuery(..)` aanroept. De haak retourneert ook alleen de relevante gegevens van de AEM GraphQL-respons op `data?.teamList?.items`, zodat de React-weergavecomponenten niet op de hoogte zijn van de bovenliggende JSON-structuren.

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
1. In de component `Person` React pareert u de routeparameter `fullName` en haalt u de persoongegevens van AEM op met de `usePersonByName(fullName)` -haak.

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


## CORS-configuratie

In dit voorbeeld is voor React-app het delen van bronnen van oorsprong (CORS) tijdens de ontwikkeling niet vereist omdat deze verbinding maakt met AEM met een lokale proxy. Het gebruik van een lokale proxy is alleen bedoeld om een snelle ontwikkeling te vergemakkelijken en niet om gevallen van niet-ontwikkelingsgebruik te behandelen.

In productiescenario&#39;s is het echter doorgaans de beste manier om de clienttoepassing rechtstreeks vanuit de browser van de gebruiker te laten communiceren met AEM. Om dit toe te laten, [&#x200B; CORS kan in AEM &#x200B;](../deployment/overview.md) moeten worden gevormd om haalverzoeken van uw Web toe te staan app.

## Probeer de app

Herzie app [&#x200B; http://localhost :3000/ &#x200B;](http://localhost:3000/) en klik _Leden_ verbindingen. Ook kunt u meer teams en/of leden toevoegen aan het Team Alpha door de Fragmenten van de Inhoud in AEM toe te voegen.

>[!IMPORTANT]
>
>Om uw implementatieveranderingen te verifiëren of als u app die na bovengenoemde veranderingen werkt niet kunt krijgen, verwijs de [&#x200B; basis-tutorial &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial) `solution` tak.

## Onder de hood

Open browser **Hulpmiddelen van de Ontwikkelaar** > **Netwerk** en _Filter_ voor `all-teams` verzoek. De GraphQL API-aanvraag `/graphql/execute.json/my-project/all-teams` wordt uitgevoerd tegen `http://localhost:3000` en **NOT** tegen de waarde van `REACT_APP_HOST_URI` , bijvoorbeeld `<https://publish-pxxx-exxx.adobeaemcloud.com` . De verzoeken worden gemaakt tegen het React toepassingsdomein omdat [&#x200B; volmachtsopstelling &#x200B;](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) gebruikend `http-proxy-middleware` module wordt toegelaten.


![&#x200B; GraphQL API verzoek via Volmacht &#x200B;](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


Bekijk het hoofd `../setupProxy.js` dossier en binnen `../proxy/setupProxy.auth.**.js` dossiers merken hoe `/content` en `/graphql` de wegen proxied zijn en erop wijzen het geen statisch middel is.

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

Het gebruiken van de lokale volmacht is geen geschikte optie voor productieplaatsing en meer details kunnen bij _sectie van de Plaatsing van de Productie_ worden gevonden.

## Gefeliciteerd!{#congratulations}

Gefeliciteerd! U hebt de React-app gemaakt om gegevens van AEM GraphQL API&#39;s te verbruiken en weer te geven als onderdeel van de basiszelfstudie.
