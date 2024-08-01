---
title: Journalisten en AEM
description: Leer hoe te om de aanvankelijke reeks AEM Gebeurtenissen van het dagboek terug te winnen en de details over elke gebeurtenis te onderzoeken.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 280
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14734
thumbnail: KT-14734.jpeg
exl-id: 33eb0757-f0ed-4c2d-b8b9-fa6648e87640
source-git-commit: efa0a16649c41fab8309786a766483cfeab98867
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 0%

---

# Journalisten en AEM

Leer hoe te om de aanvankelijke reeks AEM Gebeurtenissen van het dagboek terug te winnen en de details over elke gebeurtenis te onderzoeken.

>[!VIDEO](https://video.tv.adobe.com/v/3427052?quality=12&learn=on)

Journaling is een trekmethode om AEM Gebeurtenissen te verbruiken, en een dagboek is een geordende lijst van gebeurtenissen. Met Adobe I/O Events Journaling API kunt u de AEM Events ophalen uit het dagboek en deze verwerken in uw toepassing. Met deze aanpak kunt u gebeurtenissen beheren op basis van een opgegeven ervaring en deze efficiënt bulksgewijs verwerken. Verwijs naar [ het Journaling ](https://developer.adobe.com/events/docs/guides/journaling_intro/) voor diepgaande inzichten, met inbegrip van essentiële overwegingen zoals bewaartermijnen, paginering, en meer.

In het Adobe Developer Console-project wordt elke gebeurtenisregistratie automatisch ingeschakeld voor journalistiek, zodat naadloze integratie mogelijk is.

In dit voorbeeld, dat een Adobe-Geleverde _ontvangen Webtoepassing_ gebruikt staat u toe om de eerste partij van AEM Gebeurtenissen van het dagboek zonder de behoefte te halen aan opstelling uw toepassing. Deze Adobe-verstrekte Webtoepassing wordt ontvangen op [ Glitch ](https://glitch.com/), een platform gekend voor het aanbieden van een web-based milieu dat aan de bouw van en het opstellen van Webtoepassingen bevordert. De optie voor het gebruik van uw eigen toepassing is echter ook beschikbaar als u daar de voorkeur aan geeft.

## Vereisten

U hebt het volgende nodig om deze zelfstudie te voltooien:

- Het milieu van AEM as a Cloud Service met [ toegelaten AEM Gebeurtenis ](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [ Adobe Developer Console project dat voor AEM Gebeurtenissen ](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console) wordt gevormd.

## Toegang tot webtoepassing

Ga als volgt te werk om toegang te krijgen tot de door de Adobe verschafte webtoepassing:

- Verifieer u tot [ Glitch kunt toegang hebben - ontvangen Webtoepassing ](https://indigo-speckle-antler.glitch.me/) in een nieuwe browser tabel.

  ![ Glitch - ontvangen Webtoepassing ](../assets/examples/journaling/glitch-hosted-web-application.png)

## Adobe Developer Console-projectgegevens verzamelen

Om de AEM Gebeurtenissen van het dagboek te halen, geloofsbrieven zoals _identiteitskaart van de Organisatie IMS_, _identiteitskaart van de Cliënt_, en _Token van de Toegang_ worden vereist. Voer de volgende stappen uit om deze gegevens te verzamelen:

- In [ Adobe Developer Console ](https://developer.adobe.com), navigeer aan uw project en klik om het te openen.

- Onder de **sectie van Referenties**, klik de **Server-aan-Server** verbinding van OAuth om de **details van Credentials** tabel te openen.

- Klik **produceren toegangstoken** knoop om het toegangstoken te produceren.

  ![ Adobe Developer Console Project produceert Token van de Toegang ](../assets/examples/journaling/adobe-developer-console-project-generate-access-token.png)

- Kopieer het **Gegenereerde toegangstoken**, **identiteitskaart van de CLIENT**, en **identiteitskaart van de ORGANISATIE**. U hebt ze later nodig in deze zelfstudie.

  ![ Referenties van het Exemplaar van het Project van Adobe Developer Console ](../assets/examples/journaling/adobe-developer-console-project-copy-credentials.png)

- Elke gebeurtenisregistratie wordt automatisch ingeschakeld voor journalistiek. Om het _unieke het journaling API eindpunt_ van uw gebeurtenisregistratie te krijgen, klik de gebeurteniskaart die aan AEM Gebeurtenissen wordt ingetekend. Van het **lusje van de Details van de Registratie**, kopieer **JOURNALING UNIQUE API EINDPUNT**.

  ![ de Kaart van de Gebeurtenissen van het Project van Adobe Developer Console ](../assets/examples/journaling/adobe-developer-console-project-events-card.png)

## AEM Events-journaal laden

Om dingen eenvoudig te houden, haalt deze ontvangen Webtoepassing slechts de eerste partij van AEM Gebeurtenissen van het dagboek. Dit zijn de oudste beschikbare gebeurtenissen in het dagboek. Voor meer details, zie [ eerste partij van gebeurtenissen ](https://developer.adobe.com/events/docs/guides/api/journaling_api/#fetching-your-first-batch-of-events-from-the-journal).

- In het [ Glitch - ontvangen Webtoepassing ](https://indigo-speckle-antler.glitch.me/), ga **identiteitskaart van de Organisatie IMS**, **identiteitskaart van de Cliënt** in, en **Token van de Toegang** u vroeger van het project van Adobe Developer Console kopieerde en **klikt voorlegt**.

- Als dit lukt, geeft de tabelcomponent de gegevens van het AEM Events Journal weer.

  ![ AEM de Gegevens van het Dagboek van Gebeurtenissen ](../assets/examples/journaling/load-journal.png)

- Dubbelklik op de rij om de volledige gebeurtenislading weer te geven. U kunt zien dat de AEM gebeurtenisdetails alle noodzakelijke informatie hebben om de gebeurtenis in de webhaak te verwerken. Bijvoorbeeld het gebeurtenistype (`type`), gebeurtenisbron (`source`), gebeurtenis id (`event_id`), gebeurtenistijd (`time`) en gebeurtenisgegevens (`data`).

  ![ Volledige AEM Gebeurtenislading ](../assets/examples/journaling/complete-journal-data.png)

## Aanvullende bronnen

- [ WebHaakbroncode van de Glitch ](https://glitch.com/edit/#!/indigo-speckle-antler) is beschikbaar voor verwijzing. Het is een eenvoudige React toepassing die [ Adobe gebruikt Reageer Spectrum ](https://react-spectrum.adobe.com/react-spectrum/index.html) componenten om UI terug te geven.

- [ de Gebeurtenissen van de Adobe I/O die API ](https://developer.adobe.com/events/docs/guides/api/journaling_api/) reizen verstrekt gedetailleerde informatie over API als eerste, volgende, en laatste partij gebeurtenissen, paginering, en meer.
