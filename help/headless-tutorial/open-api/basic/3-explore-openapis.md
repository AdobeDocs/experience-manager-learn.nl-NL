---
title: OpenAPI-documentatie verkennen | AEM Headless Part 3
description: Ga aan de slag met Adobe Experience Manager (AEM) en OpenAPI. Ontdek de op OpenAPI gebaseerde API's voor het leveren van inhoudsfragmenten in AEM met behulp van de ingebouwde API-documentatie. Leer hoe AEM automatisch OpenAPI-schema's genereert op basis van modellen van inhoudsfragmenten. Experimenteer met het samenstellen van basisquery's met behulp van de API-documentatie.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 400
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '1209'
ht-degree: 0%

---


# Op AEM OpenAPI gebaseerde API&#39;s voor het leveren van inhoudsfragmenten verkennen

De [ Levering van het Fragment van de Inhoud van AEM met OpenAPIs ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/) in AEM verstrekt een krachtige manier om gestructureerde inhoud aan om het even welke toepassing of kanaal te leveren. In dit hoofdstuk, onderzoeken wij hoe te om OpenAPIs te gebruiken om de Fragmenten van de Inhoud via de documentatie **terug te winnen proberen het** functionaliteit.

## Vereisten {#prerequisites}

Dit is een meerdelig leerprogramma en veronderstelt de stappen die in [ worden geschetst Authoring de Fragmenten van de Inhoud ](./2-author-content-fragments.md) zijn voltooid.

Zorg ervoor dat u over het volgende beschikt:

* Hostnaam van de AEM publiceer dienst (b.v., `https://publish-<PROGRAM_ID>-e<ENVIRONMENT_ID >.adobeaemcloud.com/`) de [ Fragmenten van de Inhoud worden gepubliceerd aan ](./2-author-content-fragments.md#publish-content-fragments). Als u de AEM Preview-service publiceert, moet u de hostnaam beschikbaar hebben (bijvoorbeeld `https://preview-<PROGRAM_ID>-e<ENVIRONMENT_ID>.adobeaemcloud.com/` ).

## Doelstellingen {#objectives}

* Vertrouwd met de [ Levering van het Fragment van de Inhoud van AEM met OpenAPIs ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/).
* Oproepen APIs gebruikend API dokken **probeert het** vermogen.

## Leverings-API&#39;s

De AEM Content Fragment Delivery met OpenAPI&#39;s biedt een RESTful-interface voor het ophalen van inhoudsfragmenten. De API&#39;s die in deze zelfstudie worden besproken, zijn alleen beschikbaar op de services Publiceren en Voorvertonen van AEM en niet op de service Auteur. Andere OpenAPIs bestaat voor [ interactie met de Fragmenten van de Inhoud op de dienst van de Auteur van AEM ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/).

## API&#39;s verkennen

[ de Levering van het Fragment van de Inhoud van AEM met de documentatie van OpenAPIs ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/) heeft &quot;uitproberen het&quot;eigenschap die u toestaat om APIs te onderzoeken en hen van browser direct te testen. Dit is een geweldige manier om uzelf bekend te maken met de API-eindpunten en hun mogelijkheden.

Open de [ API van AEM Sites ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/) documenten in uw browser.

APIs is vermeld op de linkernavigatie onder de **sectie van de Levering van het Fragment**. U kunt deze sectie uitvouwen om de beschikbare API&#39;s weer te geven. Het selecteren van API toont de API details in het belangrijkste paneel, en a **probeert het** sectie op het recht-spoor dat u toestaat om API van browser direct te testen en te onderzoeken.

![ de Levering van het Fragment van de Inhoud van AEM met de docs van OpenAPIs ](./assets/3/docs-overview.png)

## Inhoudsfragmenten weergeven

1. Open de [ Levering van het Fragment van de Inhoud van AEM met ontwikkelaarsdocumenten OpenAPI ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/) in uw browser.
1. In de linkernavigatie, breid de **sectie van de Levering van het 0} Fragment uit en selecteer** Lijst alle Fragmenten van de Inhoud **API**

