---
title: React-app maken met AEM OpenAPI | Lesbestand zonder koptekst, deel 4
description: Ga aan de slag met Adobe Experience Manager (AEM) en OpenAPI. Maak een React-app waarmee inhoud/gegevens worden opgehaald van op AEM op OpenAPI gebaseerde API's voor het leveren van inhoudsfragmenten.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 900
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '874'
ht-degree: 0%

---


# Een React-app maken met AEM Content Fragment Delivery OpenAPI&#39;s

In dit hoofdstuk leert u hoe AEM Content Fragment Delivery met OpenAPI&#39;s de ervaring in externe toepassingen kan aansturen.

Een eenvoudige React app wordt gebruikt om **Team** en **Persoon** inhoud te verzoeken en te tonen die door de Levering van het Fragment van de Inhoud van AEM met OpenAPIs wordt blootgesteld. Het gebruik van React is grotendeels onbelangrijk, en de verbruikende externe toepassing zou in om het even welk kader voor om het even welk platform kunnen worden geschreven, zolang het HTTP- verzoeken aan AEM as a Cloud Service kan doen.

## Vereisten

Aangenomen wordt dat de stappen die in de vorige onderdelen van deze meerdelige zelfstudie zijn beschreven, zijn voltooid.

De volgende software moet worden geïnstalleerd:

