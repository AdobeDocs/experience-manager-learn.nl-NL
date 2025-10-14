---
title: Adobe I/O Runtime Action and AEM Events
description: Leer hoe u AEM Events kunt ontvangen met de Adobe I/O Runtime-actie en bekijk de gebeurtenisdetails zoals payload, headers en metadata.
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 457
last-substantial-update: 2024-01-29T00:00:00Z
jira: KT-14878
thumbnail: KT-14878.jpeg
exl-id: b1c127a8-24e7-4521-b535-60589a1391bf
source-git-commit: bb4f9982263a15f18b9f39b1577b61310dfbe643
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 0%

---

# Adobe I/O Runtime Action and AEM Events

Leer hoe te om de Gebeurtenissen van AEM te ontvangen gebruikend [&#x200B; Adobe I/O Runtime &#x200B;](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) Actie en de gebeurtenisdetails zoals nuttige lading, kopballen, en meta-gegevens te herzien.

>[!VIDEO](https://video.tv.adobe.com/v/3427053?quality=12&learn=on)

De Adobe I/O Runtime is een serverloos platform dat code-uitvoering toestaat als reactie op Adobe I/O Events. Zo kunt u gebeurtenisgestuurde toepassingen maken zonder dat u zich zorgen hoeft te maken over de infrastructuur.

In dit voorbeeld, creeert u een Adobe I/O Runtime [&#x200B; Actie &#x200B;](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) die de Gebeurtenissen van AEM ontvangt en de gebeurtenisdetails registreert.
https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/

De stappen op hoog niveau zijn:

- Project maken in Adobe Developer Console
- Project initialiseren voor lokale ontwikkeling
- Project configureren in Adobe Developer Console
- AEM-gebeurtenis activeren en uitvoering van handeling controleren

## Vereisten

U hebt het volgende nodig om deze zelfstudie te voltooien:

- Het milieu van AEM as a Cloud Service met [&#x200B; toegelaten de Gebeurtenis van AEM &#x200B;](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- Toegang tot [&#x200B; Adobe Developer Console &#x200B;](https://developer.adobe.com/developer-console/docs/guides/getting-started).

- [&#x200B; Adobe Developer CLI &#x200B;](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) geïnstalleerd op uw lokale machine.

## Project maken in Adobe Developer Console

Voer de volgende stappen uit om een project te maken in Adobe Developer Console:

- Navigeer aan [&#x200B; Adobe Developer Console &#x200B;](https://developer.adobe.com/) en klik **console** knoop.

- In de **Snelle sectie van het Begin**, klik **creeer project van malplaatje**. Dan, in **doorbladert malplaatjes** dialoog, uitgezochte **App Builder** malplaatje.

- Werk indien nodig de projecttitel, toepassingsnaam en werkruimte toevoegen bij. Dan, klik **sparen**.

  ![&#x200B; creeer project in Adobe Developer Console &#x200B;](../assets/examples/runtime-action/create-project.png)


## Project initialiseren voor lokale ontwikkeling

Als u Adobe I/O Runtime Action aan het project wilt toevoegen, moet u het project voor lokale ontwikkeling initialiseren. Navigeer op de lokale computer naar de locatie waar u het project wilt initialiseren en voer de volgende stappen uit:

- Project initialiseren door

  ```bash
  aio app init
  ```

- Selecteer `Organization`, `Project` u in de vorige stap creeerde, en de werkruimte. Selecteer de optie `All Templates` in de stap `What templates do you want to search for?` .

  ![&#x200B; organisatie-project-Selectie - Initialiseer project &#x200B;](../assets/examples/runtime-action/all-templates.png)

- Selecteer de optie `@adobe/generator-app-excshell` in de lijst met sjablonen.

  ![&#x200B; malplaatje van de Rekbaarheid - Initialiseer project &#x200B;](../assets/examples/runtime-action/extensibility-template.png)

- Open project in uw favoriete winde, bijvoorbeeld VSCode.

- Het geselecteerde _malplaatje van de Rekbaarheid_ (`@adobe/generator-app-excshell`) verstrekt een generische runtime actie, is de code in `src/dx-excshell-1/actions/generic/index.js` dossier. Laten we het bijwerken om het eenvoudig te houden, de gebeurtenisdetails vast te leggen en een succesreactie te retourneren. In het volgende voorbeeld wordt het echter uitgebreid om de ontvangen AEM-gebeurtenissen te verwerken.

  ```javascript
  const fetch = require("node-fetch");
  const { Core } = require("@adobe/aio-sdk");
  const {
  errorResponse,
  getBearerToken,
  stringParameters,
  checkMissingRequestInputs,
  } = require("../utils");
  
  // main function that will be executed by Adobe I/O Runtime
  async function main(params) {
  // create a Logger
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });
  
  try {
      // 'info' is the default level if not set
      logger.info("Calling the main action");
  
      // log parameters, only if params.LOG_LEVEL === 'debug'
      logger.debug(stringParameters(params));
  
      const response = {
      statusCode: 200,
      body: {
          message: "Received AEM Event, it will be processed in next example",
      },
      };
  
      // log the response status code
      logger.info(`${response.statusCode}: successful request`);
      return response;
  } catch (error) {
      // log any server errors
      logger.error(error);
      // return with 500
      return errorResponse(500, "server error", logger);
  }
  }
  
  exports.main = main;
  ```

- Implementeer ten slotte de bijgewerkte actie op Adobe I/O Runtime door deze uit te voeren.

  ```bash
  aio app deploy
  ```

## Project configureren in Adobe Developer Console

Als u AEM Events wilt ontvangen en de Adobe I/O Runtime-actie wilt uitvoeren die u in de vorige stap hebt gemaakt, configureert u het project in Adobe Developer Console.

- In Adobe Developer Console, navigeer aan het [&#x200B; project &#x200B;](https://developer.adobe.com/console/projects) dat in de vorige stap wordt gecreeerd en klik om het te openen. Selecteer de werkruimte van `Stage` . Hier is de handeling geïmplementeerd.

- Klik **toevoegen de knoop van de Dienst** en selecteren **API** optie. In **voeg API** modaal toe, selecteer **de Diensten van Adobe** > **I/O Beheer API** en klik **daarna**, volg extra configuratiestappen en klik **sparen gevormde API**.

  ![&#x200B; voegt de Dienst toe - vorm project &#x200B;](../assets/examples/runtime-action/add-io-management-api.png)

- Eveneens, klik **toevoegen de knoop van de Dienst** en selecteren **de optie van de Gebeurtenis**. In **voeg Gebeurtenissen** dialoog toe, selecteer **Experience Cloud** > **AEM Sites**, en klik **daarna**. Voer aanvullende configuratiestappen uit en selecteer AEMCS-instantie, gebeurtenistypen en andere details.

- Tot slot in **hoe te om gebeurtenissen** stap te ontvangen, breid **Runtime actie** optie uit en selecteer de _generische_ actie die in de vorige stap wordt gecreeerd. Klik **sparen gevormde gebeurtenissen**.

  ![&#x200B; Runtime Actie - vorm project &#x200B;](../assets/examples/runtime-action/select-runtime-action.png)

- Herzie de details van de Registratie van de Gebeurtenis, ook **zuivert het Vinden** lusje en verifieert het **3&rbrace; verzoek en de reactie van de Beeldsproef van de Uitdaging.**

  ![&#x200B; Gegevens van de Registratie van de Gebeurtenis &#x200B;](../assets/examples/runtime-action/debug-tracing-challenge-probe.png)


## AEM-gebeurtenissen activeren

Ga als volgt te werk om AEM-gebeurtenissen vanuit uw AEM as a Cloud Service-omgeving te activeren die zijn geregistreerd in het bovenstaande Adobe Developer Console-project:

- De toegang en login aan uw het auteursmilieu van AEM as a Cloud Service via [&#x200B; Cloud Manager &#x200B;](https://my.cloudmanager.adobe.com/).

- Afhankelijk van uw **Geabonneerde Gebeurtenissen**, creeer, werk, schrap, publiceer of unpublish een Fragment van de Inhoud.

## Gebeurtenisdetails controleren

Nadat u de bovenstaande stappen hebt uitgevoerd, ziet u dat de AEM Events worden geleverd aan de algemene actie.

U kunt de gebeurtenisdetails in **herzien zuivert het Vinden** lusje van de details van de Registratie van de Gebeurtenis.

![&#x200B; de Details van de Gebeurtenis van AEM &#x200B;](../assets/examples/runtime-action/aem-event-details.png)


## Volgende stappen

In het volgende voorbeeld verbeteren wij deze actie om de Gebeurtenissen van AEM te verwerken, de auteursdienst van terugbellen AEM om inhoudsdetails te krijgen, details in de opslag van Adobe I/O Runtime op te slaan, en hen te tonen via de Toepassing van de Enige Pagina (SPA).