Met deze API kunt u een gepagineerde lijst met alle inhoudsfragmenten van AEM ophalen per map. De eenvoudigste manier om deze API te gebruiken, is het pad naar de map met de inhoudsfragmenten op te geven.

1. Selecteer **Uitproberen het** in de bovenkant van het recht-spoor.
1. Voer de id in van de AEM-service waarmee de API verbinding maakt om de inhoudsfragmenten op te halen. Het emmertje is het eerste deel van de AEM-service-URL Publiceren (of Voorvertoning), meestal in de notatie: `publish-p<PROGRAM_ID>-e<ENVIRONMENT_ID>` of `preview-p<PROGRAM_ID>-e<ENVIRONMENT_ID>` .

Aangezien wij de publicatieservice van AEM gebruiken, plaats het emmertje aan AEM publiceer de dienstherkenningsteken. Bijvoorbeeld:

* **emmertje**: `publish-p138003-e1400351`

![ probeer het emmertje ](./assets/3/try-it-bucket.png)

Wanneer het emmertje wordt geplaatst, werkt het **server** gebied van het Doel automatisch aan volledige API URL van de publicatiedienst van AEM bij, zoals: `https://publish-p138003-e1400351.adobeaemcloud.com/adobe/contentFragments`

1. Breid de **sectie van de Veiligheid** uit en plaats **regeling van de Veiligheid** aan **niets**. De reden hiervoor is dat de AEM-publicatieservice (en de voorvertoningsservice) geen verificatie vereist voor de AEM Content Fragment Delivery met OpenAPI&#39;s.

1. Vouw de **sectie van Parameters** uit om de details van het te krijgen Fragment van de Inhoud te verstrekken.

* **curseur**: Laat leeg, wordt dit gebruikt voor paginering en dit is een eerste verzoek.
* **grens**: Laat leeg, wordt dit gebruikt om het aantal resultaten te beperken die per pagina van resultaten zijn teruggekeerd.
* **weg**: `/content/dam/my-project/en`

  >[!TIP]
  > Wanneer het ingaan van een weg, zorg ervoor zijn prefix `/content/dam/` is en **niet** met een het slepen schuine streep `/` beëindigt.

  ![ probeer het parameters ](assets/3/try-it-parameters.png)

1. Selecteer **verzenden** knoop om de API vraag uit te voeren.
1. In het **lusje van de Reactie** in **probeert het** paneel, zou u een reactie JSON moeten zien die een lijst van Inhoudsfragmenten in de gespecificeerde omslag bevat. De reactie zal gelijkaardig aan het volgende kijken:

   ![ probeer het reactie ](./assets/3/try-it-response.png)

1. De reactie bevat alle Fragmenten van de Inhoud onder de `path` omslag van de parameter 0}, met inbegrip van subfolders, zowel `/content/dam/my-project` Persoon **als** de Fragmenten van de Inhoud van het Team **.**
1. Klik door de array `items` en zoek de waarde `Team Alpha` item `id` . De id wordt gebruikt in de volgende sectie om de details van één inhoudsfragment op te halen.
1. Selecteer **verzoek** in de bovenkant van **uitgeven het** paneel en de diverse parameters in de API vraag om te zien hoe de reactie verandert. U kunt bijvoorbeeld het pad wijzigen in een andere map met Content Fragments of u kunt queryparameters toevoegen om de resultaten te filteren. Wijzig bijvoorbeeld de parameter `path` in `/content/dam/my-project/teams` in alleen inhoudsfragmenten in die map (en submappen).

## Details van inhoudsfragment ophalen

Gelijkaardig aan **maak een lijst van alle Fragmenten van de Inhoud** API, **krijgt een van het Fragment van de Inhoud** API één enkel Fragment van de Inhoud door zijn identiteitskaart samen met om het even welke facultatieve verwijzingen terug. Om deze API te onderzoeken, zullen wij het Fragment van de Inhoud van het Team verzoeken dat verscheidene Fragments van de Inhoud van Persoon van verwijzingen voorziet.

