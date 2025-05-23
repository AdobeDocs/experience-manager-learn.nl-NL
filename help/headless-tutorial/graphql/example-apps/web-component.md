---
title: Webcomponent/JS - voorbeeld met koploze AEM
description: Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze Web Component/JS toepassing toont aan hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted vragen.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10797
thumbnail: kt-10797.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 4f090809-753e-465c-9970-48cf0d1e4790
duration: 129
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 0%

---

# Webcomponent

Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze toepassing van de Component van Web toont aan hoe te om inhoud te vragen gebruikend AEM APIs gebruikend persisted vragen en een gedeelte van UI terug te geven, verwezenlijkt gebruikend zuivere code van JavaScript.

![ Component van het Web met de Hoofdloze van AEM ](./assets/web-component/web-component.png)

Bekijk de [ broncode op GitHub ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

+ [ Node.js v18 ](https://nodejs.org/en/)
+ [ Git ](https://git-scm.com/)

## AEM-vereisten

De component Web werkt met de volgende AEM-implementatieopties.

+ [ AEM as a Cloud Service ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=nl-NL)
+ De lokale opstelling die [ AEM Cloud Service SDK ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=nl-NL) gebruikt
   + Vereist [ JDK 11 ](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2fx jcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) (als het verbinden met lokale AEM 6.5 of AEM SDK)

Dit voorbeeld app baseert zich op [ basis-tutorial-solution.content.zip ](../multi-step/assets/explore-graphql-api/basic-tutorial-solution.content.zip) om worden geïnstalleerd en de vereiste [ plaatsingsconfiguraties ](../deployment/web-component.md) zijn op zijn plaats.


>[!IMPORTANT]
>
>De Component van het Web wordt ontworpen om met __AEM te verbinden publiceert__ milieu, nochtans kan het inhoud van de Auteur van AEM als de authentificatie in het 2&rbrace; [&#128279;](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) dossier van de Component van het Web &lbrace;wordt verstrekt.`person.js`

## Hoe wordt het gebruikt

1. De gegevensopslagruimte `adobe/aem-guides-wknd-graphql` klonen:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Ga naar de submap `web-component` .

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. Bewerk het `.../src/person.js` -bestand om de AEM-verbindingsgegevens op te nemen:

   Werk in het `aemHeadlessService` -object de `aemHost` bij om naar de AEM-publicatieservice te verwijzen.

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p123-e456.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   Als u verbinding maakt met een AEM Author-service, geeft u in het `aemCredentials` -object lokale AEM-gebruikersgegevens op.

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. Open een terminal en voer de opdrachten uit vanuit `aem-guides-wknd-graphql/web-component` :

   ```shell
   $ npm install
   $ npm start
   ```

1. Een nieuw browser venster opent de statische pagina van HTML die de Component van het Web in [ http://localhost:8080 ](http://localhost:8080) inbedt.
1. De _Component van het Web van Info van de Persoon_ wordt getoond op de Web-pagina.

## De code

Hieronder volgt een overzicht van hoe de Component van het Web wordt gebouwd, hoe het met AEM Headless verbindt om inhoud terug te winnen gebruikend GraphQL voortgeduurde vragen, en hoe die gegevens worden voorgesteld. De volledige code kan op [ GitHub ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component) worden gevonden.

### HTML-tag voor webcomponent

Een herbruikbare webcomponent (ook bekend als aangepast element) `<person-info>` wordt toegevoegd aan de `../src/assets/aem-headless.html` HTML-pagina. De klasse ondersteunt `host` - en `query-param-value` -kenmerken om het gedrag van de component te stimuleren. De waarde van het kenmerk `host` overschrijft `aemHost` value from `aemHeadlessService` -object in `person.js` en `query-param-value` wordt gebruikt om de te renderen persoon te selecteren.

```html
    <person-info 
        host="https://publish-p123-e456.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Webcomponentimplementatie

In `person.js` wordt de functionaliteit van de webcomponent gedefinieerd. Hieronder vindt u belangrijke markeringen.

#### Implementatie van het element PersonInfo

Het klassenobject van het `<person-info>` aangepaste element definieert de functionaliteit met behulp van de levenscyclusmethoden van `connectedCallback()` , het koppelen van een schaduwhoofdmap, het ophalen van GraphQL-query&#39;s die nog steeds worden uitgevoerd en DOM-manipulatie om de interne schaduw-DOM-structuur van het aangepaste element te maken.

```javascript
// Create a Class for our Custom Element (person-info)
class PersonInfo extends HTMLElement {

    constructor() {
        ...
        // Create a shadow root
        const shadowRoot = this.attachShadow({ mode: "open" });
        ...
    }

    ...

    // lifecycle callback :: When custom element is appended to document
    connectedCallback() {
        ...
        // Fetch GraphQL persisted query
        this.fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue).then(
            ({ data, err }) => {
                if (err) {
                    console.log("Error while fetching data");
                } else if (data?.personList?.items.length === 1) {
                    // DOM manipulation
                    this.renderPersonInfoViaTemplate(data.personList.items[0], host);
                } else {
                    console.log(`Cannot find person with name: ${queryParamValue}`);
                }
            }
        );
    }

    ...

    //Fetch API makes HTTP GET to AEM GraphQL persisted query
    async fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue) {
        ...
        const response = await fetch(
            `${headlessAPIURL}/${aemHeadlessService.persistedQueryName}${encodedParam}`,
            fetchOptions
        );
        ...
    }

    // DOM manipulation to create the custom element's internal shadow DOM structure
    renderPersonInfoViaTemplate(person, host){
        ...
        const personTemplateElement = document.getElementById('person-template');
        const templateContent = personTemplateElement.content;
        const personImgElement = templateContent.querySelector('.person_image');
        personImgElement.setAttribute('src',
            host + (person.profilePicture._dynamicUrl || person.profilePicture._path));
        personImgElement.setAttribute('alt', person.fullName);
        ...
        this.shadowRoot.appendChild(templateContent.cloneNode(true));
    }
}
```

#### Het element `<person-info>` registreren

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### Delen van bronnen van oorsprong (CORS)

Deze Component van Web baseert zich op een op AEM-Gebaseerde configuratie CORS die op het milieu van doelAEM loopt en veronderstelt dat de gastheerpagina op `http://localhost:8080` op ontwikkelingswijze en hieronder een steekproefCORS OSGi configuratie voor de lokale dienst van de Auteur van AEM loopt.

Gelieve te herzien [ plaatsingsconfiguraties ](../deployment/web-component.md) voor de respectieve dienst van AEM.
