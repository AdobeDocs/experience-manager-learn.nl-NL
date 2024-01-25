---
title: Journalisten en AEM
description: Leer hoe te om de aanvankelijke reeks AEM Gebeurtenissen van het dagboek terug te winnen en de details over elke gebeurtenis te onderzoeken.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 144
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14734
thumbnail: KT-14734.jpeg
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 0%

---


# Journalisten en AEM

Leer hoe te om de aanvankelijke reeks AEM Gebeurtenissen van het dagboek terug te winnen en de details over elke gebeurtenis te onderzoeken.

Journaling is een trekmethode om AEM Gebeurtenissen te verbruiken, en een dagboek is een geordende lijst van gebeurtenissen. Met Adobe I/O Events Journaling API kunt u de AEM Events ophalen uit het dagboek en deze verwerken in uw toepassing. Met deze aanpak kunt u gebeurtenissen beheren op basis van een opgegeven ervaring en deze efficiënt bulksgewijs verwerken. Zie de [Journalating](https://developer.adobe.com/events/docs/guides/journaling_intro/) voor diepgaande inzichten, met inbegrip van essentiële overwegingen zoals bewaartermijnen, paginering, en meer.

In het Adobe Developer-consoleproject wordt elke gebeurtenisregistratie automatisch ingeschakeld voor journalistiek, zodat naadloze integratie mogelijk is.

In dit voorbeeld gebruikt u een Adobe-geleverd _gehoste webtoepassing_ staat u toe om de eerste partij van AEM Gebeurtenissen van het dagboek zonder de behoefte te halen om uw toepassing te vestigen. Deze webtoepassing met Adobe wordt gehost op [Glitch](https://glitch.com/), een platform dat bekend staat om het aanbieden van een webomgeving die bevorderlijk is voor het ontwikkelen en implementeren van webtoepassingen. De optie voor het gebruik van uw eigen toepassing is echter ook beschikbaar als u daar de voorkeur aan geeft.

## Vereisten

U hebt het volgende nodig om deze zelfstudie te voltooien:

- as a Cloud Service omgeving AEM met [AEM Event ingeschakeld](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [Adobe Developer Console-project geconfigureerd voor AEM gebeurtenissen](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## Toegang tot webtoepassing

Ga als volgt te werk om toegang te krijgen tot de door de Adobe verschafte webtoepassing:

- Controleer of u toegang hebt tot de [Glitch - gehoste webtoepassing](https://indigo-speckle-antler.glitch.me/) in een nieuw browsertabblad.

  ![Glitch - gehoste webtoepassing](../assets/examples/journaling/glitch-hosted-web-application.png)

## Projectgegevens van Adobe Developer Console verzamelen

Om de AEM Gebeurtenissen van het dagboek, geloofsbrieven zoals te halen _IMS-organisatie-id_, _Client-id_, en _Toegangstoken_ zijn vereist. Voer de volgende stappen uit om deze gegevens te verzamelen:

- In de [Adobe Developer Console](https://developer.adobe.com), navigeer naar uw project en klik om het te openen.

- Onder de **Credentials** klikt u op de **OAuth Server-to-Server** de koppeling openen **Credentials details** tab.

- Klik op de knop **Toegangstoken genereren** knop om het toegangstoken te genereren.

  ![Toegangstoken genereren voor Adobe Developer-consolProject](../assets/examples/journaling/adobe-developer-console-project-generate-access-token.png)

- De **Gegenereerd toegangstoken**, **CLIENT-ID**, en **ORGANISATIE-ID**. U hebt ze later nodig in deze zelfstudie.

  ![Referenties van Adobe Developer Console-projectkopie](../assets/examples/journaling/adobe-developer-console-project-copy-credentials.png)

- Elke gebeurtenisregistratie wordt automatisch ingeschakeld voor journalistiek. Om de _uniek API-eindpunt voor journalisten_ Klik op de gebeurteniskaart waarop u zich hebt geabonneerd op AEM Gebeurtenissen. Van de **Registratiegegevens** -tabblad, kopieert u de **UNIEKE API-EINDPUNT VERPLAATSEN**.

  ![Adobe Developer Console Project Events Card](../assets/examples/journaling/adobe-developer-console-project-events-card.png)

## AEM Events-journaal laden

Om dingen eenvoudig te houden, haalt deze ontvangen Webtoepassing slechts de eerste partij van AEM Gebeurtenissen van het dagboek. Dit zijn de oudste beschikbare gebeurtenissen in het dagboek. Zie voor meer informatie [eerste reeks gebeurtenissen](https://developer.adobe.com/events/docs/guides/api/journaling_api/#fetching-your-first-batch-of-events-from-the-journal).

- In de [Glitch - gehoste webtoepassing](https://indigo-speckle-antler.glitch.me/), voert u de **IMS-organisatie-id**, **Client-id**, en **Toegangstoken** u hebt eerder gekopieerd uit het Adobe Developer Console-project en klikt op **Verzenden**.

- Als dit lukt, geeft de tabelcomponent de gegevens van het AEM Events Journal weer.

  ![AEM Events Journal-gegevens](../assets/examples/journaling/load-journal.png)

- Dubbelklik op de rij om de volledige gebeurtenislading weer te geven. U kunt zien dat de AEM gebeurtenisdetails alle noodzakelijke informatie hebben om de gebeurtenis in de webhaak te verwerken. Het gebeurtenistype (`type`), gebeurtenisbron (`source`), gebeurtenis-id (`event_id`), tijd van gebeurtenis (`time`) en gebeurtenisgegevens (`data`).

  ![AEM gebeurtenis Payload voltooien](../assets/examples/journaling/complete-journal-data.png)

## Aanvullende bronnen

- [Broncode van Glitch-webhaak](https://glitch.com/edit/#!/indigo-speckle-antler) is beschikbaar ter referentie. Het is een eenvoudige React-toepassing die [Adobe Reageren spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) componenten om UI terug te geven.

- [Adobe I/O Events Journaling API](https://developer.adobe.com/events/docs/guides/api/journaling_api/) biedt gedetailleerde informatie over de API, zoals eerst, volgende en laatste batch gebeurtenissen, paginering en meer.