1. Vouw de **sectie van de Levering van het Fragment** in het linkerspoor uit, en selecteer **krijgen een van het Fragment van de Inhoud** API.
1. Selecteer **Uitproberen het** in de bovenkant van het recht-spoor.
1. Controleer de `bucket` punten naar uw AEM as a Cloud Service-service Publiceren of Voorvertoning.
1. Breid de **sectie van de Veiligheid** uit en plaats **regeling van de Veiligheid** aan **niets**. De reden hiervoor is dat de AEM-publicatieservice geen verificatie vereist voor de levering van AEM-inhoudsfragmenten met OpenAPI&#39;s.
1. Breid de **sectie van Parameters** uit om de details van het te krijgen Fragment van de Inhoud te verstrekken:

In dit voorbeeld, gebruik identiteitskaart van het Fragment van de Inhoud van het Team dat in de vorige sectie wordt teruggewonnen. Bijvoorbeeld, voor deze reactie van het Fragment van Inhoud in **maak een lijst van alle Fragmenten van de Inhoud**, gebruik de waarde op het `id` gebied van `b954923a-0368-4fa2-93ea-2845f599f512`. (Uw `id` verschilt van de waarde die wordt gebruikt in de zelfstudie.)

```json
{
    "path": "/content/dam/my-project/teams/team-alpha",
    "name": "",
    "title": "Team Alpha",
    "id": "50f28a14-fec7-4783-a18f-2ce2dc017f55", // This is the Content Fragment ID
    "description": "",
    "model": {},
    "fields": {} 
}
```

* **fragmentId**: `50f28a14-fec7-4783-a18f-2ce2dc017f55`
* **verwijzingen**: `none`
* **diepte**: Laat leeg, zal de **verwijzingen** parameter de diepte van de referenced fragmenten dicteren.
* **gehydrateerd**: Laat leeg, zal de **verwijzingen** parameter de hydratie van de referenced fragmenten dicteren.
* **if-niets-Gelijke**: Laat leeg

1. Selecteer **verzenden** knoop om de API vraag uit te voeren.
1. Herzie de reactie in het **lusje van de Reactie** in **probeert het** paneel. Er wordt een JSON-reactie weergegeven met de details van het inhoudsfragment, inclusief de eigenschappen en eventuele verwijzingen ervan.
1. Selecteer **verzoek** in de bovenkant van **uitgeven het** paneel en in de **Parameters** secties, pas de `references` parameter aan `all-hydrated` aan, makend alle referenced inhoud van het Fragment van de Inhoud om in de API vraag worden omvat.

   * **fragmentId**: `50f28a14-fec7-4783-a18f-2ce2dc017f55`
   * **verwijzingen**: `all-hydrated`
   * **diepte**: Laat leeg, zal de **verwijzingen** parameter de diepte van de referenced fragmenten dicteren.
   * **gehydrateerd**: Laat leeg, zal de **verwijzingen** parameter de hydratie van de referenced fragmenten dicteren.
   * **if-niets-Gelijke**: Laat leeg

1. Selecteer **opnieuw verzenden** knoop om de API vraag opnieuw uit te voeren.
1. Herzie de reactie in het **lusje van de Reactie** in **probeert het** paneel. Er wordt een JSON-reactie weergegeven met de details van het inhoudsfragment, inclusief de eigenschappen en die van de Person-inhoudfragmenten waarnaar wordt verwezen.

De array `teamMembers` bevat nu de details van de Person Content Fragments waarnaar wordt verwezen. Door verwijzingen te typen, kunt u alle benodigde gegevens ophalen in één API-aanroep. Dit is vooral handig om het aantal aanvragen van clienttoepassingen te verminderen.

## Gefeliciteerd!

Gefeliciteerd, creeerde en uitvoerde u verscheidene Levering van het Fragment van de Inhoud van AEM met vraag OpenAPI gebruikend het de documentatie van AEM **probeert het** vermogen.

## Volgende stappen

In het volgende hoofdstuk, [ bouwt Reageren app ](./4-react-app.md), verkent u hoe een externe toepassing met de Levering van het Fragment van de Inhoud van AEM met OpenAPIs kan interactie aangaan.

