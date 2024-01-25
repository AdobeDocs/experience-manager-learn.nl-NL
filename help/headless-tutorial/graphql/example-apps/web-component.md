---
title: Webcomponent/JS - Voorbeeld AEM zonder kop
description: Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze Web Component/JS toepassing toont aan hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted query's.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10797
thumbnail: kt-10797.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM zonder hoofd as a Cloud Service" before-title="false"
exl-id: 4f090809-753e-465c-9970-48cf0d1e4790
duration: 166
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 0%

---

# Webcomponent

Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze toepassing van de Component van Web toont hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted vragen en een gedeelte van UI terug te geven, verwezenlijkt gebruikend zuivere code JavaScript.

![Webcomponent met AEM headless](./assets/web-component/web-component.png)

De weergave [broncode op GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM

De component van het Web werkt met de volgende AEM plaatsingsopties.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Lokale instelling met [de SDK van AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)
   + Vereisten [11 JDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2 Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) (als verbinding wordt gemaakt met lokale AEM 6.5 of AEM SDK)

Deze voorbeeldtoepassing is afhankelijk van [basic-tutorial-solution.content.zip](../multi-step/assets/explore-graphql-api/basic-tutorial-solution.content.zip) en de vereiste [implementatieconfiguraties](../deployment/web-component.md) zijn geïnstalleerd.


>[!IMPORTANT]
>
>De component Web wordt ontworpen om met een __AEM publiceren__ milieu, nochtans kan het inhoud van AEM Auteur als de authentificatie in de Component van het Web wordt verstrekt [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) bestand.

## Hoe wordt het gebruikt

1. Klonen met `adobe/aem-guides-wknd-graphql` opslagplaats:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Navigeren naar `web-component` subdirectory.

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. Bewerk de `.../src/person.js` bestand dat de AEM verbindingsgegevens bevat:

   In de `aemHeadlessService` object, het `aemHost` om naar uw AEM publicatieservice te verwijzen.

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p123-e456.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   Als u verbinding maakt met een AEM Auteur, kunt u in het dialoogvenster `aemCredentials` -object, lokale AEM gebruikersgegevens opgeven.

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. Open een terminal en voer de opdrachten uit vanuit `aem-guides-wknd-graphql/web-component`:

   ```shell
   $ npm install
   $ npm start
   ```

1. Een nieuw browser venster opent de statische pagina van de HTML die de Component van het Web bij insluit [http://localhost:8080](http://localhost:8080).
1. De _Persoonsgegevens_ De Component van het Web wordt getoond op de Web-pagina.

## De code

Hieronder volgt een overzicht van hoe de Component van het Web wordt gebouwd, hoe het met AEM Headless verbindt om inhoud terug te winnen gebruikend GraphQL voortgeduurde vragen, en hoe dat gegeven wordt voorgesteld. De volledige code is te vinden op [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component).

### HTML-tag van webcomponent

Een herbruikbare webcomponent (ook bekend als aangepast element) `<person-info>` wordt toegevoegd aan de `../src/assets/aem-headless.html` HTML pagina. Het ondersteunt `host` en `query-param-value` kenmerken om het gedrag van de component te bepalen. De `host` waardeoverschrijvingen van kenmerk `aemHost` waarde van `aemHeadlessService` object in `person.js`, en `query-param-value` wordt gebruikt om de persoon te selecteren die moet worden weergegeven.

```html
    <person-info 
        host="https://publish-p123-e456.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Webcomponentimplementatie

De `person.js` Hiermee definieert u de functionaliteit van de webcomponent en hieronder ziet u de belangrijkste hooglichten van de component.

#### Implementatie van het element PersonInfo

De `<person-info>` het klassenobject van het aangepaste element definieert de functionaliteit door het `connectedCallback()` levenscyclusmethoden, het koppelen van een schaduwhoofdmap, het ophalen van GraphQL-permanente query en DOM-manipulatie om de interne schaduw-DOM-structuur van het aangepaste element te maken.

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

#### Registreer de `<person-info>` element

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### Delen van bronnen van oorsprong (CORS)

Deze Component van Web baseert zich op een op AEM-Gebaseerde configuratie CORS die op het doel AEM milieu loopt en veronderstelt dat de gastheerpagina op loopt `http://localhost:8080` in ontwikkelingswijze en hieronder is een steekproefCORS OSGi configuratie voor de lokale dienst van de AEMAuteur.

Gelieve te herzien [implementatieconfiguraties](../deployment/web-component.md) voor de respectieve AEM.
