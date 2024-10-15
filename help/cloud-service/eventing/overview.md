---
title: AEM
description: Leer over AEM gebeurtenis, wat het is, waarom en wanneer om het en voorbeelden van het te gebruiken.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 540
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14649
thumbnail: KT-14649.jpeg
exl-id: 142ed6ae-1659-4849-80a3-50132b2f1a86
source-git-commit: ede52c6c9feb0b35bc3729e28591cb4e7c7600f7
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 0%

---

# AEM

Leer over AEM gebeurtenis, wat het is, waarom en wanneer om het en voorbeelden van het te gebruiken.

>[!VIDEO](https://video.tv.adobe.com/v/3426686?quality=12&learn=on)

## Wat het is

AEM Event is een cloud-native gebeurtenissysteem dat abonnementen op AEM gebeurtenissen voor verwerking in externe systemen inschakelt. Een AEM-gebeurtenis is een statuswijzigingsmelding die door AEM wordt verzonden wanneer een specifieke actie plaatsvindt. Dit kunnen bijvoorbeeld gebeurtenissen zijn wanneer een inhoudsfragment wordt gemaakt, bijgewerkt of verwijderd.

![AEM Eventing ](./assets/aem-eventing.png)

Het bovenstaande diagram visualiseerde hoe AEM as a Cloud Service gebeurtenissen produceert en hen naar de Gebeurtenissen van de Adobe I/O verzendt, die hen aan gebeurtenisabonnees beurtelings blootstelt.

Samengevat zijn er drie hoofdonderdelen:

1. **de leverancier van de Gebeurtenis:** AEM as a Cloud Service.
1. **Gebeurtenissen van de Adobe I/O:** het platform van de Ontwikkelaar voor het integreren, uitbreiden, en het bouwen van apps en ervaringen die op de producten en de technologieën van de Adobe worden gebaseerd.
1. **consument van de Gebeurtenis:** Systemen die door de klant worden bezeten die aan de AEM Gebeurtenissen intekent. Bijvoorbeeld een CRM (Customer Relationship Management), PIM (Product Information Management), OMS (Order Management System) of een aangepaste toepassing.

### Hoe anders

De [ Gebeurtenis Apache die ](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html), OSGi gebeurtenis, en [ JCR waarneming ](https://jackrabbit.apache.org/oak/docs/features/observation.html) allen aanbiedt mechanismen om aan gebeurtenissen in te tekenen en te verwerken. Nochtans, zijn deze verschillend van AEM Eventing zoals die in deze documentatie wordt besproken.

Het belangrijkste onderscheid van AEM Eventing omvat:

- De code van de gebeurtenisconsument wordt uitgevoerd buiten AEM, die niet in zelfde JVM zoals AEM loopt.
- AEM productcode is verantwoordelijk voor het definiëren van de gebeurtenissen en het verzenden ervan naar Adobe I/O Events.
- Gebeurtenisgegevens worden gestandaardiseerd en verzonden in JSON-indeling. Voor meer details, verwijs naar [ wolken ](https://cloudevents.io/).
- Om terug naar AEM te communiceren, gebruikt de gebruiker van de gebeurtenis de AEM as a Cloud Service API.


## Waarom en wanneer gebruiken

AEM Eventing biedt talrijke voordelen voor systeemarchitectuur en operationele efficiency. De belangrijkste redenen om te gebruiken AEM Gebeurtenis omvatten:

- **om gebeurtenis-gedreven Architecturen** te bouwen: vergemakkelijkt de verwezenlijking van los gekoppelde systemen die onafhankelijk kunnen schrapen en veerkrachtig aan mislukkingen zijn.
- **Lage code en lagere operationele kosten**: Vermijdt aanpassingen in AEM, die tot systemen leiden die gemakkelijker zijn te handhaven en uit te breiden, waarbij operationele kosten worden verminderd.
- **vereenvoudigt communicatie tussen AEM en externe systemen**: Elimineert punt om verbindingen te richten door de Gebeurtenissen van Adobe I/O te laten communicatie beheren, zoals het bepalen van welke AEM gebeurtenissen aan specifieke systemen of de diensten zouden moeten worden geleverd.
- **Hogere duurzaamheid van gebeurtenissen**: De Gebeurtenissen van de Adobe I/O zijn hoogst beschikbaar en scalable systeem, dat wordt ontworpen om grote volumes van gebeurtenissen te behandelen en hen betrouwbaar te leveren aan abonnees.
- **Parallelle verwerking van gebeurtenissen**: Laat de levering van gebeurtenissen aan veelvoudige abonnees gelijktijdig toe, die voor verdeelde gebeurtenisverwerking over diverse systemen toestaan.
- **Serverless toepassingsontwikkeling**: Steunt het opstellen van de code van de gebeurtenisconsument als serverloze toepassing, die systeemflexibiliteit en scalability verder verbetert.

### Beperkingen

AEM Eventing, hoewel krachtig, heeft bepaalde beperkingen om te overwegen:

- **Beschikbaarheid beperkt tot AEM as a Cloud Service**: Momenteel, AEM Eventing is uitsluitend beschikbaar voor AEM as a Cloud Service.

- **Beschikbare gebeurtenistypen**: Herzie de huidige lijst van beschikbare gebeurtenistypen [ hier ](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#available-event-types).

## Hoe te om toe te laten

Zie [ AEM Gebeurtenissen op uw Milieu van AEM Cloud Service ](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) voor volgende stappen toelaten.

## Abonneren

Om aan AEM Gebeurtenissen in te tekenen, moet u geen code in AEM schrijven, maar eerder wordt een [ Adobe Developer Console ](https://developer.adobe.com/) project gevormd. Adobe Developer Console is een gateway naar Adobe APIs, SDKs, Gebeurtenissen, Runtime, en App Builder.

In dit geval, laat het a _project_ in Adobe Developer Console u toe om aan gebeurtenissen in te tekenen die van het milieu van AEM as a Cloud Service worden uitgegeven en de gebeurtenislevering aan externe systemen te vormen.

Voor meer informatie, zie [ hoe te aan AEM Gebeurtenissen in Adobe Developer Console ](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console) intekenen.

## Hoe te verbruiken

Er zijn twee primaire methodes om AEM Gebeurtenissen te verbruiken: de _duw_ methode en de _trek_ methode.

- **Push methode**: In deze benadering, wordt de gebeurtenisconsument proactief op de hoogte gebracht door de Gebeurtenissen van Adobe I/O wanneer een gebeurtenis beschikbaar wordt. Tot de integratieopties behoren Webhooks, Adobe I/O Runtime en Amazon EventBridge.
- **Pull methode**: Hier, opinieert de gebeurtenisconsument actief Adobe I/O Gebeurtenissen om nieuwe gebeurtenissen te controleren. De primaire integratieoptie voor deze methode is de Adobe Developer Journaling API.

Voor meer informatie, zie [ AEM Gebeurtenissen die via de Gebeurtenissen van de Adobe I/O ](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#aem-events-processing-via-adobe-io) verwerken.

## Voorbeelden

<table>
  <tr>
    <td>
        <a  href="./examples/webhook.md"><img alt="Ontvang AEM gebeurtenissen op een webhaak" src="./assets/examples/webhook/webhook-example.png"/></a>
        <div><strong><a href="./examples/webhook.md"> ontvang AEM Gebeurtenissen op een webhaak </a></strong></div>
        <p>
          Gebruik de Adobe die u hebt opgegeven op de webhaak om AEM gebeurtenissen te ontvangen en de gebeurtenisdetails te bekijken.
        </p>
      </td>
      <td>
        <a  href="./examples/journaling.md"><img alt="AEM Events-journaal laden" src="./assets/examples/journaling/eventing-journal.png"/></a>
        <div><strong><a href="./examples/journaling.md"> lading AEM het dagboek van Gebeurtenissen </a></strong></div>
        <p>
          Gebruik de Adobe die is opgegeven in de webtoepassing om AEM gebeurtenissen te laden vanuit het tijdschrift en de gebeurtenisdetails te bekijken.
        </p>
      </td>
    </tr>
  <tr>
    <td>
        <a  href="./examples/runtime-action.md"><img alt="AEM Gebeurtenissen ontvangen bij Adobe I/O Runtime-actie" src="./assets/examples/runtime-action/eventing-runtime.png"/></a>
        <div><strong><a href="./examples/runtime-action.md"> ontvang AEM Gebeurtenissen op de Actie van Adobe I/O Runtime </a></strong></div>
        <p>
          Ontvang AEM gebeurtenissen en bekijk de gebeurtenisdetails.
        </p>
      </td>
      <td>
        <a  href="./examples/event-processing-using-runtime-action.md"><img alt="AEM Gebeurtenissen verwerken met Adobe I/O Runtime Action" src="./assets/examples/event-processing-using-runtime-action/event-processing.png"/></a>
        <div><strong><a href="./examples/event-processing-using-runtime-action.md"> AEM Gebeurtenissen die de Actie van Adobe I/O Runtime gebruiken </a></strong></div>
        <p>
          Leer hoe u ontvangen AEM Gebeurtenissen verwerkt met Adobe I/O Runtime Action. De gebeurtenisverwerking omvat AEM callback, persistentie van gebeurtenisgegevens en het weergeven van deze gegevens in de SPA.
        </p>
      </td>
  </tr>    
</table>
