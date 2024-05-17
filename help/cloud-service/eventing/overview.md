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
source-git-commit: c0aa4049e846a0f24f8a5f5b014dbaf5bfc8eb2d
workflow-type: tm+mt
source-wordcount: '941'
ht-degree: 0%

---

# AEM

Leer over AEM gebeurtenis, wat het is, waarom en wanneer om het en voorbeelden van het te gebruiken.

>[!VIDEO](https://video.tv.adobe.com/v/3426686?quality=12&learn=on)

>[!IMPORTANT]
>
>AEM as a Cloud Service gebeurtenis is alleen beschikbaar voor geregistreerde gebruikers in de pre-releasemodus. Als u AEM gebeurtenis op uw AEM as a Cloud Service omgeving wilt inschakelen, neemt u contact op met de <a href="mailto:grp-aem-events@adobe.com">AEM-team</a>.

## Wat het is

AEM Event is een cloud-native gebeurtenissysteem dat abonnementen op AEM gebeurtenissen voor verwerking in externe systemen inschakelt. Een AEM-gebeurtenis is een statuswijzigingsmelding die door AEM wordt verzonden wanneer een specifieke actie plaatsvindt. Dit kunnen bijvoorbeeld gebeurtenissen zijn wanneer een inhoudsfragment wordt gemaakt, bijgewerkt of verwijderd.

![AEM](./assets/aem-eventing.png)

Het bovenstaande diagram visualiseerde hoe AEM as a Cloud Service gebeurtenissen veroorzaakt en hen naar de Gebeurtenissen van de Adobe I/O verzendt, die hen aan gebeurtenisabonnees beurtelings blootstelt.

Samengevat zijn er drie hoofdonderdelen:

1. **Gebeurtenisprovider:** AEM as a Cloud Service.
1. **Adobe I/O-gebeurtenissen:** Ontwikkelaarsplatform voor het integreren, uitbreiden en ontwikkelen van apps en ervaringen op basis van producten en technologieën van de Adobe.
1. **Consumenten van gebeurtenissen:** Systemen die eigendom zijn van de klant en die zich abonneren op de AEM Events. Bijvoorbeeld een CRM (Customer Relationship Management), PIM (Product Information Management), OMS (Order Management System) of een aangepaste toepassing.

### Hoe anders

De [Apache Sling-gebeurtenis](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html), OSGi, en [JCR-waarneming](https://jackrabbit.apache.org/oak/docs/features/observation.html) alle bieden mechanismen aan om aan gebeurtenissen te ondertekenen en te verwerken. Nochtans, zijn deze verschillend van AEM Eventing zoals die in deze documentatie wordt besproken.

Het belangrijkste onderscheid van AEM Eventing omvat:

- De code van de gebeurtenisconsument wordt uitgevoerd buiten AEM, die niet in zelfde JVM zoals AEM loopt.
- AEM productcode is verantwoordelijk voor het definiëren van de gebeurtenissen en het verzenden ervan naar Adobe I/O Events.
- Gebeurtenisgegevens worden gestandaardiseerd en verzonden in JSON-indeling. Raadpleeg voor meer informatie [wolken](https://cloudevents.io/).
- Om terug naar AEM te communiceren, gebruikt de gebeurtenisconsument de AEM as a Cloud Service API.


## Waarom en wanneer gebruiken

AEM Eventing biedt talrijke voordelen voor systeemarchitectuur en operationele efficiency. De belangrijkste redenen om te gebruiken AEM Gebeurtenis omvatten:

- **Gebeurtenisgestuurde architecturen maken**: vergemakkelijkt het maken van losjes gekoppelde systemen die onafhankelijk kunnen schalen en bestand zijn tegen storingen.
- **Lage code en lagere operationele kosten**: Voorkomt aanpassingen in AEM, wat leidt tot systemen die eenvoudiger te onderhouden en uit te breiden zijn, waardoor de operationele kosten worden verminderd.
- **Communicatie tussen AEM en externe systemen vereenvoudigen**: Elimineert punt aan punt verbindingen door de Gebeurtenissen van Adobe I/O te laten communicatie beheren, zoals het bepalen van welke AEM gebeurtenissen aan specifieke systemen of de diensten zouden moeten worden geleverd.
- **Hogere duurzaamheid van gebeurtenissen**: Adobe I/O Events is een uiterst beschikbaar en schaalbaar systeem dat is ontworpen om grote volumes aan gebeurtenissen af te handelen en deze betrouwbaar aan abonnees te leveren.
- **Parallelle verwerking van gebeurtenissen**: Laat de levering van gebeurtenissen aan veelvoudige abonnees gelijktijdig toe, die voor verdeelde gebeurtenisverwerking over diverse systemen toestaan.
- **Ontwikkeling van serverloze toepassingen**: Ondersteunt de implementatie van de code voor gebeurtenisverbruikers als een serverloze toepassing, waardoor de systeemflexibiliteit en schaalbaarheid verder worden verbeterd.

### Beperkingen

AEM Eventing, hoewel krachtig, heeft bepaalde beperkingen om te overwegen:

- **Beschikbaarheid beperkt tot AEM as a Cloud Service**: Op dit moment is AEM Event exclusief beschikbaar voor AEM as a Cloud Service.
- **Beperkte ondersteuning voor gebeurtenissen**: Vanaf nu worden alleen AEM gebeurtenissen van Content Fragment ondersteund. De verwachting is echter dat het bereik in de toekomst zal toenemen met de toevoeging van meer gebeurtenissen.

## Hoe te om toe te laten

AEM Eventing wordt ingeschakeld per AEM as a Cloud Service omgeving en alleen beschikbaar voor omgevingen in de pre-releasemodus. Contact opnemen met de <a href="mailto:grp-aem-events@adobe.com">AEM-team</a> om uw AEM met AEM Event toe te laten.

Indien al ingeschakeld, zie [AEM Gebeurtenissen inschakelen in uw AEM Cloud Service-omgeving](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) voor de volgende stappen.

## Abonneren

Als u zich wilt abonneren op AEM gebeurtenissen, hoeft u geen code in AEM te schrijven, maar een [Adobe Developer Console](https://developer.adobe.com/) project wordt gevormd. De Adobe Developer-console is een gateway naar Adobe-API&#39;s, SDK&#39;s, Events, Runtime en App Builder.

In dit geval _project_ in de Adobe Developer Console kunt u zich abonneren op gebeurtenissen die vanuit AEM as a Cloud Service omgeving worden verzonden en de levering van de gebeurtenis aan externe systemen configureren.

Zie voor meer informatie [Abonneren op AEM gebeurtenissen in de Adobe Developer-console](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## Hoe te verbruiken

Er zijn twee primaire methoden voor het gebruik van AEM Events: de _duwen_ en de _trekken_ methode.

- **Push, methode**: In deze aanpak wordt de gebeurtenisgebruiker proactief op de hoogte gebracht door Adobe I/O Events wanneer een gebeurtenis beschikbaar wordt. Tot de integratieopties behoren Webhooks, Adobe I/O Runtime en Amazon EventBridge.
- **Pull, methode**: Hier opiniepeilt de gebeurtenisconsument actief Adobe I/O Events om te controleren op nieuwe gebeurtenissen. De primaire integratieoptie voor deze methode is de Adobe Developer Journaling API.

Zie voor meer informatie [AEM Gebeurtenissen verwerken via Adobe I/O Events](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#aem-events-processing-via-adobe-io).

## Voorbeelden

<table>
  <tr>
    <td>
        <a  href="./examples/webhook.md"><img alt="Ontvang AEM gebeurtenissen op een webhaak" src="./assets/examples/webhook/webhook-example.png"/></a>
        <div><strong><a href="./examples/webhook.md">Ontvang AEM gebeurtenissen op een webhaak</a></strong></div>
        <p>
          Gebruik de Adobe die u hebt opgegeven op de webhaak om AEM gebeurtenissen te ontvangen en de gebeurtenisdetails te bekijken.
        </p>
      </td>
      <td>
        <a  href="./examples/journaling.md"><img alt="AEM Events-journaal laden" src="./assets/examples/journaling/eventing-journal.png"/></a>
        <div><strong><a href="./examples/journaling.md">AEM Events-journaal laden</a></strong></div>
        <p>
          Gebruik de Adobe die is opgegeven in de webtoepassing om AEM gebeurtenissen te laden vanuit het tijdschrift en de gebeurtenisdetails te bekijken.
        </p>
      </td>
    </tr>
  <tr>
    <td>
        <a  href="./examples/runtime-action.md"><img alt="AEM Gebeurtenissen ontvangen bij Adobe I/O Runtime-actie" src="./assets/examples/runtime-action/eventing-runtime.png"/></a>
        <div><strong><a href="./examples/runtime-action.md">AEM Gebeurtenissen ontvangen bij Adobe I/O Runtime-actie</a></strong></div>
        <p>
          Ontvang AEM gebeurtenissen en bekijk de gebeurtenisdetails.
        </p>
      </td>
      <td>
        <a  href="./examples/event-processing-using-runtime-action.md"><img alt="AEM Gebeurtenissen verwerken met Adobe I/O Runtime Action" src="./assets/examples/event-processing-using-runtime-action/event-processing.png"/></a>
        <div><strong><a href="./examples/event-processing-using-runtime-action.md">AEM Gebeurtenissen verwerken met Adobe I/O Runtime Action</a></strong></div>
        <p>
          Leer hoe u ontvangen AEM Gebeurtenissen verwerkt met Adobe I/O Runtime Action. De gebeurtenisverwerking omvat AEM callback, persistentie van gebeurtenisgegevens en het weergeven van deze gegevens in de SPA.
        </p>
      </td>
  </tr>    
  <tr>
    <td>
        <a  href="./examples/assets-pim-integration.md"><img alt="AEM Assets-gebeurtenissen voor PIM-integratie" src="./assets/examples/assets-pim-integration/PIM-integration-tile.png"/></a>
        <div><strong><a href="./examples/assets-pim-integration.md">AEM Assets-gebeurtenissen voor PIM-integratie</a></strong></div>
        <p>
          Leer hoe u AEM Assets en PIM-systemen (Product Information Management) kunt integreren voor updates van metagegevens.
        </p>
      </td>
  </tr>  
</table>