* [ Node.js v22+ ](https://nodejs.org/en)
* [ Code van Visual Studio ](https://code.visualstudio.com/)

## Doelstellingen

Leer hoe u:

* Download en start het voorbeeld React app.
* Invoke AEM Content Fragment Delivery with OpenAPI APIs for a list of teams, and their referenced members.
* Invoke AEM Content Fragment Delivery with OpenAPI APIs to retrieve a team member&#39;s details.

## CORS instellen op AEM as a Cloud Service

Dit voorbeeld React-app wordt lokaal uitgevoerd (op `http://localhost:3000` ) en maakt verbinding met de AEM Content Fragment Delivery van de AEM Publish-service met OpenAPI&#39;s. Om deze verbinding mogelijk te maken, moet CORS (Cross-Origin Resource Sharing) zijn geconfigureerd op de AEM-service Publiceren (of Voorvertoning).

Volg de [ instructies bij het plaatsen van een KUUROORD die op `http://localhost:3000` loopt om verzoeken CORS aan AEM toe te staan publiceer dienst ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#different-domains).

### Lokale CORS-proxy

Alternatief, voor ontwikkeling, stel a [ Lokale Volmacht CORS ](https://www.npmjs.com/package/local-cors-proxy) in werking die faciliteiten een CORS-vriendschappelijke verbinding aan AEM.

```bash
$ npm install --global lcp
$ lcp --proxyUrl https://publish-p<PROGRAM_ID>-e<ENVIRONMENT_ID>.adobeaemcloud.com
```

Werk de `--proxyUrl` -waarde bij naar de AEM-URL voor publiceren (of voorvertonen).

Open de API&#39;s voor levering van AEM-inhoudsfragmenten op `http://localhost:8010/proxy` terwijl de lokale CORS-proxy wordt uitgevoerd om CORS-problemen te voorkomen.

## De voorbeeldtoepassing Reageren klonen

Er is een uitgelijnde voorbeeldtoepassing React geïmplementeerd met de code die vereist is voor interactie met AEM Content Fragment Delivery met OpenAPI&#39;s en de gegevens van het weergaveteam en de persoon die van hen zijn verkregen.

De steekproef React app broncode is [ beschikbaar op Github.com ](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic).

Zo krijgt u de React-app:

1. Kloon de steekproefWKND OpenAPI React app van [ Github.com ](https://github.com/adobe/aem-tutorials) van de [`headless_open-api_basic` markering ](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-tutorials.git
   $ cd aem-tutorials  
   $ git fetch --tags
   $ git tag
   $ git checkout tags/headless_open-api_basic
   ```

1. Navigeer naar de map `headless/open-api/basic` en open deze in uw IDE.

   ```shell
   $ cd ~/Code/aem-tutorials/headless/open-api/basic
   $ code .
   ```

1. Werk `.env` bij om verbinding te maken met de AEM as a Cloud Service-publicatieservice, zoals hier waar de Content Fragments worden gepubliceerd. Dit kan verwijzen naar de AEM Preview-service als u de app wilt testen met de AEM Preview-service (en de Content Fragments worden daar gepubliceerd).

   ```
   # AEM Publish (or Preview) service that provides Content Fragments
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   ```

   Wanneer het gebruiken van [ Lokale Volmacht CORS ](#local-cors-proxy), plaats `REACT_APP_HOST_URI` aan `http://localhost:8010/proxy`.

   ```
   # AEM Publish (or Preview) service that provides Content Fragments
   REACT_APP_HOST_URI=http://localhost:8010/proxy
   ```

1. De React-app starten

   ```shell
   $ cd ~/Code/aem-tutorials/headless/open-api/basic
   $ npm install
   $ npm start
   ```

1. React app begint op ontwikkelingswijze op [ http://localhost :3000/ ](http://localhost:3000/). Wijzigingen die tijdens de zelfstudie in de React-app worden aangebracht, worden direct in de webbrowser doorgevoerd.

>[!IMPORTANT]
>
>   Deze React-app is gedeeltelijk geïmplementeerd. Voer de stappen in deze zelfstudie uit om de implementatie te voltooien. De JavaScript-bestanden die implementatiewerk nodig hebben, hebben de volgende opmerking. Zorg ervoor dat u de code in die bestanden toevoegt/bijwerkt met de code die in deze zelfstudie is opgegeven.
>
>
>  //************************************
>  >  // TODO: Implementeer dit door de stappen van de zelfstudie voor AEM Headless te volgen
>  >  //************************************
>

## Anatomie van de React-app

De voorbeeldtoepassing React bevat drie hoofdonderdelen die moeten worden bijgewerkt.

1. Het `.env` -bestand bevat de URL van de AEM-service Publiceren (of Voorvertoning).
1. In `src/components/Teams.js` wordt een lijst met teams en hun leden weergegeven.
1. In `src/components/Person.js` worden de gegevens van één teamlid weergegeven.

## Teamfunctionaliteit implementeren

Ontwikkel de functionaliteit om de Teams en hun leden op de belangrijkste mening van React te tonen app. Deze functionaliteit vereist:

* Een nieuwe [ haak React useEffect ](https://react.dev/reference/react/useEffect#useeffect) die de **Lijst alle Fragmenten API van de Inhoud** via een haalverzoek aanhaalt, en krijgt dan de `fullName` waarde voor elke `teamMember` voor vertoning.

Zodra de app is voltooid, wordt de hoofdweergave van de app gevuld met de teamgegevens uit AEM.

![ mening van Teams ](./assets/4/teams.png)

1. Open `src/components/Teams.js`.

1. Voer de **component van Teams** uit om de lijst van teams van [ van de Lijst te halen alle Fragmenten API van de Inhoud ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/#operation/fragments/getFragments), en de teaminhoud terug te geven. Dit wordt opgedeeld in de volgende stappen:

1. Creeer a `useEffect` haak die AEM **oproept van alle Fragmenten van de Inhoud {** API een lijst maken en de gegevens in de staat van de component van de Reactie opslaat.
1. Voor elk **teruggekeerd de Fragment van de Inhoud van het Team**, haalt **een Fragment van de Inhoud** API aan om volledig gehydrateerde details van het team, met inbegrip van zijn leden en hun `fullNames` te halen.
1. De teamgegevens renderen met de functie `Team` .

   ```javascript
   import { useEffect, useState } from "react";
   import { Link } from "react-router-dom";
   import "./Teams.scss";
   
   function Teams() {
   
     // The teams folder is the only folder-tree that is allowed to contain Team Content Fragments.
     const TEAMS_FOLDER = '/content/dam/my-project/en/teams';
   
     // State to store the teams data
     const [teams, setTeams] = useState(null);
   
     useEffect(() => {
       /**
       * Fetches all teams and their associated member details
       * This is a two-step process:
       * 1. First, get all team content fragments from the specified folder
       * 2. Then, for each team, fetch the full details including hydrated references to get the team member names
       */
       const fetchData = async () => {
         try {
           // Step 1: Fetch all teams from the teams folder
           const response = await fetch(
             `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments?path=${TEAMS_FOLDER}`
           );
           const allTeams = (await response.json()).items || [];
   
           // Step 2: Fetch detailed information for each team with hydrated references
           const hydratedTeams = [];
           for (const team of allTeams) {
             const hydratedTeamResponse = await fetch(
               `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments/${team.id}?references=direct-hydrated`
             );
             hydratedTeams.push(await hydratedTeamResponse.json());
           }
   
           setTeams(hydratedTeams);
         } catch (error) {
           console.error("Error fetching content fragments:", error);
         }
       };
   
       fetchData();
     }, [TEAMS_FOLDER]);
   
     // Show loading state while teams data is being fetched
     if (!teams) {
       return <div>Loading teams...</div>;
     }
   
     // Render the teams
     return (
       <div className="teams">
         {teams.map((team, index) => {
           return (
             <Team 
               key={index} 
               {..team}
             />
           );
         })}
       </div>
     );
   }
   
   /**
   * Team component - renders a single team with its details and members
   * @param {string} fields - The authorable fields
   * @param {Object} references - Hydrated references containing member details such as fullName
   */
   function Team({ fields, references, path }) {
     if (!fields.title || !fields.teamMembers) {
       return null;
     }
   
     return (
       <div className="team">
         <h2 className="team__title">{fields.title}</h2>
         {/* Render description as HTML using dangerouslySetInnerHTML */}
         <p 
           className="team__description" 
           dangerouslySetInnerHTML={{ __html: fields.description.value }}
         />
         <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
             {/* Render each team member as a link to their detail page */}
             {fields.teamMembers.map((teamMember, index) => {
               return (
                 <li key={index} className="team__member">
                   <Link to={`/person/${teamMember}`}>
                     {/* Display the full name from the hydrated reference */}
                     {references[teamMember].value.fields.fullName}
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

## Functionaliteit van personen implementeren

Met de [ functionaliteit van Teams ](#implement-teams-functionality) volledig, voer de functionaliteit uit om de vertoning op de details van een teamlid, of van de persoon te behandelen.

![ mening van de Persoon ](./assets/4/person.png)

Dit doet u als volgt:

1. Openen `src/components/Person.js`
1. In de component `Person` React pareert u de parameter `id` route. Let op: de Routes van de React-app zijn eerder ingesteld om de `id` URL-parameter te accepteren (zie `/src/App.js` ).
1. Vets de persoongegevens van AEM [ krijgen API van het Fragment van de Inhoud ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/#operation/fragments/getFragment).

   ```javascript
   import "./Person.scss";
   import { useEffect, useState } from "react";
   import { useParams } from "react-router-dom";
   
   /**
   * Person component - displays detailed information about a single person
   * Fetches person data from AEM using the ID from the URL parameters
   */
   function Person() {
     // Get the person ID from the URL parameter
     const { id } = useParams();
   
     // State to store the person data
     const [person, setPerson] = useState(null);
   
     useEffect(() => {
       /**
       * Fetches person data from AEM Content Fragment Delivery API
       * Uses the ID from URL parameters to get the specific person's details
       */
       const fetchData = async () => {
         try {
           /* Hydrate references for access to profilePicture asset path */
           const response = await fetch(
             `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments/${id}?references=direct-hydrated`
           );
           const json = await response.json();
           setPerson(json || null);
         } catch (error) {
           console.error("Error fetching person data:", error);
         }
       };
       fetchData();
     }, [id]); // Re-fetch when ID changes
   
     // Show loading state while person data is being fetched
     if (!person) {
       return <div>Loading person...</div>;
     }
   
     return (
       <div className="person">
         {/* Person profile image - Look up the profilePicture reference in the references object */}
         <img
           className="person__image"
           src={process.env.REACT_APP_HOST_URI + person.references[person.fields.profilePicture].value.path}
           alt={person.fields.fullName}
         />
         {/* Display person's occupations */}
         <div className="person__occupations">
           {person.fields.occupation.map((occupation, index) => {
             return (
               <span key={index} className="person__occupation">
                 {occupation}
               </span>
             );
           })}
         </div>
   
         {/* Person's main content: name and biography */}
         <div className="person__content">
           <h1 className="person__full-name">{person.fields.fullName}</h1>
           {/* Render biography as HTML content */}
           <div
             className="person__biography"
             dangerouslySetInnerHTML={{ __html: person.fields.biographyText.value }}
           />
         </div>
       </div>
     );  
   }
   
   export default Person;
   ```

### De voltooide code ophalen

De volledige broncode voor dit hoofdstuk is [ beschikbaar op Github.com ](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end).

```bash
$ git fetch --tags
$ git tag
$ git checkout tags/headless_open-api_basic_4-end
```

## Probeer de app

Herzie app [ http://localhost :3000/ ](http://localhost:3000/) en klik _lid van het Team_ verbindingen. Ook kunt u meer teams en/of leden aan het Team Alpha toevoegen door de Fragments van de Inhoud in de dienst van de Auteur van AEM toe te voegen en hen te publiceren.

## Onder de kap

Open de browser **Hulpmiddelen van de Ontwikkelaar > van het Netwerk** console en **Filter** voor `/adobe/contentFragments` halen verzoeken aangezien u met React app in wisselwerking staat.

## Gefeliciteerd!

Gefeliciteerd! U hebt een React-app gemaakt om inhoudsfragmenten van AEM Content Fragment Delivery met OpenAPI&#39;s te gebruiken en weer te geven.
